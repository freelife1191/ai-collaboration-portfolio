# Notion Blog Performance Refactoring Plan

## 현재 상태 분석 (Current State Analysis)

### 1. 데이터 흐름 (Data Flow)

#### 빌드 시점 (Build Time)
```
generateStaticParams()
  → listPublishedPosts()
    → Notion API (데이터베이스 쿼리)
    → 캐시 저장 (10분 TTL)

각 페이지별 generateMetadata()
  → getPostBySlug(slug)
    → Notion API (페이지 + 블록 데이터)
    → fetchBlocksRecursively() (재귀적 블록 가져오기)
    → 캐시 저장

페이지 렌더링 (RSC)
  → getPostBySlug(slug) (메타데이터와 중복 호출)
    → notionRenderer.renderBlocks()
    → HTML 생성 및 캐시
```

#### 런타임 (Runtime - Static Export)
```
- 모든 HTML이 빌드 타임에 생성됨
- 런타임에는 Notion API 호출 없음
- 정적 파일만 서빙
```

### 2. 성능 이슈 분석 (Performance Issues)

#### ✅ 좋은 점 (Strengths)
1. **캐싱 시스템**: `withCache()` 래퍼로 중복 API 호출 방지
2. **병렬 처리**: `Promise.all()` 사용으로 자식 블록 병렬 로딩
3. **재시도 로직**: 5xx 에러에 대한 exponential backoff
4. **Static Export**: 런타임 성능 최적 (API 호출 없음)

#### ⚠️ 문제점 (Issues)

##### Issue 1: 중복 API 호출 (Duplicate API Calls)
```typescript
// src/app/posts/[slug]/page.tsx
export async function generateMetadata({ params }) {
  const post = await notionClient.getPostBySlug(slug) // 첫 번째 호출
  // ...
}

export default async function PostPage({ params }) {
  const post = await notionClient.getPostBySlug(slug) // 두 번째 호출 (중복!)
  // ...
}
```
**영향**: 같은 페이지에 대해 2번의 API 호출 발생 (캐시가 있어도 첫 호출은 캐싱되기 전)

##### Issue 2: 재귀적 블록 로딩의 순차 처리
```typescript
// src/services/notion/client.ts:501-598
async function fetchBlocksRecursively(blockId: string) {
  // ✅ 병렬 처리는 되지만, 깊이가 깊어질수록 성능 저하
  await Promise.all(blocks.map(async (block) => {
    if (block.has_children) {
      const children = await fetchBlocksRecursively(block.id) // 재귀 호출
    }
  }))
}
```
**영향**: 블록 계층이 깊으면 (3-4 depth) 시간이 누적됨

##### Issue 3: 렌더링 시점의 불명확성
```typescript
// client.ts:647-653
const html = process.env.NODE_ENV === 'development'
  ? await notionRenderer.renderBlocks(content) // 개발: 캐시 없음
  : await withCache(...) // 프로덕션: 캐시 있음
```
**영향**: 개발 환경에서는 매번 렌더링되어 성능 테스트 어려움

##### Issue 4: 캐시 TTL 일관성 부족
```typescript
// 각기 다른 TTL 값 사용
POSTS_LIST: 10분
POST_BY_SLUG: 10분
POST_BLOCKS: 10분
YOUTUBE_INFO: 1시간
```
**영향**: 빌드 중 캐시 만료로 불필요한 재호출 가능

---

## 중복 로직 분석 (Duplicate Logic Analysis)

### 발견된 중복 패턴

#### 1. fetchBlocksRecursively 함수 중복
**위치**:
- `src/services/notion/client.ts:501-598` (getPostBySlug 내부)
- `src/services/notion/client.ts:704-757` (getAboutPage 내부)

**중복 코드 분석**:
```typescript
// getPostBySlug 내부 (501-598줄)
async function fetchBlocksRecursively(blockId: string): Promise<BlockObjectResponse[]> {
  const cacheKey = CACHE_KEYS.POST_BLOCKS(blockId);
  const cachedBlocks = await withCache(cacheKey, async () => {
    // 블록 가져오기 로직
    const blocksResponse = await fetch(/* ... */);
    const blocks = await blocksResponse.json();

    // 자식 블록 재귀 처리
    await Promise.all(blocks.map(async (block) => {
      if (block.has_children) {
        const children = await fetchBlocksRecursively(block.id);
        // ...
      }
    }));

    return blocks;
  }, 10 * 60 * 1000);

  return cachedBlocks;
}

// getAboutPage 내부 (704-757줄) - 거의 동일한 로직
async function fetchBlocksRecursively(blockId: string): Promise<BlockObjectResponse[]> {
  // ... 위와 동일한 구조
}
```

**문제점**:
- 97줄의 코드가 두 군데 중복됨
- synced_block, child_database 처리 로직이 getPostBySlug에만 있음
- 유지보수 시 두 곳 모두 수정해야 함

