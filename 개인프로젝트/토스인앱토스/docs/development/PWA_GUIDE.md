# PWA (Progressive Web App) 개발 가이드

> **작성일**: 2025-10-27
> **목표**: 토스 앱인토스 플랫폼에서 PWA를 활용한 비용 효율적 앱 개발 전략

---

## 🎯 핵심 원칙

1. **PWA는 토스 앱인토스의 보완재**: 네이티브 앱 대신이 아닌, 웹 앱 버전의 고도화 수단
2. **수익화 제약 인지**: AdMob 직접 지원 불가, 웹 기반 결제만 가능
3. **점진적 향상(Progressive Enhancement)**: 기본 웹 앱 → PWA 기능 추가
4. **Next.js 네이티브 PWA 기능 활용**: 2024년 이후 공식 지원 활용

---

## 📊 PWA vs 네이티브 앱 vs 토스 앱인토스 비교

### 비교표

| 항목 | 네이티브 앱 | 토스 앱인토스 | PWA |
|------|-----------|-------------|-----|
| **개발 비용** | 높음 (iOS/Android 각각) | 중간 (WebView 기반) | 낮음 (웹 기술) |
| **배포** | 앱스토어 심사 필요 | 토스 4단계 검수 | 웹 호스팅만 |
| **업데이트** | 앱스토어 재심사 | 토스 재검수 | 즉시 반영 |
| **오프라인 지원** | ✅ 완전 지원 | ⚠️ 제한적 | ✅ 서비스 워커 캐싱 |
| **푸시 알림** | ✅ 완전 지원 | ✅ 지원 | ⚠️ iOS 제한적 |
| **설치** | 앱스토어 필수 | 토스 앱 내장 | 선택적 (홈 화면 추가) |
| **성능** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ |
| **AdMob 광고** | ✅ 완전 지원 | ✅ 완전 지원 | ❌ 직접 불가* |
| **Google/Apple IAP** | ✅ 완전 지원 | ⚠️ 제한적 | ❌ 불가 |
| **토스페이** | ⚠️ SDK 연동 | ✅ 네이티브 지원 | ✅ 웹 연동 |
| **SEO** | ❌ 불가 | ❌ 불가 | ✅ 완전 지원 |
| **하드웨어 접근** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐ |

> **참고**: AdMob은 PWA를 WebView로 래핑한 네이티브 앱에서만 사용 가능

---

## ⚠️ PWA의 토스 앱인토스 적용 시 제약사항

### 1. **수익화 제약 (CRITICAL)**

#### 광고 수익화 불가
- ❌ **Google AdMob 직접 지원 불가**: PWA는 앱스토어에 배포되지 않아 AdMob SDK 사용 불가
- ⚠️ **우회 방법**: PWA를 WebView로 래핑한 네이티브 앱으로 변환 시 AdMob 사용 가능 (하지만 이는 더 이상 순수 PWA가 아님)
- ✅ **대안**: Google AdSense (웹 광고) 사용 가능하지만 eCPM 낮음 (₩500-1,500 vs AdMob ₩3,000-5,000)

#### 인앱 결제 제약
- ❌ **Google Play Billing / Apple IAP 불가**: 앱스토어 미배포로 인한 제약
- ✅ **웹 기반 결제만 가능**: 토스페이, Stripe, PayPal 등
- ⚠️ **수수료 차이**: 토스페이 3% vs Google Play 15-30%

### 2. **플랫폼별 기능 제한**

#### iOS 제약 (심각)
- ❌ **푸시 알림 제한**: iOS 16.4 이전 버전은 PWA 푸시 알림 미지원
- ⚠️ **홈 화면 추가 후 제약**: Safari에서 추가한 PWA는 일부 Web API 제한
- ❌ **백그라운드 동기화 불가**: iOS Safari는 Background Sync API 미지원

#### Android 지원
- ✅ **대부분 PWA 기능 지원**: Chrome 기반으로 완벽 호환
- ✅ **푸시 알림 완전 지원**: FCM (Firebase Cloud Messaging) 사용 가능
- ✅ **백그라운드 동기화 지원**: 오프라인 데이터 동기화 가능

### 3. **토스 앱인토스 플랫폼 통합 제약**

#### mTLS 인증서
- ⚠️ **복잡도 증가**: PWA에서 mTLS 인증서 관리가 네이티브보다 복잡
- ✅ **해결 방법**: 백엔드 서버(Vercel/Cloudflare Workers)에서 mTLS 처리

