# YouTube Creator Platform - 2025 기술스택

**최종 업데이트**: 2025-11-04
**Next.js Version**: 16 (Turbopack, React 19.2)
**배포 환경**: Vercel

---

## 📋 기술스택 개요

이 문서는 YouTube Creator Platform SaaS 프로젝트의 공식 기술스택을 정의합니다. Next.js 16의 최신 기능을 활용하며, 2025년 업계 트렌드를 반영한 모던 웹 개발 스택입니다.

---

## 🎯 핵심 프레임워크

### Next.js 16 (2025년 10월 출시)
```json
{
  "framework": "Next.js 16",
  "features": [
    "Turbopack (안정화) - 5-10x 빠른 Fast Refresh",
    "Cache Components - 'use cache' 디렉티브",
    "React 19.2 통합 - View Transitions, useEffectEvent",
    "React Compiler (안정화) - 자동 메모이제이션",
    "Layout 중복 제거 최적화"
  ],
  "requirements": {
    "node": ">=20.9.0",
    "typescript": ">=5.1.0"
  }
}
```

**주요 신기능**:
- **Turbopack**: 개발 서버 시작 5-10배 빠름, 빌드 2-5배 빠름
- **Cache Components**: 명시적 캐싱 (`use cache`)으로 성능 최적화
- **React Compiler**: 수동 메모이제이션 불필요 (자동 `useMemo`, `useCallback`)
- **Next.js Devtools MCP**: Model Context Protocol 통합 디버깅

**공식 문서**: https://nextjs.org/blog/next-16

---

### React 19.2
```json
{
  "version": "19.2+",
  "features": [
    "View Transitions API",
    "useEffectEvent (Effects에서 비반응 로직 추출)",
    "Activity (백그라운드 UI 상태 유지)",
    "향상된 서버 컴포넌트"
  ]
}
```

---

## 🎨 스타일링 & UI

### Tailwind CSS 4.0
```bash
npm install tailwindcss@next
```

**특징**:
- CSS 변수 기반 설정
- 향상된 성능
- 네이티브 Cascade Layers 지원

---

### shadcn/ui
```bash
npx shadcn@latest init
```

**컴포넌트 예시**:
```typescript
import { Button } from "@/components/ui/button"
import { Card } from "@/components/ui/card"
import { Dialog } from "@/components/ui/dialog"
```

**특징**:
- Radix UI 기반 무장애 접근성
- 복사-붙여넣기 방식 (패키지 의존성 없음)
- Tailwind CSS와 완벽한 통합
- 커스터마이징 완전 자유

**공식 사이트**: https://ui.shadcn.com

---

### Lucide Icons
```bash
npm install lucide-react
```

**사용 예시**:
```typescript
import { Youtube, TrendingUp, Edit, Download } from "lucide-react"

<Youtube className="h-6 w-6" />
```

**특징**:
- 1,500+ 아이콘
- Tree-shaking 지원 (사용한 아이콘만 번들)
- shadcn/ui와 완벽 호환

---

### Framer Motion
```bash
npm install framer-motion
```

**애니메이션 예시**:
```typescript
import { motion } from "framer-motion"

<motion.div
  initial={{ opacity: 0 }}
  animate={{ opacity: 1 }}
  transition={{ duration: 0.5 }}
>
  Dashboard Content
</motion.div>
```

**사용 케이스**:
- 페이지 전환 애니메이션
- 모달/다이얼로그 등장 효과
- 데이터 시각화 애니메이션
- 인터랙티브 UI 요소

---

## 🗄️ 상태 관리

### Zustand
```bash
npm install zustand
```

**Store 예시**:
```typescript
// stores/userStore.ts
import { create } from 'zustand'

interface UserState {
  user: User | null
  setUser: (user: User) => void
}

export const useUserStore = create<UserState>((set) => ({
  user: null,
  setUser: (user) => set({ user })
}))
```

**특징**:
- 경량 (1KB gzipped)
- Redux보다 간단한 API
- TypeScript 완벽 지원
- DevTools 지원

