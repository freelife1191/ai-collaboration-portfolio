# Notion-Powered Blog Automator - PRD

> **버전**: v2.0 (실제 구현 기준)
> **최종 업데이트**: 2025-10-23

개발 레퍼런서 문서들을 바탕으로 구체화 시킨 PRD V2

---

## 1. 프로젝트 개요

### 1.1 프로젝트 정의
- **프로젝트명**: Notion-Powered Blog Automator
- **목표**: Notion을 CMS로 사용하고 GitHub Actions를 통해 10분마다 자동으로 GitHub Pages에 배포되는 정적 블로그 시스템
- **핵심 가치**:
  - 별도의 관리자 페이지 불필요
  - Notion에서 글 작성 및 관리
  - GitHub Actions가 자동으로 빌드 및 배포
  - 무료 호스팅 (GitHub Pages)

### 1.2 레퍼런스
- **디자인 참고**: [https://hong-minji.github.io/works/](https://hong-minji.github.io/works/)

---

## 2. 기술 스택 (Technology Stack)

### 2.1 필수 기술 스택 (MANDATORY)
다음 기술들은 반드시 사용해야 하며, 대체할 수 없습니다:

| 카테고리 | 기술 | 버전 | 필수 여부 | 사용 목적 |
|---------|------|------|-----------|----------|
| **프레임워크** | Next.js | 15.x | ✅ 필수 | App Router, RSC, Static Export |
| **언어** | TypeScript | 5.x | ✅ 필수 | 타입 안정성 |
| **스타일링** | Tailwind CSS | 3.x | ✅ 필수 | 유틸리티 CSS 프레임워크 |
| | @tailwindcss/typography | - | ✅ 필수 | 마크다운 스타일링 |
| **UI 컴포넌트** | shadcn/ui | - | ✅ 필수 | 일관된 디자인 시스템 |
| **아이콘** | Lucide React | - | ✅ 필수 | 1000+ 트리셰이킹 아이콘 |
| **애니메이션** | Framer Motion | - | ✅ 필수 | 선언적 애니메이션 |
| **검증** | Zod | - | ✅ 필수 | 타입 안전 스키마 검증 |
| **CMS** | Notion API | - | ✅ 필수 | 콘텐츠 관리 시스템 |
| **배포** | GitHub Actions | - | ✅ 필수 | CI/CD 자동화 |
| **호스팅** | GitHub Pages | - | ✅ 필수 | 정적 사이트 호스팅 |

### 2.2 테스트 프레임워크
| 도구 | 용도 | 현재 상태 |
|------|------|-----------|
| Vitest | 단위 테스트 | 170개 테스트 케이스 |
| Playwright | E2E 테스트 | 5개 테스트 파일 |

### 2.3 추가 라이브러리
- Prism.js (코드 하이라이팅)
- Unified 생태계 (마크다운 처리)
- React 19.x

---

## 3. 시스템 아키텍처

### 3.1 데이터 흐름
```
┌──────────────┐
│   Notion     │ ← 콘텐츠 작성 및 관리
│   (CMS)      │
└──────┬───────┘
       │ Notion API
       ▼
┌──────────────┐
│ GitHub       │ ← 10분마다 자동 실행
│ Actions      │   (cron: '*/10 * * * *')
└──────┬───────┘
       │ Build & Deploy
       ▼
┌──────────────┐
│ Next.js      │ ← Static Export
│ (SSG)        │   (out/ 디렉토리)
└──────┬───────┘
       │ HTML/CSS/JS
       ▼
┌──────────────┐
│ GitHub       │ ← 무료 호스팅
│ Pages        │   (CDN)
└──────────────┘
```

### 3.2 URL 구조
| 페이지 | URL 패턴 | 예시 |
|--------|----------|------|
| 홈 | `/` | `https://username.github.io/repo/` |
| 포스트 | `/posts/[slug]` | `https://username.github.io/repo/posts/my-post` |
| About | `/about` | `https://username.github.io/repo/about` |
| RSS | `/rss.xml` | `https://username.github.io/repo/rss.xml` |
| Sitemap | `/sitemap.xml` | `https://username.github.io/repo/sitemap.xml` |

### 3.3 배포 프로세스
1. **트리거**: 10분마다 자동 실행 또는 main 브랜치 Push
2. **환경 설정**: Node.js 20, 의존성 설치
3. **품질 검증**: Vitest 단위 테스트, ESLint
4. **데이터 Fetch**: Notion API로 Status = "Publish"인 글 가져오기
5. **빌드**: Next.js Static Export (out/ 디렉토리)
6. **배포**: GitHub Pages에 업로드

---

## 4. Notion 데이터 구조

### 4.1 Posts 데이터베이스 (필수)
블로그 글 관리를 위한 메인 데이터베이스

| 속성명 | 타입 | 필수 | 설명 | 예시 |
|--------|------|------|------|------|
| `Title` | title | ✅ | 포스트 제목 | "Next.js 시작하기" |
| `Slug` | rich_text | ✅ | URL 경로 (영문, 숫자, 하이픈만) | "nextjs-getting-started" |
| `Status` | select | ✅ | 발행 상태 | Publish, Draft, Hidden, Wait |
| `Date` | date | ✅ | 발행 날짜 | 2025-10-23 |
| `Tags` | multi_select | ⭕ | 포스트 태그 | ["Next.js", "React"] |
| `Label` | select | ⭕ | 카테고리 라벨 | "개발", "일상" |
| `Description` | rich_text | ⭕ | 포스트 설명 (SEO) | "Next.js 기본 개념 소개" |
| `CoverImage` | files | ⭕ | 커버 이미지 | 이미지 파일 |
| `Author` | rich_text | ⭕ | 작성자 | "John Doe" |

**Status 값 설명:**
- `Publish`: 블로그에 공개됨
- `Draft`: 작성 중 (블로그에 표시 안 됨)
- `Hidden`: 임시 숨김
- `Wait`: 발행 대기

### 4.2 Profile 데이터베이스 (선택)
프로필 정보 및 소셜 링크 관리

| 속성명 | 타입 | 필수 | 설명 |
|--------|------|------|------|
| `Name` | title | ✅ | 이름 |
| `ProfileImage` | files | ⭕ | 프로필 사진 |
| `JobTitle` | rich_text | ⭕ | 직함 |
| `Bio` | rich_text | ⭕ | 자기소개 |
| `HomeTitle` | rich_text | ⭕ | 홈 페이지 제목 |
| `HomeDescription` | rich_text | ⭕ | 홈 페이지 설명 |
| `GitHub` | url | ⭕ | GitHub 링크 |
| `Instagram` | url | ⭕ | Instagram 링크 |
| `Email` | email | ⭕ | 이메일 |

### 4.3 Site 데이터베이스 (선택)
사이트 설정, Analytics, AdSense 관리

| 속성명 | 타입 | 필수 | 설명 |
|--------|------|------|------|
| `SiteTitle` | title | ✅ | 사이트 제목 |
| `SiteDescription` | rich_text | ⭕ | 사이트 설명 |
| `GA4MeasurementId` | rich_text | ⭕ | Google Analytics 4 측정 ID |
| `AdSensePublisherId` | rich_text | ⭕ | Google AdSense Publisher ID |

### 4.4 About 페이지 (선택)
- **타입**: Notion 페이지
- **용도**: About 메뉴에 표시되는 자기소개 페이지
- **설정**: `NOTION_ABOUT_PAGE_ID` 환경 변수에 페이지 ID 입력

---

## 5. 기능 요구사항

### 5.1 완료된 핵심 기능 (✅)
| 기능 | 상태 | 설명 |
|------|------|------|
| 블로그 포스팅 | ⚠️ 검증 중 | Notion CMS 연동, Status 기반 발행 |
| About 페이지 | ✅ 완료 | Notion 페이지 기반 자기소개 |
| 프로필 설정 | ✅ 완료 | Notion 데이터베이스 기반 프로필 관리 |
| 댓글 시스템 | ✅ 완료 | Giscus (GitHub Discussions 기반) |
| 다크 모드 | ✅ 완료 | 시스템 설정 기반 자동 전환 |
| 페이지네이션 | ✅ 완료 | 4개씩 표시 (커스터마이징 가능) |
| 필터링 | ✅ 완료 | 월별, 태그, 라벨 필터링 |
| 목차 (TOC) | ✅ 완료 | 자동 생성 |
| 코드 하이라이팅 | ✅ 완료 | Prism.js 기반 |
| 유튜브 임베드 | ✅ 완료 | 북마크 블록 자동 변환 |
| SEO 최적화 | ⚠️ 검증 중 | Open Graph, Twitter Cards, JSON-LD |
| Sitemap | ⚠️ 검증 중 | 자동 생성 (/sitemap.xml) |
| RSS 피드 | ⚠️ 검증 중 | 자동 생성 (/rss.xml) |
| 이미지 최적화 | ✅ 완료 | Priority loading, Blur placeholder |
| Google Analytics | ⚠️ 검증 중 | GA4 통합 |
| Google AdSense | ⚠️ 검증 중 | 반응형 광고 |
| HTTPS | ✅ 완료 | GitHub Pages 자동 제공 |

### 5.2 개발 중 기능 (🔧)
| 기능 | 상태 | 설명 |
|------|------|------|
| 테마 설정 | 🔧 개발 중 | 색상, 폰트 커스터마이징 |

### 5.3 주요 기능 상세

#### 5.3.1 홈페이지
- 프로필 사이드바 (프로필 이미지, 이름, 자기소개, 소셜 링크)
- 포스트 목록 (4개씩 페이지네이션)
- 필터링 (월별, 태그, 라벨)
- 다크/라이트 모드 토글

#### 5.3.2 포스트 상세 페이지
- Notion 블록 렌더링 (Heading, Paragraph, Code, Image, List, Table 등)
- 목차 (TOC)
- 커버 이미지
- 발행일, 태그, 라벨 표시
- Google AdSense 광고 (상단/하단)
- Giscus 댓글

#### 5.3.3 About 페이지
- Notion 페이지 렌더링
- 자유로운 콘텐츠 구성

---

## 6. 성능 최적화

### 6.1 정적 사이트 생성 (SSG)
- 모든 페이지를 빌드 타임에 사전 생성
- 런타임에 Notion API 호출 없음
- CDN을 통한 초고속 로딩

### 6.2 빌드 타임 캐싱
- 캐시 TTL: 10분
- 중복 Notion API 호출 방지

### 6.3 네비게이션 최적화
- Next.js Link prefetch 활성화
- Framer Motion 최적화 (클릭 이벤트 비차단)
- 이미지 pointer-events-none

### 6.4 이미지 최적화
- 첫 3개 이미지 우선 로딩 (LCP 개선)
- Blur placeholder (CLS 방지)
- 중요 이미지 Preload

---

## 7. 환경 변수

### 7.1 필수 환경 변수
```bash
NOTION_API_KEY=secret_xxxxxxxxxxxx
NOTION_DATABASE_ID=11223344aabb11223344aabb11223344
```

### 7.2 선택 환경 변수
```bash
# 프로필 정보
NOTION_PROFILE_DATABASE_ID=87654321dcba87654321dcba87654321

# 사이트 설정
NOTION_SITE_DATABASE_ID=12345678abcd12345678abcd12345678

# About 페이지
NOTION_ABOUT_PAGE_ID=a1b2c3d4e5f6789012345678abcdef12

# 사이트 URL
NEXT_PUBLIC_SITE_URL=https://username.github.io/repo-name

# Giscus 댓글
NEXT_PUBLIC_GISCUS_REPO=username/repo-name
NEXT_PUBLIC_GISCUS_REPO_ID=R_kgDOGxxxxxxx
NEXT_PUBLIC_GISCUS_CATEGORY=General
NEXT_PUBLIC_GISCUS_CATEGORY_ID=DIC_kwDOGxxxxxxx
```

---

## 8. 개발 명령어

```bash
# 개발 서버 (Turbopack)
npm run dev

# 프로덕션 빌드
npm run build

# 빌드 미리보기
npm run start

# 린팅
npm run lint

# 단위 테스트 (Vitest, 170개)
npm run test
npm run test -- --watch

# E2E 테스트 (Playwright, 5개)
npm run test:e2e
npm run test:e2e:ui
```

---

## 9. 제약사항 및 고려사항

### 9.1 기술적 제약
- **필수 기술 스택 변경 불가**: shadcn/ui, Lucide React, Framer Motion, Zod는 필수
- **Server-Side Rendering 불가**: Notion renderer는 HTML 문자열 생성 (React 컴포넌트 사용 불가)
- **GitHub Pages Base Path**: `next.config.ts`에 Repository 이름 설정 필요

### 9.2 Notion API 제약
- Rate Limit: 초당 3회 요청
- 빌드 타임에만 API 호출 (런타임 호출 없음)
- Integration 권한 필요

---

## 10. 성공 기준

### 10.1 성능 지표
- Lighthouse Performance: 90+ 점
- LCP (Largest Contentful Paint): < 2.5초
- CLS (Cumulative Layout Shift): < 0.1
- FCP (First Contentful Paint): < 1.8초

### 10.2 품질 지표
- 단위 테스트 커버리지: 170개 이상
- E2E 테스트 통과: 5개 파일
- ESLint 에러 0개
- TypeScript strict mode 준수

---

## 11. 참고 문서

- [Notion 설정 가이드](../docs/NOTION_SETUP_GUIDE.md)
- [Tailwind Typography 가이드](../docs/TAILWIND_TYPOGRAPHY_GUIDE.md)
- [트러블슈팅 가이드](../docs/TROUBLESHOOTING_GUIDE.md)
- [Giscus 설정 가이드](../docs/GISCUS_SETUP_GUIDE.md)

---

**문서 버전**: v2.0
**마지막 업데이트**: 2025-10-23