#### 토스 네이티브 API 접근
- ⚠️ **제한적 접근**: 토스 로그인, 토스페이 등 일부 API는 WebView 환경에서만 완전 지원
- ✅ **대안**: 웹 기반 OAuth 로그인 및 토스페이 웹 결제 사용

---

## ✅ PWA가 적합한 앱 유형 (토스 앱인토스 기준)

### 적합한 앱 (PWA 권장)

#### 1. **텍스트 기반 AI 앱**
- ✅ **AI 이력서 작성기**
  - 이유: 텍스트 처리 중심, 하드웨어 접근 불필요
  - 수익화: 토스페이 결제 + AdSense 광고
  - 장점: 즉시 배포, SEO 노출로 유입 증가

- ✅ **AI 법률 상담 챗봇**
  - 이유: 대화형 UI, 오프라인 캐싱 활용 가능
  - 수익화: 토스페이 프리미엄 구독
  - 장점: 업데이트 빈번한 법률 정보 즉시 반영

#### 2. **정보 제공형 앱**
- ✅ **AI 감정 일기**
  - 이유: 텍스트 입력 중심, 푸시 알림 필수 아님
  - 수익화: 토스페이 결제
  - 장점: 서비스 워커로 오프라인 일기 작성 지원

- ✅ **AI 명함 스캔 & 네트워킹**
  - 이유: OCR은 Google Cloud Vision API로 처리 (서버 사이드)
  - 수익화: 토스페이 프리미엄
  - 장점: 명함 데이터 웹 기반 동기화

### 부적합한 앱 (네이티브 권장)

#### 1. **실시간 카메라/미디어 처리 앱**
- ❌ **AI 운동 자세 교정 코치**
  - 이유: 실시간 비디오 처리 성능 부족, 하드웨어 접근 제한
  - PWA 제약: MediaStream API 성능 한계
  - 권장: React Native

- ❌ **AI 헤어스타일 시뮬레이터**
  - 이유: 고품질 이미지 처리, 카메라 접근 빈번
  - PWA 제약: 이미지 처리 성능 저하
  - 권장: React Native

#### 2. **광고 수익 의존도 높은 앱**
- ❌ **AI 영어 회화 튜터**
  - 이유: 보상형 광고로 무료 크레딧 제공 전략 불가
  - PWA 제약: AdMob 보상형 광고 미지원
  - 권장: React Native (AdMob 완전 지원)

- ❌ **AI 요리 레시피 추천기**
  - 이유: 광고 수익 극대화 필요 (네이티브 광고, 전면 광고)
  - PWA 제약: AdSense eCPM 낮음
  - 권장: React Native

---

## 🛠️ Next.js PWA 구현 가이드 (2025년 최신)

### 1. Next.js 공식 PWA 지원 (권장)

> **2024년 가을 이후 Next.js는 PWA를 네이티브로 지원**합니다. `next-pwa` 같은 외부 패키지 불필요.

#### Step 1: Manifest 파일 생성

**app/manifest.ts** (TypeScript 권장):

```typescript
import type { MetadataRoute } from 'next'

export default function manifest(): MetadataRoute.Manifest {
  return {
    name: 'AI 이력서 작성기',
    short_name: '이력서AI',
    description: '토스 앱인토스 기반 AI 이력서 자동 생성 서비스',
    start_url: '/',
    display: 'standalone',
    background_color: '#ffffff',
    theme_color: '#0064FF', // 토스 블루
    orientation: 'portrait',
    icons: [
      {
        src: '/icon-192x192.png',
        sizes: '192x192',
        type: 'image/png',
        purpose: 'any maskable'
      },
      {
        src: '/icon-512x512.png',
        sizes: '512x512',
        type: 'image/png',
        purpose: 'any maskable'
      }
    ],
    screenshots: [
      {
        src: '/screenshot-mobile.png',
        sizes: '750x1334',
        type: 'image/png',
        form_factor: 'narrow'
      }
    ]
  }
}
```

또는 **public/manifest.json** (JSON):

```json
{
  "name": "AI 이력서 작성기",
  "short_name": "이력서AI",
  "description": "토스 앱인토스 기반 AI 이력서 자동 생성 서비스",
  "start_url": "/",
  "display": "standalone",
  "background_color": "#ffffff",
  "theme_color": "#0064FF",
  "orientation": "portrait",
  "icons": [
    {
      "src": "/icon-192x192.png",
      "sizes": "192x192",
      "type": "image/png",
      "purpose": "any maskable"
    },
    {
      "src": "/icon-512x512.png",
      "sizes": "512x512",
      "type": "image/png",
      "purpose": "any maskable"
    }
  ]
}
```

