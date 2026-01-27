# UI/UX 레이아웃 가이드 - 2025 Modern Patterns

> 모던 CSS Grid, Container Queries, Fluid Typography를 활용한 레이아웃 패턴

---

## 목차

1. [Modern Layout 기초](#modern-layout-기초)
2. [CSS Grid Patterns](#css-grid-patterns)
3. [Container Queries](#container-queries)
4. [Page Structure Templates](#page-structure-templates)
5. [Dashboard Layouts](#dashboard-layouts)
6. [Blog Layouts](#blog-layouts)
7. [E-commerce Layouts](#e-commerce-layouts)
8. [Chat Layouts](#chat-layouts)
9. [Responsive Strategies](#responsive-strategies)

---

## Modern Layout 기초

### Grid-First Approach

**철학**: CSS Grid를 주요 레이아웃 도구로 사용하고, Flexbox는 1차원 정렬에만 활용

**기본 그리드 시스템:**
```css
.layout-grid {
  display: grid;
  grid-template-columns:
    [full-start] minmax(1rem, 1fr)
    [content-start] minmax(0, 1200px)
    [content-end] minmax(1rem, 1fr)
    [full-end];
  gap: var(--spacing-6);
}

.layout-grid > * {
  grid-column: content;
}

.layout-grid > .full-width {
  grid-column: full;
}
```

**레이아웃 예시:**
```
┌────────────────────────────────────┐
│         Full Width Header          │
├────────────────────────────────────┤
│  ┌──────────────────────────────┐  │
│  │    Content (max 1200px)      │  │
│  │                              │  │
│  │  ┌────────────────────────┐  │  │
│  │  │   Nested Content       │  │  │
│  │  └────────────────────────┘  │  │
│  └──────────────────────────────┘  │
├────────────────────────────────────┤
│       Full Width Footer            │
└────────────────────────────────────┘
```

---

## CSS Grid Patterns

### 1. Bento Grid (Asymmetric Grid)

```css
.bento-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 1rem;
  grid-auto-rows: minmax(150px, auto);
}

/* Large item */
.bento-large {
  grid-column: span 2;
  grid-row: span 2;
}

/* Wide item */
.bento-wide {
  grid-column: span 2;
}

/* Tall item */
.bento-tall {
  grid-row: span 2;
}
```

**비주얼 예시:**
```
┌──────────┬─────────┬─────────┐
│          │  Card   │  Card   │
│  Large   ├─────────┼─────────┤
│  2x2     │  Card   │  Card   │
├──────────┴─────────┤         │
│   Wide Card 2x1    │  Tall   │
├──────────┬─────────┤  1x2    │
│  Card    │  Card   ├─────────┤
└──────────┴─────────┴─────────┘
```

### 2. Sidebar Layout

```css
.sidebar-layout {
  display: grid;
  grid-template-columns: 250px 1fr;
  grid-template-rows: 64px 1fr;
  grid-template-areas:
    "sidebar header"
    "sidebar main";
  min-height: 100vh;
}

.sidebar {
  grid-area: sidebar;
  background: var(--bg-secondary);
  border-right: 1px solid var(--border);
}

.header {
  grid-area: header;
  background: var(--bg-primary);
  border-bottom: 1px solid var(--border);
}

.main {
  grid-area: main;
  padding: 2rem;
}

/* Responsive: hide sidebar on mobile */
@media (max-width: 768px) {
  .sidebar-layout {
    grid-template-columns: 1fr;
    grid-template-areas:
      "header"
      "main";
  }

  .sidebar {
    display: none;
  }
}
```

**구조:**
```
┌─────────┬──────────────────────┐
│         │      Header          │
│ Sidebar ├──────────────────────┤
│         │                      │
│  Nav    │    Main Content      │
│         │                      │
│         │                      │
└─────────┴──────────────────────┘
```

### 3. Holy Grail Layout

```css
.holy-grail {
  display: grid;
  grid-template-columns: 200px 1fr 200px;
  grid-template-rows: auto 1fr auto;
  grid-template-areas:
    "header header header"
    "left main right"
    "footer footer footer";
  min-height: 100vh;
}

.header { grid-area: header; }
.left-sidebar { grid-area: left; }
.main-content { grid-area: main; }
.right-sidebar { grid-area: right; }
.footer { grid-area: footer; }

/* Tablet: collapse right sidebar */
@media (max-width: 1024px) {
  .holy-grail {
    grid-template-columns: 200px 1fr;
    grid-template-areas:
      "header header"
      "left main"
      "footer footer";
  }
  .right-sidebar { display: none; }
}

/* Mobile: single column */
@media (max-width: 768px) {
  .holy-grail {
    grid-template-columns: 1fr;
    grid-template-areas:
      "header"
      "main"
      "footer";
  }
  .left-sidebar { display: none; }
}
```

**구조:**
```
┌────────────────────────────────┐
│          Header                │
├──────┬─────────────────┬───────┤
│      │                 │       │
│ Left │   Main Content  │ Right │
│ Side │                 │ Side  │
│      │                 │       │
├──────┴─────────────────┴───────┤
│          Footer                │
└────────────────────────────────┘
```

### 4. Masonry Layout

```css
.masonry {
  column-count: 3;
  column-gap: 1rem;
}

.masonry-item {
  break-inside: avoid;
  margin-bottom: 1rem;
}

@media (max-width: 1024px) {
  .masonry {
    column-count: 2;
  }
}

@media (max-width: 640px) {
  .masonry {
    column-count: 1;
  }
}
```

---

## Container Queries

### Responsive Component Pattern

```css
.card-container {
  container-type: inline-size;
  container-name: card;
}

/* Small card: vertical layout */
@container card (max-width: 399px) {
  .card-content {
    display: flex;
    flex-direction: column;
  }

  .card-image {
    width: 100%;
    height: 200px;
  }
}

/* Medium card: 2-column layout */
@container card (min-width: 400px) and (max-width: 599px) {
  .card-content {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 1rem;
  }
}

/* Large card: 3-column layout */
@container card (min-width: 600px) {
  .card-content {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    gap: 1.5rem;
  }
}
```

**Benefits:**
- 컴포넌트 자체 크기에 따라 반응
- 재사용 가능한 컴포넌트
- 뷰포트 크기에 독립적

---

## Page Structure Templates

### 1. Dashboard Template

```css
.dashboard {
  display: grid;
  grid-template-columns: 240px 1fr;
  grid-template-rows: 64px 1fr;
  grid-template-areas:
    "sidebar header"
    "sidebar content";
  min-height: 100vh;
}

.dashboard-header {
  grid-area: header;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 2rem;
  background: var(--bg-primary);
  border-bottom: 1px solid var(--border);
}

.dashboard-sidebar {
  grid-area: sidebar;
  background: var(--bg-secondary);
  padding: 1rem;
}

.dashboard-content {
  grid-area: content;
  padding: 2rem;
  background: var(--bg-tertiary);
}
```

**구조:**
```
┌──────────┬───────────────────────────────┐
│          │  Header                       │
│          │  [Search] [Profile] [Notif]   │
│ Sidebar  ├───────────────────────────────┤
│          │                               │
│ - Home   │   ┌─────┐ ┌─────┐ ┌─────┐    │
│ - Stats  │   │Card │ │Card │ │Card │    │
│ - Users  │   └─────┘ └─────┘ └─────┘    │
│ - Sett   │                               │
│          │   ┌─────────────────────┐     │
│          │   │      Chart          │     │
│          │   └─────────────────────┘     │
└──────────┴───────────────────────────────┘
```

### 2. Landing Page Template

```css
.landing {
  display: grid;
  grid-template-columns: 1fr;
  grid-auto-rows: min-content;
}

.hero {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 4rem;
  align-items: center;
  min-height: 100vh;
  padding: 0 2rem;
}

.features {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 2rem;
  padding: 4rem 2rem;
}

.cta {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  padding: 4rem 2rem;
  background: var(--bg-gradient);
}
```

**구조:**
```
┌──────────────────────────────────────┐
│  HERO SECTION                        │
│  ┌────────────┐  ┌────────────┐     │
│  │   Text     │  │   Image    │     │
│  │   CTA      │  │            │     │
│  └────────────┘  └────────────┘     │
├──────────────────────────────────────┤
│  FEATURES                            │
│  ┌────┐  ┌────┐  ┌────┐  ┌────┐    │
│  │ 1  │  │ 2  │  │ 3  │  │ 4  │    │
│  └────┘  └────┘  └────┘  └────┘    │
├──────────────────────────────────────┤
│  CTA SECTION                         │
│  [ Sign Up Now ]                     │
└──────────────────────────────────────┘
```

### 3. Article/Blog Template

```css
.article-layout {
  display: grid;
  grid-template-columns:
    [full-start] minmax(1rem, 1fr)
    [wide-start] minmax(0, 100px)
    [content-start] minmax(0, 700px) [content-end]
    minmax(0, 100px) [wide-end]
    minmax(1rem, 1fr) [full-end];
  gap: 2rem;
}

.article-layout > * {
  grid-column: content;
}

.article-layout > .wide {
  grid-column: wide;
}

.article-layout > .full {
  grid-column: full;
}
```

**구조:**
```
┌──────────────────────────────────────┐
│  Full Width Hero Image               │
├──────────────────────────────────────┤
│   ┌──────────────────────────────┐   │
│   │  Title & Meta                │   │
│   │  Author, Date, Tags          │   │
│   ├──────────────────────────────┤   │
│   │  Article Content             │   │
│   │  Paragraph 1                 │   │
│   │  Paragraph 2                 │   │
│   └──────────────────────────────┘   │
│  ┌────────────────────────────────┐  │
│  │  Wide Image / Quote            │  │
│  └────────────────────────────────┘  │
│   ┌──────────────────────────────┐   │
│   │  Article Content (cont.)     │   │
│   └──────────────────────────────┘   │
└──────────────────────────────────────┘
```

---

## Dashboard Layouts

### Admin Dashboard

```css
.admin-dashboard {
  display: grid;
  grid-template-columns: 280px 1fr;
  grid-template-rows: 72px 1fr;
  grid-template-areas:
    "sidebar header"
    "sidebar main";
  min-height: 100vh;
}

.admin-header {
  grid-area: header;
  display: flex;
  align-items: center;
  gap: 2rem;
  padding: 0 2rem;
  background: var(--bg-primary);
  border-bottom: 1px solid var(--border);
  box-shadow: 0 2px 4px rgba(0,0,0,0.05);
}

.admin-sidebar {
  grid-area: sidebar;
  background: var(--bg-secondary);
  padding: 1.5rem;
  overflow-y: auto;
}

.admin-main {
  grid-area: main;
  padding: 2rem;
  background: var(--bg-tertiary);
  overflow-y: auto;
}

/* Stats Grid */
.stats-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 1.5rem;
  margin-bottom: 2rem;
}

/* Chart Grid */
.chart-grid {
  display: grid;
  grid-template-columns: 2fr 1fr;
  gap: 1.5rem;
}

@media (max-width: 1024px) {
  .chart-grid {
    grid-template-columns: 1fr;
  }
}
```

**대시보드 구조:**
```
┌──────────┬─────────────────────────────────┐
│          │  Header                         │
│  Logo    │  [Search] [User] [Settings]     │
├──────────┼─────────────────────────────────┤
│          │  Stats Grid                     │
│ Menu     │  ┌─────┐ ┌─────┐ ┌─────┐ ┌────┐│
│ - Home   │  │Users│ │Sales│ │Rev  │ │... ││
│ - Users  │  └─────┘ └─────┘ └─────┘ └────┘│
│ - Prod   │                                 │
│ - Orders │  Charts                         │
│ - Stats  │  ┌──────────────────┐ ┌───────┐│
│ - Sett   │  │  Main Chart      │ │ Pie   ││
│          │  │                  │ │ Chart ││
│          │  └──────────────────┘ └───────┘│
└──────────┴─────────────────────────────────┘
```

### Analytics Dashboard

```css
.analytics-dashboard {
  display: grid;
  grid-template-columns: 200px 1fr 300px;
  grid-template-rows: 64px 1fr;
  grid-template-areas:
    "sidebar header header"
    "sidebar main detail";
  min-height: 100vh;
  gap: 1px;
  background: var(--border);
}

.analytics-sidebar {
  grid-area: sidebar;
  background: var(--bg-primary);
}

.analytics-header {
  grid-area: header;
  background: var(--bg-primary);
}

.analytics-main {
  grid-area: main;
  background: var(--bg-primary);
  padding: 2rem;
}

.analytics-detail {
  grid-area: detail;
  background: var(--bg-primary);
  padding: 1.5rem;
  overflow-y: auto;
}
```

---

## Blog Layouts

### Blog Grid

```css
.blog-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(350px, 1fr));
  gap: 2rem;
  padding: 2rem;
}

.blog-card {
  display: grid;
  grid-template-rows: 200px auto 1fr auto;
  background: var(--bg-primary);
  border-radius: var(--radius-lg);
  overflow: hidden;
  box-shadow: var(--shadow-md);
  transition: transform 0.3s ease;
}

.blog-card:hover {
  transform: translateY(-4px);
  box-shadow: var(--shadow-lg);
}

.blog-card-image {
  grid-row: 1;
  object-fit: cover;
}

.blog-card-title {
  grid-row: 2;
  padding: 1.5rem;
}

.blog-card-excerpt {
  grid-row: 3;
  padding: 0 1.5rem;
}

.blog-card-meta {
  grid-row: 4;
  padding: 1.5rem;
  border-top: 1px solid var(--border);
}
```

**블로그 그리드:**
```
┌─────────────────────────────────────────┐
│  Blog Grid                              │
│  ┌─────────┐ ┌─────────┐ ┌─────────┐   │
│  │ ┌─────┐ │ │ ┌─────┐ │ │ ┌─────┐ │   │
│  │ │Image│ │ │ │Image│ │ │ │Image│ │   │
│  │ └─────┘ │ │ └─────┘ │ │ └─────┘ │   │
│  │ Title   │ │ Title   │ │ Title   │   │
│  │ Excerpt │ │ Excerpt │ │ Excerpt │   │
│  │ Meta    │ │ Meta    │ │ Meta    │   │
│  └─────────┘ └─────────┘ └─────────┘   │
│  ┌─────────┐ ┌─────────┐ ┌─────────┐   │
│  │ ...     │ │ ...     │ │ ...     │   │
│  └─────────┘ └─────────┘ └─────────┘   │
└─────────────────────────────────────────┘
```

### Blog Post Layout

```css
.blog-post {
  display: grid;
  grid-template-columns:
    [full-start] minmax(1rem, 1fr)
    [content-start] minmax(0, 700px) [content-end]
    minmax(1rem, 1fr) [full-end];
  gap: 2rem;
}

.blog-post-header {
  grid-column: content;
  padding: 4rem 0 2rem;
}

.blog-post-image {
  grid-column: full;
  width: 100%;
  max-height: 500px;
  object-fit: cover;
}

.blog-post-content {
  grid-column: content;
}

.blog-post-toc {
  position: sticky;
  top: 2rem;
  grid-column: content-end / full-end;
  padding: 2rem;
  background: var(--bg-secondary);
  border-radius: var(--radius-lg);
}
```

---

## E-commerce Layouts

### Product Grid

```css
.product-grid-layout {
  display: grid;
  grid-template-columns: 250px 1fr;
  gap: 2rem;
  padding: 2rem;
}

.filter-sidebar {
  grid-column: 1;
}

.product-grid {
  grid-column: 2;
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  gap: 1.5rem;
}

@media (max-width: 768px) {
  .product-grid-layout {
    grid-template-columns: 1fr;
  }

  .filter-sidebar {
    display: none;
  }
}
```

**구조:**
```
┌────────┬───────────────────────────────┐
│        │  ┌────┐ ┌────┐ ┌────┐ ┌────┐ │
│ Filter │  │ P1 │ │ P2 │ │ P3 │ │ P4 │ │
│        │  └────┘ └────┘ └────┘ └────┘ │
│ Price  │  ┌────┐ ┌────┐ ┌────┐ ┌────┐ │
│ Brand  │  │ P5 │ │ P6 │ │ P7 │ │ P8 │ │
│ Color  │  └────┘ └────┘ └────┘ └────┘ │
│ Size   │  ┌────┐ ┌────┐ ┌────┐ ┌────┐ │
│        │  │ P9 │ │P10 │ │P11 │ │P12 │ │
│        │  └────┘ └────┘ └────┘ └────┘ │
└────────┴───────────────────────────────┘
```

### Product Detail

```css
.product-detail {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 3rem;
  padding: 2rem;
  max-width: 1200px;
  margin: 0 auto;
}

.product-images {
  grid-column: 1;
  display: grid;
  grid-template-rows: 1fr auto;
  gap: 1rem;
}

.product-main-image {
  aspect-ratio: 1;
  object-fit: cover;
  border-radius: var(--radius-lg);
}

.product-thumbnails {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 0.5rem;
}

.product-info {
  grid-column: 2;
}

@media (max-width: 768px) {
  .product-detail {
    grid-template-columns: 1fr;
  }
}
```

---

## Chat Layouts

### Chat Application

```css
.chat-layout {
  display: grid;
  grid-template-columns: 300px 1fr 250px;
  grid-template-rows: 64px 1fr;
  grid-template-areas:
    "channels-header messages-header details-header"
    "channels messages details";
  height: 100vh;
}

.channels-header {
  grid-area: channels-header;
  background: var(--bg-secondary);
  border-bottom: 1px solid var(--border);
}

.channels-list {
  grid-area: channels;
  background: var(--bg-secondary);
  overflow-y: auto;
}

.messages-header {
  grid-area: messages-header;
  background: var(--bg-primary);
  border-bottom: 1px solid var(--border);
}

.messages-area {
  grid-area: messages;
  display: grid;
  grid-template-rows: 1fr auto;
  background: var(--bg-primary);
}

.messages-list {
  overflow-y: auto;
  padding: 1rem;
}

.message-input {
  padding: 1rem;
  border-top: 1px solid var(--border);
}

.details-panel {
  grid-area: details;
  background: var(--bg-secondary);
  padding: 1.5rem;
  overflow-y: auto;
}

@media (max-width: 1024px) {
  .chat-layout {
    grid-template-columns: 280px 1fr;
    grid-template-areas:
      "channels-header messages-header"
      "channels messages";
  }

  .details-header,
  .details-panel {
    display: none;
  }
}

@media (max-width: 768px) {
  .chat-layout {
    grid-template-columns: 1fr;
    grid-template-areas:
      "messages-header"
      "messages";
  }

  .channels-header,
  .channels-list {
    display: none;
  }
}
```

**채팅 레이아웃:**
```
┌──────────┬─────────────────────┬─────────┐
│ Channels │  Messages           │ Details │
│ Header   │  Header             │ Header  │
├──────────┼─────────────────────┼─────────┤
│ # general│ ┌─────────────────┐ │ Members │
│ # random │ │ Message 1       │ │ - Alice │
│ # dev    │ └─────────────────┘ │ - Bob   │
│          │ ┌─────────────────┐ │ - Carol │
│ @alice   │ │ Message 2       │ │         │
│ @bob     │ └─────────────────┘ │ Files   │
│          │ ┌─────────────────┐ │ - doc.pdf│
│          │ │ Message 3       │ │ - img.png│
│          │ └─────────────────┘ │         │
│          ├─────────────────────┤         │
│          │ [Type message...]   │         │
└──────────┴─────────────────────┴─────────┘
```

---

## Responsive Strategies

### Mobile-First Breakpoints

```css
/* Mobile First */
.responsive-grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: 1rem;
}

/* Tablet (768px+) */
@media (min-width: 768px) {
  .responsive-grid {
    grid-template-columns: 1fr 1fr;
    gap: 1.5rem;
  }
}

/* Desktop (1024px+) */
@media (min-width: 1024px) {
  .responsive-grid {
    grid-template-columns: repeat(3, 1fr);
    gap: 2rem;
  }
}

/* Large Desktop (1280px+) */
@media (min-width: 1280px) {
  .responsive-grid {
    grid-template-columns: repeat(4, 1fr);
    gap: 2.5rem;
  }
}
```

### Fluid Typography

```css
:root {
  --fluid-min-width: 320;
  --fluid-max-width: 1200;
  --fluid-screen: 100vw;
  --fluid-bp: calc(
    (var(--fluid-screen) - var(--fluid-min-width) / 16 * 1rem) /
    (var(--fluid-max-width) - var(--fluid-min-width))
  );
}

.fluid-text {
  font-size: clamp(1rem, 0.5rem + 2vw, 2rem);
  line-height: 1.5;
}

.fluid-heading {
  font-size: clamp(2rem, 1rem + 5vw, 6rem);
  line-height: 1.1;
}
```

### Flexible Spacing

```css
.flexible-spacing {
  padding: clamp(1rem, 3vw, 3rem);
  gap: clamp(0.5rem, 2vw, 2rem);
}
```

---

## 레퍼런스

### 학습 자료
- **CSS Grid Guide**: css-tricks.com/snippets/css/complete-guide-grid/
- **Container Queries**: developer.mozilla.org/en-US/docs/Web/CSS/CSS_Container_Queries
- **Modern CSS**: web.dev/learn/css/

### 도구
- **CSS Grid Generator**: cssgrid-generator.netlify.app
- **Layout Patterns**: web.dev/patterns/layout/

### 레이아웃 참고 사이트
- **Figma Community**: figma.com/community
- **Dribbble**: dribbble.com
- **Behance**: behance.net
- **Awwwards**: awwwards.com
- **Mobbin**: mobbin.com
- **Page Flows**: pageflows.com
- **UI Patterns**: ui-patterns.com

### shadcn/ui 레이아웃 리소스
- **shadcn/ui Blocks 1**: shadcnblocks.com/blocks - 다양한 블록 레이아웃 패턴
- **shadcn/ui Blocks 2**: shadcnui-blocks.com/templates - 완성된 템플릿
- **shadcn/ui Batch Tool**: shadcn.batchtool.com - 레이아웃 유틸리티
- **Magic UI Pro**: pro.magicui.design - 프리미엄 레이아웃 템플릿
- **shadcn/ui 공식**: ui.shadcn.com - 컴포넌트 기반 레이아웃

---

이 레이아웃 가이드는 2025년 모던 CSS 기술을 활용하여 유연하고 반응형이며 유지보수 가능한 레이아웃을 구축하기 위한 실용적인 패턴을 제공합니다.