#### 2. 블록 API 호출 패턴 중복
```typescript
// 여러 곳에서 반복되는 패턴
const blocksResponse = await fetch(
  `https://api.notion.com/v1/blocks/${blockId}/children`,
  {
    method: 'GET',
    headers: {
      'Authorization': `Bearer ${apiKey}`,
      'Notion-Version': '2022-06-28',
    },
  }
);
```

#### 3. Notion 데이터베이스 쿼리 패턴 중복
```typescript
// listPublishedPosts, getSiteSettings, getSiteConfig 등에서 반복
const response = await fetch(
  `https://api.notion.com/v1/databases/${databaseId}/query`,
  {
    method: 'POST',
    headers: {
      'Authorization': `Bearer ${apiKey}`,
      'Notion-Version': '2022-06-28',
      'Content-Type': 'application/json',
    },
    body: JSON.stringify(requestBody),
  }
);
```

### 중복 제거 계획

#### Phase 0: 중복 로직 제거 (최우선 - Phase 1 전에 수행)

##### Step 0.1: 공통 블록 로더 모듈 생성
```typescript
// src/services/notion/block-loader.ts (신규 파일)
import type { BlockObjectResponse } from '@notionhq/client/build/src/api-endpoints';
import { withCache, CACHE_KEYS } from '@/lib/cache';

interface BlockLoaderOptions {
  apiKey: string;
  enableSyncedBlocks?: boolean;
  enableChildDatabases?: boolean;
  notionClient?: any; // NotionClientApi for child_database queries
}

/**
 * 재사용 가능한 블록 로더
 * getPostBySlug, getAboutPage 등에서 공통으로 사용
 */
export async function fetchBlocksRecursively(
  blockId: string,
  options: BlockLoaderOptions
): Promise<BlockObjectResponse[]> {
  const { apiKey, enableSyncedBlocks = true, enableChildDatabases = true, notionClient } = options;

  const cacheKey = CACHE_KEYS.POST_BLOCKS(blockId);
  const cachedBlocks = await withCache(
    cacheKey,
    async () => {
      const blocksResponse = await fetch(
        `https://api.notion.com/v1/blocks/${blockId}/children`,
        {
          method: 'GET',
          headers: {
            'Authorization': `Bearer ${apiKey}`,
            'Notion-Version': '2022-06-28',
          },
        }
      );

      if (!blocksResponse.ok) {
        const errorBody = await blocksResponse.text();

        // 400 에러이고 unsupported block type인 경우 경고만 출력하고 빈 배열 반환
        if (blocksResponse.status === 400 && errorBody.includes('is not supported via the API')) {
          console.warn(`[WARN] Skipping unsupported block type for blockId ${blockId}`);
          return [];
        }

        throw new Error(`Notion blocks API returned ${blocksResponse.status} for block ${blockId}: ${errorBody}`);
      }

      const blocksData = await blocksResponse.json();
      const blocks = (blocksData.results || []) as BlockObjectResponse[];

      // 병렬 처리: has_children이 true인 블록의 자식을 병렬로 가져오기
      await Promise.all(
        blocks.map(async (block) => {
          // synced_block 특별 처리
          if (enableSyncedBlocks && block.type === 'synced_block' && 'type' in block) {
            try {
              const syncedBlock = (block as any).synced_block;

              if (syncedBlock?.synced_from?.block_id) {
                const sourceBlockId = syncedBlock.synced_from.block_id;
                const children = await fetchBlocksRecursively(sourceBlockId, options);
                (block as any).children = children;
              } else if (block.has_children) {
                const children = await fetchBlocksRecursively(block.id, options);
                (block as any).children = children;
              }
            } catch (error) {
              console.warn(`[WARN] Failed to fetch synced_block children for ${block.id}:`, error);
            }
          }
          // 일반 블록의 자식 처리
          else if (block.has_children && 'type' in block) {
            const children = await fetchBlocksRecursively(block.id, options);
            const blockType = block.type;
            const blockData = (block as Record<string, unknown>)[blockType];
            if (blockData && typeof blockData === 'object') {
              (blockData as Record<string, unknown>).children = children;
            }
          }

          // child_database 블록 처리
          if (enableChildDatabases && block.type === 'child_database' && 'type' in block && notionClient) {
            try {
              const databaseId = block.id;
              const [databaseRows, propertyNames] = await Promise.all([
                notionClient.queryDatabase(databaseId, 100),
                notionClient.getDatabaseSchema(databaseId)
              ]);

              const blockData = (block as Record<string, unknown>)[block.type];
              if (blockData && typeof blockData === 'object') {
                (blockData as Record<string, unknown>).database_rows = databaseRows;
                (blockData as Record<string, unknown>).property_names = propertyNames;
              }
            } catch (error) {
              console.warn(`[WARN] Failed to fetch child_database data for ${block.id}:`, error);
            }
          }
        })
      );

      return blocks;
    },
    10 * 60 * 1000 // 10분 캐시
  );

  return cachedBlocks;
}
```

##### Step 0.2: Notion API 헬퍼 함수 생성
```typescript
// src/services/notion/api-helpers.ts (신규 파일)
export interface NotionApiOptions {
  apiKey: string;
  version?: string;
}