#### Step 2: Metadata 설정

**app/layout.tsx**:

```typescript
import type { Metadata, Viewport } from 'next'

export const metadata: Metadata = {
  title: 'AI 이력서 작성기',
  description: '토스 앱인토스 기반 AI 이력서 자동 생성 서비스',
  manifest: '/manifest.json', // manifest.ts 사용 시 자동 생성
  appleWebApp: {
    capable: true,
    statusBarStyle: 'default',
    title: '이력서AI'
  },
  formatDetection: {
    telephone: false
  }
}

export const viewport: Viewport = {
  themeColor: '#0064FF', // Next.js 16+ 권장 방식
  width: 'device-width',
  initialScale: 1,
  maximumScale: 1,
  userScalable: false
}

export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="ko">
      <head>
        {/* iOS Safari 전용 메타 태그 */}
        <link rel="apple-touch-icon" href="/icon-192x192.png" />
        <meta name="apple-mobile-web-app-capable" content="yes" />
        <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent" />
      </head>
      <body>{children}</body>
    </html>
  )
}
```

#### Step 3: 서비스 워커 구현 (옵션)

**public/sw.js** (기본 캐싱 전략):

```javascript
const CACHE_NAME = 'toss-resume-ai-v1';
const urlsToCache = [
  '/',
  '/offline.html', // 오프라인 폴백 페이지
  '/icon-192x192.png',
  '/icon-512x512.png'
];

// 설치 이벤트: 정적 자산 캐싱
self.addEventListener('install', (event) => {
  event.waitUntil(
    caches.open(CACHE_NAME)
      .then((cache) => cache.addAll(urlsToCache))
  );
  self.skipWaiting(); // 즉시 활성화
});

// 활성화 이벤트: 오래된 캐시 삭제
self.addEventListener('activate', (event) => {
  event.waitUntil(
    caches.keys().then((cacheNames) => {
      return Promise.all(
        cacheNames.map((cacheName) => {
          if (cacheName !== CACHE_NAME) {
            return caches.delete(cacheName);
          }
        })
      );
    })
  );
  self.clients.claim();
});

// Fetch 이벤트: Network First 전략 (API 호출)
self.addEventListener('fetch', (event) => {
  if (event.request.url.includes('/api/')) {
    // API 요청: Network First
    event.respondWith(
      fetch(event.request)
        .catch(() => caches.match('/offline.html'))
    );
  } else {
    // 정적 자산: Cache First
    event.respondWith(
      caches.match(event.request)
        .then((response) => response || fetch(event.request))
    );
  }
});
```

**서비스 워커 등록 (app/layout.tsx)**:

```typescript
'use client'

import { useEffect } from 'react'

export default function RootLayout({ children }) {
  useEffect(() => {
    if ('serviceWorker' in navigator) {
      navigator.serviceWorker
        .register('/sw.js')
        .then((registration) => {
          console.log('서비스 워커 등록 성공:', registration.scope)
        })
        .catch((error) => {
          console.error('서비스 워커 등록 실패:', error)
        })
    }
  }, [])

  return (
    <html lang="ko">
      <body>{children}</body>
    </html>
  )
}
```

---

### 2. Serwist 활용 (고급 캐싱 전략)

> **next-pwa 패키지는 더 이상 권장하지 않습니다** (App Router 미지원). 대신 **Serwist** 사용.

#### Step 1: 설치

```bash
npm install @serwist/next @serwist/window
```

#### Step 2: next.config.mjs 설정

```javascript
import withSerwist from '@serwist/next';

/** @type {import('next').NextConfig} */
const nextConfig = {
  reactStrictMode: true,
};

export default withSerwist({
  swSrc: 'app/sw.ts',
  swDest: 'public/sw.js',
  disable: process.env.NODE_ENV === 'development', // 개발 환경 비활성화
  reloadOnOnline: true, // 온라인 복귀 시 재로드
})(nextConfig);
```

#### Step 3: 서비스 워커 구현 (app/sw.ts)

```typescript
import { defaultCache } from '@serwist/next/worker';
import type { PrecacheEntry } from '@serwist/precaching';
import { Serwist } from 'serwist';

declare const self: ServiceWorkerGlobalScope & {
  __SW_MANIFEST: (PrecacheEntry | string)[] | undefined;
};

const serwist = new Serwist({
  precacheEntries: self.__SW_MANIFEST,
  skipWaiting: true,
  clientsClaim: true,
  navigationPreload: true,
  runtimeCaching: defaultCache,
  fallbacks: {
    entries: [
      {
        url: '/offline',
        matcher({ request }) {
          return request.destination === 'document';
        },
      },
    ],
  },
});

serwist.addEventListeners();
```

