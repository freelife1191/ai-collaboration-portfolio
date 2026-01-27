# React Native 게임 엔진 개발 가이드

> **작성일**: 2025-10-27
> **목표**: 토스 앱인토스용 React Native 게임 개발 시 필수 기술 스택 및 최적화 가이드
> **참고**: `REACT_NATIVE_GAME_DEVLOPMENT_TIP.md` 기반 실전 검증 내용 포함

---

## 🎯 핵심 원칙

1. **60 FPS 유지**: 모든 게임은 60fps를 목표로 최적화
2. **JS 스레드 부하 최소화**: Reanimated worklet 활용
3. **물리 엔진 적재적소 사용**: 불필요한 물리 시뮬레이션 지양
4. **클라이언트 사이드 처리 극대화**: 서버 비용 최소화
5. **성능 모니터링 필수**: Flipper + Performance Monitor 활용

---

## 📦 필수 기술 스택

### Tier 1: 모든 게임 공통 (필수)

```json
{
  "dependencies": {
    "react-native": "^0.74.0",
    "expo": "^51.0.0",
    "react-native-reanimated": "^3.8.0",
    "react-native-gesture-handler": "^2.16.0",
    "@react-native-async-storage/async-storage": "^1.23.0",
    "expo-av": "^14.0.0"
  }
}
```

**용도:**
- `expo`: 빠른 개발 및 빌드
- `react-native-reanimated`: 고성능 애니메이션 (worklet 기반)
- `react-native-gesture-handler`: 터치/제스처 처리
- `async-storage`: 로컬 저장소 (오프라인 플레이)
- `expo-av`: 사운드 처리 (비동기)

---

### Tier 2: 게임 엔진 및 물리 엔진

```json
{
  "dependencies": {
    "react-native-game-engine": "^2.0.0",
    "matter-js": "^0.19.0",
    "@shopify/react-native-skia": "^1.2.0"
  }
}
```

**react-native-game-engine**
- **용도**: 게임 루프, 엔티티-시스템 아키텍처
- **적용 대상**: 모든 실시간 게임 (러너, 액션, 물리 퍼즐)
- **장점**:
  - 컴포넌트-시스템-엔티티 구조
  - 프레임 기반 업데이트
  - Matter.js와 완벽 통합

**matter-js (물리 엔진)**
- **용도**: 2D 물리 시뮬레이션 (중력, 충돌, 탄성)
- **적용 대상**:
  - ✅ 핀 뽑기 (중력, 충돌)
  - ✅ 공 떨어뜨리기
  - ✅ 화살 슈팅 (탄막 물리)
  - ✅ 다리 건설 (물리 시뮬레이션)
  - ❌ 퍼즐 게임 (물 정렬, 주차 탈출 등) - 불필요
- **주의사항**:
  - 과도한 오브젝트는 성능 저하 원인
  - `enableSleeping: true` 설정으로 최적화

**@shopify/react-native-skia**
- **용도**: 네이티브 수준의 캔버스 렌더링
- **적용 대상**:
  - ✅ 탄막 게임 (화살 슈팅, 슈팅 게임)
  - ✅ 러너 게임 (배경 스크롤, 다수 오브젝트)
  - ✅ 리듬 게임 (정확한 타이밍, 고FPS)
  - ⚠️ 퍼즐 게임 - 선택 (SVG로 충분한 경우 생략)
- **장점**:
  - 네이티브 캔버스로 60fps 보장
  - 수백 개 오브젝트 동시 렌더링 가능
  - Reanimated 3와 완벽 호환

---

## 🎮 게임 루프 설계

### 기본 구조 (react-native-game-engine)

