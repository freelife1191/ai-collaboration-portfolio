# UI/UX 디자인 플랜 - 2025 포트폴리오 프로젝트

> 2025년 최신 디자인 트렌드를 적용한 모던 웹 애플리케이션 UI/UX 전략

---

## 목차

1. [2025 UI/UX 트렌드](#2025-uiux-트렌드)
2. [디자인 시스템 아키텍처](#디자인-시스템-아키텍처)
3. [레이아웃 패턴](#레이아웃-패턴)
4. [컬러 팔레트](#컬러-팔레트)
5. [타이포그래피](#타이포그래피)
6. [컴포넌트 디자인](#컴포넌트-디자인)
7. [애니메이션 & 인터랙션](#애니메이션--인터랙션)
8. [반응형 디자인](#반응형-디자인)
9. [접근성](#접근성)
10. [프로젝트별 UI/UX 전략](#프로젝트별-uiux-전략)

---

## 2025 UI/UX 트렌드

### 1. AI 개인화 (AI Personalization)

**개념:**
AI와 머신러닝을 활용하여 사용자 행동, 선호도, 실시간 상호작용을 분석해 맞춤형 경험을 제공합니다.

**적용 방법:**
- **적응형 레이아웃**: 사용자 행동 패턴에 따라 UI 구성 변경
- **스마트 추천**: 과거 데이터 기반 콘텐츠/기능 제안
- **동적 콘텐츠**: 시간대, 위치, 디바이스에 따른 콘텐츠 최적화
- **예측 검색**: 사용자 입력 전 의도 파악 및 제안

**구현 예시:**
```typescript
// 사용자 행동 추적
const trackUserBehavior = () => {
  const preferences = {
    preferredTheme: localStorage.getItem('theme'),
    recentSearches: JSON.parse(localStorage.getItem('searches') || '[]'),
    viewHistory: JSON.parse(localStorage.getItem('history') || '[]'),
    interactionPatterns: analyzeClickPatterns()
  };
  return personalizeUI(preferences);
};
```

**적용 프로젝트:**
- AI 문서 관리 (프로젝트 1): 문서 추천, 스마트 검색
- LMS (프로젝트 3): 학습 경로 개인화
- 소셜 미디어 (프로젝트 6): 피드 알고리즘

---

### 2. 인터랙티브 3D 요소

**개념:**
WebGL, Three.js를 활용한 3D 요소로 깊이감과 몰입감을 제공합니다.

**적용 방법:**
- **3D 아이콘**: 회전, 확대/축소 가능한 아이콘
- **3D 카드**: 마우스 호버 시 입체감
- **파티클 효과**: 상호작용 반응 3D 파티클
- **제품 뷰어**: 360도 회전 가능한 제품 모델

**구현 예시:**
```typescript
// Three.js 3D 카드 효과
import * as THREE from 'three';

const create3DCard = (element: HTMLElement) => {
  const scene = new THREE.Scene();
  const camera = new THREE.PerspectiveCamera(75, 1, 0.1, 1000);
  const renderer = new THREE.WebGLRenderer({ alpha: true });

  element.addEventListener('mousemove', (e) => {
    const x = (e.clientX / window.innerWidth) * 2 - 1;
    const y = -(e.clientY / window.innerHeight) * 2 + 1;
    card.rotation.y = x * 0.3;
    card.rotation.x = y * 0.3;
  });
};
```

---

### 3. Bento Grid 레이아웃

**개념:**
일본 도시락에서 영감받은 그리드 시스템으로 다양한 크기의 콘텐츠를 모듈식 배치합니다.

**핵심 특징:**
- 유연한 그리드 구조
- 시각적 계층 명확
- 반응형 친화적
- 인지 부하 감소

**CSS Grid 구현:**
```css
.bento-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 1rem;
  grid-auto-rows: minmax(150px, auto);
}

.bento-item-large {
  grid-column: span 2;
  grid-row: span 2;
}

.bento-item-wide {
  grid-column: span 2;
}

.bento-item-tall {
  grid-row: span 2;
}
```

**레이아웃 예시:**
```
┌────────┬────────┬────────┐
│        │  Card  │  Card  │
│  Hero  ├────────┼────────┤
│  2x2   │  Card  │  Card  │
├────────┴────────┤        │
│  Wide Card 2x1  │  Tall  │
├────────┬────────┤  1x2   │
│  Card  │  Card  ├────────┤
└────────┴────────┴────────┘
```

---

### 4. Pantone 2025: Mocha Mousse

**색상 정보:**
- **Pantone**: 17-1230
- **Hex**: #A47551
- **RGB**: 164, 117, 81
- **느낌**: 따뜻함, 신뢰감, 세련됨

**색상 조합:**
```css
:root {
  /* Primary - Mocha Mousse */
  --mocha-mousse: #A47551;
  --mocha-dark: #8B5E3C;
  --mocha-light: #C49977;

  /* Complementary */
  --soft-blue: #7BA3C7;
  --muted-gold: #D4AF37;
  --sage-green: #8FA98E;

  /* Neutral */
  --beige: #F5F1ED;
  --charcoal: #2C2C2C;
  --warm-white: #FAFAF8;
}
```

**그라데이션:**
```css
.mocha-gradient {
  background: linear-gradient(135deg,
    var(--mocha-light) 0%,
    var(--mocha-mousse) 50%,
    var(--mocha-dark) 100%
  );
}
```

---

### 5. 네온 컬러 & 다크 모드

**다크 모드 컬러 시스템:**
```css
[data-theme="light"] {
  --bg-primary: #FFFFFF;
  --bg-secondary: #F5F5F5;
  --text-primary: #1A1A1A;
  --text-secondary: #666666;
  --border: #E0E0E0;
}

[data-theme="dark"] {
  --bg-primary: #0A0A0A;
  --bg-secondary: #1A1A1A;
  --text-primary: #F5F5F5;
  --text-secondary: #A0A0A0;
  --border: #2A2A2A;

  /* Neon Accents */
  --neon-cyan: #00F0FF;
  --neon-magenta: #FF00FF;
  --neon-green: #00FF88;
  --neon-orange: #FF6B00;
}
```

**네온 효과:**
```css
.neon-button {
  background: transparent;
  border: 2px solid var(--neon-cyan);
  color: var(--neon-cyan);
  text-shadow: 0 0 10px var(--neon-cyan);
  box-shadow: 0 0 20px rgba(0, 240, 255, 0.3);
  transition: all 0.3s ease;
}

.neon-button:hover {
  background: var(--neon-cyan);
  color: var(--bg-primary);
  box-shadow: 0 0 40px rgba(0, 240, 255, 0.6);
}
```

---

### 6. 대담한 타이포그래피

**트렌드:**
- Variable Fonts
- 대형 헤드라인 (80px+)
- Serif 부활
- Kinetic Typography

**폰트 시스템:**
```css
@font-face {
  font-family: 'InterVariable';
  src: url('/fonts/Inter-Variable.woff2') format('woff2');
  font-weight: 100 900;
}

:root {
  --font-display: 'Playfair Display', serif;
  --font-body: 'InterVariable', sans-serif;
  --font-mono: 'JetBrains Mono', monospace;

  --text-xs: 0.75rem;
  --text-sm: 0.875rem;
  --text-base: 1rem;
  --text-lg: 1.125rem;
  --text-xl: 1.25rem;
  --text-2xl: 1.5rem;
  --text-3xl: 1.875rem;
  --text-4xl: 2.25rem;
  --text-5xl: 3rem;
  --text-6xl: 3.75rem;
  --text-7xl: 4.5rem;
  --text-8xl: 6rem;
}
```

**대형 헤드라인:**
```css
.hero-headline {
  font-family: var(--font-display);
  font-size: clamp(3rem, 10vw, 8rem);
  font-weight: 700;
  line-height: 1.1;
  letter-spacing: -0.02em;
  background: linear-gradient(135deg, var(--mocha-mousse), var(--muted-gold));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}
```

---

### 7. Glassmorphism

**구현:**
```css
.glass-card {
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px) saturate(180%);
  -webkit-backdrop-filter: blur(10px) saturate(180%);
  border-radius: 12px;
  border: 1px solid rgba(255, 255, 255, 0.2);
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
}

[data-theme="dark"] .glass-card {
  background: rgba(20, 20, 20, 0.7);
  border: 1px solid rgba(255, 255, 255, 0.1);
}
```

---

### 8. Micro-interactions

**예시:**
```typescript
// Ripple 효과
const createRipple = (e: React.MouseEvent<HTMLButtonElement>) => {
  const button = e.currentTarget;
  const circle = document.createElement('span');
  const diameter = Math.max(button.clientWidth, button.clientHeight);
  const radius = diameter / 2;

  circle.style.width = circle.style.height = `${diameter}px`;
  circle.style.left = `${e.clientX - button.offsetLeft - radius}px`;
  circle.style.top = `${e.clientY - button.offsetTop - radius}px`;
  circle.classList.add('ripple');

  button.appendChild(circle);
  setTimeout(() => circle.remove(), 600);
};
```

---

## 디자인 시스템 아키텍처

### Design Tokens

```typescript
// tokens/colors.ts
export const colors = {
  primary: {
    50: '#FFF8F3',
    100: '#FFEEE0',
    200: '#FFDCC1',
    300: '#FFC89F',
    400: '#FFB07D',
    500: '#A47551',
    600: '#8B5E3C',
    700: '#73482B',
    800: '#5C341C',
    900: '#45200F'
  },
  neutral: {
    50: '#FAFAF8',
    100: '#F5F5F3',
    200: '#E8E8E6',
    300: '#D1D1CF',
    400: '#B0B0AE',
    500: '#898987',
    600: '#6C6C6A',
    700: '#525250',
    800: '#3A3A38',
    900: '#242422'
  },
  accent: {
    cyan: '#00F0FF',
    magenta: '#FF00FF',
    green: '#00FF88',
    orange: '#FF6B00'
  },
  semantic: {
    success: '#10B981',
    warning: '#F59E0B',
    error: '#EF4444',
    info: '#3B82F6'
  }
};

// tokens/spacing.ts
export const spacing = {
  0: '0',
  1: '0.25rem',
  2: '0.5rem',
  3: '0.75rem',
  4: '1rem',
  5: '1.25rem',
  6: '1.5rem',
  8: '2rem',
  10: '2.5rem',
  12: '3rem',
  16: '4rem',
  20: '5rem',
  24: '6rem',
  32: '8rem'
};

// tokens/radius.ts
export const borderRadius = {
  none: '0',
  sm: '0.25rem',
  base: '0.5rem',
  md: '0.75rem',
  lg: '1rem',
  xl: '1.5rem',
  '2xl': '2rem',
  full: '9999px'
};
```

---

## 레이아웃 패턴

### 1. Grid-First Approach

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

### 2. Container Queries

```css
.card-container {
  container-type: inline-size;
  container-name: card;
}

@container card (min-width: 400px) {
  .card-content {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 1rem;
  }
}
```

### 3. Fluid Grid

```css
.fluid-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(min(250px, 100%), 1fr));
  gap: clamp(1rem, 3vw, 2rem);
}
```

---

## 컬러 팔레트

### 프로젝트별 주요 컬러

| 프로젝트 | Primary | Accent | Background |
|---------|---------|--------|------------|
| AI 문서 관리 | Mocha Mousse | Cyan Neon | Warm White |
| 실시간 협업 | Soft Blue | Magenta Neon | Light Gray |
| LMS | Sage Green | Muted Gold | Beige |
| 포트폴리오 | Charcoal | Orange Neon | White |
| E-commerce MSA | Muted Gold | Green Neon | Beige |
| 소셜 미디어 | Magenta | Cyan | Dark |
| ADHD 관리 | Soft Blue | Green | Light |
| Blog | Mocha Mousse | Gold | Cream |
| E-commerce | Muted Gold | Orange | White |
| Chat | Cyan | Magenta | Dark |

---

## 타이포그래피

### 폰트 조합

**디스플레이:**
- Playfair Display (Serif)
- Bebas Neue (Sans-serif)

**본문:**
- Inter Variable
- Noto Sans KR

**코드:**
- JetBrains Mono
- Fira Code

### 타이포그래피 스케일

```css
.text-scale {
  --text-xs: clamp(0.75rem, 0.9vw, 0.875rem);
  --text-sm: clamp(0.875rem, 1vw, 1rem);
  --text-base: clamp(1rem, 1.2vw, 1.125rem);
  --text-lg: clamp(1.125rem, 1.4vw, 1.25rem);
  --text-xl: clamp(1.25rem, 1.6vw, 1.5rem);
  --text-2xl: clamp(1.5rem, 2vw, 1.875rem);
  --text-3xl: clamp(1.875rem, 2.5vw, 2.25rem);
  --text-4xl: clamp(2.25rem, 3vw, 3rem);
  --text-5xl: clamp(3rem, 4vw, 3.75rem);
  --text-6xl: clamp(3.75rem, 5vw, 4.5rem);
}
```

---

## 컴포넌트 디자인

### Button System

```css
.btn-primary {
  background: var(--mocha-mousse);
  color: white;
  padding: 0.75rem 1.5rem;
  border-radius: var(--radius-lg);
  font-weight: 600;
  transition: all 0.2s ease;
  box-shadow: 0 4px 6px rgba(164, 117, 81, 0.2);
}

.btn-primary:hover {
  background: var(--mocha-dark);
  transform: translateY(-2px);
  box-shadow: 0 6px 12px rgba(164, 117, 81, 0.3);
}

.btn-neon {
  background: transparent;
  border: 2px solid var(--neon-cyan);
  color: var(--neon-cyan);
  padding: 0.75rem 1.5rem;
  border-radius: var(--radius-lg);
  font-weight: 600;
  text-shadow: 0 0 10px var(--neon-cyan);
  box-shadow: 0 0 20px rgba(0, 240, 255, 0.3);
  transition: all 0.3s ease;
}

.btn-neon:hover {
  background: var(--neon-cyan);
  color: var(--bg-primary);
  box-shadow: 0 0 40px rgba(0, 240, 255, 0.6);
}
```

### Card System

```css
.card-glass {
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px);
  border-radius: var(--radius-xl);
  border: 1px solid rgba(255, 255, 255, 0.2);
  padding: 1.5rem;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease;
}

.card-glass:hover {
  transform: translateY(-4px);
}

.card-bento {
  background: var(--bg-secondary);
  border-radius: var(--radius-lg);
  padding: 1.5rem;
  border: 1px solid var(--border);
  transition: all 0.3s ease;
}

.card-bento:hover {
  border-color: var(--mocha-mousse);
  box-shadow: 0 10px 30px rgba(164, 117, 81, 0.1);
}
```

---

## 애니메이션 & 인터랙션

### Page Transitions

```typescript
const pageVariants = {
  initial: {
    opacity: 0,
    y: 20
  },
  animate: {
    opacity: 1,
    y: 0,
    transition: {
      duration: 0.4,
      ease: 'easeOut'
    }
  },
  exit: {
    opacity: 0,
    y: -20,
    transition: {
      duration: 0.3
    }
  }
};
```

### Loading States

```css
.skeleton {
  background: linear-gradient(
    90deg,
    var(--bg-secondary) 0%,
    var(--bg-tertiary) 50%,
    var(--bg-secondary) 100%
  );
  background-size: 200% 100%;
  animation: skeleton-loading 1.5s ease-in-out infinite;
  border-radius: var(--radius-base);
}

@keyframes skeleton-loading {
  0% { background-position: 200% 0; }
  100% { background-position: -200% 0; }
}

.spinner {
  border: 3px solid var(--border);
  border-top-color: var(--mocha-mousse);
  border-radius: 50%;
  width: 40px;
  height: 40px;
  animation: spin 0.8s linear infinite;
}

@keyframes spin {
  to { transform: rotate(360deg); }
}
```

---

## 반응형 디자인

### Breakpoints

```typescript
export const breakpoints = {
  xs: '320px',
  sm: '640px',
  md: '768px',
  lg: '1024px',
  xl: '1280px',
  '2xl': '1536px'
};
```

### Mobile-First

```css
.component {
  padding: 1rem;
  font-size: 1rem;
}

@media (min-width: 768px) {
  .component {
    padding: 1.5rem;
    font-size: 1.125rem;
  }
}

@media (min-width: 1024px) {
  .component {
    padding: 2rem;
    font-size: 1.25rem;
  }
}
```

---

## 접근성

### WCAG 2.1 AA

```typescript
const FocusableButton = () => (
  <button
    className="btn-primary"
    aria-label="Submit form"
    onKeyDown={(e) => {
      if (e.key === 'Enter' || e.key === ' ') {
        handleSubmit();
      }
    }}
  >
    Submit
  </button>
);
```

---

## 구현 체크리스트

### 공통
- [ ] Design tokens 정의
- [ ] Dark mode 지원
- [ ] Responsive breakpoints
- [ ] Loading states
- [ ] Error states
- [ ] Empty states
- [ ] Accessibility (ARIA, keyboard)
- [ ] Focus management
- [ ] Micro-interactions
- [ ] Page transitions

---

## 레퍼런스

### 디자인 영감
- **Figma Community**: figma.com/community
- **Dribbble**: dribbble.com
- **Behance**: behance.net
- **Awwwards**: awwwards.com

### UI 패턴
- **Mobbin**: mobbin.com
- **Page Flows**: pageflows.com
- **UI Patterns**: ui-patterns.com

### 컴포넌트 라이브러리
- **shadcn/ui**: ui.shadcn.com
- **shadcn/ui Blocks 1**: shadcnblocks.com/blocks
- **shadcn/ui Blocks 2**: shadcnui-blocks.com/templates
- **shadcn/ui Templates**: shadcn.batchtool.com
- **Magic UI Pro**: pro.magicui.design

### shadcn/ui 리소스
- **공식 사이트**: shadcn.com
- **컴포넌트 모음**: ui.shadcn.com
- **AI 서비스 디자인**: shadcn.io - AI 서비스 특화 디자인 리소스
- **Blocks 예제 1**: shadcnblocks.com/blocks - 다양한 블록 패턴
- **Blocks 예제 2**: shadcnui-blocks.com/templates - 템플릿 모음
- **Batch Tool**: shadcn.batchtool.com - 유틸리티 컬렉션
- **Magic UI Pro**: pro.magicui.design - 프리미엄 템플릿

---

이 UI/UX 플랜은 2025년 최신 디자인 트렌드를 반영하여 모던하고 사용자 친화적인 웹 애플리케이션을 구축하기 위한 종합 가이드입니다.