---

### TanStack Query (React Query) v5
```bash
npm install @tanstack/react-query
```

**사용 예시**:
```typescript
import { useQuery } from '@tanstack/react-query'

export function useChannelAnalytics(channelId: string) {
  return useQuery({
    queryKey: ['analytics', channelId],
    queryFn: () => fetchAnalytics(channelId),
    staleTime: 5 * 60 * 1000, // 5분
  })
}
```

**역할**:
- 서버 상태 관리 (API 데이터 캐싱)
- 자동 재시도 및 백그라운드 갱신
- Optimistic Updates
- Pagination & Infinite Scroll

---

## 💾 백엔드/데이터 & 인증

### Supabase
```bash
npm install @supabase/supabase-js @supabase/ssr
```

**클라이언트 설정**:
```typescript
// lib/supabase/client.ts
import { createBrowserClient } from '@supabase/ssr'

export const supabase = createBrowserClient(
  process.env.NEXT_PUBLIC_SUPABASE_URL!,
  process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!
)
```

**제공 기능**:
- **PostgreSQL Database**: 실시간 DB
- **Authentication**: Google OAuth, Email/Password
- **Storage**: 파일 업로드 (영상 썸네일, 프로필 사진)
- **Realtime**: WebSocket 기반 실시간 업데이트
- **Edge Functions**: 서버리스 함수

**공식 문서**: https://supabase.com/docs

---

### Prisma ORM
```bash
npm install prisma @prisma/client
npx prisma init
```

**스키마 예시**:
```prisma
// prisma/schema.prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

model User {
  id            String        @id @default(cuid())
  email         String        @unique
  name          String?
  image         String?
  channels      Channel[]
  scripts       Script[]
  subscription  Subscription?
  createdAt     DateTime      @default(now())
  updatedAt     DateTime      @updatedAt
}

model Channel {
  id              String   @id @default(cuid())
  userId          String
  youtubeId       String   @unique
  title           String
  subscriberCount Int      @default(0)
  user            User     @relation(fields: [userId], references: [id])
  videos          Video[]
  createdAt       DateTime @default(now())
  updatedAt       DateTime @updatedAt
}

model Script {
  id        String   @id @default(cuid())
  userId    String
  title     String
  content   String   @db.Text
  topic     String?
  language  String   @default("ko")
  user      User     @relation(fields: [userId], references: [id])
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}
```

**명령어**:
```bash
npx prisma migrate dev --name init
npx prisma generate
npx prisma studio  # DB GUI
```

**특징**:
- 타입 안전 쿼리
- 자동 마이그레이션
- Supabase PostgreSQL 완벽 호환
- Prisma Studio (DB 관리 GUI)

---

### Clerk (인증)
```bash
npm install @clerk/nextjs
```

**Next.js 16 설정**:
```typescript
// app/layout.tsx
import { ClerkProvider } from '@clerk/nextjs'

export default function RootLayout({ children }) {
  return (
    <ClerkProvider>
      <html lang="ko">
        <body>{children}</body>
      </html>
    </ClerkProvider>
  )
}
```

**Middleware**:
```typescript
// middleware.ts
import { clerkMiddleware } from '@clerk/nextjs/server'

export default clerkMiddleware()

export const config = {
  matcher: [
    '/((?!_next|[^?]*\\.(?:html?|css|js(?!on)|jpe?g|webp|png|gif|svg|ttf|woff2?|ico|csv|docx?|xlsx?|zip|webmanifest)).*)',
    '/(api|trpc)(.*)',
  ],
}
```

**YouTube OAuth 연동**:
```typescript
// 2025년 4월 업데이트: Clerk ↔ Supabase 통합
import { createClerkSupabaseClient } from '@clerk/clerk-sdk-node'

const supabase = createClerkSupabaseClient({
  supabaseUrl: process.env.NEXT_PUBLIC_SUPABASE_URL!,
  supabaseKey: process.env.SUPABASE_SERVICE_ROLE_KEY!,
})
```