```typescript
import { GameEngine } from 'react-native-game-engine';
import Matter from 'matter-js';

// 물리 엔진 초기화
const engine = Matter.Engine.create({ enableSleeping: false });
const world = engine.world;
world.gravity.y = 1.0; // 중력 설정

// 시스템 함수 (매 프레임 실행)
const Physics = (entities, { time }) => {
  const delta = time.delta; // delta time (ms)

  // 물리 엔진 업데이트
  Matter.Engine.update(engine, delta);

  return entities; // 업데이트된 엔티티 반환
};

const Collision = (entities, { events }) => {
  // 충돌 감지 로직
  Matter.Events.on(engine, 'collisionStart', (event) => {
    event.pairs.forEach((pair) => {
      if (isHeroAndTreasure(pair)) {
        collectTreasure();
      } else if (isHeroAndEnemy(pair)) {
        gameOver();
      }
    });
  });

  return entities;
};

// GameEngine 컴포넌트
<GameEngine
  systems={[Physics, Collision, Movement]}
  entities={initialEntities}
  running={isGameRunning}
  onEvent={(e) => console.log(e)}
/>
```

### Delta Time 기반 이동 (프레임 독립적)

```typescript
// ❌ 나쁜 예: 프레임 의존적
const Movement = (entities) => {
  entities.player.position.x += 5; // 60fps에서만 정확
  return entities;
};

// ✅ 좋은 예: Delta Time 기반
const Movement = (entities, { time }) => {
  const delta = time.delta / 1000; // 초 단위
  const speed = 100; // 100px/s

  entities.player.position.x += speed * delta; // 모든 프레임레이트에서 동일
  return entities;
};
```

### 타이머 처리 주의사항

```typescript
// ⚠️ 주의: setInterval은 JS thread를 막을 수 있음
// ❌ 나쁜 예
setInterval(() => {
  updateGame();
}, 16); // 약 60fps

// ✅ 좋은 예: Reanimated worklet 사용
import { useFrameCallback } from 'react-native-reanimated';

useFrameCallback((frameInfo) => {
  'worklet'; // UI 스레드에서 실행

  // 게임 로직 업데이트
  updateGameState(frameInfo.timeSincePreviousFrame);
});
```

---

## 🚀 성능 최적화 전략

### 1. 그래픽 렌더링 최적화

#### SVG vs Skia 선택 가이드

| 게임 타입 | 오브젝트 수 | 권장 렌더링 | 이유 |
|----------|-----------|-----------|------|
| 퍼즐 (물 정렬, 주차) | < 30개 | SVG | 충분한 성능 |
| 물리 퍼즐 (핀 뽑기) | 30-100개 | SVG + Skia | 혼합 사용 |
| 탄막 슈팅 | 100-500개 | Skia 필수 | 네이티브 성능 필요 |
| 러너 게임 | 50-200개 | Skia 권장 | 배경 스크롤 + 다수 오브젝트 |
| 리듬 게임 | 100-300개 | Skia 필수 | 정확한 타이밍 필요 |

#### Skia 기본 사용법

```typescript
import { Canvas, Circle, Group } from '@shopify/react-native-skia';
import { useSharedValue } from 'react-native-reanimated';

function BulletHellGame() {
  const bullets = useSharedValue([]); // 탄막 배열

  return (
    <Canvas style={{ flex: 1 }}>
      <Group>
        {bullets.value.map((bullet, i) => (
          <Circle
            key={i}
            cx={bullet.x}
            cy={bullet.y}
            r={5}
            color="red"
          />
        ))}
      </Group>
    </Canvas>
  );
}
```

---

### 2. 물리 엔진 최적화

#### Matter.js 최적화 설정

```typescript
const engine = Matter.Engine.create({
  enableSleeping: true, // 비활성 오브젝트 절전 모드
  positionIterations: 6, // 정확도 vs 성능 (기본: 6)
  velocityIterations: 4, // 정확도 vs 성능 (기본: 4)
});

// Broad-phase 알고리즘 선택 (성능 중요 시)
engine.broadphase = Matter.Grid.create(); // 그리드 기반 (빠름)
// 또는
engine.broadphase = Matter.SAP.create(); // SAP 알고리즘 (정확함)
```

#### 충돌 감지 최적화

```typescript
// 충돌 레이어 설정 (불필요한 충돌 계산 방지)
const CATEGORY = {
  PLAYER: 0x0001,
  ENEMY: 0x0002,
  BULLET: 0x0004,
  WALL: 0x0008,
};

Matter.Body.create({
  collisionFilter: {
    category: CATEGORY.PLAYER,
    mask: CATEGORY.ENEMY | CATEGORY.WALL, // 적과 벽하고만 충돌
  }
});
```

