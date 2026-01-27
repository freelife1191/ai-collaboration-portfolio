# Notion 블록 타입 지원 현황 분석 및 개선 계획

## 📊 현재 지원 현황

### ✅ 완전 지원 (27개 블록 타입)

현재 프로젝트의 `src/services/notion/renderer.ts`에서 구현된 블록 타입:

**텍스트 & 기본 콘텐츠 (9개)**
- ✅ `paragraph` - 문단 (line 233)
- ✅ `heading_1`, `heading_2`, `heading_3` - 제목 (line 290)
- ✅ `bulleted_list_item` - 불릿 리스트 (line 310)
- ✅ `numbered_list_item` - 숫자 리스트 (line 317)
- ✅ `to_do` - 체크박스 (line 324)
- ✅ `toggle` - 접기/펼치기 (line 333)
- ✅ `quote` - 인용구 (line 467)
- ✅ `callout` - 콜아웃 (line 474)
- ✅ `code` - 코드 블록 (line 350)

**레이아웃 블록 (6개)**
- ✅ `divider` - 구분선 (line 491)
- ✅ `breadcrumb` - 경로 표시 (line 981)
- ✅ `column_list` - 컬럼 레이아웃 (line 645)
- ✅ `column` - 컬럼 (line 663)
- ✅ `table` - 테이블 (line 594)
- ✅ `table_of_contents` - 목차 (line 833)

**미디어 블록 (5개)**
- ✅ `image` - 이미지 (line 495)
- ✅ `video` - 비디오 (line 518)
- ✅ `audio` - 오디오 (line 861)
- ✅ `file` - 파일 (line 573)
- ✅ `pdf` - PDF (line 790)

**임베드 & 링크 블록 (4개)**
- ✅ `bookmark` - 북마크 (line 670)
- ✅ `embed` - 임베드 (line 720)
- ✅ `link_preview` - 링크 미리보기 (line 810)
- ✅ `link_to_page` - 페이지 링크 (line 1016)

**데이터베이스 블록 (2개)**
- ✅ `child_database` - 하위 데이터베이스 (line 890)
- ✅ `child_page` - 하위 페이지 (line 958)

**특수 블록 (3개)**
- ✅ `equation` - 수식 (line 845)
- ✅ `synced_block` - 동기화 블록 (line 921)
- ✅ `template` - 템플릿 (line 996)

### ⚠️ 누락된 블록 타입

Notion API 공식 문서에 명시되어 있지만 현재 구현되지 않은 블록:

1. **`table_row`** - 테이블 행 (개별 처리)
   - 현재는 `table` 블록의 자식으로만 처리
   - 독립적인 `table_row` 블록은 미지원

2. **Rich Text 내 Mention 타입**
   - ✅ `page` mention - 구현됨 (line 187)
   - ✅ `user` mention - 구현됨 (line 192)
   - ✅ `date` mention - 구현됨 (line 198)
   - ✅ `link_preview` mention - 구현됨 (line 202)
   - ⚠️ `database` mention - 미구현

3. **`unsupported`** 블록
   - Notion API에서 아직 지원하지 않는 블록 타입의 플레이스홀더
   - 현재는 console.warn만 출력 (line 150)

## 🔍 다른 블로그 서비스의 Notion 변환 방식

### 1. react-notion-x (가장 인기)

**특징:**
- Notion의 비공식 API 사용 (`notion-types` 패키지)
- 거의 모든 블록 타입 지원 (calendar view 제외)
- 실제 Notion UI와 거의 동일한 렌더링
- Database/Collection 완전 지원
- 번들 크기: ~28kb (gzipped)

**장점:**
- 가장 완전한 블록 타입 지원
- Notion UI와 동일한 디자인
- Database view (table, gallery, list, board) 지원

**단점:**
- 비공식 API 사용 (공식 토큰 시스템 불가)
- 번들 크기가 큼
- Next.js 전용 설계

