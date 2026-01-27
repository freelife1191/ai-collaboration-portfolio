# 테스트 및 검증 계획서
## [프로젝트 이름] - [프로젝트 설명]

**문서 버전**: 1.0
**작성일**: [작성일]
**작성자**: [작성자]
**테스트 프레임워크 버전**: [버전]

---

## 📋 목차

1. [테스트 전략 개요](#테스트-전략-개요)
2. [테스트 환경 설정](#테스트-환경-설정)
3. [단위 테스트 (Unit Tests)](#단위-테스트)
4. [통합 테스트 (Integration Tests)](#통합-테스트)
5. [E2E 테스트 (End-to-End Tests)](#e2e-테스트)
6. [성능 테스트 (Performance Tests)](#성능-테스트)
7. [보안 검토 (Security Review)](#보안-검토)
8. [코드 리뷰 프로세스](#코드-리뷰-프로세스)
9. [CI/CD 파이프라인](#cicd-파이프라인)
10. [테스트 커버리지 목표](#테스트-커버리지-목표)
11. [버그 추적 및 관리](#버그-추적-및-관리)
12. [테스트 자동화](#테스트-자동화)
13. [품질 메트릭](#품질-메트릭)

---

## 테스트 전략 개요

### 테스트 피라미드

```
         /\
        /  \
       / E2E \         [비율]% - [N]개 테스트
      /______\
     /        \
    / Integration \    [비율]% - [N]개 테스트
   /______________\
  /                \
 /   Unit Tests     \  [비율]% - [N]개 테스트
/____________________\
```

### 테스트 원칙

1. **[원칙 1]**: [설명]
2. **[원칙 2]**: [설명]
3. **[원칙 3]**: [설명]
4. **[원칙 4]**: [설명]
5. **[원칙 5]**: [설명]

### 테스트 범위

| 계층 | 테스트 유형 | 목표 커버리지 | 우선순위 |
|------|------------|--------------|----------|
| **[계층 1]** | Unit | [%] | 높음 |
| **[계층 2]** | Integration | [%] | 높음 |
| **[계층 3]** | E2E | [%] | 중간 |
| **[계층 4]** | Performance | [기준] | 중간 |
| **[계층 5]** | Security | [기준] | 높음 |

---

## 테스트 환경 설정

### 기술 스택

| 도구/라이브러리 | 용도 | 버전 |
|----------------|------|------|
| [테스트 프레임워크] | Unit/Integration 테스팅 | [버전] |
| [E2E 프레임워크] | E2E 테스팅 | [버전] |
| [성능 도구] | 성능 테스팅 | [버전] |
| [보안 도구] | 보안 스캐닝 | [버전] |
| [커버리지 도구] | 코드 커버리지 측정 | [버전] |
| [Mocking 라이브러리] | API/DB 모킹 | [버전] |

### 테스트 환경

**Development**:
```bash
[ENV_VAR_1]=[값]
[ENV_VAR_2]=[값]
[ENV_VAR_3]=[테스트DB URL]
```

**CI/CD**:
```bash
[ENV_VAR_1]=[값]
[ENV_VAR_2]=[값]
[ENV_VAR_3]=[CI DB URL]
```

### 설치 및 설정

```bash
# 테스트 라이브러리 설치
[패키지 관리자] add -D [테스트 프레임워크] [assertion 라이브러리]
[패키지 관리자] add -D [E2E 프레임워크] [playwright/cypress]
[패키지 관리자] add -D [커버리지 도구] [mocking 라이브러리]

# 설정 파일 생성
npx [테스트 프레임워크] init
```

**[설정 파일명]**:
```typescript
// [설정 파일 경로]
import { defineConfig } from '[테스트 프레임워크]'

export default defineConfig({
  testEnvironment: '[환경]',
  setupFilesAfterEnv: ['[setup 파일 경로]'],
  collectCoverageFrom: [
    '[경로 패턴1]',
    '[경로 패턴2]',
    '![제외 패턴]',
  ],
  coverageThresholds: {
    global: {
      branches: [%],
      functions: [%],
      lines: [%],
      statements: [%],
    },
  },
  testMatch: [
    '[테스트 파일 패턴]',
  ],
  moduleNameMapper: {
    '^@/(.*)$': '<rootDir>/[경로]/$1',
  },
})
```

---

## 단위 테스트

### 테스트 전략

**목표**:
- [목표 1]
- [목표 2]
- [목표 3]

**범위**:
- [범위 1]: [설명]
- [범위 2]: [설명]
- [범위 3]: [설명]

### 테스트 구조

```
[테스트 디렉토리]/
├── unit/
│   ├── [카테고리1]/
│   │   ├── [기능1].test.ts
│   │   ├── [기능2].test.ts
│   │   └── [기능3].test.ts
│   ├── [카테고리2]/
│   │   ├── [기능1].test.ts
│   │   └── [기능2].test.ts
│   └── [카테고리3]/
│       └── [기능].test.ts
```

### 예제: [기능] 단위 테스트

**테스트 대상 코드**:
```typescript
// [소스 파일 경로]
export function [함수명](
  [파라미터1]: [타입],
  [파라미터2]: [타입]
): [반환타입] {
  if (!validate[함수](([파라미터1])) {
    throw new Error('[에러 메시지]')
  }

  const [변수] = [처리로직]([파라미터2])

  return {
    [필드1]: [값],
    [필드2]: [값],
    [필드3]: [변수],
  }
}
```

**테스트 코드**:
```typescript
// [테스트 파일 경로]
import { describe, it, expect, beforeEach, afterEach } from '[테스트 프레임워크]'
import { [함수명] } from '[소스 경로]'

describe('[함수명]', () => {
  // Setup
  beforeEach(() => {
    // [setup 로직]
  })

  afterEach(() => {
    // [cleanup 로직]
  })

  describe('[시나리오 그룹 1]', () => {
    it('should [기대 동작 1]', () => {
      // Arrange
      const [입력1] = '[테스트 값]'
      const [입력2] = { [필드]: '[값]' }

      // Act
      const result = [함수명]([입력1], [입력2])

      // Assert
      expect(result).toBeDefined()
      expect(result.[필드1]).toBe('[예상값]')
      expect(result.[필드2]).toContain('[예상값]')
    })

    it('should [기대 동작 2]', () => {
      const [입력] = '[잘못된 값]'

      expect(() => [함수명]([입력], {})).toThrow('[에러 메시지]')
    })
  })

  describe('[시나리오 그룹 2]', () => {
    it('should [기대 동작 3]', async () => {
      // Arrange
      const [mock데이터] = { [필드]: '[값]' }

      // Act
      const result = await [비동기함수]([mock데이터])

      // Assert
      expect(result).toEqual(
        expect.objectContaining({
          [필드]: expect.any([타입]),
        })
      )
    })
  })

  describe('Edge Cases', () => {
    it('should handle [엣지 케이스 1]', () => {
      expect([함수명](null, {})).toBeNull()
    })

    it('should handle [엣지 케이스 2]', () => {
      const [대용량입력] = new Array([크기]).fill('[값]')
      expect(() => [함수명]([대용량입력], {})).not.toThrow()
    })
  })
})
```

### Mocking 전략

**API 호출 Mocking**:
```typescript
// [테스트 파일]
import { vi } from '[테스트 프레임워크]'
import { [API함수] } from '[경로]'

vi.mock('[경로]', () => ({
  [API함수]: vi.fn(),
}))

describe('[컴포넌트/함수명]', () => {
  it('should [기대 동작]', async () => {
    // Mock 구현
    const [mock함수] = [API함수] as jest.MockedFunction<typeof [API함수]>
    [mock함수].mockResolvedValue({
      [필드]: '[값]',
      [필드]: '[값]',
    })

    // 테스트 실행
    const result = await [함수]()

    // 검증
    expect([mock함수]).toHaveBeenCalledWith(
      expect.objectContaining({
        [파라미터]: '[값]',
      })
    )
    expect(result).toEqual(expect.any([타입]))
  })
})
```

**Database Mocking**:
```typescript
// [테스트 파일]
import { vi } from '[테스트 프레임워크]'
import { [DB클라이언트] } from '[경로]'

vi.mock('[경로]', () => ({
  [DB클라이언트]: {
    from: vi.fn(() => ({
      select: vi.fn(() => ({
        eq: vi.fn(() => ({
          single: vi.fn(() => Promise.resolve({
            data: { [필드]: '[값]' },
            error: null,
          })),
        })),
      })),
    })),
  },
}))
```

### 테스트 커버리지 실행

```bash
# 전체 단위 테스트 실행
[패키지 관리자] test:unit

# 커버리지와 함께 실행
[패키지 관리자] test:unit --coverage

# Watch 모드
[패키지 관리자] test:unit --watch

# 특정 파일만 테스트
[패키지 관리자] test:unit [파일명]
```

### 단위 테스트 체크리스트

**[카테고리 1]**:
- [ ] [함수/클래스 1] - [N]개 테스트 케이스
- [ ] [함수/클래스 2] - [N]개 테스트 케이스
- [ ] [함수/클래스 3] - [N]개 테스트 케이스

**[카테고리 2]**:
- [ ] [함수/클래스 1] - [N]개 테스트 케이스
- [ ] [함수/클래스 2] - [N]개 테스트 케이스

**[카테고리 3]**:
- [ ] [함수/클래스 1] - [N]개 테스트 케이스
- [ ] [함수/클래스 2] - [N]개 테스트 케이스

---

## 통합 테스트

### 테스트 전략

**목표**:
- [목표 1]
- [목표 2]
- [목표 3]

**범위**:
- [범위 1]: [설명]
- [범위 2]: [설명]
- [범위 3]: [설명]

### 테스트 구조

```
[테스트 디렉토리]/
├── integration/
│   ├── api/
│   │   ├── [엔드포인트1].test.ts
│   │   ├── [엔드포인트2].test.ts
│   │   └── [엔드포인트3].test.ts
│   ├── database/
│   │   ├── [테이블1].test.ts
│   │   └── [테이블2].test.ts
│   └── services/
│       ├── [서비스1].test.ts
│       └── [서비스2].test.ts
```

### 예제: API 통합 테스트

**Server Action 테스트**:
```typescript
// [테스트 파일 경로]
import { describe, it, expect, beforeAll, afterAll } from '[테스트 프레임워크]'
import { [ServerAction] } from '[경로]'
import { createClient } from '[경로]'

describe('[ServerAction] Integration', () => {
  let [테스트클라이언트]: any
  let [테스트데이터]: any

  beforeAll(async () => {
    // 테스트 DB 연결
    [테스트클라이언트] = createClient()

    // 테스트 데이터 생성
    const { data } = await [테스트클라이언트]
      .from('[테이블명]')
      .insert({
        [필드]: '[테스트값]',
        [필드]: '[테스트값]',
      })
      .select()
      .single()

    [테스트데이터] = data
  })

  afterAll(async () => {
    // 테스트 데이터 정리
    await [테스트클라이언트]
      .from('[테이블명]')
      .delete()
      .eq('id', [테스트데이터].id)
  })

  it('should [통합 동작 1]', async () => {
    // Arrange
    const formData = new FormData()
    formData.append('[필드]', '[값]')

    // Act
    const result = await [ServerAction](formData)

    // Assert
    expect(result).toBeDefined()
    expect(result.[필드]).toBe('[예상값]')

    // DB 검증
    const { data } = await [테스트클라이언트]
      .from('[테이블명]')
      .select()
      .eq('id', result.id)
      .single()

    expect(data.[필드]).toBe('[예상값]')
  })

  it('should [통합 동작 2] with [조건]', async () => {
    // 복잡한 통합 시나리오 테스트
    const [단계1결과] = await [함수1]()
    const [단계2결과] = await [함수2]([단계1결과])
    const [최종결과] = await [함수3]([단계2결과])

    expect([최종결과]).toMatchObject({
      [필드]: expect.any([타입]),
      [필드]: expect.arrayContaining([
        expect.objectContaining({
          [필드]: '[값]',
        }),
      ]),
    })
  })
})
```

**Database 통합 테스트**:
```typescript
// [테스트 파일 경로]
import { describe, it, expect, beforeEach } from '[테스트 프레임워크]'
import { createClient } from '[경로]'

describe('Database Integration - [테이블명]', () => {
  let [클라이언트]: any

  beforeEach(() => {
    [클라이언트] = createClient()
  })

  describe('CRUD Operations', () => {
    it('should create [리소스]', async () => {
      const { data, error } = await [클라이언트]
        .from('[테이블명]')
        .insert({
          [필드]: '[값]',
          [필드]: '[값]',
        })
        .select()
        .single()

      expect(error).toBeNull()
      expect(data).toHaveProperty('id')
      expect(data.[필드]).toBe('[값]')
    })

    it('should read [리소스]', async () => {
      const { data, error } = await [클라이언트]
        .from('[테이블명]')
        .select('*')
        .eq('[필드]', '[값]')

      expect(error).toBeNull()
      expect(data).toBeInstanceOf(Array)
      expect(data.length).toBeGreaterThan(0)
    })

    it('should update [리소스]', async () => {
      const { data, error } = await [클라이언트]
        .from('[테이블명]')
        .update({ [필드]: '[새값]' })
        .eq('id', '[테스트ID]')
        .select()
        .single()

      expect(error).toBeNull()
      expect(data.[필드]).toBe('[새값]')
    })

    it('should delete [리소스]', async () => {
      const { error } = await [클라이언트]
        .from('[테이블명]')
        .delete()
        .eq('id', '[테스트ID]')

      expect(error).toBeNull()
    })
  })

  describe('Relations', () => {
    it('should handle [관계 1] correctly', async () => {
      const { data, error } = await [클라이언트]
        .from('[테이블명]')
        .select(`
          *,
          [관계테이블](*)
        `)
        .eq('id', '[ID]')
        .single()

      expect(error).toBeNull()
      expect(data.[관계테이블]).toBeDefined()
      expect(Array.isArray(data.[관계테이블])).toBe(true)
    })
  })

  describe('RLS Policies', () => {
    it('should enforce [RLS 정책]', async () => {
      // [다른 사용자]로 접근 시도
      const { data, error } = await [클라이언트]
        .from('[테이블명]')
        .select()
        .eq('[필드]', '[다른사용자ID]')

      // [접근 거부] 확인
      expect(data).toEqual([])
      // 또는 에러 확인
      // expect(error).toBeDefined()
    })
  })
})
```

### 외부 API 통합 테스트

```typescript
// [테스트 파일]
import { describe, it, expect } from '[테스트 프레임워크]'
import { [API함수] } from '[경로]'

describe('[외부 서비스] Integration', () => {
  it('should [API 동작]', async () => {
    const result = await [API함수]({
      [파라미터]: '[테스트값]',
    })

    expect(result).toBeDefined()
    expect(result.[필드]).toMatch(/[정규식]/)
  }, [타임아웃]) // 외부 API는 타임아웃 길게 설정
})
```

### 통합 테스트 실행

```bash
# 통합 테스트 실행
[패키지 관리자] test:integration

# 특정 통합 테스트만 실행
[패키지 관리자] test:integration -- [파일명]
```

### 통합 테스트 체크리스트

**API Layer**:
- [ ] [엔드포인트 1] - CRUD 테스트
- [ ] [엔드포인트 2] - 인증/권한 테스트
- [ ] [엔드포인트 3] - 에러 처리 테스트

**Database Layer**:
- [ ] [테이블 1] - CRUD + RLS
- [ ] [테이블 2] - Relations
- [ ] [테이블 3] - Constraints

**Service Layer**:
- [ ] [서비스 1] - 외부 API 통합
- [ ] [서비스 2] - 데이터 처리 파이프라인
- [ ] [서비스 3] - 비즈니스 로직

---

## E2E 테스트

### 테스트 전략

**목표**:
- [목표 1]
- [목표 2]
- [목표 3]

**범위**:
- [범위 1]: [설명]
- [범위 2]: [설명]

### 테스트 도구 선택

**[Playwright/Cypress]**:
```bash
# 설치
[패키지 관리자] add -D [E2E 프레임워크]

# 초기화
npx [E2E 프레임워크] install
```

**설정**:
```typescript
// [설정 파일 경로]
import { defineConfig, devices } from '[E2E 프레임워크]'

export default defineConfig({
  testDir: './[테스트 디렉토리]',
  fullyParallel: true,
  forbidOnly: !!process.env.CI,
  retries: process.env.CI ? [N] : 0,
  workers: process.env.CI ? [N] : undefined,
  reporter: 'html',
  use: {
    baseURL: '[베이스 URL]',
    trace: 'on-first-retry',
    screenshot: 'only-on-failure',
  },

  projects: [
    {
      name: 'chromium',
      use: { ...devices['Desktop Chrome'] },
    },
    {
      name: 'firefox',
      use: { ...devices['Desktop Firefox'] },
    },
    {
      name: 'webkit',
      use: { ...devices['Desktop Safari'] },
    },
    {
      name: 'Mobile Chrome',
      use: { ...devices['Pixel 5'] },
    },
    {
      name: 'Mobile Safari',
      use: { ...devices['iPhone 12'] },
    },
  ],

  webServer: {
    command: '[실행 명령어]',
    url: '[URL]',
    reuseExistingServer: !process.env.CI,
  },
})
```

### 테스트 구조

```
e2e/
├── auth/
│   ├── sign-in.spec.ts
│   ├── sign-up.spec.ts
│   └── sign-out.spec.ts
├── [기능1]/
│   ├── [시나리오1].spec.ts
│   └── [시나리오2].spec.ts
├── [기능2]/
│   └── [시나리오].spec.ts
└── fixtures/
    ├── test-data.ts
    └── helpers.ts
```

### 예제: [핵심 플로우] E2E 테스트

**인증 플로우**:
```typescript
// e2e/auth/sign-in.spec.ts
import { test, expect } from '[E2E 프레임워크]'

test.describe('[인증 기능]', () => {
  test.beforeEach(async ({ page }) => {
    await page.goto('[URL]')
  })

  test('should [로그인 시나리오]', async ({ page }) => {
    // 로그인 페이지로 이동
    await page.click('[로그인 버튼 셀렉터]')
    await expect(page).toHaveURL(/sign-in/)

    // 자격증명 입력
    await page.fill('[이메일 입력 셀렉터]', '[테스트 이메일]')
    await page.fill('[비밀번호 입력 셀렉터]', '[테스트 비밀번호]')

    // 제출
    await page.click('[제출 버튼 셀렉터]')

    // 대시보드로 리다이렉트 확인
    await expect(page).toHaveURL(/dashboard/)
    await expect(page.locator('[사용자명 셀렉터]')).toContainText('[사용자명]')
  })

  test('should show error for invalid credentials', async ({ page }) => {
    await page.goto('[로그인 URL]')
    await page.fill('[이메일 입력 셀렉터]', '[잘못된 이메일]')
    await page.fill('[비밀번호 입력 셀렉터]', '[잘못된 비밀번호]')
    await page.click('[제출 버튼 셀렉터]')

    await expect(page.locator('[에러 메시지 셀렉터]')).toBeVisible()
    await expect(page.locator('[에러 메시지 셀렉터]')).toContainText('[에러 텍스트]')
  })
})
```

**[핵심 기능] 플로우**:
```typescript
// e2e/[기능]/[시나리오].spec.ts
import { test, expect } from '[E2E 프레임워크]'

test.describe('[기능명] Flow', () => {
  test.use({ storageState: '[인증 상태 파일]' })

  test('should complete [전체 플로우]', async ({ page }) => {
    // 1. [단계 1]
    await page.goto('[URL]')
    await page.click('[버튼 셀렉터]')

    // 2. [단계 2]
    await page.fill('[입력 셀렉터]', '[값]')
    await page.selectOption('[셀렉트 셀렉터]', '[옵션]')

    // 3. [단계 3] - 파일 업로드
    const fileInput = page.locator('[파일 입력 셀렉터]')
    await fileInput.setInputFiles('[테스트 파일 경로]')

    // 4. [단계 4] - 제출
    await page.click('[제출 버튼 셀렉터]')

    // 5. [단계 5] - 결과 확인
    await expect(page.locator('[성공 메시지 셀렉터]')).toBeVisible()
    await expect(page.locator('[결과 셀렉터]')).toContainText('[예상 텍스트]')

    // 6. [단계 6] - 데이터 표시 확인
    const [데이터항목들] = page.locator('[목록 셀렉터]')
    await expect([데이터항목들]).toHaveCount([예상 개수])
  })

  test('should handle [에러 시나리오]', async ({ page }) => {
    await page.goto('[URL]')

    // [잘못된 입력]
    await page.fill('[입력 셀렉터]', '[잘못된 값]')
    await page.click('[제출 버튼 셀렉터]')

    // [에러 확인]
    await expect(page.locator('[에러 셀렉터]')).toBeVisible()
  })
})
```

**검색 기능 테스트**:
```typescript
// e2e/[기능]/search.spec.ts
import { test, expect } from '[E2E 프레임워크]'

test.describe('Search Functionality', () => {
  test('should [검색 시나리오]', async ({ page }) => {
    await page.goto('[검색 페이지 URL]')

    // 검색 입력
    await page.fill('[검색 입력 셀렉터]', '[검색어]')
    await page.press('[검색 입력 셀렉터]', 'Enter')

    // 결과 대기
    await page.waitForSelector('[결과 셀렉터]')

    // 결과 검증
    const results = page.locator('[결과 아이템 셀렉터]')
    await expect(results).toHaveCount([예상 개수])

    // 첫 번째 결과 클릭
    await results.first().click()

    // 상세 페이지 확인
    await expect(page).toHaveURL(/[URL 패턴]/)
    await expect(page.locator('[제목 셀렉터]')).toBeVisible()
  })
})
```

### Page Object Model (POM)

```typescript
// e2e/fixtures/pages/[페이지명].page.ts
import { Page, Locator } from '[E2E 프레임워크]'

export class [페이지명]Page {
  readonly page: Page
  readonly [요소명1]: Locator
  readonly [요소명2]: Locator
  readonly [요소명3]: Locator

  constructor(page: Page) {
    this.page = page
    this.[요소명1] = page.locator('[셀렉터]')
    this.[요소명2] = page.locator('[셀렉터]')
    this.[요소명3] = page.locator('[셀렉터]')
  }

  async goto() {
    await this.page.goto('[URL]')
  }

  async [액션명](text: string) {
    await this.[요소명1].fill(text)
    await this.[요소명2].click()
  }

  async [검증메서드]() {
    await expect(this.[요소명3]).toBeVisible()
    return await this.[요소명3].textContent()
  }
}

// 사용 예제
// test('should...', async ({ page }) => {
//   const [페이지] = new [페이지명]Page(page)
//   await [페이지].goto()
//   await [페이지].[액션명]('[값]')
//   const result = await [페이지].[검증메서드]()
//   expect(result).toContain('[예상값]')
// })
```

### E2E 테스트 실행

```bash
# 모든 E2E 테스트 실행
[패키지 관리자] test:e2e

# UI 모드로 실행
[패키지 관리자] test:e2e --ui

# 특정 브라우저만
[패키지 관리자] test:e2e --project=chromium

# 헤드리스 모드
[패키지 관리자] test:e2e --headed

# 디버그 모드
[패키지 관리자] test:e2e --debug
```

### E2E 테스트 체크리스트

**인증 플로우**:
- [ ] 회원가입 - 성공/실패 케이스
- [ ] 로그인 - 성공/실패 케이스
- [ ] 로그아웃
- [ ] 비밀번호 재설정

**[핵심 기능 1]**:
- [ ] [시나리오 1] - Happy Path
- [ ] [시나리오 2] - 에러 처리
- [ ] [시나리오 3] - Edge Cases

**[핵심 기능 2]**:
- [ ] [시나리오 1] - 생성 플로우
- [ ] [시나리오 2] - 수정 플로우
- [ ] [시나리오 3] - 삭제 플로우

**반응형 테스트**:
- [ ] Desktop (1920x1080)
- [ ] Tablet (768x1024)
- [ ] Mobile (375x667)

---

## 성능 테스트

### 성능 목표

| 메트릭 | 목표 | 허용 범위 |
|--------|------|----------|
| **First Contentful Paint (FCP)** | < [N]ms | < [N]ms |
| **Largest Contentful Paint (LCP)** | < [N]ms | < [N]ms |
| **Time to Interactive (TTI)** | < [N]ms | < [N]ms |
| **Total Blocking Time (TBT)** | < [N]ms | < [N]ms |
| **Cumulative Layout Shift (CLS)** | < [N] | < [N] |
| **API Response Time** | < [N]ms | < [N]ms |
| **Database Query Time** | < [N]ms | < [N]ms |

### Lighthouse 테스트

```bash
# Lighthouse CI 설치
npm install -g @lhci/cli

# 설정 파일 생성
```

**lighthouserc.json**:
```json
{
  "ci": {
    "collect": {
      "url": ["[URL1]", "[URL2]", "[URL3]"],
      "numberOfRuns": 3,
      "settings": {
        "preset": "desktop"
      }
    },
    "assert": {
      "assertions": {
        "categories:performance": ["error", {"minScore": 0.9}],
        "categories:accessibility": ["error", {"minScore": 0.9}],
        "categories:best-practices": ["error", {"minScore": 0.9}],
        "categories:seo": ["error", {"minScore": 0.9}]
      }
    },
    "upload": {
      "target": "temporary-public-storage"
    }
  }
}
```

```bash
# Lighthouse 실행
lhci autorun
```

### 부하 테스트

**[K6/Artillery] 설정**:
```bash
# 설치
npm install -g [부하테스트도구]
```

**부하 테스트 스크립트**:
```javascript
// [스크립트 파일명]
import http from 'k6/http';
import { check, sleep } from 'k6';

export const options = {
  stages: [
    { duration: '[N]s', target: [N] },  // Ramp up
    { duration: '[N]s', target: [N] },  // Stay at peak
    { duration: '[N]s', target: 0 },     // Ramp down
  ],
  thresholds: {
    http_req_duration: ['p(95)<[N]'],  // 95% 요청 [N]ms 이내
    http_req_failed: ['rate<0.01'],     // 에러율 1% 미만
  },
};

export default function () {
  const res = http.get('[API URL]');

  check(res, {
    'status is 200': (r) => r.status === 200,
    'response time < [N]ms': (r) => r.timings.duration < [N],
  });

  sleep(1);
}
```

```bash
# 부하 테스트 실행
k6 run [스크립트 파일]
```

### 데이터베이스 성능 테스트

```sql
-- 쿼리 성능 분석
EXPLAIN ANALYZE
SELECT [필드]
FROM [테이블]
WHERE [조건]
ORDER BY [필드]
LIMIT [N];

-- 인덱스 사용 확인
SELECT indexname, indexdef
FROM pg_indexes
WHERE tablename = '[테이블명]';

-- 느린 쿼리 로깅
-- [데이터베이스] 설정에서 활성화
```

### 성능 테스트 체크리스트

**Frontend Performance**:
- [ ] Lighthouse Score > [N]
- [ ] Core Web Vitals 통과
- [ ] 번들 크기 < [N]KB
- [ ] 이미지 최적화

**Backend Performance**:
- [ ] API 응답 시간 < [N]ms (p95)
- [ ] 동시 사용자 [N]명 처리
- [ ] 데이터베이스 쿼리 최적화
- [ ] 캐싱 전략 검증

---

## 보안 검토

### 보안 테스트 범위

**OWASP Top 10 검증**:
1. [취약점 1]: [설명]
2. [취약점 2]: [설명]
3. [취약점 3]: [설명]
4. [취약점 4]: [설명]
5. [취약점 5]: [설명]

### 자동화된 보안 스캔

**[Snyk/OWASP ZAP]**:
```bash
# Snyk 설치
npm install -g snyk

# 의존성 취약점 스캔
snyk test

# 코드 취약점 스캔
snyk code test

# 컨테이너 스캔 (Docker 사용 시)
snyk container test [이미지명]
```

**npm audit**:
```bash
# 취약점 검사
npm audit

# 자동 수정
npm audit fix

# 강제 업데이트
npm audit fix --force
```

### 인증/권한 보안 테스트

```typescript
// [테스트 파일]
import { describe, it, expect } from '[테스트 프레임워크]'

describe('Security - Authentication', () => {
  it('should reject [무인증 요청]', async () => {
    const response = await fetch('[보호된 엔드포인트]')
    expect(response.status).toBe(401)
  })

  it('should reject [만료된 토큰]', async () => {
    const response = await fetch('[보호된 엔드포인트]', {
      headers: {
        Authorization: 'Bearer [만료된토큰]'
      }
    })
    expect(response.status).toBe(401)
  })

  it('should reject [권한 없는 접근]', async () => {
    const response = await fetch('[관리자 엔드포인트]', {
      headers: {
        Authorization: 'Bearer [일반사용자토큰]'
      }
    })
    expect(response.status).toBe(403)
  })
})

describe('Security - Input Validation', () => {
  it('should prevent [SQL Injection]', async () => {
    const maliciousInput = "'; DROP TABLE [테이블]; --"
    const result = await [함수](maliciousInput)

    expect(result).not.toContain('DROP TABLE')
  })

  it('should prevent [XSS]', async () => {
    const maliciousInput = '<script>alert("XSS")</script>'
    const result = await [함수](maliciousInput)

    expect(result).not.toContain('<script>')
  })

  it('should validate [파일 업로드]', async () => {
    const maliciousFile = new File(['malicious'], 'test.exe', {
      type: 'application/x-msdownload'
    })

    await expect([업로드함수](maliciousFile)).rejects.toThrow('[에러 메시지]')
  })
})
```

### RLS (Row Level Security) 테스트

```typescript
// [테스트 파일]
describe('Security - RLS Policies', () => {
  it('should enforce [RLS 정책 1]', async () => {
    // [사용자 A]로 로그인
    const [클라이언트A] = [createAuthenticatedClient]('[사용자A ID]')

    // [사용자 B]의 데이터 접근 시도
    const { data, error } = await [클라이언트A]
      .from('[테이블]')
      .select()
      .eq('[소유자필드]', '[사용자B ID]')

    // 접근 거부 확인
    expect(data).toEqual([])
    expect(error).toBeNull() // RLS는 에러 대신 빈 결과 반환
  })

  it('should allow [자신의 데이터 접근]', async () => {
    const [클라이언트] = [createAuthenticatedClient]('[사용자 ID]')

    const { data, error } = await [클라이언트]
      .from('[테이블]')
      .select()
      .eq('[소유자필드]', '[사용자 ID]')

    expect(error).toBeNull()
    expect(data.length).toBeGreaterThan(0)
  })
})
```

### 보안 체크리스트

**인증/권한**:
- [ ] JWT 토큰 검증
- [ ] 토큰 만료 처리
- [ ] RBAC 권한 체계
- [ ] RLS 정책 테스트

**입력 검증**:
- [ ] SQL Injection 방지
- [ ] XSS 방지
- [ ] CSRF 방지
- [ ] 파일 업로드 검증

**데이터 보안**:
- [ ] 암호화 (전송 중/저장)
- [ ] 민감 정보 마스킹
- [ ] 환경 변수 보안
- [ ] API 키 관리

**네트워크 보안**:
- [ ] HTTPS 강제
- [ ] CORS 설정
- [ ] Rate Limiting
- [ ] DDoS 방지

---

## 코드 리뷰 프로세스

### 코드 리뷰 체크리스트

**기능성**:
- [ ] 요구사항 충족
- [ ] 엣지 케이스 처리
- [ ] 에러 핸들링
- [ ] 로깅 적절성

**코드 품질**:
- [ ] [코딩 컨벤션] 준수
- [ ] 중복 코드 최소화
- [ ] 함수/클래스 크기 적절
- [ ] 명확한 네이밍

**테스트**:
- [ ] 단위 테스트 작성
- [ ] 테스트 커버리지 > [%]
- [ ] 테스트 케이스 충분
- [ ] Mock 적절히 사용

**성능**:
- [ ] 불필요한 렌더링 없음
- [ ] 최적화된 쿼리
- [ ] 메모리 누수 없음
- [ ] 번들 크기 영향 최소

**보안**:
- [ ] 인증/권한 체크
- [ ] 입력 검증
- [ ] 민감 정보 노출 없음
- [ ] 보안 베스트 프랙티스

### Pull Request 템플릿

```markdown
## [PR 타입] - [간단한 설명]

### 변경 사항
- [변경 사항 1]
- [변경 사항 2]
- [변경 사항 3]

### 관련 이슈
Closes #[이슈번호]

### 테스트
- [ ] 단위 테스트 추가/업데이트
- [ ] 통합 테스트 추가/업데이트
- [ ] E2E 테스트 추가/업데이트
- [ ] 로컬 환경에서 테스트 완료

### 스크린샷 (UI 변경 시)
[스크린샷 또는 GIF]

### 체크리스트
- [ ] 코드가 [스타일 가이드]를 따름
- [ ] Self-review 완료
- [ ] 코드에 주석 추가 (복잡한 로직)
- [ ] 문서 업데이트
- [ ] 브레이킹 체인지 없음 (또는 명시)
- [ ] 모든 테스트 통과
```

### 리뷰어 가이드라인

**리뷰 시 확인사항**:
1. **[항목 1]**: [설명]
2. **[항목 2]**: [설명]
3. **[항목 3]**: [설명]
4. **[항목 4]**: [설명]
5. **[항목 5]**: [설명]

**피드백 원칙**:
- [원칙 1]
- [원칙 2]
- [원칙 3]

---

## CI/CD 파이프라인

### GitHub Actions 워크플로우

**.github/workflows/test.yml**:
```yaml
name: Test Suite

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main, develop]

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: '[버전]'
      - run: [패키지 관리자] install
      - run: [패키지 관리자] run lint

  unit-tests:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: '[버전]'
      - run: [패키지 관리자] install
      - run: [패키지 관리자] run test:unit --coverage
      - uses: codecov/codecov-action@v3
        with:
          files: ./coverage/coverage-final.json

  integration-tests:
    runs-on: ubuntu-latest
    services:
      [데이터베이스]:
        image: [이미지]:[태그]
        env:
          [ENV_VAR]: [값]
        ports:
          - [포트]:[포트]
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: '[버전]'
      - run: [패키지 관리자] install
      - run: [패키지 관리자] run test:integration
        env:
          [ENV_VAR]: [값]

  e2e-tests:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: '[버전]'
      - run: [패키지 관리자] install
      - run: npx [E2E 프레임워크] install
      - run: [패키지 관리자] run build
      - run: [패키지 관리자] run test:e2e
      - uses: actions/upload-artifact@v3
        if: failure()
        with:
          name: playwright-report
          path: playwright-report/

  security-scan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - run: npm audit
      - uses: snyk/actions/node@master
        env:
          SNYK_TOKEN: ${{ secrets.SNYK_TOKEN }}

  lighthouse:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: '[버전]'
      - run: [패키지 관리자] install
      - run: [패키지 관리자] run build
      - run: npm install -g @lhci/cli
      - run: lhci autorun
```

### 파이프라인 단계

```
┌──────────────┐
│   PR 생성     │
└──────┬───────┘
       │
       ├─> Lint
       ├─> Unit Tests
       ├─> Integration Tests
       ├─> Security Scan
       │
       ↓
┌──────────────┐
│  코드 리뷰    │
└──────┬───────┘
       │
       ├─> E2E Tests
       ├─> Performance Tests
       ├─> Build Check
       │
       ↓
┌──────────────┐
│    Merge     │
└──────┬───────┘
       │
       ├─> Deploy to Staging
       ├─> Smoke Tests
       │
       ↓
┌──────────────┐
│   Production │
└──────────────┘
```

---

## 테스트 커버리지 목표

### 커버리지 기준

| 계층 | 목표 커버리지 | 최소 커버리지 |
|------|--------------|--------------|
| **[계층 1]** | [%] | [%] |
| **[계층 2]** | [%] | [%] |
| **[계층 3]** | [%] | [%] |
| **전체** | [%] | [%] |

### 커버리지 측정

```bash
# 전체 커버리지 측정
[패키지 관리자] test -- --coverage

# 리포트 생성
[패키지 관리자] test -- --coverage --coverageReporters=html

# 리포트 확인
open coverage/index.html
```

### 커버리지 리포트

**package.json**:
```json
{
  "jest": {
    "collectCoverageFrom": [
      "[경로 패턴]",
      "![제외 패턴]"
    ],
    "coverageThresholds": {
      "global": {
        "branches": [%],
        "functions": [%],
        "lines": [%],
        "statements": [%]
      }
    }
  }
}
```

---

## 버그 추적 및 관리

### 버그 분류

| 심각도 | 설명 | 대응 시간 |
|--------|------|----------|
| **Critical** | [설명] | [N]시간 |
| **High** | [설명] | [N]일 |
| **Medium** | [설명] | [N]주 |
| **Low** | [설명] | [N]주 |

### 버그 리포트 템플릿

```markdown
## 🐛 버그 리포트

### 설명
[버그에 대한 명확하고 간결한 설명]

### 재현 단계
1. [단계 1]
2. [단계 2]
3. [단계 3]

### 예상 동작
[예상했던 동작]

### 실제 동작
[실제로 발생한 동작]

### 스크린샷
[해당되는 경우 스크린샷 추가]

### 환경
- OS: [e.g. macOS 13.0]
- Browser: [e.g. Chrome 110]
- Version: [e.g. 1.2.3]

### 추가 정보
[추가적인 컨텍스트]

### 심각도
- [ ] Critical
- [ ] High
- [ ] Medium
- [ ] Low
```

### 버그 수정 프로세스

1. **[단계 1]**: [설명]
2. **[단계 2]**: [설명]
3. **[단계 3]**: [설명]
4. **[단계 4]**: [설명]
5. **[단계 5]**: [설명]

---

## 테스트 자동화

### Pre-commit Hooks

**Husky 설정**:
```bash
# Husky 설치
npx husky-init && [패키지 관리자] install

# Pre-commit hook 설정
```

**.husky/pre-commit**:
```bash
#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"

# Lint staged files
npx lint-staged

# Run affected tests
[패키지 관리자] run test:affected
```

**lint-staged 설정**:
```json
{
  "lint-staged": {
    "*.{ts,tsx}": [
      "eslint --fix",
      "[패키지 관리자] run test:unit -- --findRelatedTests"
    ],
    "*.{json,md}": [
      "prettier --write"
    ]
  }
}
```

### 자동화된 테스트 실행

**package.json scripts**:
```json
{
  "scripts": {
    "test": "[테스트 명령어]",
    "test:unit": "[단위 테스트 명령어]",
    "test:integration": "[통합 테스트 명령어]",
    "test:e2e": "[E2E 테스트 명령어]",
    "test:coverage": "[커버리지 명령어]",
    "test:watch": "[watch 모드 명령어]",
    "test:affected": "[변경된 파일 테스트]"
  }
}
```

---

## 품질 메트릭

### 추적 메트릭

| 메트릭 | 현재 값 | 목표 값 | 상태 |
|--------|---------|---------|------|
| **테스트 커버리지** | [%] | [%] | [🟢/🟡/🔴] |
| **버그 밀도** | [N]/KLOC | < [N]/KLOC | [🟢/🟡/🔴] |
| **평균 수정 시간** | [N]시간 | < [N]시간 | [🟢/🟡/🔴] |
| **테스트 실행 시간** | [N]분 | < [N]분 | [🟢/🟡/🔴] |
| **코드 복잡도** | [N] | < [N] | [🟢/🟡/🔴] |

### 품질 대시보드

**모니터링 도구**:
- [도구 1]: [용도]
- [도구 2]: [용도]
- [도구 3]: [용도]

**리포팅**:
- [주기]: [리포트 내용]
- [주기]: [리포트 내용]

---

## 테스트 실행 가이드

### 로컬 개발 환경

```bash
# 1. 모든 테스트 실행
[패키지 관리자] test

# 2. Watch 모드 (개발 중)
[패키지 관리자] test:watch

# 3. 특정 파일만
[패키지 관리자] test [파일명]

# 4. 커버리지 확인
[패키지 관리자] test:coverage
```

### CI 환경

```bash
# 1. Lint
[패키지 관리자] run lint

# 2. Unit Tests
[패키지 관리자] run test:unit --coverage

# 3. Integration Tests
[패키지 관리자] run test:integration

# 4. E2E Tests
[패키지 관리자] run test:e2e

# 5. Build
[패키지 관리자] run build
```

---

## 테스트 베스트 프랙티스

### DO ✅

1. **[베스트 프랙티스 1]**: [설명]
2. **[베스트 프랙티스 2]**: [설명]
3. **[베스트 프랙티스 3]**: [설명]
4. **[베스트 프랙티스 4]**: [설명]
5. **[베스트 프랙티스 5]**: [설명]

### DON'T ❌

1. **[안티 패턴 1]**: [설명]
2. **[안티 패턴 2]**: [설명]
3. **[안티 패턴 3]**: [설명]
4. **[안티 패턴 4]**: [설명]
5. **[안티 패턴 5]**: [설명]

---

## 테스트 문서화

### 테스트 케이스 문서

각 주요 기능에 대한 테스트 케이스는 다음 형식으로 문서화:

```markdown
## [기능명] 테스트 케이스

### TC-[번호]: [테스트 케이스 이름]

**목적**: [테스트 목적]

**전제 조건**:
- [전제 조건 1]
- [전제 조건 2]

**테스트 단계**:
1. [단계 1]
2. [단계 2]
3. [단계 3]

**예상 결과**:
- [예상 결과 1]
- [예상 결과 2]

**실제 결과**: [Pass/Fail]

**비고**: [추가 정보]
```

### 테스트 실행 결과 보고서

**📄 TEST-REPORT.md 작성 및 유지**:

프로젝트 진행 중 모든 테스트 실행 결과는 `TEST-REPORT.md`에 기록합니다:

- **업데이트 주기**: 매 Day/Phase 완료 후
- **포함 내용**:
  - 전체 테스트 현황 (Test Suites, Test Cases, 통과율)
  - Day별 테스트 진행 현황 및 결과
  - 커버리지 리포트 (Statements, Branch, Functions, Lines)
  - 테스트 실행 방법 및 명령어
  - 테스트 실패 이력 및 해결 방법
  - 테스트 계획 및 예정 항목

**TEST-REPORT.md 예시 구조**:
```markdown
## 📊 전체 테스트 현황
총 테스트 스위트: [N] passed, [N] total
총 테스트 케이스: [N] passed, [N] total

## 🎯 Day별 테스트 진행 현황
### Day N: [단계명] (YYYY-MM-DD)
- 테스트 대상: [설명]
- 결과: ✅ PASS
- 커버리지: [N]%
```

**참고**: TEST-VERIFY.md(현재 문서)는 테스트 **전략 및 계획**을, TEST-REPORT.md는 실제 테스트 **실행 결과 및 이력**을 기록합니다.

---

## 마무리

### 테스트 완료 체크리스트

**코드 품질**:
- [ ] 모든 단위 테스트 통과
- [ ] 코드 커버리지 > [%]
- [ ] Lint 에러 0개
- [ ] Type 에러 0개

**기능 검증**:
- [ ] 모든 통합 테스트 통과
- [ ] E2E 테스트 통과
- [ ] 크로스 브라우저 테스트 완료
- [ ] 반응형 테스트 완료

**성능 & 보안**:
- [ ] Lighthouse Score > [N]
- [ ] 부하 테스트 통과
- [ ] 보안 스캔 통과
- [ ] 취약점 0개

**프로세스**:
- [ ] 코드 리뷰 완료
- [ ] PR 승인
- [ ] CI/CD 파이프라인 통과
- [ ] 문서 업데이트
- [ ] TEST-REPORT.md 업데이트 (테스트 결과 기록)

**테스트 문서**:
- [ ] TEST-VERIFY.md (전략/계획) 최신화
- [ ] TEST-REPORT.md (실행 결과) 업데이트
- [ ] CHECKLIST.md 테스트 섹션 완료 표시

---

**작성일**: [날짜]
**최종 업데이트**: [날짜]
**다음 리뷰**: [날짜]

---

이 문서는 프로젝트의 품질을 보장하기 위한 포괄적인 테스트 및 검증 계획을 제공합니다.