#### 오브젝트 풀링 (Object Pooling)

```typescript
// ❌ 나쁜 예: 매번 생성/삭제 (GC 부하)
function shootBullet() {
  const bullet = Matter.Bodies.circle(x, y, 5);
  Matter.World.add(world, bullet);

  setTimeout(() => {
    Matter.World.remove(world, bullet); // GC 발생
  }, 3000);
}

// ✅ 좋은 예: 오브젝트 풀링
class BulletPool {
  private pool: Matter.Body[] = [];

  constructor(size: number) {
    for (let i = 0; i < size; i++) {
      const bullet = Matter.Bodies.circle(0, 0, 5);
      bullet.isStatic = true; // 비활성 상태
      this.pool.push(bullet);
    }
  }

  get(): Matter.Body {
    const bullet = this.pool.pop();
    if (bullet) {
      bullet.isStatic = false; // 활성화
      return bullet;
    }
    return Matter.Bodies.circle(0, 0, 5); // 풀이 비면 새로 생성
  }

  release(bullet: Matter.Body) {
    bullet.isStatic = true; // 비활성화
    Matter.Body.setPosition(bullet, { x: -1000, y: -1000 }); // 화면 밖으로
    this.pool.push(bullet);
  }
}
```

---

### 3. 상태 관리 최적화

#### 게임 상태 vs 앱 상태 분리

```typescript
// ❌ 나쁜 예: Redux로 게임 상태 관리 (과도한 렌더링)
import { useDispatch } from 'react-redux';

function Game() {
  const dispatch = useDispatch();

  // 매 프레임마다 Redux 액션 → 리렌더링 지옥
  useEffect(() => {
    setInterval(() => {
      dispatch(updatePlayerPosition(x, y)); // ❌
    }, 16);
  }, []);
}

// ✅ 좋은 예: 게임 상태는 로컬 관리
function Game() {
  const gameStateRef = useRef({
    player: { x: 0, y: 0 },
    enemies: [],
    score: 0,
  });

  // 게임 로직은 ref로 관리 (리렌더링 없음)
  const updateGame = useCallback(() => {
    gameStateRef.current.player.x += 1;
    // UI 업데이트는 필요 시에만
  }, []);
}

// 앱 상태는 Zustand로 관리
import create from 'zustand';

const useGameStore = create((set) => ({
  level: 1,
  coins: 0,
  gems: 0,
  // 유저 프로필, 재화 등만 Zustand에 저장
  addCoins: (amount) => set((state) => ({ coins: state.coins + amount })),
}));
```

#### Feature 단위 폴더 구조

```
src/
├── entities/         # 게임 엔티티 정의
│   ├── Player.ts
│   ├── Enemy.ts
│   └── Bullet.ts
├── systems/          # 게임 시스템 (물리, 충돌, AI)
│   ├── Physics.ts
│   ├── Collision.ts
│   └── Movement.ts
├── assets/           # 리소스
│   ├── images/
│   └── sounds/
├── screens/          # 화면
│   ├── GameScreen.tsx
│   └── MenuScreen.tsx
└── utils/            # 유틸리티
    ├── pool.ts
    └── helpers.ts
```

---

### 4. 사운드 처리 최적화

#### expo-av 사용 (비동기 처리)

```typescript
import { Audio } from 'expo-av';

class SoundManager {
  private sounds: Map<string, Audio.Sound> = new Map();

  // 사전 로드 (게임 시작 시)
  async preloadSounds() {
    const soundFiles = {
      coin: require('./assets/sounds/coin.mp3'),
      jump: require('./assets/sounds/jump.mp3'),
      gameover: require('./assets/sounds/gameover.mp3'),
    };

    // 비동기로 병렬 로드
    await Promise.all(
      Object.entries(soundFiles).map(async ([key, file]) => {
        const { sound } = await Audio.Sound.createAsync(file);
        this.sounds.set(key, sound);
      })
    );
  }

  // 재생 (비동기, JS thread 블로킹 없음)
  async play(key: string) {
    const sound = this.sounds.get(key);
    if (sound) {
      await sound.replayAsync(); // 처음부터 재생
    }
  }

  // 정리 (메모리 해제)
  async unloadAll() {
    for (const sound of this.sounds.values()) {
      await sound.unloadAsync();
    }
    this.sounds.clear();
  }
}

// 사용 예시
const soundManager = new SoundManager();

// 게임 시작 시
await soundManager.preloadSounds();

// 게임 중
soundManager.play('coin'); // 비동기지만 즉시 반환
```