/**
 * Notion API 공통 fetch 래퍼
 */
export async function notionFetch(
  url: string,
  options: RequestInit & { apiKey: string }
): Promise<Response> {
  const { apiKey, ...fetchOptions } = options;

  return fetch(url, {
    ...fetchOptions,
    headers: {
      'Authorization': `Bearer ${apiKey}`,
      'Notion-Version': '2022-06-28',
      ...fetchOptions.headers,
    },
  });
}

/**
 * 데이터베이스 쿼리 헬퍼
 */
export async function queryNotionDatabase(
  databaseId: string,
  apiKey: string,
  body?: Record<string, any>
): Promise<any> {
  const response = await notionFetch(
    `https://api.notion.com/v1/databases/${databaseId}/query`,
    {
      method: 'POST',
      apiKey,
      headers: {
        'Content-Type': 'application/json',
      },
      body: body ? JSON.stringify(body) : undefined,
    }
  );

  if (!response.ok) {
    throw new Error(`Database query failed: ${response.status}`);
  }

  return response.json();
}

/**
 * 페이지 가져오기 헬퍼
 */
export async function getNotionPage(
  pageId: string,
  apiKey: string
): Promise<any> {
  const response = await notionFetch(
    `https://api.notion.com/v1/pages/${pageId}`,
    {
      method: 'GET',
      apiKey,
    }
  );

  if (!response.ok) {
    throw new Error(`Page fetch failed: ${response.status}`);
  }

  return response.json();
}
```

##### Step 0.3: client.ts 리팩토링
```typescript
// src/services/notion/client.ts
import { fetchBlocksRecursively } from './block-loader';
import { queryNotionDatabase, getNotionPage } from './api-helpers';

// getPostBySlug 내부
async getPostBySlug(slug: string) {
  // ... 기존 코드 ...

  // 기존의 fetchBlocksRecursively 함수 정의 제거
  // 공통 모듈 사용
  const content = await fetchBlocksRecursively(pageId, {
    apiKey,
    enableSyncedBlocks: true,
    enableChildDatabases: true,
    notionClient: clientApi,
  });

  // ... 기존 코드 ...
}

// getAboutPage 내부
async getAboutPage() {
  // ... 기존 코드 ...

  // 기존의 fetchBlocksRecursively 함수 정의 제거
  // 공통 모듈 사용
  const content = await fetchBlocksRecursively(aboutPageId, {
    apiKey,
    enableSyncedBlocks: false, // About 페이지에서는 불필요
    enableChildDatabases: false,
    notionClient: undefined,
  });

  // ... 기존 코드 ...
}
```

**예상 효과**:
- 중복 코드 194줄 제거
- 버그 수정 시 한 곳만 수정
- 테스트 및 유지보수 용이

---

## 리팩토링 계획 (Refactoring Plan)

### Phase 1: 중복 API 호출 제거 (Eliminate Duplicate API Calls)

#### 목표
- `generateMetadata`와 페이지 렌더링에서 동일한 데이터를 2번 호출하는 문제 해결
- Next.js의 Request Memoization 활용

#### 작업 내용

##### Step 1.1: Request Memoization 추가
```typescript
// src/lib/request-memo.ts (신규 파일)
import { cache } from 'react'
import type { PostDetail } from '@/services/notion/client'

/**
 * Next.js Request Memoization을 활용한 데이터 fetch
 * 동일 요청 내에서 중복 호출 방지
 */
export const getPostBySlugMemo = cache(async (slug: string): Promise<PostDetail | null> => {
  const notionClient = createNotionClient()
  return notionClient.getPostBySlug(slug)
})

export const listPublishedPostsMemo = cache(async () => {
  const notionClient = createNotionClient()
  return notionClient.listPublishedPosts()
})
```

##### Step 1.2: 페이지에서 메모이제이션 함수 사용
```typescript
// src/app/posts/[slug]/page.tsx
import { getPostBySlugMemo } from '@/lib/request-memo'

export async function generateMetadata({ params }) {
  const post = await getPostBySlugMemo(slug) // 메모이제이션된 함수
  // ...
}

export default async function PostPage({ params }) {
  const post = await getPostBySlugMemo(slug) // 동일 요청에서는 캐시된 값 반환
  // ...
}
```

**예상 효과**: 빌드 시 API 호출 50% 감소 (페이지당 1회로 제한)

---

### Phase 2: 빌드 시점 캐시 최적화 (Build-Time Cache Optimization)

#### 목표
- 빌드 중 캐시 만료 방지
- 모든 데이터를 한 번만 가져오도록 보장

#### 작업 내용

##### Step 2.1: 빌드 모드 감지 및 캐시 TTL 조정
```typescript
// src/lib/cache.ts
const isBuildTime = process.env.NEXT_PHASE === 'phase-production-build'

