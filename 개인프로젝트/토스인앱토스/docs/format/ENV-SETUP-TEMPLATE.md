# 개발 환경 초기 설정 가이드
## [프로젝트명] - 환경 설정 체크리스트

> **작성일**: YYYY-MM-DD
> **예상 소요 시간**: [N-M]시간
> **우선순위**: 🔴 필수 (개발 시작 전 완료)

---

## 📋 목차

1. [Phase 0: 사전 준비](#phase-0-사전-준비)
2. [Phase 1: 로컬 개발 도구 설치](#phase-1-로컬-개발-도구-설치)
3. [Phase 2: 서비스 계정 생성](#phase-2-서비스-계정-생성)
4. [Phase 3: API 키 및 토큰 수집](#phase-3-api-키-및-토큰-수집)
5. [Phase 4: 데이터베이스 설정](#phase-4-데이터베이스-설정)
6. [Phase 5: MCP 서버 설치 및 설정](#phase-5-mcp-서버-설치-및-설정)
7. [Phase 6: 환경 변수 설정](#phase-6-환경-변수-설정)
8. [Phase 7: 환경 검증](#phase-7-환경-검증)
9. [트러블슈팅](#트러블슈팅)
10. [보안 체크리스트](#보안-체크리스트)

---

## Phase 0: 사전 준비

**목표**: 필요한 서비스 및 도구 목록 확인

### 기술 스택 확인

**이 프로젝트에서 사용하는 기술:**

- **Frontend**: [프론트엔드 기술]
- **Backend**: [백엔드 기술]
- **Database**: [데이터베이스]
- **BaaS**: [BaaS 서비스]
- **Auth**: [인증 서비스]
- **AI/LLM**: [AI 서비스]
- **Payment**: [결제 서비스]
- **Deployment**: [배포 플랫폼]
- **Monitoring**: [모니터링 서비스]

### 필요한 계정 목록

**체크리스트: 계정 생성 필요 여부 확인**

- [ ] GitHub 계정 (코드 저장소)
- [ ] [BaaS 서비스] 계정 (예: Supabase, Firebase, Convex)
- [ ] [인증 서비스] 계정 (예: Clerk, Auth0)
- [ ] [AI 서비스] 계정 (예: OpenAI, Anthropic)
- [ ] [배포 플랫폼] 계정 (예: Vercel, Railway, Render)
- [ ] [모니터링 서비스] 계정 (예: Sentry, LogRocket)
- [ ] [결제 서비스] 계정 (예: Stripe, Lemon Squeezy)
- [ ] [기타 서비스] 계정

**완료 기준**: ✅ 필요한 모든 서비스 확인 완료

---

## Phase 1: 로컬 개발 도구 설치

**목표**: 개발에 필요한 로컬 도구 설치 및 검증

### 1.1 필수 도구 설치

#### Node.js 및 패키지 매니저

**Node.js 설치 확인:**
```bash
# Node.js 버전 확인 (권장: v[N.N.N]+)
node --version

# npm 버전 확인
npm --version
```

**패키지 매니저 설치:**

- [ ] npm (Node.js 기본 포함)
- [ ] pnpm 설치
```bash
npm install -g pnpm
pnpm --version
```
- [ ] yarn 설치 (선택 사항)
```bash
npm install -g yarn
yarn --version
```

#### Git 설치

```bash
# Git 버전 확인
git --version

# Git 사용자 설정
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

- [ ] Git 설치 확인
- [ ] Git 사용자 설정 완료
- [ ] GitHub SSH 키 등록 (선택 사항)

#### Docker 설치 (데이터베이스 로컬 실행용)

```bash
# Docker 버전 확인
docker --version
docker-compose --version
```

- [ ] Docker Desktop 설치
- [ ] Docker 실행 확인

#### 에디터 및 확장 프로그램

- [ ] VSCode 설치
- [ ] 필수 확장 프로그램 설치
  - [ ] ESLint
  - [ ] Prettier
  - [ ] [언어별 확장 프로그램] (예: TypeScript, Python)
  - [ ] [프레임워크 확장 프로그램] (예: React, Vue)
  - [ ] Database Client (예: PostgreSQL, MongoDB)

**완료 기준**: ✅ `node`, `git`, `docker` 명령어 정상 동작

---

### 1.2 CLI 도구 설치

**프로젝트별 필요 도구:**

- [ ] [프레임워크] CLI 설치
```bash
# 예시: Next.js
npm install -g create-next-app

# 예시: NestJS
npm install -g @nestjs/cli
```

- [ ] [배포 플랫폼] CLI 설치
```bash
# 예시: Vercel CLI
npm install -g vercel

# 예시: Railway CLI
brew install railway
# 또는
npm install -g @railway/cli
```

- [ ] [BaaS] CLI 설치
```bash
# 예시: Supabase CLI
brew install supabase/tap/supabase
# 또는
npm install -g supabase

# 예시: Firebase CLI
npm install -g firebase-tools
```

**완료 기준**: ✅ 모든 CLI 도구 설치 및 버전 확인 완료

---

## Phase 2: 서비스 계정 생성

**목표**: 필요한 모든 서비스 계정 생성 및 프로젝트 초기화

### 2.1 BaaS 서비스 계정

#### [BaaS 서비스명] (예: Supabase)

- [ ] [서비스] 계정 생성 ([URL])
- [ ] 새 프로젝트 생성
  - **프로젝트명**: [프로젝트명]
  - **리전**: [리전 선택] (예: Northeast Asia Seoul)
  - **데이터베이스 비밀번호**: [비밀번호 저장 위치]
- [ ] 프로젝트 URL 복사
  - `[환경 변수명]`: [값]
- [ ] API Keys 복사
  - `anon key`: [값]
  - `service_role key`: [값] (⚠️ 보안 주의)

**저장 위치**: `.env.local` (절대 커밋하지 말 것!)

#### [기타 BaaS] (예: Convex, Firebase)

- [ ] 계정 생성 및 프로젝트 초기화
- [ ] API 키 복사
- [ ] 설정 파일 다운로드

**완료 기준**: ✅ BaaS 프로젝트 생성 및 API 키 보관 완료

---

### 2.2 인증 서비스 계정

#### [인증 서비스명] (예: Clerk)

- [ ] [서비스] 계정 생성 ([URL])
- [ ] Application 생성
  - **애플리케이션명**: [애플리케이션명]
  - **로그인 방법 선택**: Email, Google, GitHub 등
- [ ] API Keys 복사
  - `[환경 변수_1]`: [값]
  - `[환경 변수_2]`: [값]
- [ ] Webhook 설정 (필요 시)
  - Endpoint URL: `[백엔드 URL]/webhooks/[서비스명]`
  - Secret: [값]

**완료 기준**: ✅ 인증 서비스 설정 완료, API 키 보관

---

### 2.3 OAuth 소셜 로그인 설정

**목표**: 소셜 로그인(Google, Kakao, Naver, GitHub 등) OAuth 앱 등록 및 키 발급

#### Google OAuth

- [ ] Google Cloud Console 접속 (https://console.cloud.google.com)
- [ ] 새 프로젝트 생성 또는 기존 프로젝트 선택
- [ ] OAuth 동의 화면 구성
  - **User Type**: External (개인) / Internal (조직)
  - **앱 정보**: 앱 이름, 사용자 지원 이메일, 로고 등
  - **범위(Scopes)**: email, profile
- [ ] 사용자 인증 정보 → OAuth 2.0 클라이언트 ID 만들기
  - **애플리케이션 유형**: 웹 애플리케이션
  - **승인된 리디렉션 URI**:
    - 로컬: `http://localhost:[포트]/api/auth/callback/google`
    - 프로덕션: `https://[도메인]/api/auth/callback/google`
- [ ] 클라이언트 ID 및 클라이언트 보안 비밀번호 복사
  - `GOOGLE_CLIENT_ID`: [값]
  - `GOOGLE_CLIENT_SECRET`: [값]

**완료 기준**: ✅ Google OAuth 클라이언트 ID 발급 및 저장

#### Kakao OAuth

- [ ] Kakao Developers 접속 (https://developers.kakao.com)
- [ ] 내 애플리케이션 → 애플리케이션 추가하기
  - **앱 이름**: [애플리케이션명]
  - **사업자명**: [사업자명]
- [ ] 앱 설정 → 플랫폼 → Web 플랫폼 등록
  - **사이트 도메인**:
    - 로컬: `http://localhost:[포트]`
    - 프로덕션: `https://[도메인]`
- [ ] 제품 설정 → 카카오 로그인 활성화
  - **Redirect URI**:
    - 로컬: `http://localhost:[포트]/api/auth/callback/kakao`
    - 프로덕션: `https://[도메인]/api/auth/callback/kakao`
  - **동의 항목**: 닉네임, 프로필 사진, 카카오계정(이메일)
- [ ] 앱 키 복사
  - `KAKAO_REST_API_KEY`: [REST API 키]
  - `KAKAO_CLIENT_SECRET`: [보안] → Client Secret 발급 후 복사

**완료 기준**: ✅ Kakao REST API 키 발급 및 저장

#### Naver OAuth

- [ ] Naver Developers 접속 (https://developers.naver.com)
- [ ] Application → 애플리케이션 등록
  - **애플리케이션 이름**: [애플리케이션명]
  - **사용 API**: 네이버 로그인
  - **제공 정보**: 이메일, 닉네임, 프로필 사진
  - **서비스 URL**:
    - 로컬: `http://localhost:[포트]`
    - 프로덕션: `https://[도메인]`
  - **Callback URL**:
    - 로컬: `http://localhost:[포트]/api/auth/callback/naver`
    - 프로덕션: `https://[도메인]/api/auth/callback/naver`
- [ ] Client ID 및 Client Secret 복사
  - `NAVER_CLIENT_ID`: [값]
  - `NAVER_CLIENT_SECRET`: [값]

**완료 기준**: ✅ Naver Client ID 발급 및 저장

#### GitHub OAuth

- [ ] GitHub Settings → Developer settings → OAuth Apps
- [ ] New OAuth App 클릭
  - **Application name**: [애플리케이션명]
  - **Homepage URL**:
    - 로컬: `http://localhost:[포트]`
    - 프로덕션: `https://[도메인]`
  - **Authorization callback URL**:
    - 로컬: `http://localhost:[포트]/api/auth/callback/github`
    - 프로덕션: `https://[도메인]/api/auth/callback/github`
- [ ] Client ID 및 Client Secret 생성 및 복사
  - `GITHUB_CLIENT_ID`: [값]
  - `GITHUB_CLIENT_SECRET`: [값]

**완료 기준**: ✅ GitHub OAuth App 등록 및 키 저장

**⚠️ OAuth 보안 주의사항:**
- [ ] 모든 Redirect URI를 정확하게 설정 (오타 시 인증 실패)
- [ ] Client Secret은 절대 클라이언트에 노출하지 않음 (서버에서만 사용)
- [ ] 프로덕션 배포 시 Redirect URI를 프로덕션 도메인으로 변경
- [ ] 각 플랫폼별 사용자 동의 항목 최소화 (필요한 정보만 요청)

---

### 2.4 AI/LLM 서비스 계정

#### OpenAI

- [ ] OpenAI 계정 생성 (https://platform.openai.com)
- [ ] API Key 생성
  - Settings → API Keys → Create new secret key
  - `OPENAI_API_KEY`: sk-...
- [ ] Usage Limits 설정 (권장: Hard limit 설정)
- [ ] Billing 정보 등록

#### Anthropic (Claude)

- [ ] Anthropic 계정 생성 (https://console.anthropic.com)
- [ ] API Key 생성
  - `ANTHROPIC_API_KEY`: sk-ant-...
- [ ] Usage Limits 설정

**완료 기준**: ✅ AI API 키 생성 및 보관, 사용량 제한 설정 완료

---

### 2.5 배포 플랫폼 계정

#### [배포 플랫폼 1] (예: Vercel)

- [ ] 계정 생성 (https://vercel.com)
- [ ] GitHub 연동
- [ ] 팀 생성 (선택 사항)

#### [배포 플랫폼 2] (예: Railway, Render)

- [ ] 계정 생성
- [ ] GitHub 연동
- [ ] 결제 정보 등록 (무료 티어 확인)

**완료 기준**: ✅ 배포 플랫폼 계정 생성 및 GitHub 연동 완료

---

### 2.6 모니터링 서비스 계정

#### Sentry

- [ ] Sentry 계정 생성 (https://sentry.io)
- [ ] 프로젝트 생성 (Frontend, Backend 각각)
- [ ] DSN 복사
  - `SENTRY_DSN`: https://[KEY]@[ORGANIZATION].ingest.sentry.io/[PROJECT_ID]

**완료 기준**: ✅ 모니터링 서비스 설정 완료

---

### 2.7 결제 서비스 계정 (선택 사항)

**목표**: 결제 서비스 계정 생성 및 API 키 발급

#### Stripe (해외 결제)

- [ ] Stripe 계정 생성 (https://stripe.com)
- [ ] Test Mode API Keys 복사
  - `STRIPE_PUBLISHABLE_KEY`: pk_test_...
  - `STRIPE_SECRET_KEY`: sk_test_...
- [ ] Webhook Secret 설정 (개발 시작 후)
  - Stripe CLI 설치: `brew install stripe/stripe-cli/stripe`
  - Webhook 로컬 테스트: `stripe listen --forward-to localhost:[포트]/api/webhooks/stripe`
  - `STRIPE_WEBHOOK_SECRET`: whsec_...

**완료 기준**: ✅ Stripe API 키 발급 및 저장

#### Toss Payments (한국 결제)

- [ ] Toss Payments 계정 생성 (https://www.tosspayments.com)
- [ ] 개발자센터 → 내 애플리케이션 → 앱 등록
  - **상호명**: [상호명]
  - **사업자등록번호**: [번호] (개인은 생년월일)
- [ ] Test Mode 키 복사 (테스트 결제용)
  - `TOSS_CLIENT_KEY`: test_ck_...
  - `TOSS_SECRET_KEY`: test_sk_...
- [ ] Webhook 설정
  - **Webhook URL**: `https://[도메인]/api/webhooks/toss`
  - `TOSS_WEBHOOK_SECRET`: [값]
- [ ] 결제 수단 설정
  - 카드, 계좌이체, 가상계좌, 간편결제 등

**⚠️ 실결제 전환 시**:
- [ ] 사업자등록증 제출
- [ ] Live Mode 키로 변경: `live_ck_...`, `live_sk_...`
- [ ] PG사 심사 완료

**완료 기준**: ✅ Toss Payments API 키 발급 및 저장

#### 아임포트 / PortOne (통합 결제)

- [ ] 아임포트 계정 생성 (https://portone.io)
- [ ] 새 가맹점 추가
  - **가맹점명**: [가맹점명]
  - **PG사 선택**: KG이니시스, NHN KCP, 나이스페이먼츠 등
- [ ] API 키 복사
  - `IAMPORT_REST_API_KEY`: [값]
  - `IAMPORT_REST_API_SECRET`: [값]
- [ ] 가맹점 식별코드 복사
  - `IAMPORT_IMP_CODE`: imp[번호]

**완료 기준**: ✅ 아임포트 API 키 발급 및 저장

**결제 서비스 선택 가이드:**
- **해외 사용자 대상**: Stripe 추천
- **한국 사용자 대상**: Toss Payments 추천
- **복수 PG사 관리**: 아임포트 추천 (여러 PG사를 하나의 인터페이스로 관리)

---

## Phase 3: API 키 및 토큰 수집

**목표**: 모든 서비스의 API 키를 안전하게 수집 및 저장

### API 키 관리 체크리스트

**수집 완료 확인:**

| 서비스 | 환경 변수명 | 값 저장 위치 | 상태 |
|--------|-------------|-------------|------|
| [BaaS] | `SUPABASE_URL` | .env.local | [ ] |
| [BaaS] | `SUPABASE_ANON_KEY` | .env.local | [ ] |
| [BaaS] | `SUPABASE_SERVICE_ROLE_KEY` | .env | [ ] |
| [인증] | `CLERK_PUBLISHABLE_KEY` | .env.local | [ ] |
| [인증] | `CLERK_SECRET_KEY` | .env | [ ] |
| **OAuth 소셜 로그인** | | | |
| Google | `GOOGLE_CLIENT_ID` | .env.local | [ ] |
| Google | `GOOGLE_CLIENT_SECRET` | .env | [ ] |
| Kakao | `KAKAO_REST_API_KEY` | .env.local | [ ] |
| Kakao | `KAKAO_CLIENT_SECRET` | .env | [ ] |
| Naver | `NAVER_CLIENT_ID` | .env.local | [ ] |
| Naver | `NAVER_CLIENT_SECRET` | .env | [ ] |
| GitHub | `GITHUB_CLIENT_ID` | .env.local | [ ] |
| GitHub | `GITHUB_CLIENT_SECRET` | .env | [ ] |
| **AI 서비스** | | | |
| OpenAI | `OPENAI_API_KEY` | .env | [ ] |
| Anthropic | `ANTHROPIC_API_KEY` | .env | [ ] |
| **결제 서비스** | | | |
| Stripe | `STRIPE_PUBLISHABLE_KEY` | .env.local | [ ] |
| Stripe | `STRIPE_SECRET_KEY` | .env | [ ] |
| Toss | `TOSS_CLIENT_KEY` | .env.local | [ ] |
| Toss | `TOSS_SECRET_KEY` | .env | [ ] |
| 아임포트 | `IAMPORT_REST_API_KEY` | .env | [ ] |
| 아임포트 | `IAMPORT_REST_API_SECRET` | .env | [ ] |

**보안 가이드:**
- ✅ `.env.local` 파일을 `.gitignore`에 추가
- ✅ `.env.example` 파일 생성 (키 이름만, 값은 제외)
- ✅ API 키를 절대 public 저장소에 커밋하지 않음
- ✅ 팀원과 공유 시 1Password, Bitwarden 등 암호 관리 도구 사용

**완료 기준**: ✅ 모든 API 키 수집 및 안전하게 보관 완료

---

## Phase 4: 데이터베이스 설정

**목표**: 데이터베이스 초기 설정 및 연결 테스트

### 4.1 [데이터베이스명] 설정 (예: PostgreSQL)

#### 옵션 1: [BaaS] 내장 데이터베이스 사용

**Supabase PostgreSQL 예시:**

- [ ] Supabase 프로젝트에서 Database 메뉴 접속
- [ ] Connection String 복사
  - `DATABASE_URL`: postgresql://postgres:[PASSWORD]@[HOST]:5432/postgres
- [ ] Connection Pooling 설정 (Prisma 사용 시)
  - `DIRECT_URL`: [Direct connection]
  - `DATABASE_URL`: [Pooled connection]

#### 옵션 2: 로컬 Docker 데이터베이스

**PostgreSQL Docker Compose 예시:**

- [ ] `docker-compose.yml` 파일 생성
```yaml
version: '3.8'
services:
  postgres:
    image: postgres:[버전]
    environment:
      POSTGRES_USER: [사용자명]
      POSTGRES_PASSWORD: [비밀번호]
      POSTGRES_DB: [데이터베이스명]
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```

- [ ] Docker Compose 실행
```bash
docker-compose up -d

# 연결 테스트
docker exec -it [컨테이너명] psql -U [사용자명] -d [데이터베이스명]
```

- [ ] `.env.local`에 연결 문자열 추가
```bash
DATABASE_URL="postgresql://[사용자명]:[비밀번호]@localhost:5432/[데이터베이스명]"
```

**완료 기준**: ✅ 데이터베이스 연결 테스트 성공

---

### 4.2 데이터베이스 클라이언트 설정

**추천 도구:**

- [ ] TablePlus (GUI 클라이언트)
- [ ] DBeaver (무료 오픈소스)
- [ ] VSCode Database 확장 프로그램

**연결 테스트:**
- [ ] 데이터베이스 클라이언트로 연결 성공
- [ ] 간단한 쿼리 실행 테스트 (`SELECT 1;`)

**완료 기준**: ✅ 데이터베이스 GUI 도구로 연결 확인

---

### 4.3 ORM 설정 (선택 사항)

#### Prisma 예시

- [ ] Prisma 설치
```bash
[패키지 관리자] add -D prisma
[패키지 관리자] add @prisma/client
```

- [ ] Prisma 초기화
```bash
npx prisma init
```

- [ ] `schema.prisma` 설정 확인
```prisma
datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

generator client {
  provider = "prisma-client-js"
}
```

- [ ] `.env`에 `DATABASE_URL` 설정 확인

**완료 기준**: ✅ Prisma 초기화 완료

---

## Phase 5: MCP 서버 설치 및 설정

**목표**: 사용하는 AI 개발 도구에 MCP 서버 설치 및 연동

### 5.1 MCP 서버 종류 확인

**이 프로젝트에서 사용할 MCP 서버:**

- [ ] `@modelcontextprotocol/server-filesystem` (파일 시스템 접근)
- [ ] `@modelcontextprotocol/server-github` (GitHub 연동)
- [ ] `@anthropic-ai/database-tools-mcp` (데이터베이스 도구)
- [ ] `@modelcontextprotocol/server-fetch` (HTTP 요청)
- [ ] `@modelcontextprotocol/server-git` (Git 작업)
- [ ] 커스텀 MCP 서버

---

### 5.2 AI 개발 도구별 MCP 설정

**사용 중인 도구를 선택하세요:**

- [ ] Claude Code (Claude Desktop)
- [ ] Cursor
- [ ] Windsurf (Codeium)
- [ ] Warp (AI Terminal)
- [ ] Gemini Code Assist
- [ ] Qwen Coder
- [ ] 기타 MCP 지원 도구

---

### 5.3 Claude Code (Claude Desktop) 설정

#### 설정 파일 위치

**macOS:**
```bash
~/Library/Application Support/Claude/claude_desktop_config.json
```

**Windows:**
```bash
%APPDATA%\Claude\claude_desktop_config.json
```

**Linux:**
```bash
~/.config/Claude/claude_desktop_config.json
```

#### 설정 파일 예시

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "/Users/[username]/projects/my-app"
      ]
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_..."
      }
    },
    "postgres": {
      "command": "npx",
      "args": [
        "-y",
        "@anthropic-ai/database-tools-mcp"
      ],
      "env": {
        "DATABASE_URL": "postgresql://user:password@localhost:5432/dbname"
      }
    }
  }
}
```

**체크리스트:**
- [ ] 설정 파일 생성/편집
- [ ] 프로젝트 경로를 절대 경로로 설정
- [ ] 환경 변수에 민감 정보 포함 시 주의
- [ ] Claude Desktop 재시작

**테스트:**
```
Claude Desktop 열기 → "Can you list files in my project?" 질문 → MCP 도구 사용 확인
```

---

### 5.4 Cursor 설정

**Cursor는 `.cursor/mcp.json` 파일을 사용합니다.**

#### 설정 파일 위치

```bash
[프로젝트 루트]/.cursor/mcp.json
```

#### 설정 파일 예시

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "${workspaceFolder}"
      ]
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "${env:GITHUB_TOKEN}"
      }
    }
  }
}
```

**체크리스트:**
- [ ] 프로젝트 루트에 `.cursor/` 디렉토리 생성
- [ ] `mcp.json` 파일 생성
- [ ] `${workspaceFolder}` 변수 사용 (프로젝트 경로 자동)
- [ ] `${env:VAR_NAME}` 형식으로 환경 변수 참조
- [ ] Cursor 재시작

**⚠️ 주의:**
- `.cursor/mcp.json`을 `.gitignore`에 추가 (민감 정보 포함 시)
- 또는 `.cursor/mcp.json.example` 생성 (팀 공유용)

**테스트:**
```
Cursor에서 Cmd+K → "Show me the project structure" → MCP 응답 확인
```

---

### 5.5 Windsurf (Codeium) 설정

**Windsurf는 설정 UI를 통해 MCP를 관리합니다.**

#### 설정 방법

1. Windsurf 열기
2. **Settings** → **Extensions** → **MCP Servers**
3. **Add Server** 클릭
4. 서버 정보 입력:

**Filesystem Server:**
```
Name: filesystem
Command: npx
Arguments: ["-y", "@modelcontextprotocol/server-filesystem", "/path/to/project"]
```

**GitHub Server:**
```
Name: github
Command: npx
Arguments: ["-y", "@modelcontextprotocol/server-github"]
Environment Variables:
  GITHUB_TOKEN=ghp_...
```

**Database Server:**
```
Name: postgres
Command: npx
Arguments: ["-y", "@anthropic-ai/database-tools-mcp"]
Environment Variables:
  DATABASE_URL=postgresql://...
```

**체크리스트:**
- [ ] MCP Servers 설정 메뉴 접근
- [ ] 각 서버 추가 및 설정
- [ ] 환경 변수 설정 (민감 정보 주의)
- [ ] Windsurf 재시작

**테스트:**
```
Windsurf AI Chat → "Can you access the database?" → MCP 연결 확인
```

---

### 5.6 Warp (AI Terminal) 설정

**Warp는 `~/.warp/mcp.json` 설정 파일을 사용합니다.**

#### 설정 파일 위치

```bash
~/.warp/mcp.json
```

#### 설정 파일 예시

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "~/"
      ]
    },
    "git": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-git"]
    }
  }
}
```

**체크리스트:**
- [ ] `~/.warp/` 디렉토리 생성 (없는 경우)
- [ ] `mcp.json` 파일 생성
- [ ] Warp 재시작

**테스트:**
```bash
# Warp 터미널에서
warp-ai "Show me recent git commits"
```

---

### 5.7 Gemini Code Assist 설정

**Google Cloud의 Gemini Code Assist는 별도 설정 파일이 필요합니다.**

#### 설정 파일 위치

```bash
~/.config/gemini-code-assist/mcp.json
```

#### 설정 파일 예시

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "${workspaceFolder}"
      ]
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "${env:GITHUB_TOKEN}"
      }
    }
  }
}
```

**체크리스트:**
- [ ] Google Cloud Console에서 Gemini API 활성화
- [ ] `~/.config/gemini-code-assist/` 디렉토리 생성
- [ ] `mcp.json` 파일 생성
- [ ] VSCode/IDE 재시작

**⚠️ 참고:**
- Gemini Code Assist는 VSCode Extension으로 제공됩니다
- MCP 지원은 최신 버전에서 제공될 수 있으므로 문서 확인 필요

---

### 5.8 Qwen Coder 설정

**Qwen Coder는 자체 설정 파일을 사용합니다.**

#### 설정 파일 위치

```bash
~/.qwen-coder/config.json
```

#### 설정 파일 예시

```json
{
  "mcp": {
    "enabled": true,
    "servers": {
      "filesystem": {
        "command": "npx",
        "args": [
          "-y",
          "@modelcontextprotocol/server-filesystem",
          "/path/to/project"
        ]
      }
    }
  }
}
```

**체크리스트:**
- [ ] Qwen Coder 설치
- [ ] `~/.qwen-coder/` 디렉토리 확인
- [ ] `config.json` 파일 수정
- [ ] Qwen Coder 재시작

---

### 5.9 Codex CLI 설정

**OpenAI Codex CLI는 환경 변수 방식으로 MCP를 설정합니다.**

#### 설정 방법

```bash
# ~/.zshrc 또는 ~/.bashrc에 추가
export CODEX_MCP_CONFIG="$HOME/.codex/mcp.json"
```

#### MCP 설정 파일

```bash
# ~/.codex/mcp.json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "${HOME}/projects"
      ]
    }
  }
}
```

**체크리스트:**
- [ ] `~/.codex/` 디렉토리 생성
- [ ] `mcp.json` 파일 생성
- [ ] 환경 변수 설정 (`~/.zshrc` 또는 `~/.bashrc`)
- [ ] 터미널 재시작: `source ~/.zshrc`

---

### 5.10 공통 MCP 서버 설정 예시

**프로젝트에서 자주 사용하는 MCP 서버 설정:**

#### Filesystem (파일 시스템 접근)

```json
{
  "filesystem": {
    "command": "npx",
    "args": [
      "-y",
      "@modelcontextprotocol/server-filesystem",
      "/absolute/path/to/project"
    ]
  }
}
```

#### GitHub (GitHub API 연동)

```json
{
  "github": {
    "command": "npx",
    "args": ["-y", "@modelcontextprotocol/server-github"],
    "env": {
      "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_..."
    }
  }
}
```

**GitHub Token 발급:**
1. GitHub → Settings → Developer settings → Personal access tokens
2. Generate new token (classic)
3. Scopes: `repo`, `read:org`, `read:user`
4. 토큰 복사 후 환경 변수에 저장

#### Database (PostgreSQL 연동)

```json
{
  "postgres": {
    "command": "npx",
    "args": ["-y", "@anthropic-ai/database-tools-mcp"],
    "env": {
      "DATABASE_URL": "postgresql://user:password@localhost:5432/dbname"
    }
  }
}
```

#### Git (Git 작업)

```json
{
  "git": {
    "command": "npx",
    "args": [
      "-y",
      "@modelcontextprotocol/server-git",
      "/path/to/repo"
    ]
  }
}
```

#### Fetch (HTTP 요청)

```json
{
  "fetch": {
    "command": "npx",
    "args": ["-y", "@modelcontextprotocol/server-fetch"]
  }
}
```

---

### 5.11 MCP 서버 테스트

**각 도구별 테스트 방법:**

| 도구 | 테스트 명령 | 예상 결과 |
|------|------------|----------|
| Claude Code | "List files in my project" | 파일 목록 출력 |
| Cursor | Cmd+K → "Show project structure" | MCP 응답 |
| Windsurf | AI Chat → "Access database" | DB 연결 확인 |
| Warp | `warp-ai "Show git log"` | Git 커밋 목록 |

**일반 테스트 체크리스트:**
- [ ] 파일 시스템 접근 테스트
- [ ] GitHub API 연동 테스트
- [ ] 데이터베이스 연결 테스트
- [ ] Git 작업 테스트
- [ ] HTTP 요청 테스트

---

### 5.12 트러블슈팅

#### 문제 1: MCP 서버 연결 실패

**증상:**
```
Error: Cannot connect to MCP server
```

**해결 방법:**
1. 설정 파일 경로 확인
2. JSON 문법 오류 검증 (쉼표, 중괄호)
3. npx 설치 확인: `npm install -g npx`
4. MCP 서버 패키지 수동 설치:
```bash
npm install -g @modelcontextprotocol/server-filesystem
npm install -g @modelcontextprotocol/server-github
```
5. AI 도구 완전 재시작 (프로세스 종료 후 재실행)

#### 문제 2: 환경 변수 인식 안됨

**증상:**
```
Error: DATABASE_URL is not defined
```

**해결 방법:**
1. 환경 변수 형식 확인: `${env:VAR_NAME}` (Cursor)
2. 직접 값 입력 (테스트용): `"DATABASE_URL": "postgresql://..."`
3. 시스템 환경 변수 설정:
```bash
# macOS/Linux
export DATABASE_URL="postgresql://..."

# Windows
set DATABASE_URL=postgresql://...
```

#### 문제 3: 권한 오류

**증상:**
```
Error: Permission denied accessing /path/to/project
```

**해결 방법:**
1. 프로젝트 경로 권한 확인
2. 절대 경로 사용 (상대 경로 대신)
3. 홈 디렉토리 변수 사용: `${HOME}/projects/my-app`

---

### 5.13 보안 주의사항

**⚠️ MCP 설정 시 보안 체크:**

- [ ] **민감 정보 분리**: DB 비밀번호, API 키는 환경 변수로 관리
- [ ] **설정 파일 .gitignore**: MCP 설정 파일을 Git에 커밋하지 않음
```bash
# .gitignore에 추가
.cursor/mcp.json
~/.claude/claude_desktop_config.json
```
- [ ] **예시 파일 생성**: 팀 공유용 `.cursor/mcp.json.example` 생성
```json
{
  "mcpServers": {
    "github": {
      "env": {
        "GITHUB_TOKEN": "YOUR_TOKEN_HERE"
      }
    }
  }
}
```
- [ ] **최소 권한 원칙**: GitHub Token은 필요한 Scope만 부여
- [ ] **로컬 DB 사용**: 프로덕션 DB 대신 로컬 개발 DB 연결

**완료 기준**: ✅ 사용하는 모든 AI 도구에 MCP 서버 설정 완료 및 테스트 통과

---

## Phase 6: 환경 변수 설정

**목표**: 모든 환경 변수를 정리하고 검증

### 6.1 환경 변수 파일 생성

#### Backend `.env` 파일 예시

```bash
# Database
DATABASE_URL="postgresql://[사용자명]:[비밀번호]@[호스트]:[포트]/[DB명]"
DIRECT_URL="[Direct connection URL]"

# BaaS (예: Supabase)
SUPABASE_URL="https://[프로젝트ID].supabase.co"
SUPABASE_ANON_KEY="[Anon Key]"
SUPABASE_SERVICE_ROLE_KEY="[Service Role Key]"

# Auth (예: Clerk)
CLERK_PUBLISHABLE_KEY="[Publishable Key]"
CLERK_SECRET_KEY="[Secret Key]"
CLERK_WEBHOOK_SECRET="[Webhook Secret]"

# OAuth 소셜 로그인
GOOGLE_CLIENT_ID="[Google Client ID]"
GOOGLE_CLIENT_SECRET="[Google Client Secret]"
KAKAO_REST_API_KEY="[Kakao REST API Key]"
KAKAO_CLIENT_SECRET="[Kakao Client Secret]"
NAVER_CLIENT_ID="[Naver Client ID]"
NAVER_CLIENT_SECRET="[Naver Client Secret]"
GITHUB_CLIENT_ID="[GitHub Client ID]"
GITHUB_CLIENT_SECRET="[GitHub Client Secret]"

# AI
OPENAI_API_KEY="sk-..."
ANTHROPIC_API_KEY="sk-ant-..."

# 결제 (선택 사항)
# Stripe (해외)
STRIPE_PUBLISHABLE_KEY="pk_test_..."
STRIPE_SECRET_KEY="sk_test_..."
STRIPE_WEBHOOK_SECRET="whsec_..."

# Toss Payments (한국)
TOSS_CLIENT_KEY="test_ck_..."
TOSS_SECRET_KEY="test_sk_..."
TOSS_WEBHOOK_SECRET="[Webhook Secret]"

# 아임포트
IAMPORT_REST_API_KEY="[REST API Key]"
IAMPORT_REST_API_SECRET="[REST API Secret]"
IAMPORT_IMP_CODE="imp[번호]"

# App
NODE_ENV="development"
PORT=3000
FRONTEND_URL="http://localhost:5173"

# Security
JWT_SECRET="[랜덤 문자열 - openssl rand -base64 32]"
SESSION_SECRET="[랜덤 문자열]"

# Monitoring
SENTRY_DSN="https://[KEY]@[ORG].ingest.sentry.io/[PROJECT_ID]"
```

#### Frontend `.env.local` 파일 예시

```bash
# API
VITE_API_URL="http://localhost:3000"
VITE_BACKEND_URL="http://localhost:3000"

# BaaS
VITE_SUPABASE_URL="https://[프로젝트ID].supabase.co"
VITE_SUPABASE_ANON_KEY="[Anon Key]"

# Auth
VITE_CLERK_PUBLISHABLE_KEY="[Publishable Key]"

# OAuth (클라이언트 키만 - VITE_ 접두사 필수!)
VITE_GOOGLE_CLIENT_ID="[Google Client ID]"
VITE_KAKAO_REST_API_KEY="[Kakao REST API Key]"
VITE_NAVER_CLIENT_ID="[Naver Client ID]"
VITE_GITHUB_CLIENT_ID="[GitHub Client ID]"

# 결제 (클라이언트 키만)
VITE_STRIPE_PUBLISHABLE_KEY="pk_test_..."
VITE_TOSS_CLIENT_KEY="test_ck_..."

# Monitoring
VITE_SENTRY_DSN="https://[KEY]@[ORG].ingest.sentry.io/[PROJECT_ID]"
```

**체크리스트:**

- [ ] Backend `.env` 파일 생성
- [ ] Frontend `.env.local` 파일 생성
- [ ] 모든 필수 환경 변수 입력 완료
- [ ] `.gitignore`에 `.env*` 추가 확인

---

### 6.2 `.env.example` 파일 생성

**Backend `.env.example`:**

```bash
# Database
DATABASE_URL=
DIRECT_URL=

# BaaS (예: Supabase)
SUPABASE_URL=
SUPABASE_ANON_KEY=
SUPABASE_SERVICE_ROLE_KEY=

# Auth (예: Clerk)
CLERK_PUBLISHABLE_KEY=
CLERK_SECRET_KEY=
CLERK_WEBHOOK_SECRET=

# AI
OPENAI_API_KEY=
ANTHROPIC_API_KEY=

# App
NODE_ENV=development
PORT=3000
FRONTEND_URL=http://localhost:5173

# Security
JWT_SECRET=
SESSION_SECRET=

# Monitoring
SENTRY_DSN=
```

**Frontend `.env.example`:**

```bash
# API
VITE_API_URL=http://localhost:3000
VITE_BACKEND_URL=http://localhost:3000

# BaaS
VITE_SUPABASE_URL=
VITE_SUPABASE_ANON_KEY=

# Auth
VITE_CLERK_PUBLISHABLE_KEY=

# Monitoring
VITE_SENTRY_DSN=
```

- [ ] `.env.example` 파일 커밋 (값 제외, 키 이름만)
- [ ] README.md에 환경 변수 설정 가이드 추가

**완료 기준**: ✅ 환경 변수 파일 생성 및 `.env.example` 커밋

---

## Phase 7: 환경 검증

**목표**: 모든 설정이 올바르게 작동하는지 검증

### 7.1 로컬 개발 환경 테스트

#### Backend 테스트

```bash
# 의존성 설치
[패키지 관리자] install

# 데이터베이스 마이그레이션 (Prisma 예시)
npx prisma migrate dev

# 개발 서버 실행
[패키지 관리자] run dev

# 서버 정상 동작 확인
curl http://localhost:3000/health
```

**체크리스트:**
- [ ] 의존성 설치 성공
- [ ] 데이터베이스 연결 성공
- [ ] 마이그레이션 적용 성공
- [ ] 개발 서버 실행 성공
- [ ] Health check 엔드포인트 응답 확인

#### Frontend 테스트

```bash
# 의존성 설치
[패키지 관리자] install

# 개발 서버 실행
[패키지 관리자] run dev

# 브라우저에서 접속
open http://localhost:5173
```

**체크리스트:**
- [ ] 의존성 설치 성공
- [ ] 개발 서버 실행 성공
- [ ] 브라우저에서 페이지 렌더링 확인
- [ ] 콘솔 에러 없음

---

### 7.2 서비스 연결 테스트

**API 키 검증:**

- [ ] [BaaS] 연결 테스트
```bash
# Supabase 예시
npx supabase db remote --db-url="[DATABASE_URL]" --json
```

- [ ] [인증 서비스] 연결 테스트
  - 로그인/회원가입 플로우 테스트

- [ ] [AI 서비스] 연결 테스트
```bash
# OpenAI API 테스트
curl https://api.openai.com/v1/models \
  -H "Authorization: Bearer $OPENAI_API_KEY"
```

- [ ] [기타 서비스] 연결 테스트

**완료 기준**: ✅ 모든 서비스 연결 정상 확인

---

### 7.3 통합 테스트

**End-to-End 테스트:**

- [ ] Backend API 호출 성공
- [ ] Frontend → Backend 통신 성공
- [ ] 데이터베이스 CRUD 동작 확인
- [ ] 인증 플로우 동작 확인

**완료 기준**: ✅ 전체 스택 통합 테스트 성공

---

## 트러블슈팅

### 문제 1: 데이터베이스 연결 실패

**증상:**
```
Error: connect ECONNREFUSED 127.0.0.1:5432
```

**해결 방법:**
1. Docker 컨테이너 실행 확인: `docker ps`
2. 포트 충돌 확인: `lsof -i :5432`
3. 연결 문자열 검증: `DATABASE_URL` 환경 변수 확인
4. 방화벽 설정 확인

---

### 문제 2: API 키 인증 실패

**증상:**
```
401 Unauthorized - Invalid API Key
```

**해결 방법:**
1. `.env` 파일에 API 키 올바르게 입력했는지 확인
2. 환경 변수 로드 확인: `console.log(process.env.API_KEY)`
3. API 키 앞뒤 공백 제거
4. 서비스 대시보드에서 API 키 재생성

---

### 문제 3: CORS 에러

**증상:**
```
Access to fetch at 'http://localhost:3000' from origin 'http://localhost:5173' has been blocked by CORS policy
```

**해결 방법:**
1. Backend CORS 설정 확인
```typescript
// NestJS 예시
app.enableCors({
  origin: process.env.FRONTEND_URL,
  credentials: true,
});
```
2. Frontend 요청에 `credentials: 'include'` 추가

---

### 문제 4: MCP 서버 연결 실패

**증상:**
- Claude Desktop에서 MCP 도구 사용 불가

**해결 방법:**
1. `claude_desktop_config.json` 경로 확인
2. JSON 문법 오류 검증 (쉼표, 중괄호)
3. MCP 서버 명령어 및 경로 확인
4. Claude Desktop 완전 재시작 (종료 후 재실행)
5. MCP 서버 로그 확인

---

## 보안 체크리스트

**최종 보안 점검:**

- [ ] `.env` 파일이 `.gitignore`에 포함됨
- [ ] API 키가 코드에 하드코딩되지 않음
- [ ] `service_role_key` 등 민감한 키는 서버에서만 사용
- [ ] 프로덕션 환경과 개발 환경 API 키 분리
- [ ] Webhook Secret 설정 완료
- [ ] CORS 설정이 프로덕션 도메인만 허용
- [ ] JWT Secret이 충분히 복잡함 (최소 32자)
- [ ] API Rate Limiting 설정 (프로덕션)
- [ ] 데이터베이스 접근 권한 최소화
- [ ] MCP 서버 설정에 민감 정보 환경 변수화

**완료 기준**: ✅ 모든 보안 항목 검증 완료

---

## 최종 체크리스트

### 환경 설정 완료 확인

**Phase별 완료 상태:**

| Phase | 내용 | 상태 | 완료 날짜 |
|-------|------|------|-----------|
| Phase 0 | 사전 준비 | [ ] | - |
| Phase 1 | 로컬 도구 설치 | [ ] | - |
| Phase 2 | 서비스 계정 생성 | [ ] | - |
| Phase 3 | API 키 수집 | [ ] | - |
| Phase 4 | 데이터베이스 설정 | [ ] | - |
| Phase 5 | MCP 서버 설정 | [ ] | - |
| Phase 6 | 환경 변수 설정 | [ ] | - |
| Phase 7 | 환경 검증 | [ ] | - |

**전체 완료 기준**: ✅ 모든 Phase 완료, 개발 환경 검증 성공

---

## 다음 단계

**환경 설정 완료 후:**

1. **PLAN.md 진행** - Week 1 Day 1부터 개발 시작
2. **Git 저장소 초기화** - 초기 커밋 생성
3. **팀원과 환경 설정 공유** - `.env.example` 및 이 문서 공유
4. **개발 시작!** 🚀

---

**작성일**: YYYY-MM-DD
**최종 수정일**: YYYY-MM-DD
**검증자**: [이름]
**상태**: ⏳ 진행 중 / ✅ 완료

---

## 참고 자료

- [서비스] 공식 문서: [URL]
- [도구] 설치 가이드: [URL]
- [프레임워크] 환경 변수 가이드: [URL]
- MCP 서버 설정 가이드: [URL]