**사용 사례:**
```tsx
import { NotionRenderer } from 'react-notion-x';

<NotionRenderer
  recordMap={recordMap}
  fullPage={true}
  darkMode={false}
/>
```

### 2. notion-to-md (공식 API 호환)

**특징:**
- Notion 공식 API 사용
- Markdown/MDX/HTML/LaTeX 변환
- 플러그인 시스템으로 커스터마이징 가능
- 자동으로 자식 블록 fetch

**장점:**
- 공식 API 토큰 사용 가능
- Markdown 기반 SSG와 통합 용이
- 가벼운 번들 크기
- 플러그인으로 확장 가능

**단점:**
- UI 렌더링은 직접 구현 필요
- Database view 미지원
- 일부 블록 타입 제한적 지원

**사용 사례:**
```javascript
const n2m = new NotionToMarkdown({ notionClient: notion });
const mdblocks = await n2m.pageToMarkdown(pageId);
const mdString = n2m.toMarkdownString(mdblocks);
```

### 3. notion-blocks-to-markdown

**특징:**
- `notion-to-md`와 유사하나 자동 fetch 없음
- 이미 fetch된 블록만 변환
- 더 가벼운 구현

## 📋 현재 프로젝트 구현 분석

### 강점

1. **Rich Text 처리 우수**
   - Bold, Italic, Strikethrough, Underline, Code 지원
   - Link, Mention (page, user, date, link_preview) 지원
   - Color 속성 고려

2. **미디어 블록 완전 지원**
   - YouTube, Vimeo 임베드 자동 인식
   - 반응형 비디오 컨테이너
   - 이미지 lazy loading

3. **레이아웃 블록 우수**
   - Column layout 반응형 그리드
   - Table with header support
   - Toggle 인터랙티브 구현

4. **코드 블록 고급 기능**
   - 30개 이상 언어 지원
   - 복사 버튼 내장
   - 언어 레이블 표시

### 개선 필요 사항

1. **Database Mention 미지원**
   - `renderTextElement`에서 database mention 처리 추가 필요

2. **Unsupported Block 처리**
   - 현재는 console.warn만 출력하고 빈 문자열 반환
   - 사용자에게 명확한 플레이스홀더 표시 필요

3. **Child Block Fetching**
   - 일부 블록(toggle, callout 등)의 자식 블록은 수동 처리
   - 자동 fetch 로직 없음 (NotionClient에서 처리)

4. **Table Row 독립 블록**
   - `table_row`를 독립 블록으로 처리하는 케이스 미지원

5. **수식(Equation) 렌더링**
   - KaTeX 라이브러리 미설치
   - 현재는 `$$` 표기만 출력 (클라이언트에서 처리 필요)

## 🎯 개선 계획

### Phase 1: 즉시 개선 가능 (1-2시간)

#### 1.1 Database Mention 지원 추가

**위치:** `src/services/notion/renderer.ts:183-206`

```typescript
// Database mention 추가
else if (mention.type === 'database' && mention.database) {
  const databaseId = mention.database.id;
  content = `<a href="/databases/${databaseId}" class="text-blue-600 dark:text-blue-400 hover:text-blue-800 dark:hover:text-blue-300 underline inline-flex items-center gap-1">
    <svg class="w-3 h-3" fill="none" stroke="currentColor" viewBox="0 0 24 24">
      <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 7v10c0 2.21 3.582 4 8 4s8-1.79 8-4V7M4 7c0 2.21 3.582 4 8 4s8-1.79 8-4M4 7c0-2.21 3.582-4 8-4s8 1.79 8 4"></path>
    </svg>
    ${content}
  </a>`;
}
```

#### 1.2 Unsupported Block 시각화

**위치:** `src/services/notion/renderer.ts:149-152`