#### 사운드 풀링 (동시 재생 지원)

```typescript
class SoundPool {
  private pool: Audio.Sound[] = [];
  private currentIndex = 0;

  async create(file: any, poolSize: number = 3) {
    for (let i = 0; i < poolSize; i++) {
      const { sound } = await Audio.Sound.createAsync(file);
      this.pool.push(sound);
    }
  }

  async play() {
    const sound = this.pool[this.currentIndex];
    await sound.replayAsync();
    this.currentIndex = (this.currentIndex + 1) % this.pool.length;
  }
}

// 탄막 게임에서 동시에 여러 발 발사음 재생 가능
const shootSoundPool = new SoundPool();
await shootSoundPool.create(require('./shoot.mp3'), 5); // 풀 크기 5
```

---

## 🔍 성능 디버깅

### Expo + Flipper 조합

```bash
# Flipper 설치 (macOS)
brew install --cask flipper

# React Native DevTools 플러그인 활성화
npx expo install react-devtools
```

### Performance Monitor 활용

```typescript
import { PerformanceObserver, performance } from 'react-native-performance';

// 게임 시작 시 모니터링 활성화
useEffect(() => {
  const observer = new PerformanceObserver((list) => {
    list.getEntries().forEach((entry) => {
      if (entry.name === 'frame') {
        console.log('FPS:', 1000 / entry.duration);
        console.log('JS Thread Time:', entry.jsThreadTime);
        console.log('Native Thread Time:', entry.nativeThreadTime);
      }
    });
  });

  observer.observe({ entryTypes: ['measure', 'mark'] });

  return () => observer.disconnect();
}, []);

// 프레임 측정
function measureFrame() {
  performance.mark('frame-start');

  // 게임 로직 실행
  updateGame();

  performance.mark('frame-end');
  performance.measure('frame', 'frame-start', 'frame-end');
}
```

### 목표 성능 지표

| 지표 | 목표 | 허용 범위 | 조치 필요 |
|------|------|---------|----------|
| FPS | 60fps | 55-60fps | < 55fps |
| JS Thread Time | < 16ms | 16-20ms | > 20ms |
| Memory Usage | < 150MB | 150-200MB | > 200MB |
| Bundle Size | < 30MB | 30-40MB | > 40MB |
| Load Time | < 3초 | 3-5초 | > 5초 |

---

## 🎯 게임 타입별 권장 스택

### 1. 물리 기반 퍼즐 (핀 뽑기, 공 떨어뜨리기 등)

```json
{
  "필수": [
    "react-native-game-engine",
    "matter-js",
    "react-native-reanimated"
  ],
  "선택": [
    "@shopify/react-native-skia"
  ]
}
```

**게임 루프 예시:**
```typescript
const Physics = (entities, { time }) => {
  Matter.Engine.update(engine, time.delta);
  return entities;
};

const PinRemoval = (entities, { touches }) => {
  touches.filter(t => t.type === 'press').forEach(touch => {
    const pin = findPinAtPosition(touch.event.pageX, touch.event.pageY);
    if (pin) {
      Matter.World.remove(world, pin.body);
      delete entities[pin.id];
    }
  });
  return entities;
};
```

---

### 2. 로직 퍼즐 (물 정렬, 주차 탈출 등)

```json
{
  "필수": [
    "react-native-svg",
    "react-native-reanimated",
    "react-native-gesture-handler"
  ],
  "불필요": [
    "matter-js",
    "react-native-game-engine"
  ]
}
```