**특징**:
- Google OAuth (YouTube API 스코프 포함)
- 사전 구축된 UI 컴포넌트
- **Supabase 네이티브 통합** (2025년 3월 출시)
- 세션 관리 및 보안
- 멀티 테넌시 지원

**공식 문서**: https://clerk.com/docs/quickstarts/nextjs

---

### NextAuth.js v5 (Auth.js) - 대안
```bash
npm install next-auth@beta @auth/prisma-adapter
```

**설정 예시**:
```typescript
// auth.ts
import NextAuth from "next-auth"
import Google from "next-auth/providers/google"
import { PrismaAdapter } from "@auth/prisma-adapter"
import { prisma } from "./lib/prisma"

export const { handlers, auth, signIn, signOut } = NextAuth({
  adapter: PrismaAdapter(prisma),
  providers: [
    Google({
      clientId: process.env.GOOGLE_CLIENT_ID!,
      clientSecret: process.env.GOOGLE_CLIENT_SECRET!,
      authorization: {
        params: {
          scope: "openid email profile https://www.googleapis.com/auth/youtube.readonly",
          prompt: "consent",
          access_type: "offline",
          response_type: "code",
        },
      },
    }),
  ],
})
```

**Clerk vs NextAuth 비교**:
| 기능 | Clerk | NextAuth.js |
|------|-------|-------------|
| 가격 | 무료 (10K MAU), $25/월부터 | 100% 무료 오픈소스 |
| UI | 사전 구축 컴포넌트 | 직접 구현 필요 |
| Supabase 통합 | 네이티브 지원 (2025) | JWT 커스텀 필요 |
| 설정 난이도 | 쉬움 | 중간 |
| 커스터마이징 | 제한적 | 완전 자유 |

**권장 사항**:
- **MVP 단계**: Clerk (빠른 개발)
- **장기 운영**: NextAuth.js (비용 절감, 완전한 제어)

---

## 🤖 LLM & AI

### OpenRouter API
```bash
npm install openai
```

**클라이언트 설정**:
```typescript
// lib/openrouter.ts
import OpenAI from 'openai'

export const openrouter = new OpenAI({
  apiKey: process.env.OPENROUTER_API_KEY,
  baseURL: 'https://openrouter.ai/api/v1/',
  defaultHeaders: {
    'HTTP-Referer': process.env.NEXT_PUBLIC_SITE_URL,
    'X-Title': 'YouTube Creator Platform',
  },
})
```

**스크립트 생성 API**:
```typescript
// app/api/scripts/generate/route.ts
import { openrouter } from '@/lib/openrouter'
import { NextRequest, NextResponse } from 'next/server'

export async function POST(req: NextRequest) {
  const { topic, videoType, tone } = await req.json()

  const completion = await openrouter.chat.completions.create({
    model: 'anthropic/claude-3.5-sonnet',  // 또는 무료 모델
    messages: [
      {
        role: 'system',
        content: 'You are a professional YouTube scriptwriter.',
      },
      {
        role: 'user',
        content: `Generate a ${videoType} script about: ${topic}. Tone: ${tone}`,
      },
    ],
  })

  return NextResponse.json({
    script: completion.choices[0].message.content
  })
}
```

**지원 모델** (2025):
- **GPT-4 Turbo**: `openai/gpt-4-turbo`
- **Claude 3.5 Sonnet**: `anthropic/claude-3.5-sonnet`
- **Gemini Pro**: `google/gemini-pro`
- **무료 모델**: `deepseek/deepseek-r1-distill-llama-70b:free`

**장점**:
- 단일 API로 여러 LLM 접근
- 자동 폴백 (모델 장애 시)
- 비용 최적화 (가장 저렴한 모델 자동 선택)
- 무료 모델 제공

**공식 사이트**: https://openrouter.ai

---

### Vercel AI SDK (대안)
```bash
npm install ai @ai-sdk/openai @ai-sdk/anthropic
```

