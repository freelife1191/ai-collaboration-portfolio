# UI/UX 디자인 및 상세 화면 설계
## [프로젝트 이름] - [프로젝트 설명]

**문서 버전**: [버전]
**작성일**: [작성일]
**작성자**: [작성자]
**디자인 시스템**: [디자인 시스템명]

---

## ⚠️ 반드시 지켜야 하는 디자인 가이드

### 필수 참고 문서
이 프로젝트의 모든 UI/UX 디자인 및 상세 화면 설계 시 **반드시** 다음 문서들을 꼼꼼히 읽고 적용해야 합니다:

1. **`@UIUX_PLAN.md`** - 2025 UI/UX 트렌드 및 전략
   - 2025 UI/UX 트렌드 (AI 개인화, 3D 요소, Bento Grid, Glassmorphism 등)
   - 디자인 시스템 아키텍처
   - 컬러 팔레트
   - 타이포그래피
   - 컴포넌트 디자인
   - 애니메이션 & 인터랙션
   - 반응형 디자인
   - 접근성

2. **`@UIUX_LAYOUT_GUIDE.md`** - 모던 레이아웃 패턴
   - CSS Grid Patterns
   - Container Queries
   - Page Structure Templates
   - Dashboard Layouts
   - Responsive Strategies

### 핵심 기술 스택 (변경 불가)

**UI 구성 기술**:
- ✅ **TailwindCSS** - 모든 스타일링
- ✅ **shadcn/ui** - 컴포넌트 라이브러리
- ✅ **Lucide** - 아이콘 시스템

> ⚠️ **중요**: UI 구성은 위 3가지 기술만 사용합니다. 다른 CSS 프레임워크나 컴포넌트 라이브러리는 사용하지 않습니다.

### 리소스 가이드라인

**애니메이션**:
- ✅ **Framer Motion** - 모든 애니메이션 및 인터랙션
- 사용 예: 페이지 전환, 컴포넌트 애니메이션, 스크롤 효과, 마이크로 인터랙션

**이미지**:
- ✅ **Unsplash** (unsplash.com)
- 고품질 무료 이미지
- 프로젝트 테마에 맞는 이미지 선택

**일러스트 및 그래픽**:
- ✅ **unDraw** (undraw.co/illustrations)
- 맞춤형 컬러 조정 가능
- 일관된 스타일의 일러스트

**UI 패턴 및 배경**:
- ✅ **Hero Patterns** (heropatterns.com)
- SVG 기반 패턴
- TailwindCSS와 완벽 호환

### 디자인 원칙

1. **일관성 (Consistency)**
   - 모든 화면에서 동일한 디자인 토큰 사용
   - 컴포넌트 재사용성 최대화
   - 색상, 타이포그래피, 간격 시스템 일관 적용

2. **접근성 (Accessibility)**
   - WCAG 2.1 AA 이상 준수
   - 키보드 네비게이션 지원
   - 스크린 리더 호환성
   - 적절한 색상 대비

3. **반응형 (Responsive)**
   - Mobile-First 접근
   - Container Queries 활용
   - Fluid Typography 적용
   - 모든 디바이스 지원 (320px~2560px)

4. **성능 (Performance)**
   - 최소한의 애니메이션 사용
   - 이미지 최적화 (WebP, lazy loading)
   - CSS-in-JS 대신 TailwindCSS 사용
   - 번들 크기 최소화

### 컴포넌트 참고 리소스