export function getBuildTimeTTL(baseTTL: number): number {
  if (isBuildTime) {
    return 60 * 60 * 1000 // 빌드 중에는 1시간 캐시 (충분히 긴 시간)
  }
  return getTTL(baseTTL) // 기존 로직
}
```

##### Step 2.2: 빌드 시작 시 캐시 워밍 (Optional)
```typescript
// scripts/build-cache-warmup.ts (신규 파일)
/**
 * 빌드 전 모든 데이터를 미리 캐시에 저장
 * package.json의 build script에서 실행
 */
async function warmupCache() {
  const notionClient = createNotionClient()

  console.log('[Cache Warmup] Fetching all posts...')
  const posts = await notionClient.listPublishedPosts()

  console.log(`[Cache Warmup] Fetching ${posts.length} post details...`)
  await Promise.all(posts.map(post =>
    notionClient.getPostBySlug(post.slug)
  ))

  console.log('[Cache Warmup] Complete!')
}
```

**예상 효과**: 빌드 중 캐시 만료로 인한 재호출 0%

---

### Phase 3: 블록 로딩 최적화 (Block Loading Optimization)

#### 목표
- 재귀적 블록 로딩 성능 개선
- 깊은 계층 구조에서도 빠른 로딩

#### 작업 내용

##### Step 3.1: 배치 블록 로딩
```typescript
// src/services/notion/block-loader.ts (신규 파일)
/**
 * 여러 블록을 한 번에 로딩하는 배치 함수
 */
async function fetchBlocksBatch(blockIds: string[]): Promise<Map<string, BlockObjectResponse[]>> {
  // 최대 10개씩 병렬 처리
  const batchSize = 10
  const batches = []

  for (let i = 0; i < blockIds.length; i += batchSize) {
    const batch = blockIds.slice(i, i + batchSize)
    batches.push(
      Promise.all(batch.map(async (id) => {
        const blocks = await fetchBlocksForId(id)
        return [id, blocks] as const
      }))
    )
  }

  const results = await Promise.all(batches)
  return new Map(results.flat())
}
```

##### Step 3.2: BFS (Breadth-First Search) 방식으로 변경
```typescript
// 기존: DFS (Depth-First) - 재귀적으로 깊이 우선 탐색
// 문제: 깊은 depth에서 성능 저하

// 개선: BFS (Breadth-First) - 같은 레벨의 블록을 한 번에 로딩
async function fetchBlocksRecursivelyBFS(rootBlockId: string) {
  const allBlocks = new Map<string, BlockObjectResponse[]>()
  const queue: string[] = [rootBlockId]

  while (queue.length > 0) {
    // 현재 레벨의 모든 블록을 병렬로 가져오기
    const currentLevelIds = queue.splice(0, queue.length)
    const blocksMap = await fetchBlocksBatch(currentLevelIds)

    // 결과 저장 및 다음 레벨 큐에 추가
    for (const [blockId, blocks] of blocksMap) {
      allBlocks.set(blockId, blocks)

      blocks.forEach(block => {
        if (block.has_children) {
          queue.push(block.id)
        }
      })
    }
  }

  return allBlocks
}
```

**예상 효과**: 깊은 계층 구조에서 로딩 시간 30-40% 감소

---

### Phase 4: 렌더링 최적화 (Rendering Optimization)

#### 목표
- 렌더링 성능 개선
- 불필요한 렌더링 제거

#### 작업 내용

##### Step 4.1: 렌더링 결과 캐싱 강화
```typescript
// src/services/notion/client.ts
const html = await withCache(
  CACHE_KEYS.POST_RENDERED(slug),
  async () => {
    // 렌더링 시작 로깅
    console.time(`[Render] ${slug}`)
    const result = await notionRenderer.renderBlocks(content)
    console.timeEnd(`[Render] ${slug}`)
    return result
  },
  process.env.NODE_ENV === 'development'
    ? 1 * 60 * 1000  // 개발: 1분 (빠른 피드백)
    : 24 * 60 * 60 * 1000  // 프로덕션: 24시간 (긴 캐시)
)
```

##### Step 4.2: 렌더러 성능 프로파일링 추가
```typescript
// src/services/notion/renderer.ts
renderBlock(block: NotionBlock): string {
  const startTime = performance.now()

  try {
    const result = /* 기존 렌더링 로직 */

    const duration = performance.now() - startTime
    if (duration > 100) {
      console.warn(`[Slow Render] ${block.type} took ${duration}ms`)
    }

    return result
  } catch (error) {
    // ...
  }
}
```

**예상 효과**: 렌더링 병목 지점 파악 및 개선

---

### Phase 5: 모니터링 및 검증 (Monitoring & Verification)

#### 목표
- 빌드 시 데이터 fetch가 정확히 한 번만 일어나는지 검증
- 성능 개선 효과 측정

#### 작업 내용

##### Step 5.1: API 호출 추적 로거
```typescript
// src/lib/api-tracker.ts (신규 파일)
class ApiCallTracker {
  private calls = new Map<string, number>()

  track(key: string) {
    const count = this.calls.get(key) || 0
    this.calls.set(key, count + 1)
  }