**스트리밍 응답**:
```typescript
import { streamText } from 'ai'
import { openai } from '@ai-sdk/openai'

export async function POST(req: Request) {
  const { messages } = await req.json()

  const result = streamText({
    model: openai('gpt-4-turbo'),
    messages,
  })

  return result.toDataStreamResponse()
}
```

---

## 💳 결제 & 알림

### Toss Payments
```bash
npm install @tosspayments/payment-widget-sdk
```

**결제 위젯 통합**:
```typescript
'use client'

import { useEffect, useRef } from 'react'
import { loadTossPayments } from '@tosspayments/payment-widget-sdk'

export function TossPaymentWidget() {
  const paymentWidgetRef = useRef(null)
  const clientKey = process.env.NEXT_PUBLIC_TOSS_CLIENT_KEY

  useEffect(() => {
    loadTossPayments(clientKey).then((tossPayments) => {
      const paymentWidget = tossPayments.payment({
        customerKey: 'user-unique-id',
      })
      paymentWidgetRef.current = paymentWidget
    })
  }, [clientKey])

  const handlePayment = async () => {
    const paymentWidget = paymentWidgetRef.current

    await paymentWidget.requestPayment({
      orderId: 'order-id',
      orderName: 'Pro 플랜 구독',
      amount: 19900,
      successUrl: `${window.location.origin}/payment/success`,
      failUrl: `${window.location.origin}/payment/fail`,
    })
  }

  return (
    <div>
      <div id="payment-widget" />
      <button onClick={handlePayment}>결제하기</button>
    </div>
  )
}
```

**구독 결제 (정기 결제)**:
```typescript
// app/api/subscribe/route.ts
export async function POST(req: Request) {
  const { plan } = await req.json()

  // Toss Payments 빌링키 발급
  const response = await fetch(
    'https://api.tosspayments.com/v1/billing/authorizations/issue',
    {
      method: 'POST',
      headers: {
        Authorization: `Basic ${Buffer.from(
          process.env.TOSS_SECRET_KEY + ':'
        ).toString('base64')}`,
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        customerKey: 'user-unique-id',
        authKey: 'auth-key-from-widget',
      }),
    }
  )

  const { billingKey } = await response.json()

  // 정기 결제 등록
  // ...
}
```

**특징**:
- 한국 시장 최적화
- 간편 결제 (토스페이, 카카오페이, 네이버페이)
- 정기 결제 (구독) 지원
- 국내 모든 카드사 지원
- 실시간 결제 알림

**공식 문서**: https://docs.tosspayments.com

---

### SOLAPI (문자/알림톡)
```bash
npm install solapi
```

**SMS 발송**:
```typescript
// lib/solapi.ts
import { SolapiMessageService } from 'solapi'

const messageService = new SolapiMessageService(
  process.env.SOLAPI_API_KEY!,
  process.env.SOLAPI_API_SECRET!
)

export async function sendSMS(to: string, text: string) {
  return await messageService.send({
    to,
    from: process.env.SOLAPI_SENDER_NUMBER!,
    text,
  })
}

export async function sendAlimtalk(to: string, templateCode: string, params: object) {
  return await messageService.send({
    to,
    from: process.env.SOLAPI_SENDER_NUMBER!,
    type: 'ATA',  // 알림톡
    kakaoOptions: {
      pfId: process.env.SOLAPI_KAKAO_PF_ID!,
      templateId: templateCode,
      variables: params,
    },
  })
}
```

**사용 예시**:
```typescript
// 구독 완료 알림
await sendAlimtalk(
  user.phone,
  'SUBSCRIPTION_COMPLETE',
  {
    userName: user.name,
    planName: 'Pro 플랜',
    amount: '19,900원',
  }
)

// 채널 마일스톤 알림
await sendSMS(
  user.phone,
  `축하합니다! ${channelName} 채널이 구독자 10,000명을 달성했습니다!`
)
```

**특징**:
- SMS, LMS, MMS 발송
- 카카오 알림톡/친구톡
- 대량 발송 지원
- 발송 결과 웹훅

