# 앱 개발 꿀팁 가이드 (2025)

> **작성일**: 2025-10-27
> **목표**: 토스 앱인토스 앱 개발 시 필요한 리소스 출처, 개발 트렌드, 최적화 기법 총정리

---

## 📋 목차

1. [디자인 리소스 출처](#1-디자인-리소스-출처)
2. [React Native 앱 개발](#2-react-native-앱-개발)
3. [웹앱 개발 (Next.js + PWA)](#3-웹앱-개발-nextjs--pwa)
4. [게임 개발 리소스](#4-게임-개발-리소스)
5. [2025년 UI/UX 트렌드](#5-2025년-uiux-트렌드)
6. [성능 최적화 기법](#6-성능-최적화-기법)
7. [개발 워크플로우 최적화](#7-개발-워크플로우-최적화)

---

## 1. 디자인 리소스 출처

### 🎨 무료 아이콘 & 이미지 사이트

#### 1.1 아이콘 리소스

| 사이트 | 특징 | 포맷 | 상업적 이용 | URL |
|--------|------|------|-------------|-----|
| **Flaticon** | 800만+ 리소스, 한국어 지원 | PNG, SVG, EPS, PSD | ✅ (출처 표기 필요) | flaticon.com |
| **Icons8** | 컬러/세부사항 변경, 편집 기능 | PNG, SVG | ✅ | icons8.com |
| **Freepik** | 다양한 스타일, 대량 리소스 | SVG, EPS, PNG, PSD | ✅ (출처 표기 필요) | freepik.com |
| **IconStore** | 개성 있는 테마 아이콘 | SVG, PNG | ✅ | iconstore.co |
| **PNG트리** | 아이콘, 벡터, 배경, 템플릿 | PNG, AI, PSD | ✅ (출처 표기) | pngtree.com |

#### 1.2 디자인 툴

| 툴 | 용도 | 무료 티어 | 특징 |
|----|------|-----------|------|
| **Canva** | AI 아이콘 생성, 디자인 | ✅ | AI 기반 아이콘 자동 생성 |
| **Figma** | UI/UX 디자인, 프로토타입 | ✅ | 협업 기능, React Native 플러그인 |
| **Adobe Express** | 간단한 그래픽 편집 | ✅ | Adobe 에코시스템 통합 |

#### 💡 꿀팁: 리소스 활용 전략

```typescript
// 1. 아이콘 일괄 다운로드 및 통합 관리
// assets/icons/ 디렉토리 구조
assets/
  icons/
    common/       // 공통 아이콘 (홈, 설정, 검색 등)
    feature/      // 기능별 아이콘
    social/       // 소셜 미디어 아이콘
  images/
    splash/       // 스플래시 화면
    onboarding/   // 온보딩 이미지
    backgrounds/  // 배경 이미지

// 2. SVG를 React Native 컴포넌트로 변환
// react-native-svg-transformer 사용
import HomeIcon from '../assets/icons/common/home.svg';

<HomeIcon width={24} height={24} fill="#000" />
```

**저작권 체크리스트:**
- [ ] 상업적 이용 가능 여부 확인
- [ ] 출처 표기 요구사항 확인 (보통 앱 설정 > 오픈소스 라이선스에 명시)
- [ ] 재배포 금지 조항 확인
- [ ] 리소스별 라이선스 문서화 (licenses.json 생성)

---

## 2. React Native 앱 개발

### 🛠️ 필수 UI 컴포넌트 라이브러리 (2025)

#### 2.1 추천 라이브러리

| 라이브러리 | 디자인 시스템 | 난이도 | 사용 사례 | GitHub Stars |
|-----------|-------------|--------|----------|--------------|
| **React Native Paper** | Material Design | ⭐⭐ | 구글 스타일 앱, 빠른 프로토타입 | 12.5k+ |
| **Gluestack-UI** | 모듈형, 커스텀 가능 | ⭐⭐⭐ | 브랜드 정체성 강한 앱 | 5k+ (NativeBase 후속) |
| **React Native Elements** | 유연한 기본 컴포넌트 | ⭐ | 초보자, 간단한 앱 | 24k+ |
| **Tamagui** | 웹/모바일 통합 | ⭐⭐⭐⭐ | 크로스 플랫폼 (RN + Web) | 8k+ |

#### 2.2 라이브러리 선택 기준

```typescript
// 프로젝트 타입별 추천

// 1. 빠른 MVP 개발 (2-4주)
// → React Native Paper
import { Button, Card, Text } from 'react-native-paper';

// 2. 브랜드 커스터마이징 중요
// → Gluestack-UI
import { Button, Box, Text } from '@gluestack-ui/themed';

// 3. 웹 + 모바일 동시 개발 (PWA + RN)
// → Tamagui
import { Button, YStack, Text } from 'tamagui';

// 4. 초보자 프로젝트
// → React Native Elements
import { Button, Card, Text } from 'react-native-elements';
```

#### 💡 꿀팁: 라이브러리 통합 전략

**1단계: 테마 시스템 구축**
```typescript
// theme.ts - 모든 라이브러리에 적용 가능한 디자인 토큰
export const theme = {
  colors: {
    primary: '#1E88E5',
    secondary: '#26A69A',
    background: '#FFFFFF',
    surface: '#F5F5F5',
    error: '#EF5350',
  },
  spacing: {
    xs: 4,
    sm: 8,
    md: 16,
    lg: 24,
    xl: 32,
  },
  typography: {
    h1: { fontSize: 32, fontWeight: 'bold' },
    h2: { fontSize: 24, fontWeight: '600' },
    body: { fontSize: 16, fontWeight: 'normal' },
  },
};
```

**2단계: 공통 컴포넌트 래퍼 생성**
```typescript
// components/Button.tsx - 라이브러리 교체 시 한 곳만 수정
import { Button as PaperButton } from 'react-native-paper';

export const Button = ({ children, ...props }) => (
  <PaperButton mode="contained" {...props}>
    {children}
  </PaperButton>
);
```

#### 2.3 성능 최적화 필수 라이브러리

| 라이브러리 | 용도 | 필수도 |
|-----------|------|--------|
| `react-native-reanimated` | 60fps 애니메이션 | ⭐⭐⭐⭐⭐ |
| `react-native-gesture-handler` | 제스처 인식 | ⭐⭐⭐⭐⭐ |
| `react-native-fast-image` | 이미지 캐싱 | ⭐⭐⭐⭐ |
| `@tanstack/react-query` | 서버 상태 관리 | ⭐⭐⭐⭐⭐ |
| `zustand` | 클라이언트 상태 관리 | ⭐⭐⭐⭐ |

---

## 3. 웹앱 개발 (Next.js + PWA)

### 🌐 Next.js PWA 구축 (2025년 최신)

#### 3.1 공식 PWA 지원 (권장)

**2024년 가을부터 Next.js가 공식 PWA 지원 시작** - 외부 패키지 없이 구현 가능!

```typescript
// app/manifest.ts (또는 app/manifest.json)
import type { MetadataRoute } from 'next';

export default function manifest(): MetadataRoute.Manifest {
  return {
    name: 'Toss AI App',
    short_name: 'AI App',
    description: '토스 앱인토스 AI 앱',
    start_url: '/',
    display: 'standalone',
    background_color: '#ffffff',
    theme_color: '#1E88E5',
    icons: [
      {
        src: '/icons/icon-192.png',
        sizes: '192x192',
        type: 'image/png',
      },
      {
        src: '/icons/icon-512.png',
        sizes: '512x512',
        type: 'image/png',
      },
    ],
  };
}
```

```typescript
// app/layout.tsx
export const metadata = {
  manifest: '/manifest.json', // 또는 '/manifest.ts'
  appleWebApp: {
    capable: true,
    statusBarStyle: 'default',
    title: 'AI App',
  },
};
```

#### 3.2 서비스 워커 등록 (필요 시)

```typescript
// public/sw.js - 오프라인 지원
self.addEventListener('install', (event) => {
  event.waitUntil(
    caches.open('v1').then((cache) => {
      return cache.addAll([
        '/',
        '/styles/main.css',
        '/scripts/main.js',
        '/images/logo.png',
      ]);
    })
  );
});

self.addEventListener('fetch', (event) => {
  event.respondWith(
    caches.match(event.request).then((response) => {
      return response || fetch(event.request);
    })
  );
});
```

#### 💡 꿀팁: PWA 최적화 전략

**1. 앱스토어 없이 배포 (설치 비용 절감)**
- iOS: Safari에서 "홈 화면에 추가"
- Android: Chrome에서 "홈 화면에 추가"
- **수수료 0%** (앱스토어 30% vs PWA 0%)

**2. 푸시 알림 (Web Push API)**
```typescript
// lib/notifications.ts
export async function subscribeToPush() {
  const registration = await navigator.serviceWorker.ready;

  const subscription = await registration.pushManager.subscribe({
    userVisibleOnly: true,
    applicationServerKey: process.env.NEXT_PUBLIC_VAPID_KEY,
  });

  // 백엔드로 구독 정보 전송
  await fetch('/api/push/subscribe', {
    method: 'POST',
    body: JSON.stringify(subscription),
  });
}
```

**3. PWA vs 네이티브 앱 선택 기준**

| 기준 | PWA 선택 | React Native 선택 |
|------|----------|------------------|
| **개발 속도** | 1-2주 | 3-4주 |
| **배포 비용** | 무료 | 앱스토어 수수료 (iOS $99/년, Play $25) |
| **네이티브 기능** | 제한적 (카메라, 위치 OK) | 전체 접근 가능 |
| **성능** | 웹 수준 | 네이티브 수준 |
| **사용 사례** | 콘텐츠 중심 앱, AI 채팅 | 게임, 복잡한 애니메이션 |

---

## 4. 게임 개발 리소스

> **⭐ 상세 가이드**: [reports/GAME_APP_DEVELOPMENT_ANALYSIS_20251027.md](../reports/GAME_APP_DEVELOPMENT_ANALYSIS_20251027.md)
>
> React Native 게임 개발에 대한 더 상세한 정보는 위 분석 리포트를 참조하세요:
> - 게임 개발 커뮤니티 10개 (Discord, Reddit)
> - 무료 리소스 사이트 15개 (itch.io, Kenney.nl, OpenGameArt 등)
> - 2024-2025 최신 기술 스택 (React Native Skia + Reanimated 4)
> - 성능 최적화 및 배포 전략
> - 2025년 게임 트렌드 (AI 통합, 하이브리드 캐주얼)

### 🎮 React Native 게임 개발

#### 4.1 2D 캐주얼 게임 스택

```typescript
// package.json
{
  "dependencies": {
    "react-native-game-engine": "^2.0.0",  // 게임 루프 엔진
    "matter-js": "^0.19.0",                // 2D 물리 엔진
    "react-native-reanimated": "^3.6.0",   // 60fps 애니메이션
    "react-native-gesture-handler": "^2.14.0", // 터치 제스처
    "react-native-sound": "^0.11.2"        // 사운드 효과
  }
}
```

**간단한 게임 예제 (플랫포머)**
```typescript
// Game.tsx
import { GameEngine } from 'react-native-game-engine';
import Matter from 'matter-js';

const engine = Matter.Engine.create({ enableSleeping: false });
const world = engine.world;

// 플레이어 생성
const player = Matter.Bodies.rectangle(50, 200, 50, 50);
Matter.World.add(world, player);

// 게임 루프
const physics = (entities, { time }) => {
  Matter.Engine.update(engine, time.delta);
  return entities;
};

export default function Game() {
  return (
    <GameEngine
      systems={[physics]}
      entities={{ player }}
    />
  );
}
```

#### 4.2 웹 게임 개발 (Phaser.js + React)

**Phaser.js는 2025년 가장 인기 있는 HTML5 게임 프레임워크** (GitHub 40,000+ stars)

```bash
# 공식 React 템플릿 사용
npx create-phaser-game my-game --template react
```

```typescript
// GameScene.ts
import Phaser from 'phaser';

export class GameScene extends Phaser.Scene {
  constructor() {
    super('GameScene');
  }

  preload() {
    this.load.image('player', '/assets/player.png');
  }

  create() {
    const player = this.add.sprite(400, 300, 'player');

    // React 컴포넌트와 통신
    this.game.events.emit('score-updated', 100);
  }
}
```

```typescript
// App.tsx - React와 Phaser 통합
import { useEffect, useRef } from 'react';
import Phaser from 'phaser';
import { GameScene } from './GameScene';

export default function PhaserGame() {
  const gameRef = useRef<Phaser.Game>();

  useEffect(() => {
    const config = {
      type: Phaser.AUTO,
      width: 800,
      height: 600,
      scene: [GameScene],
      parent: 'game-container',
    };

    gameRef.current = new Phaser.Game(config);

    // 이벤트 리스닝
    gameRef.current.events.on('score-updated', (score) => {
      console.log('Score:', score);
    });

    return () => gameRef.current?.destroy(true);
  }, []);

  return <div id="game-container" />;
}
```

#### 4.3 게임 에셋 리소스

##### 그래픽 에셋

| 사이트 | 특징 | 가격 | URL |
|--------|------|------|-----|
| **Unity Asset Store** | 2D/3D 에셋, 플러그인 | 무료 ~ 유료 | assetstore.unity.com |
| **ACON 리소스뱅크** | 한국 디지털 에셋 스토어 | 무료 ~ 유료 | acon3d.com |
| **OpenGameArt** | 오픈소스 게임 아트 | 무료 | opengameart.org |
| **itch.io Game Assets** | 인디 게임 에셋 마켓 | 무료 ~ 유료 | itch.io/game-assets |
| **Kenney.nl** | 완전 무료 게임 에셋 | 무료 (CC0) | kenney.nl |

##### 사운드 & 음악 리소스

| 사이트 | 종류 | 개수 | 저작권 | URL |
|--------|------|------|--------|-----|
| **Pixabay Sound Effects** | 효과음, BGM | 110,000+ | 무료 (출처 불필요) | pixabay.com/sound-effects |
| **Adobe Audition (무료)** | 게임 효과음 | 450+ | 무료 | adobe.com/products/audition |
| **SellBuyMusic** | BGM, 효과음 (한국) | 수만 개 | 유료/무료 | sellbuymusic.com |
| **GameSounds** | 게임 전용 효과음 | 수천 개 | 무료 (출처 필요) | gamesound.or.kr |
| **Taira Komori** | 일본 무료 효과음 | 1,000+ | 무료 (출처 표기) | taira-komori.jpn.org |
| **Freesound** | 커뮤니티 사운드 | 500,000+ | CC 라이선스 | freesound.org |

#### 💡 꿀팁: 게임 리소스 최적화

```typescript
// 1. 이미지 스프라이트 시트 사용 (메모리 절감)
// TexturePacker 등으로 생성
{
  "frames": {
    "player_idle_1.png": { "x": 0, "y": 0, "w": 64, "h": 64 },
    "player_idle_2.png": { "x": 64, "y": 0, "w": 64, "h": 64 }
  }
}

// 2. 사운드 파일 최적화
// - iOS: CAF 포맷 (압축률 우수)
// - Android: OGG 포맷
// - 웹: MP3 (호환성)
// ffmpeg로 변환:
// ffmpeg -i input.wav -c:a libvorbis -q:a 4 output.ogg

// 3. 에셋 지연 로딩
const loadAssets = async (level: number) => {
  const assets = await import(`./assets/level${level}`);
  return assets;
};
```

#### 4.4 추천 2D 게임 엔진 (React Native 외)

| 엔진 | 언어 | 플랫폼 | 난이도 | 특징 |
|------|------|--------|--------|------|
| **Godot** | GDScript (Python 유사) | 멀티플랫폼 | ⭐⭐⭐ | 완전 무료 오픈소스 |
| **Solar2D** | Lua | 모바일 | ⭐⭐ | 모바일 게임 특화 |
| **Construct 3** | 비주얼 스크립팅 | 웹/모바일 | ⭐ | 코딩 없이 개발 |

---

## 5. 2025년 UI/UX 트렌드

### 🎨 주목해야 할 디자인 트렌드

#### 5.1 AI 기반 디자인 협업

**AI 디자인 도구:**
- **Adobe Firefly**: 텍스트로 UI 생성
- **Galileo AI**: Figma 플러그인, 프롬프트로 UI 자동 생성
- **UX Pilot AI**: 사용자 플로우 자동 생성

```typescript
// AI 생성 컴포넌트 활용 예시
// Galileo AI로 생성 → Figma → React Native 코드 추출
// v0.dev (Vercel)로 웹 컴포넌트 생성

// 프롬프트 예시:
// "이력서 작성 앱의 메인 화면을 만들어줘.
//  상단에 프로그레스 바, 중앙에 입력 폼, 하단에 다음 버튼"
```

#### 5.2 아날로그 감성 디자인

**손으로 그린 듯한 UI 요소:**
```typescript
// react-native-skia로 손글씨 효과
import { Canvas, Path, Skia } from '@shopify/react-native-skia';

const HandDrawnButton = () => {
  const path = Skia.Path.Make();
  path.moveTo(10, 10);
  path.lineTo(200, 10);
  // 약간의 떨림 효과 추가
  path.quadTo(205, 12, 200, 50);

  return (
    <Canvas style={{ width: 210, height: 60 }}>
      <Path path={path} color="#000" strokeWidth={2} />
    </Canvas>
  );
};
```

**트렌드 적용 사례:**
- 필름 카메라 질감 필터 (Instagram 스타일)
- 스크랩북 레이아웃 (Pinterest 스타일)
- 손글씨 폰트 사용 (Handlee, Caveat 등)

#### 5.3 몰입형 3D 그래픽

```typescript
// React Three Fiber (웹)
import { Canvas } from '@react-three/fiber';
import { OrbitControls } from '@react-three/drei';

function App() {
  return (
    <Canvas>
      <ambientLight intensity={0.5} />
      <pointLight position={[10, 10, 10]} />
      <mesh>
        <boxGeometry args={[1, 1, 1]} />
        <meshStandardMaterial color="orange" />
      </mesh>
      <OrbitControls />
    </Canvas>
  );
}
```

#### 5.4 맥시멀리즘 (Maximalism)

**대담한 디자인 요소:**
- 비대칭 레이아웃
- 강렬한 컬러 팔레트 (네온, 그라데이션)
- 다양한 타이포그래피 믹스
- 텍스처 레이어링

```typescript
// 그라데이션 + 애니메이션 예시
import { LinearGradient } from 'expo-linear-gradient';
import Animated, { useSharedValue, withRepeat, withTiming } from 'react-native-reanimated';

const MaximalButton = () => {
  const rotation = useSharedValue(0);

  rotation.value = withRepeat(withTiming(360, { duration: 3000 }), -1);

  return (
    <Animated.View style={{ transform: [{ rotate: `${rotation.value}deg` }] }}>
      <LinearGradient
        colors={['#FF6B6B', '#FFE66D', '#4ECDC4']}
        start={{ x: 0, y: 0 }}
        end={{ x: 1, y: 1 }}
      >
        <Text style={{ fontSize: 24, fontWeight: 'bold' }}>Click Me!</Text>
      </LinearGradient>
    </Animated.View>
  );
};
```

#### 5.5 제로 UI (Zero UI)

**음성 및 제스처 기반 인터랙션:**
```typescript
// 음성 명령 예시 (react-native-voice)
import Voice from '@react-native-voice/voice';

Voice.onSpeechResults = (e) => {
  const command = e.value[0];

  if (command.includes('이력서 작성')) {
    navigation.navigate('ResumeBuilder');
  }
};

await Voice.start('ko-KR');
```

#### 5.6 윤리적 디자인 & 웰빙

**사용자 웰빙을 고려한 기능:**
- 화면 시간 제한 (일일 사용 제한)
- 알림 스누즈 기능
- 다크모드 자동 전환 (저녁 시간)
- 휴식 리마인더

```typescript
// 화면 시간 추적 예시
import AsyncStorage from '@react-native-async-storage/async-storage';
import { useEffect } from 'react';

const ScreenTimeTracker = () => {
  useEffect(() => {
    const startTime = Date.now();

    return () => {
      const endTime = Date.now();
      const sessionTime = endTime - startTime;

      AsyncStorage.getItem('totalScreenTime').then((total) => {
        const newTotal = parseInt(total || '0') + sessionTime;
        AsyncStorage.setItem('totalScreenTime', newTotal.toString());

        // 2시간 초과 시 경고
        if (newTotal > 2 * 60 * 60 * 1000) {
          Alert.alert('휴식 시간', '2시간 동안 사용하셨어요. 잠시 휴식하세요!');
        }
      });
    };
  }, []);
};
```

#### 5.7 다크모드 진화

```typescript
// 컨텍스트별 다크모드 (시간대 기반)
import { useColorScheme } from 'react-native';
import { useEffect, useState } from 'react';

const useAdaptiveTheme = () => {
  const systemTheme = useColorScheme();
  const [theme, setTheme] = useState(systemTheme);

  useEffect(() => {
    const hour = new Date().getHours();

    // 저녁 6시~아침 7시 자동 다크모드
    if (hour >= 18 || hour < 7) {
      setTheme('dark');
    } else {
      setTheme('light');
    }
  }, []);

  return theme;
};
```

---

## 6. 성능 최적화 기법

### ⚡ 코드 레벨 최적화

#### 6.1 React 렌더링 최적화

```typescript
// ❌ 나쁜 예: 불필요한 리렌더링
function UserList({ users }) {
  return users.map(user => <UserCard user={user} />);
}

// ✅ 좋은 예: useMemo + React.memo
import { useMemo } from 'react';

const UserCard = React.memo(({ user }) => {
  return <Text>{user.name}</Text>;
});

function UserList({ users }) {
  const sortedUsers = useMemo(() => {
    return users.sort((a, b) => a.name.localeCompare(b.name));
  }, [users]);

  return sortedUsers.map(user => (
    <UserCard key={user.id} user={user} />
  ));
}
```

#### 6.2 이미지 최적화

```typescript
// React Native Fast Image (캐싱 + 성능)
import FastImage from 'react-native-fast-image';

<FastImage
  source={{
    uri: 'https://example.com/image.jpg',
    priority: FastImage.priority.high,
    cache: FastImage.cacheControl.immutable,
  }}
  resizeMode={FastImage.resizeMode.cover}
/>

// 웹: Next.js Image 컴포넌트
import Image from 'next/image';

<Image
  src="/profile.jpg"
  width={500}
  height={500}
  placeholder="blur"  // 블러 프리뷰
  loading="lazy"      // 지연 로딩
/>
```

#### 6.3 리스트 렌더링 최적화

```typescript
// React Native FlatList 최적화
import { FlatList } from 'react-native';

<FlatList
  data={items}
  renderItem={({ item }) => <ItemCard item={item} />}
  keyExtractor={(item) => item.id}
  // 성능 최적화 props
  initialNumToRender={10}          // 초기 렌더링 수
  maxToRenderPerBatch={10}         // 배치당 렌더링 수
  windowSize={5}                   // 렌더링 윈도우 크기
  removeClippedSubviews={true}     // 화면 밖 뷰 제거
  getItemLayout={(data, index) => ({
    length: ITEM_HEIGHT,
    offset: ITEM_HEIGHT * index,
    index,
  })}
/>
```

#### 6.4 네트워크 최적화

```typescript
// React Query로 요청 캐싱 + 자동 재검증
import { useQuery } from '@tanstack/react-query';

function useUserProfile(userId: string) {
  return useQuery({
    queryKey: ['user', userId],
    queryFn: () => fetch(`/api/users/${userId}`).then(r => r.json()),
    staleTime: 5 * 60 * 1000,  // 5분간 캐시 유지
    gcTime: 10 * 60 * 1000,     // 10분 후 가비지 컬렉션
  });
}
```

### 🔧 빌드 최적화

#### 6.5 번들 크기 줄이기

```bash
# React Native
npx react-native-bundle-visualizer

# Next.js
npm install @next/bundle-analyzer
```

```javascript
// next.config.js
const withBundleAnalyzer = require('@next/bundle-analyzer')({
  enabled: process.env.ANALYZE === 'true',
});

module.exports = withBundleAnalyzer({
  // 트리 쉐이킹 최적화
  compiler: {
    removeConsole: process.env.NODE_ENV === 'production',
  },
});
```

#### 6.6 성능 모니터링 도구

| 도구 | 플랫폼 | 용도 |
|------|--------|------|
| **Android Studio Profiler** | Android | CPU, 메모리, 네트워크 분석 |
| **Xcode Instruments** | iOS | 메모리 누수, 성능 병목 |
| **Flipper** | React Native | 네트워크, 레이아웃 디버깅 |
| **Firebase Performance** | 웹/모바일 | 실시간 성능 모니터링 |
| **Lighthouse** | 웹 | PWA 성능 점수, SEO |

---

## 7. 개발 워크플로우 최적화

### 🚀 CI/CD 자동화

#### 7.1 React Native 배포 자동화 (EAS)

```yaml
# eas.json
{
  "build": {
    "production": {
      "android": {
        "buildType": "app-bundle",
        "gradleCommand": ":app:bundleRelease"
      },
      "ios": {
        "buildConfiguration": "Release"
      }
    }
  },
  "submit": {
    "production": {
      "android": {
        "serviceAccountKeyPath": "./secrets/google-play-key.json",
        "track": "internal"
      },
      "ios": {
        "appleId": "your-apple-id@example.com",
        "ascAppId": "1234567890"
      }
    }
  }
}
```

```bash
# 빌드 + 자동 배포
eas build --platform all --auto-submit
```

#### 7.2 웹 배포 자동화 (Vercel)

```yaml
# .github/workflows/deploy.yml
name: Deploy to Vercel

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: amondnet/vercel-action@v20
        with:
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
          vercel-org-id: ${{ secrets.VERCEL_ORG_ID }}
          vercel-project-id: ${{ secrets.VERCEL_PROJECT_ID }}
```

### 💡 꿀팁: 개발 생산성 향상

#### 7.3 코드 제너레이터 활용

```bash
# Plop.js로 컴포넌트 템플릿 자동 생성
npm install --save-dev plop
```

```javascript
// plopfile.js
module.exports = function (plop) {
  plop.setGenerator('component', {
    description: 'Create a new React Native component',
    prompts: [
      {
        type: 'input',
        name: 'name',
        message: 'Component name?',
      },
    ],
    actions: [
      {
        type: 'add',
        path: 'src/components/{{pascalCase name}}/{{pascalCase name}}.tsx',
        templateFile: 'templates/Component.tsx.hbs',
      },
      {
        type: 'add',
        path: 'src/components/{{pascalCase name}}/{{pascalCase name}}.styles.ts',
        templateFile: 'templates/Component.styles.ts.hbs',
      },
      {
        type: 'add',
        path: 'src/components/{{pascalCase name}}/index.ts',
        templateFile: 'templates/index.ts.hbs',
      },
    ],
  });
};
```

```bash
# 사용
npm run plop component
# Component name? → Button
# ✔ Created src/components/Button/Button.tsx
# ✔ Created src/components/Button/Button.styles.ts
# ✔ Created src/components/Button/index.ts
```

#### 7.4 린팅 & 포매팅 자동화

```json
// .vscode/settings.json
{
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "eslint.validate": [
    "javascript",
    "javascriptreact",
    "typescript",
    "typescriptreact"
  ]
}
```

```json
// package.json
{
  "scripts": {
    "lint": "eslint . --ext .ts,.tsx",
    "lint:fix": "eslint . --ext .ts,.tsx --fix",
    "format": "prettier --write \"**/*.{ts,tsx,json,md}\""
  },
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  },
  "lint-staged": {
    "*.{ts,tsx}": ["eslint --fix", "prettier --write"]
  }
}
```

---

## 🎯 핵심 요약

### ✅ 체크리스트: 앱 개발 시작 전

**디자인 리소스:**
- [ ] 아이콘/이미지 사이트 북마크 (Flaticon, Icons8, Freepik)
- [ ] 라이선스 문서 작성 (licenses.json)
- [ ] 에셋 디렉토리 구조화 (icons, images, sounds)

**기술 스택 선택:**
- [ ] UI 라이브러리 결정 (Paper / Gluestack / Tamagui)
- [ ] 상태 관리 설정 (Zustand + React Query)
- [ ] 애니메이션 라이브러리 (Reanimated + Gesture Handler)

**성능 최적화:**
- [ ] 이미지 최적화 (FastImage / Next Image)
- [ ] 리스트 가상화 (FlatList / react-window)
- [ ] 번들 크기 분석 (Bundle Analyzer)

**개발 워크플로우:**
- [ ] CI/CD 설정 (EAS / Vercel)
- [ ] 린팅/포매팅 자동화 (ESLint + Prettier + Husky)
- [ ] 컴포넌트 제너레이터 (Plop.js)

**게임 개발 (해당 시):**
- [ ] 게임 엔진 선택 (RN Game Engine / Phaser.js)
- [ ] 에셋 다운로드 (Unity Store / Kenney / Pixabay)
- [ ] 사운드 통합 (react-native-sound)

---

## 📚 추가 학습 리소스

### 공식 문서
- [React Native 공식 문서](https://reactnative.dev/docs/getting-started)
- [Next.js PWA 가이드](https://nextjs.org/docs/app/guides/progressive-web-apps)
- [Phaser.js 튜토리얼](https://phaser.io/tutorials)

### 커뮤니티
- [React Native Korea Facebook](https://www.facebook.com/groups/reactnativekorea)
- [GeekNews](https://news.hada.io/) - 최신 개발 트렌드
- [요즘IT](https://yozm.wishket.com/) - 한국 개발 블로그

### 유튜브 채널
- Expo (React Native 공식)
- Fireship (웹 개발 트렌드)
- 노마드 코더 (한국어 강의)

---

## 🔄 문서 업데이트 이력

| 날짜 | 내용 |
|------|------|
| 2025-10-27 | 초안 작성 (웹 검색 기반 리서치) |
| 2025-10-27 | 게임 개발 섹션에 분석 리포트 참조 링크 추가 (reports/GAME_APP_DEVELOPMENT_ANALYSIS_20251027.md) |

---

**다음 단계**: 이 문서의 내용을 바탕으로 실제 앱 개발 시 각 섹션을 참고하며 진행하세요!