  report() {
    console.log('\n=== Notion API Call Report ===')
    for (const [key, count] of this.calls.entries()) {
      console.log(`${key}: ${count} calls`)
      if (count > 1) {
        console.warn(`⚠️ Duplicate calls detected for: ${key}`)
      }
    }
    console.log('==============================\n')
  }
}

export const apiTracker = new ApiCallTracker()
```

##### Step 5.2: 클라이언트에 추적 기능 통합
```typescript
// src/services/notion/client.ts
async getPostBySlug(slug: string) {
  apiTracker.track(`getPostBySlug:${slug}`)

  return withCache(
    CACHE_KEYS.POST_BY_SLUG(slug),
    // ... 기존 로직
  )
}
```

##### Step 5.3: 빌드 시 리포트 출력
```typescript
// next.config.ts
export default {
  // ... 기존 설정
  webpack: (config, { isServer }) => {
    if (isServer && process.env.NODE_ENV === 'production') {
      // 빌드 종료 시 API 호출 리포트 출력
      process.on('beforeExit', () => {
        apiTracker.report()
      })
    }
    return config
  }
}
```

##### Step 5.4: 빌드 성능 측정 스크립트
```bash
# scripts/measure-build-perf.sh (신규 파일)
#!/bin/bash

echo "=== Build Performance Test ==="
echo "Cleaning previous build..."
rm -rf .next out

echo "Starting build with timing..."
time npm run build 2>&1 | tee build.log

echo ""
echo "=== Build Stats ==="
echo "Build duration: $(grep 'Compiled successfully' build.log)"
echo "Total pages: $(ls -1 out/**/*.html | wc -l)"
echo "API calls: $(grep 'Notion API' build.log | wc -l)"
```

**예상 효과**:
- 중복 호출 즉시 감지
- 빌드 시간 측정 및 개선 추적

---

## 예상 성능 개선 (Expected Performance Improvements)

### Before (현재)
- 페이지당 Notion API 호출: **2회** (generateMetadata + 페이지 렌더링)
- 10개 포스트 빌드 시: **20회 API 호출**
- 깊은 블록 계층 (depth 3): **~300ms**
- 빌드 시간 (10 posts): **~2-3분**

### After (리팩토링 후)
- 페이지당 Notion API 호출: **1회** (메모이제이션)
- 10개 포스트 빌드 시: **10회 API 호출** (-50%)
- 깊은 블록 계층 (depth 3): **~200ms** (-33%)
- 빌드 시간 (10 posts): **~1-1.5분** (-40%)

---

## 🚨 리팩토링 엄격 규칙 (Strict Refactoring Rules)

### 필수 준수 사항 (MUST FOLLOW)

#### 규칙 1: 단계별 테스트 필수 (No Skip Testing)
- ✅ **각 Step 완료 후 즉시 테스트**
- ✅ **테스트 통과 확인 후 다음 Step 진행**
- ❌ 여러 Step을 한 번에 구현 금지
- ❌ "나중에 테스트하겠다" 금지

**예시**:
```bash
# ✅ 올바른 방법
1. Step 0.1 구현
2. npm run build (테스트)
3. 결과 확인 및 문제 해결
4. Step 0.2 구현
5. npm run build (테스트)
...

# ❌ 잘못된 방법
1. Step 0.1, 0.2, 0.3 모두 구현
2. npm run build (테스트)
3. 어디서 문제인지 찾기 어려움
```

#### 규칙 2: 빌드 성공 필수 (Build Must Succeed)
- ✅ **각 Step 후 빌드가 성공해야 함**
- ✅ **타입 에러, 런타임 에러 모두 해결**
- ❌ "일단 커밋하고 나중에 고치겠다" 금지

**체크리스트**:
```bash
# 1. 타입 체크
npm run build
# ✅ 빌드 성공 확인

# 2. 생성된 페이지 확인
ls -la out/posts/
# ✅ 모든 페이지 정상 생성 확인

# 3. 로컬 서버에서 확인
npx serve out
# ✅ http://localhost:3000 에서 모든 페이지 정상 동작 확인
```

#### 규칙 3: Git 커밋 규칙 (Git Commit Rules)
- ✅ **각 Step 완료 및 테스트 통과 후 커밋**
- ✅ **커밋 메시지는 명확하고 구체적으로**
- ✅ **롤백이 가능하도록 의미있는 단위로 커밋**
- ❌ 테스트하지 않은 코드 커밋 금지

**커밋 메시지 형식**:
```bash
# ✅ 좋은 예시
refactor: Extract fetchBlocksRecursively to block-loader module

- Create src/services/notion/block-loader.ts
- Move duplicate logic from getPostBySlug and getAboutPage
- Add BlockLoaderOptions interface for flexible configuration
- Test: All pages build successfully, no functionality regression