**공식 사이트**: https://solapi.com

---

## 📧 이메일

### Resend
```bash
npm install resend
```

**이메일 발송**:
```typescript
// lib/resend.ts
import { Resend } from 'resend'

export const resend = new Resend(process.env.RESEND_API_KEY)

// 이메일 템플릿 (React 컴포넌트)
// emails/welcome.tsx
export const WelcomeEmail = ({ userName }: { userName: string }) => (
  <div>
    <h1>안녕하세요, {userName}님!</h1>
    <p>YouTube Creator Platform에 가입하신 것을 환영합니다.</p>
  </div>
)

// 이메일 발송
import { WelcomeEmail } from '@/emails/welcome'

await resend.emails.send({
  from: 'YouTube Creator <noreply@yourplatform.com>',
  to: user.email,
  subject: '가입을 환영합니다!',
  react: WelcomeEmail({ userName: user.name }),
})
```

**특징**:
- React 컴포넌트로 이메일 작성
- 99% 전달률
- Vercel 통합 최적화
- 무료 티어 (월 3,000건)

**공식 사이트**: https://resend.com

---

## 🚀 인프라 & 배포

### Vercel
```bash
npm install -g vercel
vercel
```

**설정 파일**:
```json
// vercel.json
{
  "framework": "nextjs",
  "buildCommand": "next build",
  "installCommand": "npm install",
  "env": {
    "DATABASE_URL": "@database-url",
    "OPENROUTER_API_KEY": "@openrouter-api-key"
  }
}
```

**특징**:
- Next.js 16 네이티브 지원
- Turbopack 빌드 자동 활용
- Edge Runtime 지원
- 자동 HTTPS & CDN
- Preview Deployments (PR마다 자동 배포)
- 무료 티어 (취미 프로젝트)

**배포 플랜**:
- **Hobby**: 무료 (개인 프로젝트)
- **Pro**: $20/월 (상업용)
- **Enterprise**: 커스텀

---

## 📊 모니터링 & 분석

### Sentry
```bash
npm install @sentry/nextjs
npx @sentry/wizard@latest -i nextjs
```

**자동 설정 후**:
```typescript
// sentry.client.config.ts
Sentry.init({
  dsn: process.env.NEXT_PUBLIC_SENTRY_DSN,
  tracesSampleRate: 1.0,
  replaysSessionSampleRate: 0.1,
  replaysOnErrorSampleRate: 1.0,
})
```

**에러 트래킹**:
```typescript
try {
  await generateScript(topic)
} catch (error) {
  Sentry.captureException(error)
  throw error
}
```

**특징**:
- 자동 에러 캡처
- 성능 모니터링
- 세션 리플레이
- Next.js 16 공식 지원

---

### PostHog (Analytics)
```bash
npm install posthog-js
```

**설정**:
```typescript
// app/providers.tsx
'use client'

import posthog from 'posthog-js'
import { PostHogProvider } from 'posthog-js/react'

if (typeof window !== 'undefined') {
  posthog.init(process.env.NEXT_PUBLIC_POSTHOG_KEY!, {
    api_host: process.env.NEXT_PUBLIC_POSTHOG_HOST,
  })
}

export function PHProvider({ children }) {
  return <PostHogProvider client={posthog}>{children}</PostHogProvider>
}
```

**이벤트 트래킹**:
```typescript
import { usePostHog } from 'posthog-js/react'

const posthog = usePostHog()

posthog.capture('script_generated', {
  topic,
  videoType,
  language,
})
```

**특징**:
- 사용자 행동 분석
- 퍼널 분석
- A/B 테스팅
- 세션 리플레이
- 오픈소스 (셀프 호스팅 가능)

---

## 🧰 개발 도구

### TypeScript 5.6+
```json
// tsconfig.json
{
  "compilerOptions": {
    "target": "ES2022",
    "lib": ["ES2023", "DOM", "DOM.Iterable"],
    "jsx": "preserve",
    "module": "ESNext",
    "moduleResolution": "bundler",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "paths": {
      "@/*": ["./*"]
    }
  }
}
```

