# 현재 프로젝트 vs Tailwind Typography 데모 스타일 비교 분석

## 개요
현재 프로젝트의 스타일링과 [Tailwind CSS Typography 데모](https://tailwindcss-typography.vercel.app/)를 면밀히 비교하여 차이점과 개선점을 분석합니다.

## 주요 차이점 분석

### 1. **기본 Typography 플러그인 사용 방식**

#### 현재 프로젝트
```html
<!-- 현재 방식 -->
<div className="prose prose-gray max-w-none dark:prose-invert">
  <div className="not-prose">
    <div className="toc">...</div>
  </div>
  <div dangerouslySetInnerHTML={{ __html: contentWithIds }} />
</div>
```

#### Typography 데모
```html
<!-- 데모 방식 -->
<article class="prose">
  <h1>제목</h1>
  <p>본문 내용...</p>
  <div class="not-prose">
    <!-- 특별한 컴포넌트 -->
  </div>
</article>
```

**차이점**: 현재 프로젝트는 `prose-gray` 테마를 명시적으로 사용하고, 데모는 기본 `prose` 클래스만 사용합니다.

### 2. **색상 테마 적용**

#### 현재 프로젝트
- **테마**: `prose-gray` 명시적 사용
- **다크모드**: `dark:prose-invert` 적용
- **최대 너비**: `max-w-none`으로 제한 해제

#### Typography 데모
- **테마**: 기본 `prose` 클래스 (gray가 기본값)
- **다크모드**: 별도 설정 없음 (데모에서는 수동 토글)
- **최대 너비**: 기본 제한 유지

### 3. **커스텀 컴포넌트 스타일링**

#### 현재 프로젝트의 추가 스타일
```css
/* TOC 스타일 */
.not-prose .toc {
  @apply bg-gray-50 dark:bg-gray-800/50 border border-gray-200 dark:border-gray-700 rounded-lg p-4 my-6;
}

/* 북마크 스타일 */
.bookmark {
  @apply border border-gray-300 dark:border-gray-600 rounded-lg p-4 my-4 mx-4 hover:shadow-lg transition-all duration-200 bg-gray-50 dark:bg-gray-800;
}

/* 파일 다운로드 스타일 */
.file-download {
  @apply inline-flex items-center px-4 py-3 bg-gray-100 dark:bg-gray-800 border border-gray-300 dark:border-gray-600 rounded-lg hover:bg-gray-200 dark:hover:bg-gray-700 transition-all duration-200 mx-4 my-2 shadow-sm hover:shadow-md;
}

/* 동영상 임베딩 스타일 */
.video-embed {
  @apply w-full mx-auto my-6 rounded-lg overflow-hidden shadow-lg;
  max-width: 32rem !important;
}
```

#### Typography 데모
- **기본 요소만**: 제목, 본문, 링크, 코드, 테이블 등 기본 HTML 요소만 스타일링
- **커스텀 컴포넌트 없음**: 북마크, 파일 다운로드, 동영상 임베딩 등 Notion 특화 컴포넌트 없음

### 4. **코드 하이라이팅**

#### 현재 프로젝트
```css
/* Prism.js 토큰 색상 - GitHub 스타일 */
.token.comment { @apply text-gray-400; }
.token.punctuation { @apply text-gray-200; }
.token.property { @apply text-red-300; }
.token.selector { @apply text-green-300; }
.token.operator { @apply text-cyan-300; }
.token.atrule { @apply text-blue-300; }
.token.function { @apply text-yellow-300; }
.token.regex { @apply text-orange-300; }
```

#### Typography 데모
- **기본 코드 스타일만**: 구문 강조 없이 기본 코드 블록 스타일만 제공
- **하이라이팅 없음**: 색상 구분 없는 단순한 코드 표시

### 5. **Notion 특화 기능**

#### 현재 프로젝트만의 고유 기능
```css
/* Notion 블록 스타일 */
.callout {
  @apply border-l-4 border-blue-500 bg-blue-50 dark:bg-blue-900/20 p-4 my-4 rounded-r-md;
}

/* TOC 들여쓰기 */
.toc ul ul { @apply ml-6 mt-1; }
.toc li[data-level="1"] { @apply font-semibold text-base; }
.toc li[data-level="2"] { @apply text-sm ml-4; }

/* 토글/접기 스타일 */
.prose details { @apply my-4; }
.prose summary { @apply cursor-pointer font-semibold; }
```

#### Typography 데모
- **Notion 기능 없음**: 콜아웃, TOC, 토글 등 Notion 특화 기능 없음
- **기본 HTML만**: 표준 HTML 요소에 대한 기본 스타일링만 제공

### 6. **반응형 디자인**

#### 현재 프로젝트
```css
/* 반응형 동영상 임베딩 */
@media (max-width: 1024px) {
  .video-embed { max-width: 28rem !important; }
}
@media (max-width: 768px) {
  .video-embed { max-width: calc(100vw - 2rem) !important; }
}
@media (max-width: 480px) {
  .video-embed { max-width: calc(100vw - 1rem) !important; }
}
```

#### Typography 데모
- **기본 반응형**: Typography 플러그인의 기본 반응형 기능만 사용
- **커스텀 미디어 쿼리 없음**: 특별한 반응형 조정 없음

## 개선 권장사항

### 1. **Typography 플러그인 최적화**
```html
<!-- 현재 -->
<div className="prose prose-gray max-w-none dark:prose-invert">

<!-- 개선안 -->
<article className="prose prose-gray max-w-none dark:prose-invert">
```

### 2. **CSS 변수 활용한 커스터마이징**
```javascript
// tailwind.config.js
module.exports = {
  theme: {
    extend: {
      typography: {
        DEFAULT: {
          css: {
            // Notion 특화 색상 커스터마이징
            '--tw-prose-body': '#374151',
            '--tw-prose-headings': '#111827',
            '--tw-prose-links': '#3b82f6', // 브랜드 색상
            '--tw-prose-quote-borders': '#3b82f6',
          },
        },
      },
    },
  },
}
```

### 3. **not-prose 활용 최적화**
```html
<!-- 현재 구조 개선 -->
<article className="prose prose-gray max-w-none dark:prose-invert">
  <div className="not-prose">
    <!-- TOC, 북마크 등 특별한 컴포넌트 -->
  </div>
  
  <!-- 본문 콘텐츠 -->
  <div dangerouslySetInnerHTML={{ __html: contentWithIds }} />
</article>
```

### 4. **성능 최적화**
- 불필요한 `!important` 사용 최소화
- CSS 변수를 통한 일관된 색상 관리
- Typography 플러그인의 기본 기능 최대한 활용

## 결론

### 현재 프로젝트의 장점
1. **Notion 특화 기능**: 콜아웃, 북마크, 파일 다운로드 등 Notion 블록 지원
2. **상세한 스타일링**: TOC 들여쓰기, 코드 하이라이팅 등 세밀한 조정
3. **다크모드 지원**: 완전한 다크모드 테마 적용

### Typography 데모의 장점
1. **단순함**: 기본 HTML 요소에 대한 깔끔한 스타일링
2. **표준 준수**: 웹 표준에 따른 타이포그래피
3. **성능**: 최소한의 CSS로 최대 효과

### 권장 개선 방향
1. **Typography 플러그인 기본 기능 최대 활용**
2. **CSS 변수를 통한 일관된 테마 관리**
3. **not-prose를 활용한 컴포넌트 분리**
4. **성능과 유지보수성 고려한 구조 개선**