---

## 📈 PWA 수익화 전략 (토스 앱인토스 최적화)

### 1. 웹 광고 전략 (AdSense 대안)

#### Google AdSense 통합

**장점**:
- ✅ PWA에서 사용 가능한 유일한 Google 광고 솔루션
- ✅ 설정 간단 (스크립트 삽입)
- ✅ 자동 광고 배치

**단점**:
- ⚠️ eCPM 낮음: AdMob ₩3,000-5,000 vs AdSense ₩500-1,500
- ⚠️ 모바일 UX 저해: 광고 크기 최적화 필요

**구현 예시 (app/layout.tsx)**:

```typescript
export default function RootLayout({ children }) {
  return (
    <html lang="ko">
      <head>
        <script
          async
          src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-XXXXXXXXXXXXXXXX"
          crossOrigin="anonymous"
        />
      </head>
      <body>{children}</body>
    </html>
  )
}
```

**배너 광고 컴포넌트 (components/AdBanner.tsx)**:

```typescript
'use client'

import { useEffect } from 'react'

export default function AdBanner() {
  useEffect(() => {
    try {
      ;(window.adsbygoogle = window.adsbygoogle || []).push({})
    } catch (err) {
      console.error('AdSense error:', err)
    }
  }, [])

  return (
    <div className="ad-container">
      <ins
        className="adsbygoogle"
        style={{ display: 'block' }}
        data-ad-client="ca-pub-XXXXXXXXXXXXXXXX"
        data-ad-slot="1234567890"
        data-ad-format="auto"
        data-full-width-responsive="true"
      />
    </div>
  )
}
```

#### 대안: 네이티브 광고 네트워크

- **Taboola**: 콘텐츠 추천 광고, PWA 지원
- **Outbrain**: 네이티브 광고, 웹 친화적
- **예상 eCPM**: ₩800-2,000 (AdSense보다 높음)

---

### 2. 토스페이 결제 통합 (PWA 최적화)

#### 웹 기반 토스페이 결제

**장점**:
- ✅ PWA 완벽 호환
- ✅ 토스 앱인토스 사용자와 자연스러운 통합
- ✅ 낮은 수수료 (3%)

**구현 예시 (Server Action)**:

```typescript
'use server'

import { TossPayments } from '@tosspayments/payment-sdk-server'

const tossPayments = new TossPayments(process.env.TOSS_SECRET_KEY!)

export async function createPayment(orderId: string, amount: number) {
  try {
    const payment = await tossPayments.requestPayment({
      orderId,
      amount,
      orderName: '프리미엄 구독 (월간)',
      successUrl: `${process.env.NEXT_PUBLIC_URL}/payment/success`,
      failUrl: `${process.env.NEXT_PUBLIC_URL}/payment/fail`,
      customerName: '사용자명',
      customerEmail: 'user@example.com'
    })

    return { success: true, paymentUrl: payment.checkoutUrl }
  } catch (error) {
    return { success: false, error: error.message }
  }
}
```

**클라이언트 컴포넌트 (app/components/PremiumButton.tsx)**:

```typescript
'use client'

import { createPayment } from '@/app/actions/payment'
import { useState } from 'react'

export default function PremiumButton() {
  const [loading, setLoading] = useState(false)

  const handlePayment = async () => {
    setLoading(true)
    const orderId = `ORDER_${Date.now()}`
    const result = await createPayment(orderId, 4900)

    if (result.success) {
      window.location.href = result.paymentUrl
    } else {
      alert('결제 생성 실패')
    }
    setLoading(false)
  }

  return (
    <button onClick={handlePayment} disabled={loading}>
      {loading ? '처리 중...' : '프리미엄 구독 (₩4,900/월)'}
    </button>
  )
}
```

---

### 3. 하이브리드 수익 모델 (PWA + 네이티브)

#### 전략: PWA 우선, 네이티브 병행