# ❌ 나쁜 예시
update: refactoring
```

#### 규칙 4: 변경 사항 백업 (Backup Before Changes)
- ✅ **리팩토링 전 현재 작동하는 코드 백업**
- ✅ **각 Phase 시작 전 Git 브랜치 생성**
- ✅ **문제 발생 시 즉시 롤백 가능하도록**

**브랜치 전략**:
```bash
# Phase 시작 전
git checkout -b refactor/phase-0-remove-duplicates
# ... 작업 및 테스트 ...
git commit -m "refactor(phase-0): Complete Step 0.1 - Create block-loader"
git commit -m "refactor(phase-0): Complete Step 0.2 - Create api-helpers"

# Phase 완료 후
git checkout main
git merge refactor/phase-0-remove-duplicates

# 다음 Phase
git checkout -b refactor/phase-1-eliminate-duplicates
```

#### 규칙 5: 성능 측정 필수 (Performance Measurement Required)
- ✅ **리팩토링 전 성능 측정**
- ✅ **리팩토링 후 성능 측정**
- ✅ **성능 개선 효과 문서화**
- ❌ 감각적인 "빨라진 것 같다" 금지

**측정 스크립트**:
```bash
# scripts/measure-performance.sh (신규 생성 필요)
#!/bin/bash

echo "=== Performance Measurement ==="
echo "Date: $(date)"
echo ""

# 빌드 캐시 초기화
rm -rf .next out

# 빌드 시간 측정
echo "Starting build..."
BUILD_START=$(date +%s)
npm run build 2>&1 | tee build.log
BUILD_END=$(date +%s)
BUILD_DURATION=$((BUILD_END - BUILD_START))

echo ""
echo "=== Results ==="
echo "Build Duration: ${BUILD_DURATION}s"
echo "Total Pages: $(find out -name '*.html' | wc -l)"
echo "API Calls: $(grep -i 'getPostBySlug\|listPublishedPosts' build.log | wc -l)"
echo ""
```

#### 규칙 6: 회귀 테스트 (Regression Testing)
- ✅ **기존 기능이 정상 동작하는지 확인**
- ✅ **모든 페이지 렌더링 확인**
- ✅ **콘텐츠 누락 확인**
- ✅ **스타일 깨짐 확인**

**회귀 테스트 체크리스트**:
```markdown
### 페이지 렌더링 테스트
- [ ] 홈페이지 (/) 정상 렌더링
- [ ] 포스트 목록 표시
- [ ] 포스트 상세 페이지 (/posts/[slug]) 정상 렌더링
- [ ] 모든 Notion 블록 타입 정상 렌더링
  - [ ] 텍스트 (paragraph, heading)
  - [ ] 리스트 (bulleted, numbered)
  - [ ] 코드 블록
  - [ ] 이미지
  - [ ] 표 (table)
  - [ ] 수식 (equation)
  - [ ] 콜아웃 (callout)
  - [ ] 인용 (quote)
  - [ ] 동기화 블록 (synced_block)
  - [ ] 자식 데이터베이스 (child_database)

### 기능 테스트
- [ ] 이미지 확대 모달 동작
- [ ] KaTeX 수식 렌더링
- [ ] 태그 필터링 동작
- [ ] 목차 (TOC) 동작
- [ ] 다크모드 전환

### 성능 테스트
- [ ] 페이지 로딩 속도 (< 1초)
- [ ] 빌드 시간 (리팩토링 전보다 개선)
- [ ] API 호출 횟수 (리팩토링 전보다 감소)
```

#### 규칙 7: 문제 발생 시 즉시 중단 (Stop on Error)
- ✅ **에러 발생 시 즉시 작업 중단**
- ✅ **원인 파악 및 해결 후 진행**
- ❌ "일단 진행하면서 고치겠다" 금지

**에러 처리 프로세스**:
```markdown
1. ❌ 에러 발생
   ↓
2. 🛑 작업 중단
   ↓
3. 📝 에러 로그 저장 및 분석
   ↓
4. 🔍 원인 파악
   ↓
5. 🔧 해결 방법 수립
   ↓
6. ✅ 해결 및 테스트
   ↓
7. 📄 해결 내용 문서화
   ↓
8. ▶️ 작업 재개
```

#### 규칙 8: 코드 리뷰 체크리스트 (Self Code Review)
각 Step 완료 후 아래 항목을 스스로 체크:

```markdown
### 코드 품질
- [ ] 중복 코드 없음
- [ ] 함수/변수명이 명확함
- [ ] 주석이 적절함
- [ ] 타입이 명확함 (any 사용 최소화)
- [ ] 에러 핸들링이 적절함

### 성능
- [ ] 불필요한 API 호출 없음
- [ ] 캐싱이 적절히 적용됨
- [ ] 병렬 처리가 가능한 곳은 병렬 처리됨