```typescript
default:
  console.warn(`Unsupported block type: ${block.type}`);
  return `<div class="unsupported-block my-4 p-4 bg-yellow-50 dark:bg-yellow-900/20 border border-yellow-200 dark:border-yellow-800 rounded-lg">
    <div class="flex items-center gap-2 text-yellow-700 dark:text-yellow-400">
      <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 9v2m0 4h.01m-6.938 4h13.856c1.54 0 2.502-1.667 1.732-3L13.732 4c-.77-1.333-2.694-1.333-3.464 0L3.34 16c-.77 1.333.192 3 1.732 3z"></path>
      </svg>
      <span class="font-medium">지원되지 않는 블록 타입: ${block.type}</span>
    </div>
    <p class="text-sm text-yellow-600 dark:text-yellow-500 mt-2">
      이 블록은 현재 렌더링할 수 없습니다. Notion에서 확인해주세요.
    </p>
  </div>`;
```

#### 1.3 Table Row 독립 블록 처리

**위치:** `src/services/notion/renderer.ts:88` (switch 문에 추가)

```typescript
case 'table_row':
  return this.renderTableRow(block);
```

```typescript
private renderTableRow(block: NotionBlock): string {
  // 독립 table_row 블록 (테이블 외부에서 사용되는 경우)
  const cells = block.table_row?.cells || [];

  return `<div class="table-row-standalone my-2 p-3 bg-gray-50 dark:bg-gray-800 rounded border border-gray-200 dark:border-gray-700">
    <div class="flex flex-wrap gap-4">
      ${cells.map((cell: any, index: number) => {
        const cellText = this.renderRichText(cell || []);
        return `<div class="flex-1 min-w-[150px]">
          <span class="text-sm text-gray-500 dark:text-gray-400">열 ${index + 1}:</span>
          <div class="text-gray-700 dark:text-gray-300">${cellText}</div>
        </div>`;
      }).join('')}
    </div>
  </div>`;
}
```

### Phase 2: 중기 개선 (1일)

#### 2.1 KaTeX 수식 렌더링 구현

**설치:**
```bash
npm install katex
npm install --save-dev @types/katex
```

**클라이언트 사이드 렌더링 추가:**

`src/app/posts/[slug]/page.tsx`에 useEffect 추가:

```typescript
useEffect(() => {
  // KaTeX 수식 렌더링
  import('katex').then(katex => {
    document.querySelectorAll('.katex-expression').forEach((el) => {
      const expr = el.getAttribute('data-expr');
      if (expr) {
        try {
          katex.render(expr, el as HTMLElement, {
            displayMode: true,
            throwOnError: false
          });
        } catch (e) {
          console.error('KaTeX rendering error:', e);
        }
      }
    });
  });
}, [content]);
```

**CSS 추가 (`src/app/globals.css`):**
```css
@import 'katex/dist/katex.min.css';
```

#### 2.2 코드 블록 복사 버튼 동작 구현

현재 코드 블록에는 복사 버튼 UI만 있고 동작이 없음.

`src/app/posts/[slug]/page.tsx`에 추가:

```typescript
useEffect(() => {
  // 코드 복사 버튼 이벤트
  document.querySelectorAll('[data-copy-btn]').forEach(btn => {
    btn.addEventListener('click', async (e) => {
      const blockId = (e.currentTarget as HTMLElement).getAttribute('data-copy-btn');
      const codeBlock = document.querySelector(`[data-code-block="${blockId}"] code`);

      if (codeBlock) {
        const code = codeBlock.textContent || '';
        await navigator.clipboard.writeText(code);

        // 버튼 텍스트 변경
        const btnEl = e.currentTarget as HTMLElement;
        const originalText = btnEl.textContent;
        btnEl.textContent = 'Copied!';
        setTimeout(() => {
          btnEl.textContent = originalText;
        }, 2000);
      }
    });
  });
}, [content]);
```

### Phase 3: 장기 고려 사항 (선택적)