---

### Zod (Validation)
```bash
npm install zod
```

**사용 예시**:
```typescript
import { z } from 'zod'

export const ScriptSchema = z.object({
  topic: z.string().min(3, '주제는 3자 이상 입력해주세요'),
  videoType: z.enum(['short', 'long']),
  tone: z.enum(['casual', 'professional', 'educational']),
  language: z.string().default('ko'),
})

export type ScriptInput = z.infer<typeof ScriptSchema>
```

---

### React Hook Form
```bash
npm install react-hook-form @hookform/resolvers
```

**Form 예시**:
```typescript
import { useForm } from 'react-hook-form'
import { zodResolver } from '@hookform/resolvers/zod'

export function ScriptForm() {
  const form = useForm<ScriptInput>({
    resolver: zodResolver(ScriptSchema),
    defaultValues: {
      videoType: 'long',
      language: 'ko',
    },
  })

  const onSubmit = (data: ScriptInput) => {
    // Submit logic
  }

  return <form onSubmit={form.handleSubmit(onSubmit)}>...</form>
}
```

---

### 유틸리티 라이브러리

#### date-fns
```bash
npm install date-fns
```

**사용 예시**:
```typescript
import { format, formatDistanceToNow } from 'date-fns'
import { ko } from 'date-fns/locale'

format(new Date(), 'yyyy년 MM월 dd일', { locale: ko })
formatDistanceToNow(video.publishedAt, { addSuffix: true, locale: ko })
// "3일 전"
```

---

#### es-toolkit
```bash
npm install es-toolkit
```

**사용 예시**:
```typescript
import { debounce, chunk, uniq } from 'es-toolkit'

// Lodash 대체, 더 작고 빠름
const debouncedSearch = debounce(searchVideos, 300)
const batches = chunk(videos, 10)
const uniqueTags = uniq(tags)
```

---

#### ts-pattern
```bash
npm install ts-pattern
```

**패턴 매칭**:
```typescript
import { match } from 'ts-pattern'

const message = match(subscriptionTier)
  .with('free', () => '무료 플랜')
  .with('pro', () => 'Pro 플랜')
  .with('business', () => 'Business 플랜')
  .exhaustive()
```

---

## 🧪 테스팅

### Storybook
```bash
npx storybook@latest init
```

**컴포넌트 스토리**:
```typescript
// components/ui/button.stories.tsx
import type { Meta, StoryObj } from '@storybook/react'
import { Button } from './button'

const meta: Meta<typeof Button> = {
  title: 'UI/Button',
  component: Button,
}

export default meta
type Story = StoryObj<typeof Button>

export const Primary: Story = {
  args: {
    children: '클릭하세요',
    variant: 'default',
  },
}
```

---

## 🔍 SEO 최적화

### Metadata API (Next.js 16)
```typescript
// app/layout.tsx
import type { Metadata } from 'next'

export const metadata: Metadata = {
  title: {
    default: 'YouTube Creator Platform',
    template: '%s | YouTube Creator Platform',
  },
  description: 'AI 기반 YouTube 채널 성장 플랫폼',
  keywords: ['YouTube', '크리에이터', 'AI 스크립트', '채널 분석'],
  authors: [{ name: 'Your Company' }],
  openGraph: {
    title: 'YouTube Creator Platform',
    description: 'AI 기반 YouTube 채널 성장 플랫폼',
    url: 'https://yourplatform.com',
    siteName: 'YouTube Creator Platform',
    images: [
      {
        url: '/og-image.png',
        width: 1200,
        height: 630,
      },
    ],
    locale: 'ko_KR',
    type: 'website',
  },
  twitter: {
    card: 'summary_large_image',
    title: 'YouTube Creator Platform',
    description: 'AI 기반 YouTube 채널 성장 플랫폼',
    images: ['/og-image.png'],
  },
  icons: {
    icon: '/favicon.ico',
    apple: '/apple-touch-icon.png',
  },
}
```