### 테스트
- [ ] 빌드 성공
- [ ] 모든 페이지 정상 렌더링
- [ ] 기존 기능 정상 동작
- [ ] 성능 측정 완료
```

---

## 실행 순서 (Execution Order)

### Step-by-Step Implementation

**⚠️ 중요: 각 Step 완료 후 반드시 위의 "엄격 규칙"을 준수하여 테스트 및 커밋**

0. **Phase 0** (중복 로직 제거) - 최우선 순위
   - [ ] Step 0.1: 공통 블록 로더 모듈 생성
     - [ ] **테스트**: 빌드 성공, 모든 페이지 정상 렌더링
     - [ ] **커밋**: `refactor(phase-0): Create block-loader module`
   - [ ] Step 0.2: Notion API 헬퍼 함수 생성
     - [ ] **테스트**: 빌드 성공, API 호출 정상 동작
     - [ ] **커밋**: `refactor(phase-0): Create api-helpers module`
   - [ ] Step 0.3: client.ts 리팩토링
     - [ ] **테스트**: 빌드 성공, 기능 회귀 테스트 통과
     - [ ] **커밋**: `refactor(phase-0): Refactor client.ts to use common modules`
   - [ ] **Phase 완료 테스트**: 전체 빌드 및 성능 측정

1. **Phase 1** (중복 API 호출 제거) - 높은 우선순위
   - [ ] Step 1.1: Request memoization 함수 작성
     - [ ] **테스트**: 타입 체크 통과
     - [ ] **커밋**: `feat(phase-1): Add request memoization helpers`
   - [ ] Step 1.2: 페이지에서 메모이제이션 적용
     - [ ] **테스트**: 빌드 성공, API 호출 횟수 50% 감소 확인
     - [ ] **커밋**: `refactor(phase-1): Apply request memoization to pages`
   - [ ] **Phase 완료 테스트**: API 호출 추적 및 성능 측정

2. **Phase 5** (모니터링) - 측정 도구 구축
   - [ ] Step 5.1-5.3: API 추적 시스템 구축
     - [ ] **테스트**: 추적 로그 정상 출력
     - [ ] **커밋**: `feat(phase-5): Add API call tracker`
   - [ ] Step 5.4: 빌드 성능 측정 스크립트
     - [ ] **테스트**: 스크립트 실행 및 리포트 생성
     - [ ] **커밋**: `feat(phase-5): Add build performance measurement script`
   - [ ] **Phase 완료 테스트**: 빌드하여 리포트 확인

3. **Phase 2** (캐시 최적화)
   - [ ] Step 2.1: 빌드 모드별 TTL 조정
     - [ ] **테스트**: 빌드 중 캐시 만료 없음 확인
     - [ ] **커밋**: `perf(phase-2): Optimize cache TTL for build time`
   - [ ] Step 2.2: 캐시 워밍 (Optional)
     - [ ] **테스트**: 워밍 후 빌드 속도 개선 확인
     - [ ] **커밋**: `perf(phase-2): Add cache warmup script`
   - [ ] **Phase 완료 테스트**: 빌드 중 캐시 히트율 확인

4. **Phase 3** (블록 로딩 최적화)
   - [ ] Step 3.1: 배치 블록 로딩 구현
     - [ ] **테스트**: 병렬 로딩 확인
     - [ ] **커밋**: `perf(phase-3): Implement batch block loading`
   - [ ] Step 3.2: BFS 방식으로 변경
     - [ ] **테스트**: 깊은 계층 페이지 로딩 시간 30% 감소 확인
     - [ ] **커밋**: `perf(phase-3): Change to BFS block loading`
   - [ ] **Phase 완료 테스트**: 깊은 계층 페이지 로딩 시간 측정

5. **Phase 4** (렌더링 최적화)
   - [ ] Step 4.1: 렌더링 캐싱 강화
     - [ ] **테스트**: 캐시 히트율 확인
     - [ ] **커밋**: `perf(phase-4): Strengthen rendering cache`
   - [ ] Step 4.2: 성능 프로파일링 추가
     - [ ] **테스트**: 병목 지점 파악
     - [ ] **커밋**: `feat(phase-4): Add rendering performance profiling`
   - [ ] **Phase 완료 테스트**: 렌더링 시간 측정

### 📝 Progress Tracking Template

각 Phase 진행 시 아래 템플릿을 복사하여 진행 상황 기록:

```markdown
## Phase X Progress

**시작 시간**: YYYY-MM-DD HH:MM
**완료 시간**: YYYY-MM-DD HH:MM

### 성능 측정 (Before)
- 빌드 시간: Xs
- API 호출: X회
- 페이지 수: X개

### Step 진행 상황
- [x] Step X.1
  - 변경 파일: file1.ts, file2.ts
  - 테스트 결과: ✅ 통과
  - 커밋: abc1234
- [x] Step X.2
  - 변경 파일: file3.ts
  - 테스트 결과: ✅ 통과
  - 커밋: def5678

### 성능 측정 (After)
- 빌드 시간: Ys (-Z%)
- API 호출: Y회 (-Z%)
- 페이지 수: Y개

### 발견된 이슈
1. 이슈 설명
   - 원인: ...
   - 해결: ...

### 배운 점 / 개선 사항
- ...