#### 3.1 notion-to-md 통합 (하이브리드 접근)

현재 커스텀 렌더러가 잘 작동하고 있으므로, 완전 교체보다는 **특정 블록 타입만 notion-to-md로 처리**하는 하이브리드 방식 고려:

```typescript
import { NotionToMarkdown } from 'notion-to-md';

// 복잡한 블록만 notion-to-md 사용
const complexBlocks = ['synced_block', 'template', 'child_database'];

if (complexBlocks.includes(block.type)) {
  const n2m = new NotionToMarkdown({ notionClient });
  const mdBlocks = await n2m.blockToMarkdown(block.id);
  return this.markdownToHtml(mdBlocks);
} else {
  return this.renderBlock(block); // 기존 렌더러 사용
}
```

**장점:**
- 현재 구현 유지하면서 복잡한 블록만 위임
- Markdown 변환 후 재렌더링으로 스타일 통일성 유지

**단점:**
- 의존성 추가
- 두 가지 렌더링 방식 혼재

#### 3.2 react-notion-x 통합 (완전 교체)

**권장하지 않는 이유:**
1. 비공식 API 사용 (현재 공식 API 사용 중)
2. 현재 커스텀 렌더러가 이미 27개 블록 타입 지원
3. 번들 크기 증가
4. 현재 Tailwind 기반 디자인과 충돌 가능

**고려할 만한 경우:**
- Database view (table, gallery, board) 렌더링이 필수적인 경우
- Notion의 모든 인터랙션 그대로 재현이 필요한 경우

## ✅ 최종 권장 사항

### 즉시 적용 (Phase 1)

1. ✅ **Database Mention 지원 추가** - 10분
2. ✅ **Unsupported Block 시각화** - 10분
3. ✅ **Table Row 독립 블록 처리** - 15분

**예상 소요 시간:** 35분
**효과:** 누락된 블록 타입 완전 커버, 사용자 경험 개선

### 1주일 내 적용 (Phase 2)

4. ✅ **KaTeX 수식 렌더링** - 2시간
5. ✅ **코드 복사 버튼 동작** - 1시간

**예상 소요 시간:** 3시간
**효과:** 수식 블록 완전 지원, 코드 블록 UX 개선

### 장기 검토 (Phase 3)

6. ⏸️ **notion-to-md 하이브리드 통합** - 선택적
7. ❌ **react-notion-x 통합** - 권장하지 않음

**결론:** 현재 커스텀 렌더러가 매우 우수하며, Phase 1-2 개선으로 완전한 Notion 블록 지원 달성 가능합니다.

## 📊 비교표

| 블록 타입 | 현재 | Phase 1 후 | Phase 2 후 | react-notion-x | notion-to-md |
|----------|------|-----------|-----------|----------------|--------------|
| 기본 텍스트 | ✅ | ✅ | ✅ | ✅ | ✅ |
| 미디어 | ✅ | ✅ | ✅ | ✅ | ✅ |
| 레이아웃 | ✅ | ✅ | ✅ | ✅ | ⚠️ |
| Database Mention | ❌ | ✅ | ✅ | ✅ | ⚠️ |
| Unsupported 표시 | ⚠️ | ✅ | ✅ | ✅ | ⚠️ |
| 수식 렌더링 | ⚠️ | ⚠️ | ✅ | ✅ | ✅ |
| 코드 복사 | ⚠️ | ⚠️ | ✅ | ✅ | ❌ |
| Database View | ❌ | ❌ | ❌ | ✅ | ❌ |
| 번들 크기 | 작음 | 작음 | 중간 | 큼 | 작음 |
| 공식 API | ✅ | ✅ | ✅ | ❌ | ✅ |
| 커스터마이징 | 완전 | 완전 | 완전 | 제한적 | 중간 |

**범례:**
- ✅ 완전 지원
- ⚠️ 부분 지원 또는 개선 필요
- ❌ 미지원