**애니메이션 예시:**
```typescript
import Animated, { useSharedValue, withSpring } from 'react-native-reanimated';

function WaterBottle() {
  const waterLevel = useSharedValue(0);

  const pourWater = () => {
    waterLevel.value = withSpring(waterLevel.value + 25, {
      damping: 15,
      stiffness: 100,
    });
  };

  return (
    <Animated.View style={{ height: waterLevel }}>
      {/* 물 렌더링 */}
    </Animated.View>
  );
}
```

---

### 3. 탄막/슈팅 게임 (화살 슈팅, 스페이스 인베이더)

```json
{
  "필수": [
    "react-native-game-engine",
    "@shopify/react-native-skia",
    "matter-js",
    "react-native-reanimated"
  ]
}
```

**Skia 기반 탄막 렌더링:**
```typescript
import { Canvas, Circle, useLoop } from '@shopify/react-native-skia';
import { useDerivedValue } from 'react-native-reanimated';

function BulletHellCanvas() {
  const progress = useLoop({ duration: 16 }); // 60fps

  const bullets = useDerivedValue(() => {
    return gameState.bullets.map(b => ({
      cx: b.x,
      cy: b.y,
      r: 5,
    }));
  });

  return (
    <Canvas style={{ flex: 1 }}>
      {bullets.value.map((bullet, i) => (
        <Circle key={i} {...bullet} color="red" />
      ))}
    </Canvas>
  );
}
```

---

### 4. 러너 게임 (템플 러너, 스노우 서퍼)

```json
{
  "필수": [
    "react-native-game-engine",
    "@shopify/react-native-skia",
    "react-native-reanimated"
  ],
  "선택": [
    "matter-js"
  ]
}
```

**무한 배경 스크롤:**
```typescript
const BackgroundScroll = (entities, { time }) => {
  const delta = time.delta / 1000;
  const speed = 200; // 200px/s

  entities.background.position.x -= speed * delta;

  // 배경이 화면 밖으로 나가면 재배치
  if (entities.background.position.x < -SCREEN_WIDTH) {
    entities.background.position.x = 0;
  }

  return entities;
};
```

---

### 5. 리듬 게임 (피아노 타일, 비트마스터)

```json
{
  "필수": [
    "@shopify/react-native-skia",
    "react-native-reanimated",
    "expo-av"
  ],
  "불필요": [
    "matter-js",
    "react-native-game-engine"
  ]
}
```

**정확한 타이밍 처리:**
```typescript
import { Audio } from 'expo-av';

class RhythmEngine {
  private startTime: number = 0;
  private bpm: number = 120;

  async start() {
    const { sound } = await Audio.Sound.createAsync(require('./music.mp3'));
    await sound.playAsync();

    this.startTime = Date.now();
  }

  // 현재 비트 계산
  getCurrentBeat(): number {
    const elapsed = Date.now() - this.startTime;
    const beatDuration = 60000 / this.bpm; // ms per beat
    return elapsed / beatDuration;
  }

  // 타이밍 정확도 계산
  calculateAccuracy(tapTime: number, targetTime: number): 'perfect' | 'good' | 'miss' {
    const diff = Math.abs(tapTime - targetTime);

    if (diff < 50) return 'perfect';
    if (diff < 100) return 'good';
    return 'miss';
  }
}
```

---

### 6. 타이쿤/방치형 게임 (케이크 키우기, 농장 키우기)

```json
{
  "필수": [
    "react-native-reanimated",
    "zustand"
  ],
  "불필요": [
    "matter-js",
    "react-native-game-engine",
    "@shopify/react-native-skia"
  ]
}
```

**방치형 수익 시스템:**
```typescript
import create from 'zustand';
import { persist } from 'zustand/middleware';

const useIdleStore = create(
  persist(
    (set, get) => ({
      money: 0,
      moneyPerSecond: 1,
      lastSaveTime: Date.now(),

      // 오프라인 수익 계산
      calculateOfflineEarnings: () => {
        const now = Date.now();
        const elapsed = (now - get().lastSaveTime) / 1000; // 초
        const offlineEarnings = Math.floor(elapsed * get().moneyPerSecond);

        set({
          money: get().money + offlineEarnings,
          lastSaveTime: now,
        });

        return offlineEarnings;
      },

      upgrade: (cost: number, incomeBoost: number) => {
        if (get().money >= cost) {
          set({
            money: get().money - cost,
            moneyPerSecond: get().moneyPerSecond + incomeBoost,
          });
        }
      },
    }),
    {
      name: 'idle-game-storage',
    }
  )
);
```