### 다음 Phase 준비 사항
- ...
```

---

## 테스트 체크리스트 (Testing Checklist)

### 각 단계별 테스트

#### Phase 1 테스트
```bash
# 1. 빌드 실행
npm run build

# 2. 빌드 로그에서 확인
# - "getPostBySlug" API 호출이 페이지당 1회만 나타나는지 확인
# - 중복 호출 경고가 없는지 확인

# 3. 생성된 페이지 확인
ls -la out/posts/
# 모든 포스트 페이지가 정상적으로 생성되었는지 확인
```

#### Phase 2 테스트
```bash
# 1. 빌드 중 캐시 히트율 확인
npm run build | grep "Cache hit"

# 2. 빌드 시간 측정
time npm run build

# 3. 두 번 연속 빌드하여 캐시 효과 확인
npm run build
npm run build
# 두 번째 빌드가 더 빠른지 확인
```

#### Phase 3 테스트
```bash
# 1. 깊은 계층 구조의 테스트 페이지 생성
# 2. 블록 로딩 시간 로그 확인
npm run build | grep "Block loading"

# 3. 병렬 처리 확인
# 로그에서 동시에 여러 블록이 로딩되는지 확인
```

#### Integration 테스트
```bash
# 1. 전체 빌드 성능 측정
./scripts/measure-build-perf.sh

# 2. 생성된 HTML 검증
# - 모든 페이지가 정상적으로 렌더링되었는지
# - 콘텐츠가 누락되지 않았는지

# 3. 로컬 서버에서 확인
npm run build
npx serve out
# http://localhost:3000 에서 모든 페이지 동작 확인
```

---

## 롤백 계획 (Rollback Plan)

각 Phase별로 독립적으로 구현하므로, 문제 발생 시 해당 Phase만 롤백 가능:

1. **Phase 1 롤백**: `request-memo.ts` 파일 삭제 및 기존 코드로 복원
2. **Phase 2 롤백**: 캐시 TTL 설정 원복
3. **Phase 3 롤백**: 블록 로딩 로직 원복 (재귀 방식)
4. **Phase 4 롤백**: 렌더링 로직 원복
5. **Phase 5**: 모니터링 코드는 제거해도 기능에 영향 없음

---

## 성공 지표 (Success Metrics)

### 정량적 지표
- [ ] 빌드 시 API 호출 50% 감소 (페이지당 1회)
- [ ] 빌드 시간 30-40% 감소
- [ ] 깊은 계층 블록 로딩 시간 30% 감소
- [ ] 중복 API 호출 0건

### 정성적 지표
- [ ] 모든 페이지가 정상적으로 렌더링됨
- [ ] 개발 환경에서도 빠른 피드백 (1분 캐시)
- [ ] 빌드 로그가 명확하고 추적 가능
- [ ] 코드 가독성 및 유지보수성 향상

---

## 다음 단계 (Next Steps)

1. ✅ 현재 상태 커밋 완료
2. ✅ 리팩토링 계획 문서 작성 완료
3. ⏭️ Phase 1 구현 시작
4. ⏭️ 각 Phase별 테스트 및 검증
5. ⏭️ 최종 성능 리포트 작성

---

## 📊 Quick Summary (빠른 요약)

### 주요 개선 목표
1. **중복 로직 제거**: 194줄의 중복 코드 제거
2. **중복 API 호출 제거**: 페이지당 API 호출 50% 감소 (2회 → 1회)
3. **빌드 시간 단축**: 40% 감소 목표 (2-3분 → 1-1.5분)
4. **블록 로딩 최적화**: 깊은 계층 30-40% 성능 개선

### 리팩토링 Phase 순서
```
Phase 0 (중복 로직 제거)
  ↓
Phase 1 (중복 API 호출 제거)
  ↓
Phase 5 (모니터링 시스템)
  ↓
Phase 2 (캐시 최적화)
  ↓
Phase 3 (블록 로딩 최적화)
  ↓
Phase 4 (렌더링 최적화)
```

### 핵심 엄격 규칙
1. ✅ 각 Step 완료 후 즉시 테스트
2. ✅ 빌드 성공 필수
3. ✅ 테스트 통과 후 커밋
4. ✅ Git 브랜치로 백업
5. ✅ 성능 측정 필수
6. ✅ 회귀 테스트 필수
7. ✅ 에러 발생 시 즉시 중단
8. ✅ 코드 리뷰 체크리스트

### 시작하기 전 준비사항
```bash
# 1. 현재 성능 측정
scripts/measure-performance.sh

# 2. 백업 브랜치 생성
git checkout -b refactor/phase-0-remove-duplicates

# 3. Phase 0 시작
# 문서의 "Phase 0" 섹션 참조
```

---

**작성일**: 2025-10-23
**최종 수정**: 2025-10-23
**버전**: 2.0
**상태**: 계획 수립 완료 / 구현 대기

**변경 이력**:
- v1.0 (2025-10-23): 초기 리팩토링 계획 수립
- v2.0 (2025-10-23): 중복 로직 분석 추가, 엄격한 테스트 규칙 추가, Phase 0 추가
