# 배포 가이드 템플릿
## [프로젝트 이름] - [프로젝트 설명]

**문서 버전**: [버전]
**작성일**: [작성일]
**작성자**: [작성자]

---

## 📋 목차

1. [배포 전략 개요](#배포-전략-개요)
2. [프론트엔드 배포](#프론트엔드-배포)
3. [백엔드 배포](#백엔드-배포)
4. [데이터베이스 배포](#데이터베이스-배포)
5. [환경 변수 관리](#환경-변수-관리)
6. [CI/CD 파이프라인](#cicd-파이프라인)
7. [도메인 및 SSL 설정](#도메인-및-ssl-설정)
8. [모니터링 및 로깅](#모니터링-및-로깅)
9. [성능 최적화](#성능-최적화)
10. [보안 설정](#보안-설정)
11. [트러블슈팅](#트러블슈팅)

---

## 배포 전략 개요

### 🎯 배포 아키텍처

```
┌─────────────────────────────────────────────────────────────┐
│                    Deployment Architecture                   │
└─────────────────────────────────────────────────────────────┘

[사용자]
    │
    │ HTTPS
    │
    ▼
┌────────────────────────────────────────────────────────────┐
│                    CDN / Edge Network                       │
│              (Cloudflare / Vercel Edge)                     │
│  - 정적 파일 캐싱                                           │
│  - Edge Functions                                           │
│  - DDoS 방어                                                │
└────────────────────────┬───────────────────────────────────┘
                         │
         ┌───────────────┴───────────────┐
         │                               │
         ▼                               ▼
┌────────────────────┐         ┌────────────────────┐
│  Frontend          │         │  Backend           │
│  [플랫폼명]        │         │  [플랫폼명]        │
│                    │         │                    │
│  - React/Next.js   │         │  - Nest.js/API     │
│  - SSG/SSR         │         │  - 비즈니스 로직   │
│  - Static Assets   │         │  - 인증/권한       │
└────────┬───────────┘         └────────┬───────────┘
         │                               │
         └───────────────┬───────────────┘
                         │
                         ▼
         ┌───────────────────────────────┐
         │      Database & Storage        │
         │      [플랫폼명]                │
         │                                │
         │  - PostgreSQL / MongoDB        │
         │  - Redis (캐시)                │
         │  - File Storage (S3/R2)        │
         └────────────────────────────────┘
```

### 배포 전략 결정 가이드

**프로젝트 타입에 따른 플랫폼 선택:**

| 프로젝트 타입 | 프론트엔드 | 백엔드 | 데이터베이스 | 이유 |
|---------------|-----------|--------|--------------|------|
| **정적 사이트** | Cloudflare Pages | - | - | 무제한 무료, 빠른 CDN |
| **SSG/SSR (Next.js)** | Vercel | - | Supabase | Next.js 최적화, 간편한 배포 |
| **풀스택 (분리)** | Cloudflare Pages | Railway | Supabase | 무료 티어 최대 활용 |
| **풀스택 (통합)** | Vercel | Vercel Serverless | Supabase | 단일 플랫폼, 간편한 관리 |
| **백엔드 API** | - | Google Cloud Run | Cloud SQL | 서버리스, 사용량 기반 과금 |
| **대규모 트래픽** | Cloudflare Pages | Railway/Render | PostgreSQL | 확장성, 안정성 |

### 무료 티어 비교 (2025년 기준)

#### 프론트엔드 플랫폼 순위

**1위: Cloudflare Pages**
```
✅ 무제한 대역폭 (최대 장점)
✅ 500 빌드/월
✅ 100 커스텀 도메인
✅ 무제한 정적 요청
✅ Workers: 100K 요청/일
```

**2위: Vercel**
```
✅ 100GB 대역폭
✅ 무제한 배포
✅ 6,000 빌드 분
✅ 100GB-hours 서버리스 함수
✅ 100K 함수 요청/일
⚠️ Next.js 최적화 최고
```

**3위: Netlify**
```
⚠️ 20 빌드/월 (2025년 9월 축소됨)
✅ 100GB 대역폭
⚠️ 125K 서버리스 함수 요청/월
❌ 더 이상 적극 추천하지 않음
```

#### 백엔드 플랫폼 순위

**1위: Railway**
```
✅ 월 $5 크레딧 (~500시간)
✅ GitHub 자동 배포
✅ PostgreSQL, Redis 포함
✅ Sleep 없음 (항상 실행)
✅ Nest.js 공식 지원
```

**2위: Google Cloud Run**
```
✅ 월 2백만 요청 무료
✅ 180,000 vCPU-초
✅ 360,000 GiB-초 메모리
✅ 요청 없으면 완전 무료
⚠️ Cold Start 있음
```

**3위: Render**
```
✅ 750 인스턴스 시간/월
⚠️ 15분 idle 후 스핀다운
✅ 100GB 대역폭
⚠️ 무료 PostgreSQL (1GB, 30일 제한)
❌ 프로덕션 비추천
```

### 바이브코딩 추천 조합

**조합 1: 최대 무료 활용**
```
Frontend:  Cloudflare Pages (무제한 무료)
Backend:   Railway (월 $5 크레딧)
Database:  Supabase (500MB 무료)
Storage:   Cloudflare R2 (10GB 무료)
```

**조합 2: Next.js 최적화**
```
Frontend:  Vercel (Next.js 최적화)
Backend:   Vercel Serverless Functions
Database:  Supabase / Vercel Postgres
Storage:   Vercel Blob Storage
```

**조합 3: 서버리스 극대화**
```
Frontend:  Cloudflare Pages
Backend:   Google Cloud Run
Database:  Supabase / Firebase
Storage:   Google Cloud Storage
```

---

## 프론트엔드 배포

### Vercel 배포 가이드

#### 1. Vercel CLI 설치 및 로그인

```bash
# Vercel CLI 설치
npm install -g vercel

# 로그인
vercel login

# 프로젝트 초기화
vercel
```

#### 2. GitHub 자동 배포 설정

**`.github/workflows/vercel-deploy.yml`**:

```yaml
name: Vercel Production Deployment

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '22'

      - name: Install Vercel CLI
        run: npm install -g vercel

      - name: Pull Vercel Environment Information
        run: vercel pull --yes --environment=production --token=${{ secrets.VERCEL_TOKEN }}

      - name: Build Project Artifacts
        run: vercel build --prod --token=${{ secrets.VERCEL_TOKEN }}

      - name: Deploy to Vercel
        run: vercel deploy --prebuilt --prod --token=${{ secrets.VERCEL_TOKEN }}
```

#### 3. Vercel 프로젝트 설정

**`vercel.json`**:

```json
{
  "version": 2,
  "framework": "nextjs",
  "buildCommand": "npm run build",
  "devCommand": "npm run dev",
  "installCommand": "npm install",
  "outputDirectory": ".next",
  "regions": ["icn1", "hnd1"],
  "env": {
    "NEXT_PUBLIC_APP_URL": "@next-public-app-url",
    "DATABASE_URL": "@database-url"
  },
  "build": {
    "env": {
      "ENABLE_EXPERIMENTAL_COREPACK": "1"
    }
  },
  "functions": {
    "app/api/**/*.ts": {
      "maxDuration": 10
    }
  },
  "headers": [
    {
      "source": "/(.*)",
      "headers": [
        {
          "key": "X-Content-Type-Options",
          "value": "nosniff"
        },
        {
          "key": "X-Frame-Options",
          "value": "DENY"
        },
        {
          "key": "X-XSS-Protection",
          "value": "1; mode=block"
        }
      ]
    }
  ],
  "rewrites": [
    {
      "source": "/api/:path*",
      "destination": "https://api.example.com/:path*"
    }
  ]
}
```

#### 4. 환경 변수 설정

```bash
# Vercel 대시보드에서 설정하거나 CLI 사용
vercel env add DATABASE_URL production
vercel env add NEXT_PUBLIC_API_URL production
vercel env add AUTH_SECRET production

# 환경 변수 확인
vercel env ls
```

#### 5. 배포 및 검증

```bash
# 프로덕션 배포
vercel --prod

# 배포 상태 확인
vercel inspect [deployment-url]

# 로그 확인
vercel logs [deployment-url]
```

---

### Cloudflare Pages 배포 가이드

#### 1. Cloudflare Pages 프로젝트 생성

**Wrangler CLI 설치**:

```bash
# Wrangler 설치
npm install -g wrangler

# Cloudflare 로그인
wrangler login

# Pages 프로젝트 생성
wrangler pages project create [project-name]
```

#### 2. GitHub 연동 배포

**GitHub Actions 설정 (`.github/workflows/cloudflare-deploy.yml`)**:

```yaml
name: Deploy to Cloudflare Pages

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      deployments: write

    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '22'

      - name: Install dependencies
        run: npm ci

      - name: Build
        run: npm run build

      - name: Publish to Cloudflare Pages
        uses: cloudflare/pages-action@v1
        with:
          apiToken: ${{ secrets.CLOUDFLARE_API_TOKEN }}
          accountId: ${{ secrets.CLOUDFLARE_ACCOUNT_ID }}
          projectName: [project-name]
          directory: dist
          gitHubToken: ${{ secrets.GITHUB_TOKEN }}
```

#### 3. Cloudflare Workers (Edge Functions)

**`functions/api/hello.ts`**:

```typescript
// Cloudflare Pages Functions
export async function onRequest(context: {
  request: Request
  env: any
  params: any
}) {
  const { request, env } = context

  // CORS 헤더
  const corsHeaders = {
    'Access-Control-Allow-Origin': '*',
    'Access-Control-Allow-Methods': 'GET, POST, OPTIONS',
    'Access-Control-Allow-Headers': 'Content-Type',
  }

  // OPTIONS 요청 처리
  if (request.method === 'OPTIONS') {
    return new Response(null, { headers: corsHeaders })
  }

  // API 로직
  try {
    const data = {
      message: 'Hello from Cloudflare Pages Functions!',
      timestamp: new Date().toISOString(),
    }

    return new Response(JSON.stringify(data), {
      headers: {
        'Content-Type': 'application/json',
        ...corsHeaders,
      },
    })
  } catch (error) {
    return new Response(
      JSON.stringify({ error: 'Internal Server Error' }),
      {
        status: 500,
        headers: { 'Content-Type': 'application/json' },
      }
    )
  }
}
```

#### 4. 환경 변수 설정

```bash
# Wrangler로 환경 변수 추가
wrangler pages secret put DATABASE_URL --project-name=[project-name]
wrangler pages secret put API_KEY --project-name=[project-name]

# 환경 변수 목록 확인
wrangler pages secret list --project-name=[project-name]
```

#### 5. Custom Domain 설정

```bash
# Cloudflare 대시보드에서 도메인 추가
# 또는 Wrangler CLI 사용
wrangler pages domain add example.com --project-name=[project-name]
```

---

### Netlify 배포 가이드

#### 1. Netlify CLI 설치

```bash
# Netlify CLI 설치
npm install -g netlify-cli

# 로그인
netlify login

# 프로젝트 초기화
netlify init
```

#### 2. Netlify 설정 파일

**`netlify.toml`**:

```toml
[build]
  command = "npm run build"
  publish = "dist"
  functions = "netlify/functions"

[build.environment]
  NODE_VERSION = "22"
  NPM_VERSION = "10"

[[redirects]]
  from = "/api/*"
  to = "/.netlify/functions/:splat"
  status = 200

[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200

[[headers]]
  for = "/*"
  [headers.values]
    X-Frame-Options = "DENY"
    X-XSS-Protection = "1; mode=block"
    X-Content-Type-Options = "nosniff"
    Referrer-Policy = "strict-origin-when-cross-origin"

[[headers]]
  for = "/static/*"
  [headers.values]
    Cache-Control = "public, max-age=31536000, immutable"

[functions]
  directory = "netlify/functions"
  node_bundler = "esbuild"
```

#### 3. Netlify Functions 예제

**`netlify/functions/hello.ts`**:

```typescript
import { Handler } from '@netlify/functions'

export const handler: Handler = async (event, context) => {
  // CORS 헤더
  const headers = {
    'Access-Control-Allow-Origin': '*',
    'Access-Control-Allow-Headers': 'Content-Type',
    'Content-Type': 'application/json',
  }

  // OPTIONS 요청 처리
  if (event.httpMethod === 'OPTIONS') {
    return {
      statusCode: 204,
      headers,
      body: '',
    }
  }

  try {
    // API 로직
    const data = {
      message: 'Hello from Netlify Functions!',
      timestamp: new Date().toISOString(),
      path: event.path,
      method: event.httpMethod,
    }

    return {
      statusCode: 200,
      headers,
      body: JSON.stringify(data),
    }
  } catch (error) {
    return {
      statusCode: 500,
      headers,
      body: JSON.stringify({ error: 'Internal Server Error' }),
    }
  }
}
```

#### 4. 환경 변수 설정

```bash
# CLI로 환경 변수 설정
netlify env:set DATABASE_URL "your-database-url"
netlify env:set API_KEY "your-api-key"

# 환경 변수 목록 확인
netlify env:list
```

#### 5. 배포

```bash
# 빌드 및 배포
netlify deploy --prod

# 배포 상태 확인
netlify status

# 로그 확인
netlify functions:log hello
```

---

## 백엔드 배포

### Railway 배포 가이드 (Nest.js)

#### 1. Railway CLI 설치 및 로그인

```bash
# Railway CLI 설치 (macOS)
brew install railway

# 또는 npm
npm install -g @railway/cli

# 로그인
railway login

# 프로젝트 초기화
railway init
```

#### 2. Nest.js 프로젝트 설정

**`railway.json`**:

```json
{
  "$schema": "https://railway.app/railway.schema.json",
  "build": {
    "builder": "NIXPACKS",
    "buildCommand": "npm run build"
  },
  "deploy": {
    "startCommand": "npm run start:prod",
    "restartPolicyType": "ON_FAILURE",
    "restartPolicyMaxRetries": 10
  }
}
```

**`Procfile`** (선택사항):

```
web: npm run start:prod
```

#### 3. PostgreSQL 추가

```bash
# PostgreSQL 데이터베이스 추가
railway add -d postgres

# 환경 변수 자동 설정됨:
# DATABASE_URL
# PGHOST
# PGPORT
# PGUSER
# PGPASSWORD
# PGDATABASE
```

#### 4. 환경 변수 설정

```bash
# 환경 변수 추가
railway variables set PORT=3000
railway variables set NODE_ENV=production
railway variables set JWT_SECRET=your-secret-key

# 환경 변수 확인
railway variables
```

#### 5. 배포

```bash
# Railway에 배포
railway up

# 도메인 생성
railway domain

# 로그 확인
railway logs

# 서비스 상태 확인
railway status
```

#### 6. GitHub 자동 배포 설정

**Railway 대시보드에서 GitHub 연동**:
1. Railway 대시보드 → 프로젝트 선택
2. Settings → Deploy → Connect GitHub Repository
3. main 브랜치 선택
4. 자동 배포 활성화

---

### Google Cloud Run 배포 가이드

#### 1. Google Cloud CLI 설치

```bash
# gcloud CLI 설치 (macOS)
brew install --cask google-cloud-sdk

# 로그인
gcloud auth login

# 프로젝트 설정
gcloud config set project [PROJECT_ID]

# Artifact Registry 인증
gcloud auth configure-docker [REGION]-docker.pkg.dev
```

#### 2. Dockerfile 작성

**`Dockerfile`**:

```dockerfile
# Multi-stage build for Nest.js
FROM node:22-alpine AS builder

WORKDIR /app

# 의존성 설치
COPY package*.json ./
RUN npm ci

# 소스 복사 및 빌드
COPY . .
RUN npm run build

# Production stage
FROM node:22-alpine AS runner

WORKDIR /app

ENV NODE_ENV=production

# 필요한 파일만 복사
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/package.json ./package.json

# 비-root 사용자로 실행
RUN addgroup -g 1001 -S nodejs
RUN adduser -S nestjs -u 1001
USER nestjs

EXPOSE 8080

CMD ["node", "dist/main"]
```

**`.dockerignore`**:

```
node_modules
npm-debug.log
dist
.git
.gitignore
README.md
.env
.env.local
```

#### 3. Cloud Run 배포

```bash
# Artifact Registry에 이미지 빌드 및 푸시
gcloud builds submit --tag [REGION]-docker.pkg.dev/[PROJECT_ID]/[REPOSITORY]/[IMAGE_NAME]

# Cloud Run에 배포
gcloud run deploy [SERVICE_NAME] \
  --image [REGION]-docker.pkg.dev/[PROJECT_ID]/[REPOSITORY]/[IMAGE_NAME] \
  --platform managed \
  --region [REGION] \
  --allow-unauthenticated \
  --set-env-vars DATABASE_URL=[DATABASE_URL] \
  --set-env-vars NODE_ENV=production \
  --max-instances 10 \
  --min-instances 0 \
  --memory 512Mi \
  --cpu 1 \
  --timeout 300 \
  --port 8080

# 서비스 URL 확인
gcloud run services describe [SERVICE_NAME] --region [REGION]
```

#### 4. CI/CD 파이프라인 (Cloud Build)

**`cloudbuild.yaml`**:

```yaml
steps:
  # Build the container image
  - name: 'gcr.io/cloud-builders/docker'
    args:
      - 'build'
      - '-t'
      - '[REGION]-docker.pkg.dev/$PROJECT_ID/[REPOSITORY]/[IMAGE_NAME]:$COMMIT_SHA'
      - '-t'
      - '[REGION]-docker.pkg.dev/$PROJECT_ID/[REPOSITORY]/[IMAGE_NAME]:latest'
      - '.'

  # Push the container image to Artifact Registry
  - name: 'gcr.io/cloud-builders/docker'
    args:
      - 'push'
      - '--all-tags'
      - '[REGION]-docker.pkg.dev/$PROJECT_ID/[REPOSITORY]/[IMAGE_NAME]'

  # Deploy to Cloud Run
  - name: 'gcr.io/google.com/cloudsdktool/cloud-sdk'
    entrypoint: gcloud
    args:
      - 'run'
      - 'deploy'
      - '[SERVICE_NAME]'
      - '--image'
      - '[REGION]-docker.pkg.dev/$PROJECT_ID/[REPOSITORY]/[IMAGE_NAME]:$COMMIT_SHA'
      - '--region'
      - '[REGION]'
      - '--platform'
      - 'managed'
      - '--allow-unauthenticated'

images:
  - '[REGION]-docker.pkg.dev/$PROJECT_ID/[REPOSITORY]/[IMAGE_NAME]:$COMMIT_SHA'
  - '[REGION]-docker.pkg.dev/$PROJECT_ID/[REPOSITORY]/[IMAGE_NAME]:latest'
```

#### 5. GitHub Actions 연동

**`.github/workflows/gcp-deploy.yml`**:

```yaml
name: Deploy to Cloud Run

on:
  push:
    branches:
      - main

env:
  PROJECT_ID: ${{ secrets.GCP_PROJECT_ID }}
  REGION: asia-northeast3
  SERVICE_NAME: [service-name]
  IMAGE_NAME: [image-name]

jobs:
  deploy:
    runs-on: ubuntu-latest

    permissions:
      contents: read
      id-token: write

    steps:
      - uses: actions/checkout@v4

      - id: 'auth'
        uses: 'google-github-actions/auth@v2'
        with:
          workload_identity_provider: ${{ secrets.WIF_PROVIDER }}
          service_account: ${{ secrets.WIF_SERVICE_ACCOUNT }}

      - name: 'Set up Cloud SDK'
        uses: 'google-github-actions/setup-gcloud@v2'

      - name: 'Docker auth'
        run: gcloud auth configure-docker ${{ env.REGION }}-docker.pkg.dev

      - name: 'Build and push container'
        run: |
          docker build -t ${{ env.REGION }}-docker.pkg.dev/${{ env.PROJECT_ID }}/cloud-run-source-deploy/${{ env.IMAGE_NAME }}:${{ github.sha }} .
          docker push ${{ env.REGION }}-docker.pkg.dev/${{ env.PROJECT_ID }}/cloud-run-source-deploy/${{ env.IMAGE_NAME }}:${{ github.sha }}

      - name: 'Deploy to Cloud Run'
        run: |
          gcloud run deploy ${{ env.SERVICE_NAME }} \
            --image ${{ env.REGION }}-docker.pkg.dev/${{ env.PROJECT_ID }}/cloud-run-source-deploy/${{ env.IMAGE_NAME }}:${{ github.sha }} \
            --region ${{ env.REGION }} \
            --platform managed \
            --allow-unauthenticated
```

---

### Render 배포 가이드

#### 1. Render 설정 파일

**`render.yaml`**:

```yaml
services:
  - type: web
    name: [service-name]
    runtime: node
    buildCommand: npm install && npm run build
    startCommand: npm run start:prod
    envVars:
      - key: NODE_ENV
        value: production
      - key: DATABASE_URL
        fromDatabase:
          name: [database-name]
          property: connectionString
      - key: PORT
        value: 10000
    healthCheckPath: /health
    autoDeploy: true

databases:
  - name: [database-name]
    databaseName: [db-name]
    user: [db-user]
    plan: free
```

#### 2. GitHub 연동 배포

1. Render 대시보드 → New Web Service
2. GitHub 저장소 연결
3. Build Command: `npm install && npm run build`
4. Start Command: `npm run start:prod`
5. 환경 변수 설정
6. Create Web Service

#### 3. Health Check 엔드포인트

**`src/health/health.controller.ts`**:

```typescript
import { Controller, Get } from '@nestjs/common'

@Controller('health')
export class HealthController {
  @Get()
  check() {
    return {
      status: 'ok',
      timestamp: new Date().toISOString(),
      uptime: process.uptime(),
    }
  }
}
```

---

## 데이터베이스 배포

### Supabase 배포 가이드

#### 1. Supabase 프로젝트 생성

```bash
# Supabase CLI 설치
npm install -g supabase

# 로그인
supabase login

# 프로젝트 초기화
supabase init

# 로컬 개발 환경 시작
supabase start
```

#### 2. 데이터베이스 마이그레이션

**마이그레이션 파일 생성**:

```bash
# 마이그레이션 파일 생성
supabase migration new create_users_table

# 마이그레이션 실행
supabase db push

# 원격 프로덕션 DB에 마이그레이션
supabase db push --linked
```

**마이그레이션 파일 예시 (`supabase/migrations/[timestamp]_create_users_table.sql`)**:

```sql
-- Create users table
CREATE TABLE IF NOT EXISTS public.users (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  email TEXT UNIQUE NOT NULL,
  name TEXT,
  avatar_url TEXT,
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- Enable Row Level Security
ALTER TABLE public.users ENABLE ROW LEVEL SECURITY;

-- RLS 정책: 사용자는 자신의 데이터만 조회 가능
CREATE POLICY "Users can view own data"
  ON public.users
  FOR SELECT
  USING (auth.uid() = id);

-- RLS 정책: 사용자는 자신의 데이터만 수정 가능
CREATE POLICY "Users can update own data"
  ON public.users
  FOR UPDATE
  USING (auth.uid() = id);

-- Indexes
CREATE INDEX users_email_idx ON public.users(email);
CREATE INDEX users_created_at_idx ON public.users(created_at DESC);

-- Trigger for updated_at
CREATE OR REPLACE FUNCTION update_updated_at_column()
RETURNS TRIGGER AS $$
BEGIN
  NEW.updated_at = NOW();
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER update_users_updated_at
BEFORE UPDATE ON public.users
FOR EACH ROW
EXECUTE FUNCTION update_updated_at_column();
```

#### 3. 환경 변수 설정

```bash
# .env.local
NEXT_PUBLIC_SUPABASE_URL=https://[project-id].supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=[anon-key]
SUPABASE_SERVICE_ROLE_KEY=[service-role-key]

# Database URL (Server Actions)
DATABASE_URL=postgresql://postgres:[password]@db.[project-id].supabase.co:5432/postgres
```

#### 4. Storage 설정

```sql
-- Create storage bucket
INSERT INTO storage.buckets (id, name, public)
VALUES ('avatars', 'avatars', true);

-- Storage RLS 정책
CREATE POLICY "Avatar images are publicly accessible"
  ON storage.objects FOR SELECT
  USING (bucket_id = 'avatars');

CREATE POLICY "Users can upload own avatar"
  ON storage.objects FOR INSERT
  WITH CHECK (
    bucket_id = 'avatars' AND
    auth.uid()::text = (storage.foldername(name))[1]
  );
```

#### 5. Edge Functions 배포

```bash
# Edge Function 생성
supabase functions new hello

# 로컬 테스트
supabase functions serve hello

# 배포
supabase functions deploy hello

# 환경 변수 설정
supabase secrets set MY_SECRET=value

# 함수 로그 확인
supabase functions logs hello
```

**Edge Function 예시 (`supabase/functions/hello/index.ts`)**:

```typescript
import { serve } from 'https://deno.land/std@0.168.0/http/server.ts'
import { createClient } from 'https://esm.sh/@supabase/supabase-js@2'

serve(async (req) => {
  const { url, method } = req

  // CORS 헤더
  const corsHeaders = {
    'Access-Control-Allow-Origin': '*',
    'Access-Control-Allow-Headers':
      'authorization, x-client-info, apikey, content-type',
  }

  // OPTIONS 요청 처리
  if (method === 'OPTIONS') {
    return new Response('ok', { headers: corsHeaders })
  }

  try {
    // Supabase 클라이언트 생성
    const supabaseClient = createClient(
      Deno.env.get('SUPABASE_URL') ?? '',
      Deno.env.get('SUPABASE_ANON_KEY') ?? '',
      {
        global: {
          headers: { Authorization: req.headers.get('Authorization')! },
        },
      }
    )

    // 인증 확인
    const {
      data: { user },
    } = await supabaseClient.auth.getUser()

    if (!user) {
      throw new Error('Unauthorized')
    }

    // 비즈니스 로직
    const data = {
      message: 'Hello from Supabase Edge Function!',
      user: user.email,
      timestamp: new Date().toISOString(),
    }

    return new Response(JSON.stringify(data), {
      headers: { ...corsHeaders, 'Content-Type': 'application/json' },
      status: 200,
    })
  } catch (error) {
    return new Response(JSON.stringify({ error: error.message }), {
      headers: { ...corsHeaders, 'Content-Type': 'application/json' },
      status: 400,
    })
  }
})
```

---

### Vercel Postgres 배포 가이드

#### 1. Vercel Postgres 생성

```bash
# Vercel CLI로 Postgres 생성
vercel postgres create [database-name]

# 환경 변수 자동 연결
vercel env pull .env.local
```

#### 2. Prisma 설정

**`prisma/schema.prisma`**:

```prisma
datasource db {
  provider = "postgresql"
  url = env("POSTGRES_PRISMA_URL") // uses connection pooling
  directUrl = env("POSTGRES_URL_NON_POOLING") // uses direct connection
}

generator client {
  provider = "prisma-client-js"
}

model User {
  id        String   @id @default(cuid())
  email     String   @unique
  name      String?
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt

  @@index([email])
}
```

#### 3. 마이그레이션

```bash
# Prisma 마이그레이션 생성
npx prisma migrate dev --name init

# 프로덕션 마이그레이션 적용
npx prisma migrate deploy

# Prisma Studio 실행
npx prisma studio
```

---

## 환경 변수 관리

### 환경 변수 구조

```bash
# .env.example (Git에 커밋)
# Database
DATABASE_URL=
POSTGRES_PRISMA_URL=
POSTGRES_URL_NON_POOLING=

# Authentication
NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_ANON_KEY=
SUPABASE_SERVICE_ROLE_KEY=

# Payment
LEMON_SQUEEZY_API_KEY=
LEMON_SQUEEZY_WEBHOOK_SECRET=
TOSS_PAYMENTS_CLIENT_KEY=
TOSS_PAYMENTS_SECRET_KEY=

# App
NEXT_PUBLIC_APP_URL=http://localhost:3000
NODE_ENV=development

# Monitoring
SENTRY_DSN=
NEXT_PUBLIC_SENTRY_DSN=
```

### 환경 변수 검증

**`lib/env.ts`**:

```typescript
import { z } from 'zod'

const envSchema = z.object({
  // Database
  DATABASE_URL: z.string().url(),

  // Authentication
  NEXT_PUBLIC_SUPABASE_URL: z.string().url(),
  NEXT_PUBLIC_SUPABASE_ANON_KEY: z.string().min(1),
  SUPABASE_SERVICE_ROLE_KEY: z.string().min(1),

  // App
  NEXT_PUBLIC_APP_URL: z.string().url(),
  NODE_ENV: z.enum(['development', 'production', 'test']),

  // Monitoring (optional)
  SENTRY_DSN: z.string().url().optional(),
})

export const env = envSchema.parse(process.env)
```

### 플랫폼별 환경 변수 설정

**Vercel**:
```bash
vercel env add DATABASE_URL production
vercel env add DATABASE_URL preview
vercel env add DATABASE_URL development
```

**Railway**:
```bash
railway variables set DATABASE_URL=value
railway variables set --environment production NODE_ENV=production
```

**Cloud Run**:
```bash
gcloud run services update [SERVICE_NAME] \
  --update-env-vars DATABASE_URL=value,NODE_ENV=production
```

---

## CI/CD 파이프라인

### GitHub Actions 전체 워크플로우

**`.github/workflows/ci-cd.yml`**:

```yaml
name: CI/CD Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main, develop]

env:
  NODE_VERSION: '22'

jobs:
  # Lint & Type Check
  lint:
    name: Lint & Type Check
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Run linter
        run: npm run lint

      - name: Run type check
        run: npm run type-check

  # Unit Tests
  test:
    name: Unit Tests
    runs-on: ubuntu-latest
    needs: lint
    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Run tests
        run: npm run test:ci

      - name: Upload coverage
        uses: codecov/codecov-action@v4
        with:
          token: ${{ secrets.CODECOV_TOKEN }}

  # Build
  build:
    name: Build Application
    runs-on: ubuntu-latest
    needs: [lint, test]
    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Build
        run: npm run build
        env:
          NEXT_PUBLIC_APP_URL: ${{ secrets.NEXT_PUBLIC_APP_URL }}

      - name: Upload build artifacts
        uses: actions/upload-artifact@v4
        with:
          name: build
          path: .next

  # E2E Tests
  e2e:
    name: E2E Tests
    runs-on: ubuntu-latest
    needs: build
    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps

      - name: Download build artifacts
        uses: actions/download-artifact@v4
        with:
          name: build
          path: .next

      - name: Run E2E tests
        run: npm run test:e2e

      - name: Upload test results
        if: always()
        uses: actions/upload-artifact@v4
        with:
          name: playwright-report
          path: playwright-report

  # Deploy to Production (Vercel)
  deploy-vercel:
    name: Deploy to Vercel
    runs-on: ubuntu-latest
    needs: [build, e2e]
    if: github.ref == 'refs/heads/main'
    steps:
      - uses: actions/checkout@v4

      - name: Deploy to Vercel
        uses: amondnet/vercel-action@v25
        with:
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
          vercel-org-id: ${{ secrets.VERCEL_ORG_ID }}
          vercel-project-id: ${{ secrets.VERCEL_PROJECT_ID }}
          vercel-args: '--prod'

  # Deploy Backend (Railway)
  deploy-railway:
    name: Deploy Backend to Railway
    runs-on: ubuntu-latest
    needs: [build, e2e]
    if: github.ref == 'refs/heads/main'
    steps:
      - uses: actions/checkout@v4

      - name: Install Railway CLI
        run: npm install -g @railway/cli

      - name: Deploy to Railway
        run: railway up --service backend
        env:
          RAILWAY_TOKEN: ${{ secrets.RAILWAY_TOKEN }}

  # Deploy Database Migrations
  migrate:
    name: Run Database Migrations
    runs-on: ubuntu-latest
    needs: [deploy-vercel, deploy-railway]
    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Run migrations
        run: npm run migrate:deploy
        env:
          DATABASE_URL: ${{ secrets.DATABASE_URL }}

  # Smoke Tests (Production)
  smoke-test:
    name: Production Smoke Tests
    runs-on: ubuntu-latest
    needs: [deploy-vercel, migrate]
    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Run smoke tests
        run: npm run test:smoke
        env:
          BASE_URL: ${{ secrets.PRODUCTION_URL }}
```

---

### 배포 설정 가이드

#### GitHub Actions 권한 설정 (필수!)

**Repository Settings에서 권한 설정**:

```bash
# GitHub 저장소 페이지에서:
Settings > Actions > General > Workflow permissions

# 다음 옵션 선택:
✅ "Read and write permissions" 선택
✅ "Allow GitHub Actions to create and approve pull requests" 체크
```

**권한 설정이 필요한 이유**:
- GitHub Actions가 코드를 체크아웃하고 아티팩트를 업로드하려면 쓰기 권한 필요
- 배포 결과를 저장하고 로그를 작성하려면 쓰기 권한 필요
- Pull Request에 자동으로 배포 미리보기를 추가하려면 PR 생성/승인 권한 필요

**권한 설정 단계별 가이드**:

1. GitHub 저장소 페이지 접속
2. **Settings** 탭 클릭
3. 왼쪽 사이드바에서 **Actions** → **General** 클릭
4. 하단의 **Workflow permissions** 섹션 찾기
5. **"Read and write permissions"** 라디오 버튼 선택
6. **"Allow GitHub Actions to create and approve pull requests"** 체크박스 활성화
7. **Save** 버튼 클릭

---

### 배포 문제 해결

#### 문제 1: GitHub Actions 권한 오류

**증상**:
```
Error: Resource not accessible by integration
403: Resource not accessible by integration
```

**원인**:
- GitHub Actions의 워크플로우 권한이 "Read repository contents and packages permissions"로 제한되어 있음
- 배포 작업에 필요한 쓰기 권한이 없음

**해결 방법**:

**방법 1: Repository 설정 변경 (권장)**

```bash
# Repository Settings에서 설정 (위의 "GitHub Actions 권한 설정" 참조)
Settings > Actions > General > Workflow permissions
→ "Read and write permissions" 선택
```

**방법 2: 워크플로우에 명시적 권한 추가**

```yaml
# .github/workflows/deploy.yml
name: Deploy

on:
  push:
    branches: [main]

# 명시적 권한 추가
permissions:
  contents: write        # 코드 읽기/쓰기
  deployments: write     # 배포 생성/업데이트
  pages: write           # GitHub Pages 쓰기 (필요시)
  id-token: write        # OIDC 토큰 (필요시)
  pull-requests: write   # PR 코멘트 작성 (필요시)

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      # ... 배포 단계 ...
```

**방법 3: Personal Access Token 사용**

```yaml
# .github/workflows/deploy.yml
steps:
  - uses: actions/checkout@v4
    with:
      token: ${{ secrets.PERSONAL_ACCESS_TOKEN }}  # PAT 사용

  - name: Deploy with PAT
    run: |
      # PAT를 사용한 배포 명령
    env:
      GITHUB_TOKEN: ${{ secrets.PERSONAL_ACCESS_TOKEN }}
```

---

#### 문제 2: 배포 상태 확인

**GitHub CLI를 사용한 배포 상태 확인**:

```bash
# GitHub Actions 실행 목록 확인 (최근 5개)
gh run list --limit 5

# 예시 출력:
# STATUS  TITLE                   WORKFLOW  BRANCH  EVENT  ID          ELAPSED  AGE
# ✓       Deploy to Production    CI/CD     main    push   1234567890  2m 30s   1h
# *       Build and Test          CI/CD     main    push   1234567889  1m 15s   2h

# 특정 실행의 상세 로그 확인
gh run view <run-id>

# 또는 최근 실행 로그 확인
gh run view --log

# 실행 중인 작업 로그 실시간 확인
gh run watch

# 실패한 작업만 필터링
gh run list --status failure --limit 10
```

**GitHub Pages 배포 상태 확인**:

```bash
# GitHub Pages 상태 확인
gh api repos/username/repository-name/pages

# 예시 출력:
# {
#   "url": "https://username.github.io/repository-name/",
#   "status": "built",
#   "cname": null,
#   "custom_404": false,
#   "html_url": "https://username.github.io/repository-name/",
#   "build_type": "workflow",
#   "source": {
#     "branch": "gh-pages",
#     "path": "/"
#   }
# }

# 배포된 사이트 HTTP 상태 확인
curl -I "https://username.github.io/repository-name/"

# 배포된 사이트 내용 미리보기 (첫 20줄)
curl -s "https://username.github.io/repository-name/" | head -20
```

**Vercel 배포 상태 확인**:

```bash
# Vercel CLI로 배포 목록 확인
vercel ls

# 최근 배포 상태 확인
vercel inspect

# 배포 로그 확인
vercel logs [deployment-url]

# 프로덕션 URL 확인
vercel domains ls
```

**Railway 배포 상태 확인**:

```bash
# Railway 서비스 상태 확인
railway status

# 배포 로그 실시간 확인
railway logs

# 환경 변수 확인
railway variables

# 서비스 URL 확인
railway domain
```

**Google Cloud Run 배포 상태 확인**:

```bash
# Cloud Run 서비스 목록
gcloud run services list

# 특정 서비스 상세 정보
gcloud run services describe [SERVICE_NAME] --region [REGION]

# 최근 배포 리비전 확인
gcloud run revisions list --service [SERVICE_NAME] --region [REGION]

# 서비스 로그 확인 (실시간)
gcloud run services logs read [SERVICE_NAME] --region [REGION] --follow

# 서비스 URL 확인
gcloud run services describe [SERVICE_NAME] --region [REGION] --format="value(status.url)"
```

---

#### 문제 3: 배포 실패 디버깅

**GitHub Actions 로그 분석**:

```bash
# 실패한 작업의 로그 다운로드
gh run download <run-id>

# 특정 Job의 로그만 확인
gh run view <run-id> --log --job <job-id>

# 작업 재실행
gh run rerun <run-id>

# 실패한 작업만 재실행
gh run rerun <run-id> --failed
```

**일반적인 배포 실패 원인**:

| 오류 | 원인 | 해결 방법 |
|------|------|-----------|
| **Secrets not found** | GitHub Secrets 미설정 | Repository Settings > Secrets and variables > Actions에서 추가 |
| **Authentication failed** | 잘못된 토큰/API 키 | Secrets 값 재확인 및 업데이트 |
| **Build timeout** | 빌드 시간 초과 (60분 제한) | 빌드 캐싱 활성화, 불필요한 단계 제거 |
| **Deployment limit exceeded** | 무료 플랜 한계 초과 | 플랜 업그레이드 또는 배포 빈도 조정 |
| **Module not found** | 의존성 설치 실패 | `package-lock.json` 확인, `npm ci` 사용 |
| **Environment mismatch** | Node.js 버전 불일치 | `actions/setup-node`에서 버전 명시 |

---

#### 배포 자동화 팁

**1. Branch Protection Rules 설정**:

```bash
# Repository Settings에서:
Settings > Branches > Add branch protection rule

# main 브랜치 보호 규칙:
✅ Require status checks to pass before merging
✅ Require branches to be up to date before merging
✅ Status checks: CI/CD, Build, Test, Lint
✅ Require linear history
```

**2. 배포 승인 플로우 추가**:

```yaml
# .github/workflows/deploy.yml
jobs:
  deploy-production:
    runs-on: ubuntu-latest
    environment:
      name: production
      url: https://example.com
    # environment 사용 시 Repository Settings > Environments에서 승인자 설정 가능
    steps:
      - name: Deploy to Production
        run: |
          # 배포 명령
```

**3. 배포 알림 설정** (Slack):

```yaml
# .github/workflows/deploy.yml
- name: Notify Slack on Success
  if: success()
  uses: slackapi/slack-github-action@v1
  with:
    payload: |
      {
        "text": "✅ Deployment successful: ${{ github.event.repository.name }}"
      }
  env:
    SLACK_WEBHOOK_URL: ${{ secrets.SLACK_WEBHOOK_URL }}

- name: Notify Slack on Failure
  if: failure()
  uses: slackapi/slack-github-action@v1
  with:
    payload: |
      {
        "text": "❌ Deployment failed: ${{ github.event.repository.name }}"
      }
  env:
    SLACK_WEBHOOK_URL: ${{ secrets.SLACK_WEBHOOK_URL }}
```

**4. 배포 롤백 워크플로우**:

```yaml
# .github/workflows/rollback.yml
name: Rollback Deployment

on:
  workflow_dispatch:
    inputs:
      deployment_id:
        description: 'Deployment ID to rollback to'
        required: true
        type: string

jobs:
  rollback:
    runs-on: ubuntu-latest
    steps:
      - name: Rollback to previous deployment
        run: |
          vercel rollback ${{ inputs.deployment_id }} --token=${{ secrets.VERCEL_TOKEN }}
          # 또는
          # railway rollback ${{ inputs.deployment_id }}
```

---

## 도메인 및 SSL 설정

### Cloudflare DNS 설정

#### 1. 도메인 추가

```bash
# Cloudflare 대시보드에서:
# 1. Add a Site
# 2. 도메인 입력
# 3. DNS 레코드 추가
```

#### 2. DNS 레코드 설정

**Vercel 연결**:

```
Type: CNAME
Name: @
Target: cname.vercel-dns.com
Proxy: Enabled (Orange Cloud)

Type: CNAME
Name: www
Target: cname.vercel-dns.com
Proxy: Enabled (Orange Cloud)
```

**API 서브도메인 (Railway)**:

```
Type: CNAME
Name: api
Target: [service-name].up.railway.app
Proxy: Enabled (Orange Cloud)
```

#### 3. SSL/TLS 설정

**Cloudflare 대시보드 → SSL/TLS**:

```
SSL/TLS encryption mode: Full (strict)

Edge Certificates:
✅ Always Use HTTPS
✅ Automatic HTTPS Rewrites
✅ Opportunistic Encryption
✅ TLS 1.3
✅ HTTP Strict Transport Security (HSTS)
   - Max Age: 12 months
   - Include subdomains
   - Preload
```

---

## 모니터링 및 로깅

### Sentry 설정 (에러 추적)

#### 1. Sentry 프로젝트 생성

```bash
# Sentry CLI 설치
npm install -g @sentry/cli

# Sentry 로그인
sentry-cli login

# 프로젝트 설정
npx @sentry/wizard@latest -i nextjs
```

#### 2. Sentry 설정 파일

**`sentry.client.config.ts`**:

```typescript
import * as Sentry from '@sentry/nextjs'

Sentry.init({
  dsn: process.env.NEXT_PUBLIC_SENTRY_DSN,
  environment: process.env.NODE_ENV,

  // Performance Monitoring
  tracesSampleRate: 1.0,

  // Session Replay
  replaysSessionSampleRate: 0.1,
  replaysOnErrorSampleRate: 1.0,

  integrations: [
    new Sentry.BrowserTracing({
      tracePropagationTargets: ['localhost', /^https:\/\/yoursite\.com/],
    }),
    new Sentry.Replay({
      maskAllText: true,
      blockAllMedia: true,
    }),
  ],

  // 필터링
  beforeSend(event, hint) {
    // 로컬 환경에서는 전송하지 않음
    if (process.env.NODE_ENV === 'development') {
      return null
    }
    return event
  },
})
```

**`sentry.server.config.ts`**:

```typescript
import * as Sentry from '@sentry/nextjs'

Sentry.init({
  dsn: process.env.NEXT_PUBLIC_SENTRY_DSN,
  environment: process.env.NODE_ENV,
  tracesSampleRate: 1.0,
})
```

#### 3. 에러 보고 예제

```typescript
// 수동 에러 보고
import * as Sentry from '@sentry/nextjs'

try {
  // 위험한 작업
} catch (error) {
  Sentry.captureException(error, {
    tags: {
      section: 'payment',
    },
    extra: {
      userId: user.id,
      orderId: order.id,
    },
  })
}

// 성능 모니터링
const transaction = Sentry.startTransaction({
  name: 'Database Query',
  op: 'db.query',
})

// ... 작업 수행 ...

transaction.finish()
```

---

### Vercel Analytics 설정

#### 1. Vercel Analytics 활성화

```bash
# Vercel 대시보드에서:
# Project Settings → Analytics → Enable
```

#### 2. Next.js 통합

**`app/layout.tsx`**:

```typescript
import { Analytics } from '@vercel/analytics/react'
import { SpeedInsights } from '@vercel/speed-insights/next'

export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="ko">
      <body>
        {children}
        <Analytics />
        <SpeedInsights />
      </body>
    </html>
  )
}
```

---

### 커스텀 로깅 시스템

**`lib/logger.ts`**:

```typescript
type LogLevel = 'debug' | 'info' | 'warn' | 'error'

interface LogEntry {
  level: LogLevel
  message: string
  timestamp: string
  context?: Record<string, any>
}

class Logger {
  private serviceName: string

  constructor(serviceName: string) {
    this.serviceName = serviceName
  }

  private log(level: LogLevel, message: string, context?: Record<string, any>) {
    const entry: LogEntry = {
      level,
      message,
      timestamp: new Date().toISOString(),
      context: {
        service: this.serviceName,
        ...context,
      },
    }

    // 프로덕션 환경에서는 외부 로깅 서비스로 전송
    if (process.env.NODE_ENV === 'production') {
      this.sendToLoggingService(entry)
    }

    // 콘솔 출력
    const logFn = console[level] || console.log
    logFn(JSON.stringify(entry, null, 2))
  }

  private async sendToLoggingService(entry: LogEntry) {
    // Cloudflare Workers Analytics, Datadog, Logtail 등으로 전송
    try {
      await fetch('/api/logs', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(entry),
      })
    } catch (error) {
      console.error('Failed to send log:', error)
    }
  }

  debug(message: string, context?: Record<string, any>) {
    this.log('debug', message, context)
  }

  info(message: string, context?: Record<string, any>) {
    this.log('info', message, context)
  }

  warn(message: string, context?: Record<string, any>) {
    this.log('warn', message, context)
  }

  error(message: string, context?: Record<string, any>) {
    this.log('error', message, context)
  }
}

export const logger = new Logger(process.env.SERVICE_NAME || 'app')
```

---

## 성능 최적화

### CDN 캐싱 전략

#### 1. Cloudflare Cache Rules

**Cloudflare 대시보드 → Caching → Cache Rules**:

```
Rule 1: 정적 파일 (이미지, 폰트, CSS, JS)
When: (http.request.uri.path matches ".*\\.(jpg|jpeg|png|gif|webp|svg|css|js|woff|woff2|ttf|eot)$")
Then: Cache Level: Standard, Edge TTL: 1 month

Rule 2: API 응답
When: (http.request.uri.path matches "^/api/.*")
Then: Cache Level: Bypass

Rule 3: HTML 페이지
When: (http.request.uri.path matches ".*\\.html$" or http.request.uri.path eq "/")
Then: Cache Level: Standard, Edge TTL: 1 hour, Browser TTL: 5 minutes
```

#### 2. Next.js 캐싱 설정

**`next.config.js`**:

```javascript
/** @type {import('next').NextConfig} */
const nextConfig = {
  // Image Optimization
  images: {
    formats: ['image/avif', 'image/webp'],
    remotePatterns: [
      {
        protocol: 'https',
        hostname: '*.supabase.co',
      },
    ],
  },

  // Cache Control Headers
  async headers() {
    return [
      {
        source: '/:all*(svg|jpg|jpeg|png|gif|webp|avif)',
        headers: [
          {
            key: 'Cache-Control',
            value: 'public, max-age=31536000, immutable',
          },
        ],
      },
      {
        source: '/_next/static/:path*',
        headers: [
          {
            key: 'Cache-Control',
            value: 'public, max-age=31536000, immutable',
          },
        ],
      },
    ]
  },
}

module.exports = nextConfig
```

---

### Database Query 최적화

#### 1. Connection Pooling

**Supabase (PgBouncer)**:

```typescript
// lib/supabase/server.ts
import { createClient } from '@supabase/supabase-js'

// Connection pooling을 위한 설정
export const supabaseAdmin = createClient(
  process.env.NEXT_PUBLIC_SUPABASE_URL!,
  process.env.SUPABASE_SERVICE_ROLE_KEY!,
  {
    db: {
      schema: 'public',
    },
    auth: {
      autoRefreshToken: false,
      persistSession: false,
    },
    global: {
      headers: {
        'x-connection-mode': 'transaction', // PgBouncer transaction mode
      },
    },
  }
)
```

#### 2. Query 최적화

```sql
-- Indexes 추가
CREATE INDEX CONCURRENTLY idx_users_email ON users(email);
CREATE INDEX CONCURRENTLY idx_posts_user_id ON posts(user_id);
CREATE INDEX CONCURRENTLY idx_posts_created_at ON posts(created_at DESC);

-- Partial Index (조건부 인덱스)
CREATE INDEX CONCURRENTLY idx_posts_published
  ON posts(created_at DESC)
  WHERE status = 'published';

-- Composite Index
CREATE INDEX CONCURRENTLY idx_posts_user_status
  ON posts(user_id, status, created_at DESC);

-- EXPLAIN ANALYZE로 쿼리 성능 분석
EXPLAIN ANALYZE
SELECT * FROM posts
WHERE user_id = 'user-id'
  AND status = 'published'
ORDER BY created_at DESC
LIMIT 10;
```

---

## 보안 설정

### Security Headers

**`next.config.js`**:

```javascript
const securityHeaders = [
  {
    key: 'X-DNS-Prefetch-Control',
    value: 'on',
  },
  {
    key: 'Strict-Transport-Security',
    value: 'max-age=63072000; includeSubDomains; preload',
  },
  {
    key: 'X-Frame-Options',
    value: 'SAMEORIGIN',
  },
  {
    key: 'X-Content-Type-Options',
    value: 'nosniff',
  },
  {
    key: 'X-XSS-Protection',
    value: '1; mode=block',
  },
  {
    key: 'Referrer-Policy',
    value: 'strict-origin-when-cross-origin',
  },
  {
    key: 'Permissions-Policy',
    value: 'camera=(), microphone=(), geolocation=()',
  },
  {
    key: 'Content-Security-Policy',
    value: `
      default-src 'self';
      script-src 'self' 'unsafe-eval' 'unsafe-inline';
      style-src 'self' 'unsafe-inline';
      img-src 'self' data: https:;
      font-src 'self' data:;
      connect-src 'self' https://api.example.com;
    `.replace(/\s{2,}/g, ' ').trim(),
  },
]

/** @type {import('next').NextConfig} */
const nextConfig = {
  async headers() {
    return [
      {
        source: '/:path*',
        headers: securityHeaders,
      },
    ]
  },
}

module.exports = nextConfig
```

---

### Rate Limiting

**Cloudflare Rate Limiting**:

```
Rule: API Rate Limiting
When: (http.request.uri.path matches "^/api/.*")
Then:
  - Rate: 100 requests per minute
  - Action: Block for 60 seconds
  - Response: 429 Too Many Requests
```

**코드 기반 Rate Limiting (Upstash Redis)**:

```typescript
import { Ratelimit } from '@upstash/ratelimit'
import { Redis } from '@upstash/redis'

// Redis 클라이언트 생성
const redis = new Redis({
  url: process.env.UPSTASH_REDIS_REST_URL!,
  token: process.env.UPSTASH_REDIS_REST_TOKEN!,
})

// Rate limiter 생성
const ratelimit = new Ratelimit({
  redis,
  limiter: Ratelimit.slidingWindow(10, '10 s'), // 10초에 10번
  analytics: true,
})

// Middleware 또는 API Route에서 사용
export async function POST(request: Request) {
  const ip = request.headers.get('x-forwarded-for') ?? '127.0.0.1'

  const { success, limit, reset, remaining } = await ratelimit.limit(ip)

  if (!success) {
    return new Response('Too Many Requests', {
      status: 429,
      headers: {
        'X-RateLimit-Limit': limit.toString(),
        'X-RateLimit-Remaining': remaining.toString(),
        'X-RateLimit-Reset': reset.toString(),
      },
    })
  }

  // 정상 처리
  return new Response('OK')
}
```

---

## 트러블슈팅

### 문제 1: Vercel 배포 시 "Module not found" 에러

**증상**:
```
Error: Cannot find module '@/lib/utils'
Module not found: Can't resolve '@/components/ui/button'
```

**원인**:
- TypeScript path alias 설정 누락
- `tsconfig.json`과 Next.js 설정 불일치

**해결 방법**:

```json
// tsconfig.json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"]
    }
  }
}
```

```javascript
// next.config.js
const path = require('path')

/** @type {import('next').NextConfig} */
const nextConfig = {
  webpack: (config) => {
    config.resolve.alias = {
      ...config.resolve.alias,
      '@': path.resolve(__dirname, 'src'),
    }
    return config
  },
}

module.exports = nextConfig
```

---

### 문제 2: Railway 배포 후 Database 연결 실패

**증상**:
```
Error: P1001: Can't reach database server at `containers-us-west-XXX.railway.app`
Connection timeout
```

**원인**:
- DATABASE_URL 환경 변수가 올바르게 설정되지 않음
- PostgreSQL이 아직 준비되지 않은 상태에서 앱이 시작됨

**해결 방법**:

1. **환경 변수 확인**:
```bash
# Railway 대시보드에서 확인
railway variables

# DATABASE_URL 형식 확인
postgresql://postgres:password@host:port/database
```

2. **Healthcheck 추가**:

```typescript
// src/main.ts (Nest.js)
import { NestFactory } from '@nestjs/core'
import { AppModule } from './app.module'

async function bootstrap() {
  const app = await NestFactory.create(AppModule)

  // Health check endpoint
  app.get('/health', (req, res) => {
    res.status(200).send('OK')
  })

  await app.listen(process.env.PORT || 3000)
}
bootstrap()
```

3. **Railway 재시작 정책 설정**:

```json
// railway.json
{
  "deploy": {
    "restartPolicyType": "ON_FAILURE",
    "restartPolicyMaxRetries": 10
  }
}
```

---

### 문제 3: Cloudflare Pages 빌드 실패 (Node.js 버전)

**증상**:
```
Error: The engine "node" is incompatible with this module.
Expected version ">=22.0.0". Got "18.17.1"
```

**원인**:
- Cloudflare Pages의 기본 Node.js 버전이 낮음
- 환경 변수로 Node.js 버전 지정 필요

**해결 방법**:

1. **환경 변수 설정**:

```bash
# Cloudflare Pages 대시보드 → Settings → Environment Variables
NODE_VERSION=22
```

2. **.nvmrc 파일 추가**:

```
22
```

3. **package.json에 engines 명시**:

```json
{
  "engines": {
    "node": ">=22.0.0",
    "npm": ">=10.0.0"
  }
}
```

---

### 문제 4: Google Cloud Run Cold Start 지연

**증상**:
- 첫 요청 시 5-10초 응답 지연
- 이후 요청은 빠름

**원인**:
- Cold Start: 인스턴스가 0으로 스케일된 후 다시 시작되는 시간
- 컨테이너 이미지 크기가 큼

**해결 방법**:

1. **최소 인스턴스 설정**:

```bash
gcloud run services update [SERVICE_NAME] \
  --min-instances 1 \
  --region [REGION]

# ⚠️ 주의: 무료 티어에서는 최소 인스턴스 1개도 비용 발생 가능
```

2. **컨테이너 이미지 최적화**:

```dockerfile
# Multi-stage build로 이미지 크기 줄이기
FROM node:22-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY . .
RUN npm run build

FROM node:22-alpine AS runner
WORKDIR /app
ENV NODE_ENV=production

# 필요한 파일만 복사
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/package.json ./package.json

# 불필요한 파일 제거
RUN npm prune --production

EXPOSE 8080
CMD ["node", "dist/main"]
```

3. **Warm-up 스케줄러 추가**:

```yaml
# .github/workflows/warm-up.yml
name: Warm Up Cloud Run

on:
  schedule:
    - cron: '*/5 * * * *' # 5분마다 실행

jobs:
  warm-up:
    runs-on: ubuntu-latest
    steps:
      - name: Send warm-up request
        run: |
          curl -f https://[SERVICE_URL]/health || true
```

---

### 문제 5: Supabase RLS 정책으로 데이터 조회 불가

**증상**:
```
Error: permission denied for table users
```

**원인**:
- Row Level Security (RLS) 정책이 올바르게 설정되지 않음
- 인증 토큰이 전달되지 않음

**해결 방법**:

1. **RLS 정책 확인**:

```sql
-- RLS 활성화 확인
SELECT tablename, rowsecurity
FROM pg_tables
WHERE schemaname = 'public';

-- 정책 확인
SELECT * FROM pg_policies WHERE tablename = 'users';

-- RLS 정책 추가 (누락된 경우)
CREATE POLICY "Users can view own data"
  ON public.users
  FOR SELECT
  USING (auth.uid() = id);
```

2. **Service Role Key 사용 (관리자 권한)**:

```typescript
// RLS 우회가 필요한 경우 (Server Action)
import { createClient } from '@supabase/supabase-js'

const supabaseAdmin = createClient(
  process.env.NEXT_PUBLIC_SUPABASE_URL!,
  process.env.SUPABASE_SERVICE_ROLE_KEY!, // Service Role Key 사용
  {
    auth: {
      autoRefreshToken: false,
      persistSession: false,
    },
  }
)

// RLS 우회하여 모든 데이터 조회
const { data, error } = await supabaseAdmin
  .from('users')
  .select('*')
```

3. **클라이언트에서 인증 토큰 전달 확인**:

```typescript
// Client Component
import { createClient } from '@/lib/supabase/client'

const supabase = createClient()

// 인증된 사용자만 데이터 조회 가능
const { data: { user } } = await supabase.auth.getUser()

if (!user) {
  console.error('Not authenticated')
  return
}

const { data, error } = await supabase
  .from('users')
  .select('*')
  .eq('id', user.id) // RLS 정책에 맞춰 조회
```

---

### 일반적인 배포 이슈

| 문제 | 원인 | 해결책 |
|------|------|--------|
| **Environment variables not found** | `.env` 파일이 배포되지 않음 | 플랫폼 대시보드에서 환경 변수 설정 |
| **Build timeout** | 빌드 시간 초과 | 빌드 캐시 활성화, 의존성 최적화 |
| **Out of memory** | 빌드/런타임 메모리 부족 | 플랜 업그레이드 또는 메모리 최적화 |
| **CORS errors** | CORS 헤더 누락 | API에 CORS 헤더 추가 |
| **SSL certificate errors** | SSL 설정 문제 | Cloudflare Full (strict) 모드 설정 |
| **Database connection pool exhausted** | 너무 많은 연결 | Connection pooling 설정 (PgBouncer) |
| **Rate limit exceeded** | 무료 티어 한계 | Rate limiting 구현 또는 플랜 업그레이드 |

---

## 배포 체크리스트

### 배포 전

- [ ] 환경 변수 모두 설정
- [ ] 데이터베이스 마이그레이션 준비
- [ ] 빌드 테스트 (`npm run build`)
- [ ] 타입 체크 (`npm run type-check`)
- [ ] 린트 검사 (`npm run lint`)
- [ ] 유닛 테스트 통과 (`npm run test`)
- [ ] E2E 테스트 통과 (`npm run test:e2e`)
- [ ] Security Headers 설정 확인
- [ ] CORS 설정 확인
- [ ] Rate Limiting 설정
- [ ] Secrets 관리 확인 (Git에 포함되지 않음)

### 배포 후

- [ ] Health Check 엔드포인트 확인
- [ ] 프로덕션 URL 접속 확인
- [ ] API 엔드포인트 동작 확인
- [ ] 데이터베이스 연결 확인
- [ ] 파일 업로드/다운로드 확인
- [ ] 인증/권한 시스템 확인
- [ ] 에러 모니터링 (Sentry) 동작 확인
- [ ] Analytics 수집 확인
- [ ] Performance Metrics 확인
- [ ] SSL 인증서 확인
- [ ] Custom Domain 연결 확인
- [ ] CDN 캐싱 동작 확인

### 프로덕션 최적화

- [ ] 이미지 최적화 (WebP, AVIF)
- [ ] Code Splitting 적용
- [ ] Lazy Loading 적용
- [ ] Database Indexes 추가
- [ ] Query 최적화
- [ ] Caching 전략 구현
- [ ] CDN 활용
- [ ] Lighthouse Score 80+ 달성
- [ ] SEO 메타데이터 설정
- [ ] Sitemap 생성
- [ ] robots.txt 설정

---

## 마무리

이 배포 가이드는 **프론트엔드 (Vercel/Cloudflare Pages)**, **백엔드 (Railway/Google Cloud Run)**, **데이터베이스 (Supabase)** 조합으로 무료 또는 저비용으로 프로덕션 수준의 웹 애플리케이션을 배포하는 방법을 다룹니다.

### 추천 배포 조합 (바이브코딩)

**최대 무료 활용**:
- Frontend: Cloudflare Pages
- Backend: Railway ($5 크레딧/월)
- Database: Supabase (500MB 무료)
- Storage: Cloudflare R2 (10GB 무료)

**Next.js 최적화**:
- Frontend + Backend: Vercel
- Database: Supabase
- Storage: Vercel Blob Storage

**서버리스 극대화**:
- Frontend: Cloudflare Pages
- Backend: Google Cloud Run
- Database: Supabase / Firebase
- Storage: Google Cloud Storage

모든 플랫폼의 무료 티어를 최대한 활용하여 비용 부담 없이 학습하고 포트폴리오를 구축할 수 있습니다.