**shadcn/ui 기반 리소스**:
- [shadcn/ui 공식](https://ui.shadcn.com) - 컴포넌트 기반 레이아웃
- [AI 서비스 디자인](https://shadcn.io) - AI 서비스 특화 디자인
- [shadcn/ui Blocks 1](https://shadcnblocks.com/blocks) - 다양한 블록 패턴
- [shadcn/ui Blocks 2](https://shadcnui-blocks.com/templates) - 완성된 템플릿
- [shadcn Batch Tool](https://shadcn.batchtool.com) - 유틸리티 컬렉션
- [Magic UI Pro](https://pro.magicui.design) - 프리미엄 템플릿
- [DaisyUI](https://daisyui.com) - Tailwind CSS 기반 컴포넌트

### 레이아웃 참고 사이트
- [Figma Community](https://figma.com/community)
- [Dribbble](https://dribbble.com)
- [Awwwards](https://awwwards.com)
- [Mobbin](https://mobbin.com)

---

## 📋 목차

1. [디자인 시스템 개요](#디자인-시스템-개요)
2. [디자인 토큰](#디자인-토큰)
3. [컴포넌트 라이브러리](#컴포넌트-라이브러리)
4. [화면별 상세 설계](#화면별-상세-설계)
5. [사용자 플로우](#사용자-플로우)
6. [반응형 디자인](#반응형-디자인)
7. [접근성 가이드라인](#접근성-가이드라인)
8. [인터랙션 패턴](#인터랙션-패턴)

---

## 디자인 시스템 개요

### 디자인 철학

**[프로젝트 이름]의 디자인 원칙**:
1. **[원칙 1] ([영문명])**: [설명]
2. **[원칙 2] ([영문명])**: [설명]
3. **[원칙 3] ([영문명])**: [설명]
4. **[원칙 4] ([영문명])**: [설명]

### [연도] 디자인 트렌드 적용

참고: `[참고 문서 경로]`

**적용된 트렌드**:
1. **[트렌드 1]**: [적용 설명]
2. **[트렌드 2]**: [적용 설명]
3. **[트렌드 3]**: [적용 설명]
4. **[트렌드 4]**: [적용 설명]
5. **[트렌드 5]**: [적용 설명]
6. **[트렌드 6]**: [적용 설명]

---

## 디자인 토큰

### Color Palette

#### Primary Colors ([테마 이름])

```typescript
const colors = {
  primary: {
    50: '[HEX]',
    100: '[HEX]',
    200: '[HEX]',
    300: '[HEX]',
    400: '[HEX]',
    500: '[HEX]', // [메인 컬러 설명]
    600: '[HEX]',
    700: '[HEX]',
    800: '[HEX]',
    900: '[HEX]',
  },
}
```

#### Accent Colors ([사용 용도])

```typescript
const accentColors = {
  [컬러1]: '[HEX]',      // [사용처]
  [컬러2]: '[HEX]',   // [사용처]
  [컬러3]: '[HEX]',     // [사용처]
  [컬러4]: '[HEX]',    // [사용처]
}
```

#### Semantic Colors

```typescript
const semanticColors = {
  success: '[HEX]',   // [사용처]
  warning: '[HEX]',   // [사용처]
  error: '[HEX]',     // [사용처]
  info: '[HEX]',      // [사용처]
}
```

#### Neutral Colors

```typescript
const neutralColors = {
  50: '[HEX]',   // [사용처]
  100: '[HEX]',  // [사용처]
  200: '[HEX]',  // [사용처]
  300: '[HEX]',  // [사용처]
  400: '[HEX]',  // [사용처]
  500: '[HEX]',  // [사용처]
  600: '[HEX]',  // [사용처]
  700: '[HEX]',  // [사용처]
  800: '[HEX]',  // [사용처]
  900: '[HEX]',  // [사용처]
}
```

#### Dark Mode Colors

```css
[data-theme="dark"] {
  --bg-primary: [HEX];
  --bg-secondary: [HEX];
  --bg-tertiary: [HEX];
  --text-primary: [HEX];
  --text-secondary: [HEX];
  --border: [HEX];

  /* [효과 이름] in dark */
  --[효과명]-bg: rgba([R], [G], [B], [Alpha]);
  --[효과명]-border: rgba([R], [G], [B], [Alpha]);
}
```

---

### Typography

#### Font Families

```typescript
const fonts = {
  display: "'[폰트 이름]', serif",  // [사용처]
  body: "'[폰트 이름]', sans-serif",  // [사용처]
  mono: "'[폰트 이름]', monospace",   // [사용처]
  korean: "'[폰트 이름]', sans-serif",  // [사용처]
}
```

#### Type Scale ([타이포그래피 방식])

```css
:root {
  /* Headings */
  --text-8xl: clamp([최소], [선호], [최대]);      /* [용도] */
  --text-7xl: clamp([최소], [선호], [최대]);
  --text-6xl: clamp([최소], [선호], [최대]);
  --text-5xl: clamp([최소], [선호], [최대]);
  --text-4xl: clamp([최소], [선호], [최대]);
  --text-3xl: clamp([최소], [선호], [최대]);
  --text-2xl: clamp([최소], [선호], [최대]);
  --text-xl: clamp([최소], [선호], [최대]);

  /* Body */
  --text-lg: clamp([최소], [선호], [최대]);
  --text-base: clamp([최소], [선호], [최대]);
  --text-sm: clamp([최소], [선호], [최대]);
  --text-xs: clamp([최소], [선호], [최대]);

  /* Line Heights */
  --leading-tight: [값];
  --leading-snug: [값];
  --leading-normal: [값];
  --leading-relaxed: [값];
  --leading-loose: [값];

  /* Font Weights */
  --font-normal: [값];
  --font-medium: [값];
  --font-semibold: [값];
  --font-bold: [값];
  --font-extrabold: [값];
}
```

#### Usage Examples

```css
.[클래스명] {
  font-family: var(--font-display);
  font-size: var(--text-7xl);
  font-weight: var(--font-bold);
  line-height: var(--leading-tight);
  letter-spacing: [값];
  background: linear-gradient([각도], var(--[컬러1]), var(--[컬러2]));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.[클래스명] {
  font-family: var(--font-body);
  font-size: var(--text-base);
  line-height: var(--leading-normal);
  color: var(--text-primary);
}
```

---

### Spacing Scale

```typescript
const spacing = {
  0: '0',
  1: '[값]rem',   // [픽셀]px
  2: '[값]rem',    // [픽셀]px
  3: '[값]rem',   // [픽셀]px
  4: '[값]rem',      // [픽셀]px
  5: '[값]rem',   // [픽셀]px
  6: '[값]rem',    // [픽셀]px
  8: '[값]rem',      // [픽셀]px
  10: '[값]rem',   // [픽셀]px
  12: '[값]rem',     // [픽셀]px
  16: '[값]rem',     // [픽셀]px
  20: '[값]rem',     // [픽셀]px
  24: '[값]rem',     // [픽셀]px
  32: '[값]rem',     // [픽셀]px
}
```

---

### Border Radius

```typescript
const borderRadius = {
  none: '0',
  sm: '[값]rem',    // [픽셀]px
  base: '[값]rem',   // [픽셀]px
  md: '[값]rem',    // [픽셀]px
  lg: '[값]rem',       // [픽셀]px
  xl: '[값]rem',     // [픽셀]px
  '2xl': '[값]rem',    // [픽셀]px
  full: '9999px',   // Circle
}
```

---

### Shadows

```css
:root {
  /* Light Mode */
  --shadow-sm: [값];
  --shadow-base: [값];
  --shadow-md: [값];
  --shadow-lg: [값];
  --shadow-xl: [값];
  --shadow-2xl: [값];

  /* [효과 이름] Shadow */
  --shadow-[효과명]: [값];

  /* [효과 이름] Glow */
  --shadow-[효과명]-[컬러]: [값];
}

[data-theme="dark"] {
  --shadow-sm: [값];
  --shadow-base: [값];
  --shadow-md: [값];
  --shadow-lg: [값];
  --shadow-xl: [값];
}
```

---

## 컴포넌트 라이브러리

### Button System

#### Primary Button

```css
.btn-primary {
  /* Base */
  padding: [값];
  background: var(--[컬러명]);
  color: [컬러];
  border: none;
  border-radius: var(--radius-[크기]);
  font-weight: [값];
  font-size: var(--text-base);
  cursor: pointer;

  /* Shadow */
  box-shadow: [값];

  /* Transition */
  transition: all [시간] ease;

  /* Hover */
  &:hover {
    background: var(--[컬러명]);
    transform: translateY([값]);
    box-shadow: [값];
  }

  /* Active */
  &:active {
    transform: translateY([값]);
  }

  /* Disabled */
  &:disabled {
    background: var(--[컬러명]);
    cursor: not-allowed;
    transform: none;
  }
}
```

#### [버튼 타입] Button ([모드])

```css
.btn-[타입명] {
  background: transparent;
  border: [두께] solid var(--[컬러명]);
  color: var(--[컬러명]);
  padding: [값];
  border-radius: var(--radius-[크기]);
  font-weight: [값];
  text-shadow: [값];
  box-shadow: var(--shadow-[효과명]);
  transition: all [시간] ease;

  &:hover {
    background: var(--[컬러명]);
    color: var(--[컬러명]);
    box-shadow: [값];
  }
}
```

#### Button Variants

```typescript
type ButtonVariant = '[타입1]' | '[타입2]' | '[타입3]' | '[타입4]' | '[타입5]'
type ButtonSize = '[크기1]' | '[크기2]' | '[크기3]'

interface ButtonProps {
  variant?: ButtonVariant
  size?: ButtonSize
  icon?: React.ReactNode
  loading?: boolean
  disabled?: boolean
  onClick?: () => void
  children: React.ReactNode
}
```

**ASCII Preview**:
```
┌────────────────────────────┐
│  [  [버튼 텍스트]  ]       │  [타입 1]
│  [  [버튼 텍스트]  ]       │  [타입 2]
│  [  [버튼 텍스트]  ]       │  [타입 3]
│  [ [아이콘] [텍스트] ]     │  [타입 4]
└────────────────────────────┘
```

---

### Card System

#### [카드 타입 1]

```css
.card-[타입명] {
  background: rgba([R], [G], [B], [Alpha]);
  backdrop-filter: blur([값]) saturate([값]%);
  -webkit-backdrop-filter: blur([값]) saturate([값]%);
  border-radius: var(--radius-[크기]);
  border: [두께] solid rgba([R], [G], [B], [Alpha]);
  padding: [값];
  box-shadow: var(--shadow-[효과명]);
  transition: transform [시간] ease;

  &:hover {
    transform: translateY([값]);
  }
}

[data-theme="dark"] .card-[타입명] {
  background: var(--[컬러명]);
  border: [두께] solid var(--[컬러명]);
}
```

#### [카드 타입 2]

```css
.card-[타입명] {
  background: var(--bg-secondary);
  border-radius: var(--radius-[크기]);
  padding: [값];
  border: [두께] solid var(--border);
  transition: all [시간] ease;

  &:hover {
    border-color: var(--[컬러명]);
    box-shadow: [값];
  }
}
```

#### [카드 타입 3]

**구조**:
```
┌─────────────────────────────────────┐
│  [아이콘] [제목]          [메뉴]    │
│                                     │
│  [메타 정보 1]: [값]                │
│  [메타 정보 2]: [값]                │
│                                     │
│  [콘텐츠]: [내용]                   │
│                                     │
│  [태그1]  [태그2]                   │
└─────────────────────────────────────┘
```

```css
.[카드 클래스명] {
  display: grid;
  grid-template-rows: auto 1fr auto;
  background: var(--bg-primary);
  border: [두께] solid var(--border);
  border-radius: var(--radius-[크기]);
  padding: [값];
  gap: [값];
  transition: all [시간] ease;
  cursor: pointer;

  &:hover {
    border-color: var(--[컬러명]);
    box-shadow: var(--shadow-[크기]);
  }
}

.[카드 클래스명]__header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: [값];
}

.[카드 클래스명]__title {
  display: flex;
  align-items: center;
  gap: [값];
  font-weight: [값];
  font-size: var(--text-base);
  color: var(--text-primary);
}

.[카드 클래스명]__meta {
  display: flex;
  gap: [값];
  font-size: var(--text-sm);
  color: var(--text-secondary);
}

.[카드 클래스명]__content {
  font-size: var(--text-sm);
  color: var(--text-secondary);
  line-height: var(--leading-relaxed);
  display: -webkit-box;
  -webkit-line-clamp: [줄수];
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.[카드 클래스명]__tags {
  display: flex;
  flex-wrap: wrap;
  gap: [값];
}

.tag {
  padding: [값];
  background: var(--[컬러명]);
  color: var(--[컬러명]);
  border-radius: var(--radius-full);
  font-size: var(--text-xs);
  font-weight: [값];
}
```

---

### Input System

#### Text Input

```css
.input {
  width: 100%;
  padding: [값];
  background: var(--bg-primary);
  border: [두께] solid var(--border);
  border-radius: var(--radius-[크기]);
  font-size: var(--text-base);
  color: var(--text-primary);
  transition: all [시간] ease;

  &:focus {
    outline: none;
    border-color: var(--[컬러명]);
    box-shadow: [값];
  }

  &::placeholder {
    color: var(--text-tertiary);
  }

  &:disabled {
    background: var(--bg-disabled);
    cursor: not-allowed;
  }
}
```

#### Search Input

```css
.search-input {
  position: relative;
  display: flex;
  align-items: center;

  input {
    padding-left: [값]; /* [아이콘 공간] */
  }

  .search-icon {
    position: absolute;
    left: [값];
    color: var(--text-tertiary);
  }
}
```

#### File Upload

**구조**:
```
┌─────────────────────────────────────┐
│                                     │
│        [업로드 아이콘]               │
│                                     │
│     [드래그 앤 드롭 텍스트]          │
│                                     │
│         [또는]                      │
│                                     │
│      [ 파일 선택 버튼 ]              │
│                                     │
│  [지원 형식]: [파일 타입 목록]       │
│  [최대 크기]: [크기 제한]            │
│                                     │
└─────────────────────────────────────┘
```

---

### Modal/Dialog System

#### Modal 구조

```css
.modal-overlay {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, [투명도]);
  backdrop-filter: blur([값]);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: [값];
}

.modal-content {
  background: var(--bg-primary);
  border-radius: var(--radius-[크기]);
  padding: [값];
  max-width: [값];
  width: 90%;
  box-shadow: var(--shadow-[크기]);
  animation: [애니메이션명] [시간] ease;
}

@keyframes [애니메이션명] {
  from {
    opacity: 0;
    transform: scale([값]);
  }
  to {
    opacity: 1;
    transform: scale(1);
  }
}
```

---

### Loading/Spinner System

#### Spinner

```css
.spinner {
  width: [크기];
  height: [크기];
  border: [두께] solid var(--border);
  border-top-color: var(--[컬러명]);
  border-radius: 50%;
  animation: spin [시간] linear infinite;
}

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}
```

#### Skeleton

```css
.skeleton {
  background: linear-gradient(
    90deg,
    var(--bg-secondary) 0%,
    var(--bg-tertiary) 50%,
    var(--bg-secondary) 100%
  );
  background-size: 200% 100%;
  animation: [애니메이션명] [시간] ease-in-out infinite;
  border-radius: var(--radius-[크기]);
}

@keyframes [애니메이션명] {
  to {
    background-position: -200% 0;
  }
}
```

---

## 화면별 상세 설계

### [화면 1 이름]

**목적**: [화면 목적 설명]

**레이아웃 구조**:
```
┌─────────────────────────────────────────────────────┐
│  [헤더]                                              │
├─────────────────────────────────────────────────────┤
│                                                      │
│  [섹션 1]                                            │
│                                                      │
│  [섹션 2]                                            │
│                                                      │
│  [섹션 3]                                            │
│                                                      │
└─────────────────────────────────────────────────────┘
```

**컴포넌트 상세**:

1. **[컴포넌트 1 이름]**
   - [요소 1]: [설명]
   - [요소 2]: [설명]
   - [요소 3]: [설명]

2. **[컴포넌트 2 이름]**
   - [요소 1]: [설명]
   - [요소 2]: [설명]

**인터랙션**:
- [액션 1]: [결과]
- [액션 2]: [결과]

---

### [화면 2 이름]

**목적**: [화면 목적 설명]

**레이아웃 구조** ([레이아웃 타입]):
```
┌──────────┬───────────────────────────────────┐
│          │                                   │
│  [사이드] │  [메인 콘텐츠 영역]                │
│  바      │                                   │
│          │  ┌─────────┐  ┌─────────┐        │
│          │  │ [카드1] │  │ [카드2] │        │
│          │  └─────────┘  └─────────┘        │
│          │                                   │
│          │  ┌─────────┐  ┌─────────┐        │
│          │  │ [카드3] │  │ [카드4] │        │
│          │  └─────────┘  └─────────┘        │
│          │                                   │
└──────────┴───────────────────────────────────┘
```

**그리드 시스템**:
```css
.grid-container {
  display: grid;
  grid-template-columns: [컬럼 정의];
  gap: [값];
  padding: [값];
}

/* 반응형 */
@media (max-width: [브레이크포인트]) {
  .grid-container {
    grid-template-columns: [모바일 컬럼 정의];
  }
}
```

---

### [화면 3 이름]

**목적**: [화면 목적 설명]

**상태별 UI**:

1. **[상태 1] 상태**
   ```
   ┌─────────────────────────┐
   │  [표시 내용]             │
   │                         │
   │  [ [액션 버튼] ]         │
   └─────────────────────────┘
   ```

2. **[상태 2] 상태**
   ```
   ┌─────────────────────────┐
   │  [로딩 인디케이터]       │
   │                         │
   │  [진행률 표시]           │
   └─────────────────────────┘
   ```

3. **[상태 3] 상태**
   ```
   ┌─────────────────────────┐
   │  ✓ [성공 메시지]         │
   │                         │
   │  [내용 표시]             │
   └─────────────────────────┘
   ```

---

## 사용자 플로우

### [주요 플로우 1]

```
[시작 화면]
    ↓
[액션 1]
    ↓
[화면 2]
    ↓
[액션 2] ──→ [조건 분기]
    │              ↓
    │          [경로 A]
    ↓              ↓
[경로 B]      [완료 화면 A]
    ↓
[완료 화면 B]
```

**상세 단계**:
1. [단계 1 설명]
2. [단계 2 설명]
3. [단계 3 설명]
4. [단계 4 설명]

---

### [주요 플로우 2]

**시나리오**:
> [사용자 시나리오 설명]

**플로우**:
1. **[단계 1]** → [화면명]
2. **[단계 2]** → [화면명]
3. **[단계 3]** → [화면명]
4. **[단계 4]** → [화면명]

---

## 반응형 디자인

### 브레이크포인트

```css
:root {
  --breakpoint-sm: 640px;   /* Mobile */
  --breakpoint-md: 768px;   /* Tablet */
  --breakpoint-lg: 1024px;  /* Desktop */
  --breakpoint-xl: 1280px;  /* Large Desktop */
  --breakpoint-2xl: 1536px; /* 4K */
}
```

### [컴포넌트명] 반응형

```css
/* Mobile First */
.[컴포넌트] {
  [모바일 스타일]
}

/* Tablet */
@media (min-width: [브레이크포인트]) {
  .[컴포넌트] {
    [태블릿 스타일]
  }
}

/* Desktop */
@media (min-width: [브레이크포인트]) {
  .[컴포넌트] {
    [데스크톱 스타일]
  }
}
```

---

## 접근성 가이드라인

### WCAG [레벨] 준수

**색상 대비**:
- 일반 텍스트: 최소 [비율]
- 큰 텍스트: 최소 [비율]
- 인터페이스 요소: 최소 [비율]

**키보드 네비게이션**:
```typescript
// Focus Visible
.[요소]:focus-visible {
  outline: [두께] solid var(--[컬러명]);
  outline-offset: [값];
}

// Skip to Content
<a href="#main-content" className="skip-link">
  [텍스트]
</a>
```

**스크린 리더 지원**:
```tsx
// ARIA Labels
<button aria-label="[설명]">
  <[아이콘] />
</button>

// ARIA Live Region
<div role="status" aria-live="polite">
  [동적 콘텐츠]
</div>
```

---

## 인터랙션 패턴

### [인터랙션 1]

**트리거**: [트리거 조건]
**애니메이션**:
```css
.[클래스] {
  transition: [속성] [시간] [타이밍];
}

.[클래스]:[상태] {
  [변화된 스타일]
}
```

### [인터랙션 2]

**구현**:
```typescript
const [함수명] = () => {
  // [애니메이션 로직]
}
```

**CSS**:
```css
@keyframes [애니메이션명] {
  0% { [초기 상태] }
  50% { [중간 상태] }
  100% { [최종 상태] }
}
```

---

## 다크 모드

### 테마 전환

```typescript
const [모드, 세터] = useState<'light' | 'dark'>('light')

const [전환함수] = () => {
  const newMode = [모드] === 'light' ? 'dark' : 'light'
  [세터](newMode)
  document.documentElement.setAttribute('data-theme', newMode)
}
```

### 컬러 시스템 매핑

| 요소 | Light Mode | Dark Mode |
|------|------------|-----------|
| [요소 1] | [컬러] | [컬러] |
| [요소 2] | [컬러] | [컬러] |
| [요소 3] | [컬러] | [컬러] |
| [요소 4] | [컬러] | [컬러] |

---

이 디자인 시스템은 일관성 있고 접근성이 뛰어나며 사용자 친화적인 인터페이스를 제공합니다.