```
┌─────────────────────────────────────────┐
│ 1단계: PWA 출시 (웹 앱)                   │
├─────────────────────────────────────────┤
│ - 토스 앱인토스 WebView 배포              │
│ - 토스페이 결제 + AdSense 광고            │
│ - SEO 노출로 유기적 유입                  │
│ - 예상 수익: ₩100만/월 (MAU 5,000)       │
└─────────────────────────────────────────┘
                  ↓ 검증 완료 후
┌─────────────────────────────────────────┐
│ 2단계: React Native 앱 개발 (선택)        │
├─────────────────────────────────────────┤
│ - PWA 코드 80% 재사용 (React 기반)        │
│ - AdMob 광고 통합 (eCPM 3배 증가)         │
│ - Google Play / App Store 배포           │
│ - 예상 수익: ₩400만/월 (MAU 10,000)      │
└─────────────────────────────────────────┘
```

---

## 🎯 토스 앱인토스 PWA 체크리스트

### 기획 단계
- [ ] 앱이 텍스트/정보 중심인가? (카메라/센서 미사용)
- [ ] 광고 수익 의존도가 낮은가? (AdSense eCPM 감수 가능)
- [ ] 토스페이 결제로 충분한가? (Google/Apple IAP 불필요)
- [ ] 빈번한 업데이트가 필요한가? (PWA 장점 활용)
- [ ] SEO 노출이 중요한가? (검색 유입 기대)

### 개발 단계
- [ ] Next.js 16+ App Router 사용
- [ ] manifest.ts 또는 manifest.json 생성
- [ ] 아이콘 준비 (192x192, 512x512 PNG, maskable)
- [ ] 서비스 워커 구현 (오프라인 지원)
- [ ] HTTPS 배포 (PWA 필수 요구사항)
- [ ] 토스페이 웹 결제 통합
- [ ] AdSense 또는 대안 광고 네트워크 연동

### 배포 단계
- [ ] Vercel 또는 Cloudflare Pages 배포
- [ ] Lighthouse PWA 점수 90+ 확인
- [ ] iOS Safari 홈 화면 추가 테스트
- [ ] Android Chrome 설치 배너 테스트
- [ ] 토스 앱인토스 WebView 통합 테스트
- [ ] mTLS 인증서 서버 사이드 설정

### 수익화 단계
- [ ] GA4 이벤트 추적 설정
- [ ] AdSense 광고 단위 생성
- [ ] 토스페이 결제 Webhook 구현
- [ ] 프리미엄 기능 게이트 설정
- [ ] A/B 테스트 준비 (가격, 광고 배치)

---

## 📚 PWA 개발 참고 자료

### 공식 문서
- [Next.js PWA Guide](https://nextjs.org/docs/app/guides/progressive-web-apps) (공식)
- [Serwist Documentation](https://serwist.pages.dev/) (서비스 워커 라이브러리)
- [Web.dev PWA](https://web.dev/progressive-web-apps/) (Google 공식 가이드)

### 토스 앱인토스 관련
- [토스페이먼츠 결제창 연동](https://docs.tosspayments.com/reference/widget-sdk)
- [토스 앱인토스 WebView API](https://developers-apps-in-toss.toss.im/)

### 도구
- [Lighthouse](https://developers.google.com/web/tools/lighthouse) (PWA 점수 측정)
- [PWA Builder](https://www.pwabuilder.com/) (매니페스트 생성 도구)
- [Maskable.app](https://maskable.app/) (아이콘 테스트)

---

## 🎯 결론

### PWA 적용 권장 시나리오

✅ **PWA를 사용해야 하는 경우**:
1. 텍스트/정보 중심 AI 앱 (이력서 작성기, 법률 상담)
2. 빈번한 업데이트 필요 (법률 정보, 레시피)
3. SEO 노출 중요 (검색 유입 극대화)
4. 개발 비용 최소화 (1개 코드베이스로 모든 플랫폼)
5. 광고 수익 의존도 낮음 (프리미엄 구독 중심)

❌ **네이티브 앱을 사용해야 하는 경우**:
1. 실시간 카메라/센서 사용 (운동 코치, 헤어스타일)
2. 광고 수익 극대화 필요 (AdMob eCPM 중요)
3. 푸시 알림 핵심 기능 (iOS 사용자 많음)
4. 고성능 그래픽 처리 (게임, 이미지 편집)
5. 앱스토어 노출 필요 (오가닉 다운로드 기대)

### 하이브리드 전략 (권장)

```
Phase 1: PWA로 MVP 검증 (2주) → 빠른 출시
Phase 2: 검증 완료 시 React Native 전환 (4주) → 수익 극대화
```

이 방식으로 **초기 개발 비용 절감 + 검증 후 수익 최적화** 달성 가능.

---

**다음 문서**: [AI_APP_IDEAS.md](../plans/AI_APP_IDEAS.md)에서 각 앱별 PWA 적용 가능성 검토