---

## 📋 체크리스트

### 프로젝트 시작 전
- [ ] 게임 타입에 맞는 기술 스택 선정
- [ ] 물리 엔진 필요 여부 결정
- [ ] Skia 필요 여부 결정 (오브젝트 수 고려)
- [ ] 성능 목표 설정 (60fps, 메모리 등)

### 개발 중
- [ ] Flipper로 FPS 모니터링
- [ ] JS Thread Time 측정 (< 16ms)
- [ ] 메모리 사용량 체크 (< 150MB)
- [ ] 오브젝트 풀링 적용 (필요 시)
- [ ] Delta Time 기반 이동 구현

### 최적화 단계
- [ ] 불필요한 리렌더링 제거
- [ ] 사운드 사전 로드 완료
- [ ] 물리 엔진 최적화 (enableSleeping 등)
- [ ] 충돌 레이어 설정
- [ ] Bundle Size 최적화 (< 30MB)

### 배포 전
- [ ] 60fps 달성 확인
- [ ] 저사양 기기 테스트 (Android 7.0)
- [ ] 메모리 누수 테스트
- [ ] 배터리 소모 테스트
- [ ] 오프라인 플레이 테스트

---

## 🚨 흔한 실수 및 해결책

### 실수 1: 모든 게임에 물리 엔진 사용
```typescript
// ❌ 나쁜 예: 단순 퍼즐에 Matter.js 사용
// 물 정렬 게임에는 물리 엔진 불필요!

// ✅ 좋은 예: 애니메이션만 사용
import Animated, { withSpring } from 'react-native-reanimated';
```

### 실수 2: SVG로 수백 개 오브젝트 렌더링
```typescript
// ❌ 나쁜 예: 탄막 게임에 SVG 사용
{bullets.map(b => <Circle cx={b.x} cy={b.y} />)} // 100개 이상이면 느림

// ✅ 좋은 예: Skia Canvas 사용
<Canvas>
  {bullets.map(b => <Circle cx={b.x} cy={b.y} />)}
</Canvas>
```

### 실수 3: 게임 상태를 Redux로 관리
```typescript
// ❌ 나쁜 예: 매 프레임 Redux 액션
dispatch(updatePlayerPosition(x, y)); // 리렌더링 폭탄

// ✅ 좋은 예: useRef로 로컬 관리
const gameStateRef = useRef({ player: { x, y } });
```

### 실수 4: 사운드 동기 로드
```typescript
// ❌ 나쁜 예: 게임 중 사운드 로드
const sound = new Sound('coin.mp3'); // UI 블로킹
sound.play();

// ✅ 좋은 예: 사전 로드
await soundManager.preloadSounds(); // 게임 시작 시
soundManager.play('coin'); // 즉시 재생
```

---

## 💡 결론

### 핵심 요약
1. **물리 엔진은 필요할 때만**: 퍼즐 게임 대부분은 불필요
2. **Skia는 오브젝트 100개 이상일 때**: 그 이하면 SVG로 충분
3. **게임 상태는 로컬 관리**: Redux/Context는 앱 상태용
4. **사운드는 사전 로드**: 비동기 처리 필수
5. **60fps 달성이 최우선**: 성능 모니터링 필수

### 다음 단계
- 게임 타입별 보일러플레이트 작성
- 성능 벤치마크 테스트
- 실제 게임에 적용 및 검증

---

**참고 문서:**
- `REACT_NATIVE_GAME_DEVLOPMENT_TIP.md`: 실전 개발 팁
- `TECH_STACK.md`: 전체 기술 스택 가이드
- `DEPLOY_STACK.md`: 배포 및 최적화 전략