---

## 🏗 프로젝트 구조

```
youtube-creator-saas/
├── app/
│   ├── (auth)/
│   │   ├── sign-in/
│   │   └── sign-up/
│   ├── (dashboard)/
│   │   ├── analytics/
│   │   ├── scripts/
│   │   ├── videos/
│   │   └── settings/
│   ├── api/
│   │   ├── analytics/
│   │   ├── scripts/
│   │   ├── payments/
│   │   └── webhooks/
│   ├── layout.tsx
│   └── page.tsx
├── components/
│   ├── ui/               # shadcn/ui components
│   ├── dashboard/
│   ├── analytics/
│   └── scripts/
├── lib/
│   ├── supabase.ts
│   ├── prisma.ts
│   ├── openrouter.ts
│   ├── clerk.ts
│   └── utils.ts
├── stores/
│   ├── userStore.ts
│   └── analyticsStore.ts
├── prisma/
│   └── schema.prisma
├── emails/
│   ├── welcome.tsx
│   └── subscription.tsx
├── public/
├── .env.local
├── next.config.js
├── tailwind.config.ts
└── package.json
```

---

## 📦 완전한 package.json

```json
{
  "name": "youtube-creator-saas",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "dev": "next dev --turbo",
    "build": "next build",
    "start": "next start",
    "lint": "next lint",
    "db:push": "prisma db push",
    "db:studio": "prisma studio",
    "storybook": "storybook dev -p 6006"
  },
  "dependencies": {
    "next": "^16.0.0",
    "react": "^19.0.0",
    "react-dom": "^19.0.0",
    "@clerk/nextjs": "^6.0.0",
    "@supabase/supabase-js": "^2.50.0",
    "@supabase/ssr": "^0.7.0",
    "@prisma/client": "^6.0.0",
    "@tanstack/react-query": "^5.0.0",
    "zustand": "^5.0.0",
    "openai": "^4.0.0",
    "react-hook-form": "^7.54.0",
    "@hookform/resolvers": "^3.10.0",
    "zod": "^3.24.0",
    "lucide-react": "^0.468.0",
    "framer-motion": "^12.0.0",
    "@radix-ui/react-dialog": "^1.1.0",
    "@radix-ui/react-dropdown-menu": "^2.1.0",
    "@radix-ui/react-select": "^2.1.0",
    "tailwindcss": "^4.0.0",
    "class-variance-authority": "^0.7.1",
    "clsx": "^2.1.0",
    "tailwind-merge": "^2.7.0",
    "date-fns": "^4.1.0",
    "es-toolkit": "^2.0.0",
    "ts-pattern": "^6.0.0",
    "resend": "^5.0.0",
    "solapi": "^5.0.0",
    "@tosspayments/payment-widget-sdk": "^1.0.0",
    "@sentry/nextjs": "^9.0.0",
    "posthog-js": "^1.200.0",
    "recharts": "^2.15.0"
  },
  "devDependencies": {
    "typescript": "^5.6.0",
    "@types/node": "^22.0.0",
    "@types/react": "^19.0.0",
    "@types/react-dom": "^19.0.0",
    "prisma": "^6.0.0",
    "eslint": "^9.0.0",
    "eslint-config-next": "^16.0.0",
    "prettier": "^3.4.0",
    "prettier-plugin-tailwindcss": "^0.6.0",
    "@storybook/react": "^9.0.0",
    "@storybook/nextjs": "^9.0.0"
  }
}
```

---

## 🌐 환경 변수 (.env.local)

