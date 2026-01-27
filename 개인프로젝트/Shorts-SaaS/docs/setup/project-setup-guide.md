# YouTube Creator Platform - 프로젝트 초기화 가이드

**작성일**: 2025-11-04
**대상**: 개발자 (Frontend, Backend, Fullstack)
**난이도**: 중급

---

## 📋 목차

1. [시작하기 전에](#시작하기-전에)
2. [프로젝트 생성](#프로젝트-생성)
3. [핵심 라이브러리 설치](#핵심-라이브러리-설치)
4. [환경 변수 설정](#환경-변수-설정)
5. [Supabase 설정](#supabase-설정)
6. [Clerk 인증 설정](#clerk-인증-설정)
7. [Prisma 데이터베이스 설정](#prisma-데이터베이스-설정)
8. [프로젝트 구조](#프로젝트-구조)
9. [개발 서버 실행](#개발-서버-실행)
10. [배포](#배포)

---

## 시작하기 전에

### 필수 요구사항

```bash
# Node.js 버전 확인 (20.9 이상 필요)
node --version  # v20.9.0 이상

# npm 버전 확인
npm --version   # v10.0.0 이상

# Git 설치 확인
git --version
```

### 필요한 계정
- [ ] GitHub 계정
- [ ] Vercel 계정 (https://vercel.com)
- [ ] Supabase 계정 (https://supabase.com)
- [ ] Clerk 계정 (https://clerk.com)
- [ ] OpenRouter 계정 (https://openrouter.ai)
- [ ] Toss Payments 계정 (https://tosspayments.com) - 테스트 모드 가능
- [ ] SOLAPI 계정 (https://solapi.com) - 선택
- [ ] Resend 계정 (https://resend.com)
- [ ] Sentry 계정 (https://sentry.io)
- [ ] PostHog 계정 (https://posthog.com) - 선택

---

## 프로젝트 생성

### 옵션 1: 신규 Next.js 16 프로젝트 생성 (권장)

```bash
# 1. Next.js 16 프로젝트 생성
npx create-next-app@latest youtube-creator-saas

# 설정 옵션 선택:
# ✅ Would you like to use TypeScript? Yes
# ✅ Would you like to use ESLint? Yes
# ✅ Would you like to use Tailwind CSS? Yes
# ✅ Would you like to use `src/` directory? No
# ✅ Would you like to use App Router? Yes
# ✅ Would you like to customize the default import alias? Yes
# ✅ What import alias would you like configured? @/*

cd youtube-creator-saas
```

### 옵션 2: Easynext CLI 사용 (한국 개발자 친화적)

```bash
# Easynext CLI 설치
npm install -g @easynext/cli

# 프로젝트 생성
easynext create youtube-creator-saas

cd youtube-creator-saas

# Supabase 초기화 (선택)
easynext init supabase

# Sentry 초기화 (선택)
easynext init sentry
```

### 옵션 3: 보일러플레이트 클론 (빠른 시작)

```bash
# ixartz Next.js Boilerplate (Next.js 16 지원)
git clone https://github.com/ixartz/Next-js-Boilerplate.git youtube-creator-saas

cd youtube-creator-saas

# Git 히스토리 초기화
rm -rf .git
git init
git add .
git commit -m "Initial commit"

npm install
```

---

## 핵심 라이브러리 설치

### 1. UI & 스타일링

```bash
# shadcn/ui 초기화
npx shadcn@latest init

# 설정:
# ✅ Would you like to use TypeScript? Yes
# ✅ Which style would you like to use? Default
# ✅ Which color would you like to use as base color? Slate
# ✅ Where is your global CSS file? app/globals.css
# ✅ Would you like to use CSS variables for colors? Yes
# ✅ Where is your tailwind.config.js located? tailwind.config.ts
# ✅ Configure the import alias for components: @/components
# ✅ Configure the import alias for utils: @/lib/utils

# 필수 컴포넌트 설치
npx shadcn@latest add button
npx shadcn@latest add card
npx shadcn@latest add dialog
npx shadcn@latest add form
npx shadcn@latest add input
npx shadcn@latest add select
npx shadcn@latest add table
npx shadcn@latest add tabs
npx shadcn@latest add dropdown-menu
npx shadcn@latest add avatar
npx shadcn@latest add badge
npx shadcn@latest add toast

# Lucide Icons, Framer Motion
npm install lucide-react framer-motion
```

### 2. 상태 관리 & 데이터 Fetching

```bash
npm install zustand @tanstack/react-query
npm install @tanstack/react-query-devtools --save-dev
```

### 3. 폼 & Validation

```bash
npm install react-hook-form @hookform/resolvers zod
```

### 4. 데이터베이스 & ORM

```bash
npm install prisma @prisma/client
npm install @supabase/supabase-js @supabase/ssr

npx prisma init
```

### 5. 인증

```bash
# Clerk (권장)
npm install @clerk/nextjs

# 또는 NextAuth.js v5
# npm install next-auth@beta @auth/prisma-adapter
```

### 6. AI/LLM

```bash
# OpenRouter (OpenAI SDK 사용)
npm install openai

# 또는 Vercel AI SDK
# npm install ai @ai-sdk/openai @ai-sdk/anthropic
```

### 7. 결제 & 통신

```bash
# Toss Payments
npm install @tosspayments/payment-widget-sdk

# SOLAPI (SMS/알림톡)
npm install solapi

# Resend (이메일)
npm install resend
```

### 8. 모니터링 & 분석

```bash
# Sentry
npm install @sentry/nextjs
npx @sentry/wizard@latest -i nextjs

# PostHog (선택)
npm install posthog-js
```

### 9. 유틸리티

```bash
npm install date-fns es-toolkit ts-pattern
```

### 10. Charts

```bash
npm install recharts
```

### 11. Dev Tools

```bash
# Storybook (선택)
npx storybook@latest init

# Prettier
npm install --save-dev prettier prettier-plugin-tailwindcss
```

---

## 환경 변수 설정

### 1. `.env.local` 파일 생성

```bash
cp .env.example .env.local
# 또는
touch .env.local
```

### 2. 환경 변수 작성

```bash
# ============================================
# DATABASE (Supabase)
# ============================================
DATABASE_URL="postgresql://postgres.xxx:[password]@aws-0-ap-northeast-2.pooler.supabase.com:6543/postgres?pgbouncer=true"
DIRECT_URL="postgresql://postgres.xxx:[password]@aws-0-ap-northeast-2.pooler.supabase.com:5432/postgres"

# ============================================
# SUPABASE
# ============================================
NEXT_PUBLIC_SUPABASE_URL="https://xxx.supabase.co"
NEXT_PUBLIC_SUPABASE_ANON_KEY="eyJ..."
SUPABASE_SERVICE_ROLE_KEY="eyJ..."

# ============================================
# AUTHENTICATION (Clerk)
# ============================================
NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY="pk_test_..."
CLERK_SECRET_KEY="sk_test_..."

# Clerk URLs
NEXT_PUBLIC_CLERK_SIGN_IN_URL="/sign-in"
NEXT_PUBLIC_CLERK_SIGN_UP_URL="/sign-up"
NEXT_PUBLIC_CLERK_AFTER_SIGN_IN_URL="/dashboard"
NEXT_PUBLIC_CLERK_AFTER_SIGN_UP_URL="/onboarding"

# ============================================
# AI/LLM (OpenRouter)
# ============================================
OPENROUTER_API_KEY="sk-or-v1-..."
NEXT_PUBLIC_SITE_URL="http://localhost:3000"  # Production: https://yourplatform.com

# ============================================
# YOUTUBE API
# ============================================
YOUTUBE_API_KEY="AIza..."

# Google OAuth (Clerk에서 자동 처리하지만 직접 호출 시 필요)
GOOGLE_CLIENT_ID="xxx.apps.googleusercontent.com"
GOOGLE_CLIENT_SECRET="GOCSPX-..."

# ============================================
# PAYMENTS (Toss Payments)
# ============================================
NEXT_PUBLIC_TOSS_CLIENT_KEY="test_ck_..."
TOSS_SECRET_KEY="test_sk_..."

# ============================================
# SMS/ALIMTALK (SOLAPI)
# ============================================
SOLAPI_API_KEY="..."
SOLAPI_API_SECRET="..."
SOLAPI_SENDER_NUMBER="01012345678"
SOLAPI_KAKAO_PF_ID="..."  # 카카오 알림톡 플러스친구 ID

# ============================================
# EMAIL (Resend)
# ============================================
RESEND_API_KEY="re_..."
RESEND_FROM_EMAIL="noreply@yourplatform.com"

# ============================================
# MONITORING (Sentry)
# ============================================
NEXT_PUBLIC_SENTRY_DSN="https://xxx@xxx.ingest.sentry.io/xxx"
SENTRY_ORG="your-org"
SENTRY_PROJECT="youtube-creator-platform"
SENTRY_AUTH_TOKEN="sntrys_..."

# ============================================
# ANALYTICS (PostHog)
# ============================================
NEXT_PUBLIC_POSTHOG_KEY="phc_..."
NEXT_PUBLIC_POSTHOG_HOST="https://app.posthog.com"

# ============================================
# VERCEL (자동 설정)
# ============================================
# VERCEL_URL (Vercel이 자동 설정)
# VERCEL_ENV (Vercel이 자동 설정)
```

---

## Supabase 설정

### 1. Supabase 프로젝트 생성

1. https://supabase.com/dashboard 접속
2. "New Project" 클릭
3. 프로젝트 정보 입력:
   - **Name**: youtube-creator-platform
   - **Database Password**: 강력한 비밀번호 (저장 필수!)
   - **Region**: Northeast Asia (Seoul)
4. "Create new project" 클릭 (약 2분 소요)

### 2. 환경 변수 복사

1. 프로젝트 대시보드에서 **Settings** > **API** 이동
2. **Project URL** 복사 → `NEXT_PUBLIC_SUPABASE_URL`
3. **Project API keys** > **anon public** 복사 → `NEXT_PUBLIC_SUPABASE_ANON_KEY`
4. **service_role** 복사 → `SUPABASE_SERVICE_ROLE_KEY` (⚠️ 비밀 유지!)

### 3. Database Connection String

1. **Settings** > **Database** 이동
2. **Connection string** > **URI** 선택
3. 비밀번호 입력 후 **Connection pooling** 활성화
4. **Pooler** 문자열 복사 → `DATABASE_URL`
5. **Direct connection** 문자열 복사 → `DIRECT_URL`

### 4. Storage 버킷 생성

```sql
-- Supabase SQL Editor에서 실행
-- Storage > New Bucket
```

1. **Storage** 메뉴 클릭
2. **New bucket** 클릭
3. 버킷 생성:
   - `avatars` (Public)
   - `thumbnails` (Public)
   - `scripts` (Private)

---

## Clerk 인증 설정

### 1. Clerk 프로젝트 생성

1. https://dashboard.clerk.com 접속
2. **Create application** 클릭
3. 애플리케이션 정보:
   - **Name**: YouTube Creator Platform
   - **Sign-in options**: Google ✅
4. **Create application** 클릭

### 2. Google OAuth 설정

1. Clerk Dashboard > **Configure** > **SSO Connections**
2. **Google** 클릭
3. **Enable Google** 토글 ON
4. **YouTube Scope 추가**:
   - Advanced Settings > **Custom scopes** 입력:
   ```
   https://www.googleapis.com/auth/youtube.readonly
   https://www.googleapis.com/auth/youtube.force-ssl
   ```

### 3. 환경 변수 복사

1. **API Keys** 메뉴 클릭
2. **Publishable key** 복사 → `NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY`
3. **Secret key** 복사 → `CLERK_SECRET_KEY`

### 4. Middleware 설정

**`middleware.ts` 생성**:

```typescript
import { clerkMiddleware, createRouteMatcher } from '@clerk/nextjs/server'

const isPublicRoute = createRouteMatcher([
  '/',
  '/sign-in(.*)',
  '/sign-up(.*)',
  '/api/webhooks(.*)',
])

export default clerkMiddleware(async (auth, request) => {
  if (!isPublicRoute(request)) {
    await auth.protect()
  }
})

export const config = {
  matcher: [
    // Skip Next.js internals and all static files
    '/((?!_next|[^?]*\\.(?:html?|css|js(?!on)|jpe?g|webp|png|gif|svg|ttf|woff2?|ico|csv|docx?|xlsx?|zip|webmanifest)).*)',
    // Always run for API routes
    '/(api|trpc)(.*)',
  ],
}
```

### 5. Layout 래퍼 추가

**`app/layout.tsx`**:

```typescript
import { ClerkProvider } from '@clerk/nextjs'
import './globals.css'

export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <ClerkProvider>
      <html lang="ko">
        <body>{children}</body>
      </html>
    </ClerkProvider>
  )
}
```

### 6. Clerk ↔ Supabase 연동

**`lib/supabase/client.ts`**:

```typescript
import { createBrowserClient } from '@supabase/ssr'
import { useAuth } from '@clerk/nextjs'

export function createClerkSupabaseClient() {
  const { getToken } = useAuth()

  return createBrowserClient(
    process.env.NEXT_PUBLIC_SUPABASE_URL!,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!,
    {
      global: {
        fetch: async (url, options = {}) => {
          const clerkToken = await getToken({
            template: 'supabase',
          })

          const headers = new Headers(options?.headers)
          headers.set('Authorization', `Bearer ${clerkToken}`)

          return fetch(url, {
            ...options,
            headers,
          })
        },
      },
    }
  )
}
```

---

## Prisma 데이터베이스 설정

### 1. Prisma 스키마 작성

**`prisma/schema.prisma`**:

```prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider  = "postgresql"
  url       = env("DATABASE_URL")
  directUrl = env("DIRECT_URL")
}

model User {
  id               String        @id @default(cuid())
  clerkId          String        @unique
  email            String        @unique
  name             String?
  imageUrl         String?
  subscriptionTier String        @default("free") // "free", "pro", "business"
  channels         Channel[]
  scripts          Script[]
  subscription     Subscription?
  createdAt        DateTime      @default(now())
  updatedAt        DateTime      @updatedAt

  @@map("users")
}

model Channel {
  id              String   @id @default(cuid())
  userId          String
  youtubeId       String   @unique
  title           String
  description     String?  @db.Text
  thumbnailUrl    String?
  subscriberCount Int      @default(0)
  videoCount      Int      @default(0)
  viewCount       BigInt   @default(0)
  accessToken     String?  @db.Text
  refreshToken    String?  @db.Text
  isPrimary       Boolean  @default(false)
  user            User     @relation(fields: [userId], references: [id], onDelete: Cascade)
  videos          Video[]
  analytics       ChannelAnalytics[]
  createdAt       DateTime @default(now())
  updatedAt       DateTime @updatedAt

  @@map("channels")
}

model Video {
  id           String   @id @default(cuid())
  channelId    String
  youtubeId    String   @unique
  title        String
  description  String?  @db.Text
  thumbnailUrl String?
  publishedAt  DateTime
  duration     Int?     // seconds
  viewCount    BigInt   @default(0)
  likeCount    Int      @default(0)
  commentCount Int      @default(0)
  tags         String[]
  category     String?
  channel      Channel  @relation(fields: [channelId], references: [id], onDelete: Cascade)
  createdAt    DateTime @default(now())
  updatedAt    DateTime @updatedAt

  @@map("videos")
}

model Script {
  id        String   @id @default(cuid())
  userId    String
  title     String
  content   String   @db.Text
  topic     String?
  videoType String?  // "short", "long"
  tone      String?  // "casual", "professional", "educational"
  language  String   @default("ko")
  wordCount Int?
  version   Int      @default(1)
  user      User     @relation(fields: [userId], references: [id], onDelete: Cascade)
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt

  @@map("scripts")
}

model ChannelAnalytics {
  id                    String   @id @default(cuid())
  channelId             String
  date                  DateTime @db.Date
  subscribersGained     Int      @default(0)
  subscribersLost       Int      @default(0)
  views                 BigInt   @default(0)
  watchTimeMinutes      BigInt   @default(0)
  averageViewDuration   Decimal  @default(0) @db.Decimal(10, 2)
  engagementRate        Decimal  @default(0) @db.Decimal(5, 2)
  channel               Channel  @relation(fields: [channelId], references: [id], onDelete: Cascade)
  createdAt             DateTime @default(now())

  @@unique([channelId, date])
  @@map("channel_analytics")
}

model Subscription {
  id                  String    @id @default(cuid())
  userId              String    @unique
  tossCustomerKey     String?   @unique
  tossBillingKey      String?   @unique
  tier                String    // "pro", "business"
  status              String    // "active", "canceled", "past_due"
  currentPeriodStart  DateTime
  currentPeriodEnd    DateTime
  cancelAtPeriodEnd   Boolean   @default(false)
  user                User      @relation(fields: [userId], references: [id], onDelete: Cascade)
  createdAt           DateTime  @default(now())
  updatedAt           DateTime  @updatedAt

  @@map("subscriptions")
}
```

### 2. Prisma 클라이언트 설정

**`lib/prisma.ts`**:

```typescript
import { PrismaClient } from '@prisma/client'

const globalForPrisma = globalThis as unknown as {
  prisma: PrismaClient | undefined
}

export const prisma =
  globalForPrisma.prisma ??
  new PrismaClient({
    log: process.env.NODE_ENV === 'development' ? ['query', 'error', 'warn'] : ['error'],
  })

if (process.env.NODE_ENV !== 'production') globalForPrisma.prisma = prisma
```

### 3. 마이그레이션 실행

```bash
# Prisma 마이그레이션 생성 및 적용
npx prisma migrate dev --name init

# Prisma Client 생성
npx prisma generate

# Prisma Studio 실행 (DB GUI)
npx prisma studio
```

---

## 프로젝트 구조

```
youtube-creator-saas/
├── app/
│   ├── (auth)/
│   │   ├── sign-in/
│   │   │   └── [[...sign-in]]/
│   │   │       └── page.tsx
│   │   └── sign-up/
│   │       └── [[...sign-up]]/
│   │           └── page.tsx
│   ├── (dashboard)/
│   │   ├── dashboard/
│   │   │   └── page.tsx
│   │   ├── analytics/
│   │   │   └── page.tsx
│   │   ├── scripts/
│   │   │   ├── page.tsx
│   │   │   ├── new/
│   │   │   │   └── page.tsx
│   │   │   └── [scriptId]/
│   │   │       └── page.tsx
│   │   ├── videos/
│   │   │   └── page.tsx
│   │   ├── settings/
│   │   │   └── page.tsx
│   │   └── layout.tsx
│   ├── api/
│   │   ├── analytics/
│   │   │   ├── channel/
│   │   │   │   └── route.ts
│   │   │   └── videos/
│   │   │       └── route.ts
│   │   ├── scripts/
│   │   │   ├── generate/
│   │   │   │   └── route.ts
│   │   │   └── translate/
│   │   │       └── route.ts
│   │   ├── payments/
│   │   │   ├── create-subscription/
│   │   │   │   └── route.ts
│   │   │   └── cancel-subscription/
│   │   │       └── route.ts
│   │   └── webhooks/
│   │       ├── clerk/
│   │       │   └── route.ts
│   │       └── toss/
│   │           └── route.ts
│   ├── globals.css
│   ├── layout.tsx
│   └── page.tsx
├── components/
│   ├── ui/               # shadcn/ui components
│   ├── dashboard/
│   │   ├── sidebar.tsx
│   │   ├── header.tsx
│   │   └── metric-card.tsx
│   ├── analytics/
│   │   ├── chart.tsx
│   │   └── stats-overview.tsx
│   ├── scripts/
│   │   ├── script-form.tsx
│   │   ├── script-editor.tsx
│   │   └── script-card.tsx
│   └── providers/
│       └── query-provider.tsx
├── lib/
│   ├── prisma.ts
│   ├── supabase/
│   │   ├── client.ts
│   │   └── server.ts
│   ├── openrouter.ts
│   ├── youtube.ts
│   ├── toss-payments.ts
│   └── utils.ts
├── hooks/
│   ├── use-user.ts
│   ├── use-analytics.ts
│   └── use-scripts.ts
├── stores/
│   ├── user-store.ts
│   └── analytics-store.ts
├── prisma/
│   ├── schema.prisma
│   └── migrations/
├── public/
│   ├── favicon.ico
│   └── og-image.png
├── .env.local
├── .env.example
├── .eslintrc.json
├── .gitignore
├── middleware.ts
├── next.config.js
├── package.json
├── postcss.config.js
├── tailwind.config.ts
└── tsconfig.json
```

---

## 개발 서버 실행

### 1. 개발 서버 시작

```bash
# Turbopack 사용 (5-10x 빠름)
npm run dev --turbo

# 또는 일반 모드
npm run dev
```

### 2. 브라우저에서 확인

```
http://localhost:3000
```

### 3. Prisma Studio 실행 (데이터베이스 GUI)

```bash
# 새 터미널에서
npx prisma studio

# http://localhost:5555
```

### 4. Storybook 실행 (컴포넌트 개발)

```bash
# Storybook이 설치된 경우
npm run storybook

# http://localhost:6006
```

---

## 배포

### Vercel 배포

#### 1. GitHub Repository 생성

```bash
# Git 초기화 (이미 했다면 생략)
git init
git add .
git commit -m "Initial commit"

# GitHub에 레포지토리 생성 후
git remote add origin https://github.com/your-username/youtube-creator-saas.git
git branch -M main
git push -u origin main
```

#### 2. Vercel 프로젝트 연결

1. https://vercel.com 접속
2. **Add New** > **Project** 클릭
3. GitHub 레포지토리 선택
4. **Import** 클릭

#### 3. 환경 변수 추가

1. **Environment Variables** 섹션에서 `.env.local`의 모든 변수 추가
2. **Deployment Protection** 설정 (선택)

#### 4. 배포

1. **Deploy** 클릭
2. 배포 완료 후 도메인 확인

#### 5. Production URL 업데이트

`.env.local` 및 Vercel 환경 변수에서:
```bash
NEXT_PUBLIC_SITE_URL="https://your-app.vercel.app"
```

Clerk Dashboard에서:
- **Allowed redirect URLs** 추가
- **Allowed origins** 추가

---

## 트러블슈팅

### Prisma 마이그레이션 오류

```bash
# Prisma 캐시 초기화
npx prisma generate --force
npx prisma migrate reset
npx prisma migrate dev
```

### Supabase 연결 오류

1. Supabase Dashboard > **Database** > **Connection Pooling** 확인
2. `DATABASE_URL`에 `?pgbouncer=true` 추가 확인
3. IP 화이트리스트 설정 (필요 시)

### Clerk 인증 오류

1. Clerk Dashboard > **Allowed redirect URLs** 확인
2. `http://localhost:3000` 및 프로덕션 URL 추가
3. YouTube API 스코프 확인

### Vercel 빌드 오류

```bash
# 로컬에서 프로덕션 빌드 테스트
npm run build

# 에러 확인 후 수정
```

---

## 다음 단계

- [ ] [기능 개발 가이드](./feature-development-guide.md) 참조
- [ ] [API 문서](../api/api-documentation.md) 작성
- [ ] [테스트 가이드](./testing-guide.md) 설정
- [ ] [CI/CD 파이프라인](./cicd-setup.md) 구축

---

**작성자**: Engineering Team
**최종 업데이트**: 2025-11-04