```bash
# Database
DATABASE_URL="postgresql://user:password@host:5432/db"
DIRECT_URL="postgresql://user:password@host:5432/db"

# Supabase
NEXT_PUBLIC_SUPABASE_URL="https://xxx.supabase.co"
NEXT_PUBLIC_SUPABASE_ANON_KEY="eyJ..."
SUPABASE_SERVICE_ROLE_KEY="eyJ..."

# Clerk
NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY="pk_test_..."
CLERK_SECRET_KEY="sk_test_..."
NEXT_PUBLIC_CLERK_SIGN_IN_URL="/sign-in"
NEXT_PUBLIC_CLERK_SIGN_UP_URL="/sign-up"

# OpenRouter (LLM)
OPENROUTER_API_KEY="sk-or-..."
NEXT_PUBLIC_SITE_URL="https://yourplatform.com"

# YouTube API
YOUTUBE_API_KEY="AIza..."

# Toss Payments
NEXT_PUBLIC_TOSS_CLIENT_KEY="test_ck_..."
TOSS_SECRET_KEY="test_sk_..."

# SOLAPI (SMS)
SOLAPI_API_KEY="..."
SOLAPI_API_SECRET="..."
SOLAPI_SENDER_NUMBER="01012345678"
SOLAPI_KAKAO_PF_ID="..."

# Resend (Email)
RESEND_API_KEY="re_..."

# Sentry
NEXT_PUBLIC_SENTRY_DSN="https://..."

# PostHog
NEXT_PUBLIC_POSTHOG_KEY="phc_..."
NEXT_PUBLIC_POSTHOG_HOST="https://app.posthog.com"
```

---

## 🚦 시작 가이드

### 1. 프로젝트 생성
```bash
# Next.js 16 프로젝트 생성
npx create-next-app@latest youtube-creator-saas \
  --typescript \
  --tailwind \
  --app \
  --import-alias "@/*"

cd youtube-creator-saas
```

### 2. shadcn/ui 초기화
```bash
npx shadcn@latest init

# 필요한 컴포넌트 설치
npx shadcn@latest add button card dialog form input select
```

### 3. Prisma 초기화
```bash
npm install prisma @prisma/client
npx prisma init

# schema.prisma 작성 후
npx prisma migrate dev --name init
npx prisma generate
```

### 4. Clerk 설정
```bash
npm install @clerk/nextjs

# middleware.ts 생성 (위 예시 참조)
```

### 5. 개발 서버 실행
```bash
npm run dev --turbo
# http://localhost:3000
```

---

## 📚 참고 자료

### 공식 문서
- **Next.js 16**: https://nextjs.org/docs
- **Clerk**: https://clerk.com/docs
- **Supabase**: https://supabase.com/docs
- **Prisma**: https://www.prisma.io/docs
- **shadcn/ui**: https://ui.shadcn.com
- **OpenRouter**: https://openrouter.ai/docs
- **Toss Payments**: https://docs.tosspayments.com
- **SOLAPI**: https://docs.solapi.com
- **Resend**: https://resend.com/docs

### 추천 보일러플레이트
1. **ixartz/Next-js-Boilerplate**: https://github.com/ixartz/Next-js-Boilerplate
2. **Clerk + Supabase**: https://github.com/clerk/clerk-supabase-nextjs
3. **shadcn Taxonomy**: https://github.com/shadcn/taxonomy

---

## ✅ 기술스택 체크리스트

### 프레임워크
- [x] Next.js 16 (Turbopack, React 19.2)
- [x] TypeScript 5.6+

### UI
- [x] Tailwind CSS 4.0
- [x] shadcn/ui
- [x] Lucide Icons
- [x] Framer Motion

### 상태 관리
- [x] Zustand
- [x] TanStack Query v5

### 백엔드
- [x] Supabase (PostgreSQL, Auth, Storage)
- [x] Prisma ORM
- [x] Clerk (인증)

### AI/LLM
- [x] OpenRouter API

### 결제 & 알림
- [x] Toss Payments
- [x] SOLAPI (SMS/알림톡)
- [x] Resend (이메일)

### 인프라
- [x] Vercel
- [x] Sentry
- [x] PostHog

### 개발 도구
- [x] Zod
- [x] React Hook Form
- [x] date-fns
- [x] es-toolkit
- [x] ts-pattern
- [x] Storybook

---

**작성자**: Product & Engineering Team
**최종 검토**: 2025-11-04
