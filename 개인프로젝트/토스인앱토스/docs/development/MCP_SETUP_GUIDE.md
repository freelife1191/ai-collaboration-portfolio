# MCP 설정 가이드

## 목차

- [MCP란?](#mcp란)
- [기본 설정 방법](#기본-설정-방법)
  - [Claude Code에서 MCP 설정](#claude-code에서-mcp-설정)
  - [Cursor에서 MCP 설정](#cursor에서-mcp-설정)
  - [Windsurf에서 MCP 설정](#windsurf에서-mcp-설정)
  - [VS Code에서 MCP 설정](#vs-code에서-mcp-설정)
  - [Zed에서 MCP 설정](#zed에서-mcp-설정)
  - [Neovim에서 MCP 설정](#neovim에서-mcp-설정)
- [서비스별 MCP 설정](#서비스별-mcp-설정)
  - [1. Supabase MCP](#1-supabase-mcp)
  - [2. MongoDB Atlas MCP](#2-mongodb-atlas-mcp)
  - [3. Firebase Firestore MCP](#3-firebase-firestore-mcp)
  - [4. Convex MCP](#4-convex-mcp)
  - [5. Clerk MCP](#5-clerk-mcp)
  - [6. OpenAI API MCP](#6-openai-api-mcp)
  - [7. GitHub MCP](#7-github-mcp)
  - [8. PostgreSQL MCP](#8-postgresql-mcp)
  - [9. Redis MCP](#9-redis-mcp)
  - [10. Stripe MCP](#10-stripe-mcp)
- [트러블슈팅](#트러블슈팅)
- [참고 자료](#참고-자료)

---

## MCP란?

**MCP (Model Context Protocol)**는 AI 모델이 외부 데이터 소스, 도구, API와 안전하게 상호작용할 수 있도록 하는 표준 프로토콜입니다.

### 주요 특징

- **표준화된 연결**: 다양한 서비스를 일관된 방식으로 연결
- **보안**: 환경 변수를 통한 안전한 인증 정보 관리
- **확장성**: 필요한 서비스를 언제든지 추가/제거 가능
- **IDE 통합**: Claude Code, Cursor, Windsurf 등 주요 AI 코딩 도구 지원

### MCP가 필요한 이유

1. **데이터베이스 직접 접근**: AI가 실시간으로 DB 쿼리 및 수정
2. **API 통합**: 외부 서비스와의 자동화된 상호작용
3. **컨텍스트 확장**: AI의 작업 범위를 프로젝트 외부로 확장
4. **워크플로우 자동화**: 반복적인 작업을 AI가 자동으로 처리

---

## 기본 설정 방법

MCP 설정은 일반적으로 JSON 설정 파일을 통해 이루어집니다. 각 IDE/에디터마다 설정 파일의 위치와 이름이 다를 수 있습니다.

### Claude Code에서 MCP 설정

Claude Code는 프로젝트 루트의 `.mcp.json` 파일을 사용합니다.

#### 1. 설정 파일 생성

프로젝트 루트에 `.mcp.json` 파일을 생성합니다:

```json
{
  "mcpServers": {}
}
```

#### 2. MCP 서버 추가

필요한 서비스를 `mcpServers` 객체에 추가합니다:

```json
{
  "mcpServers": {
    "service-name": {
      "command": "npx",
      "args": ["-y", "@package/mcp-server"],
      "env": {
        "API_KEY": "${env:API_KEY}"
      }
    }
  }
}
```

#### 3. 환경 변수 설정

프로젝트 루트에 `.env` 파일을 생성하고 필요한 환경 변수를 추가합니다:

```bash
API_KEY=your_api_key_here
```

#### 4. Claude Code 재시작

설정 변경 후 Claude Code를 재시작하여 MCP 연결을 활성화합니다.

**참고**: `.env` 파일과 `.mcp.json` 파일은 `.gitignore`에 추가하여 민감한 정보가 공개되지 않도록 합니다.

---

### Cursor에서 MCP 설정

Cursor는 전역 설정과 프로젝트별 설정을 모두 지원합니다.

#### 1. 전역 설정 방법

Cursor의 전역 설정 파일 위치:

- **macOS**: `~/Library/Application Support/Cursor/User/globalStorage/mcp-settings.json`
- **Windows**: `%APPDATA%\Cursor\User\globalStorage\mcp-settings.json`
- **Linux**: `~/.config/Cursor/User/globalStorage/mcp-settings.json`

#### 2. 프로젝트별 설정 방법

프로젝트 루트에 `.cursor/mcp.json` 파일 생성:

```json
{
  "mcpServers": {
    "service-name": {
      "command": "npx",
      "args": ["-y", "@package/mcp-server"],
      "env": {
        "API_KEY": "${env:API_KEY}"
      }
    }
  }
}
```

#### 3. Cursor 설정에서 MCP 활성화

1. Cursor 설정 열기 (`Cmd/Ctrl + ,`)
2. "MCP" 검색
3. "Enable MCP Servers" 체크
4. Cursor 재시작

---

### Windsurf에서 MCP 설정

Windsurf는 VS Code 기반이며 유사한 설정 방식을 사용합니다.

#### 1. 설정 파일 위치

- **macOS**: `~/Library/Application Support/Windsurf/User/settings.json`
- **Windows**: `%APPDATA%\Windsurf\User\settings.json`
- **Linux**: `~/.config/Windsurf/User/settings.json`

#### 2. 설정 추가

`settings.json`에 MCP 설정 추가:

```json
{
  "mcp.servers": {
    "service-name": {
      "command": "npx",
      "args": ["-y", "@package/mcp-server"],
      "env": {
        "API_KEY": "${env:API_KEY}"
      }
    }
  }
}
```

#### 3. 프로젝트별 설정

프로젝트 루트에 `.vscode/settings.json` 생성:

```json
{
  "mcp.servers": {
    "service-name": {
      "command": "npx",
      "args": ["-y", "@package/mcp-server"],
      "env": {
        "API_KEY": "${env:API_KEY}"
      }
    }
  }
}
```

---

### VS Code에서 MCP 설정

VS Code에서 MCP를 사용하려면 MCP 확장 프로그램이 필요합니다.

#### 1. MCP 확장 프로그램 설치

1. VS Code 확장 마켓플레이스 열기
2. "Model Context Protocol" 검색
3. 확장 프로그램 설치

#### 2. 설정 파일 위치

- **macOS**: `~/Library/Application Support/Code/User/settings.json`
- **Windows**: `%APPDATA%\Code\User\settings.json`
- **Linux**: `~/.config/Code/User/settings.json`

#### 3. 설정 추가

```json
{
  "mcp.servers": {
    "service-name": {
      "command": "npx",
      "args": ["-y", "@package/mcp-server"],
      "env": {
        "API_KEY": "${env:API_KEY}"
      }
    }
  }
}
```

---

### Zed에서 MCP 설정

Zed 에디터는 자체적인 MCP 설정 방식을 사용합니다.

#### 1. 설정 파일 위치

- **macOS**: `~/.config/zed/settings.json`
- **Linux**: `~/.config/zed/settings.json`

#### 2. 설정 추가

```json
{
  "mcp": {
    "servers": {
      "service-name": {
        "command": "npx",
        "args": ["-y", "@package/mcp-server"],
        "env": {
          "API_KEY": "${env:API_KEY}"
        }
      }
    }
  }
}
```

#### 3. Zed 재시작

설정 변경 후 Zed를 재시작합니다.

---

### Neovim에서 MCP 설정

Neovim에서 MCP를 사용하려면 플러그인이 필요합니다.

#### 1. MCP 플러그인 설치

`lazy.nvim` 사용 시:

```lua
{
  "anthropics/mcp.nvim",
  dependencies = {
    "nvim-lua/plenary.nvim"
  },
  config = function()
    require("mcp").setup({
      servers = {
        ["service-name"] = {
          command = "npx",
          args = { "-y", "@package/mcp-server" },
          env = {
            API_KEY = os.getenv("API_KEY")
          }
        }
      }
    })
  end
}
```

#### 2. 환경 변수 설정

`~/.zshrc` 또는 `~/.bashrc`에 추가:

```bash
export API_KEY="your_api_key_here"
```

#### 3. Neovim 재시작

설정 변경 후 Neovim을 재시작합니다.

---

## 서비스별 MCP 설정

### 1. Supabase MCP

Supabase MCP를 사용하면 AI가 Supabase 데이터베이스와 직접 상호작용할 수 있습니다.

#### 설치

Supabase MCP는 npx를 통해 자동으로 설치됩니다.

#### 설정

`.mcp.json`에 다음 설정 추가:

```json
{
  "mcpServers": {
    "supabase": {
      "command": "npx",
      "args": [
        "-y",
        "@supabase/mcp-server-supabase@latest",
        "--project-ref=<YOUR_PROJECT_REF>"
      ],
      "env": {
        "SUPABASE_ACCESS_TOKEN": "${env:SUPABASE_ACCESS_TOKEN}"
      }
    }
  }
}
```

#### ACCESS_TOKEN 발급 방법

1. **Supabase 대시보드 접속**
   - https://app.supabase.com 접속
   - 로그인

2. **프로젝트 선택**
   - 좌측 메뉴에서 원하는 프로젝트 선택

3. **Project Settings 이동**
   - 좌측 하단의 톱니바퀴 아이콘(Settings) 클릭
   - "Project Settings" 선택

4. **API Keys 확인**
   - 좌측 메뉴에서 "API" 선택
   - "Project URL" 복사 (project-ref는 URL에서 확인 가능)
   - "Project API keys" 섹션에서 `service_role` 키 복사

5. **Access Token 생성**
   - 우측 상단 프로필 아이콘 클릭
   - "Account Settings" 선택
   - "Access Tokens" 탭 클릭
   - "Generate New Token" 버튼 클릭
   - 토큰 이름 입력 (예: "MCP Access")
   - 필요한 권한 선택:
     - ✅ Read access to projects
     - ✅ Write access to projects
     - ✅ Read access to organizations
   - "Generate Token" 클릭
   - **생성된 토큰을 즉시 복사** (한 번만 표시됨!)

6. **환경 변수 설정**

`.env` 파일에 추가:

```bash
SUPABASE_ACCESS_TOKEN=sbp_your_access_token_here
```

#### 프로젝트 REF 확인 방법

프로젝트 REF는 Supabase 프로젝트 URL에서 확인할 수 있습니다:

```
https://<PROJECT_REF>.supabase.co
```

예시:
```
https://abcdefghijklmnop.supabase.co
프로젝트 REF: abcdefghijklmnop
```

#### 테스트

MCP 연결 테스트를 위한 명령어:

```bash
# 프로젝트 정보 확인
npx @supabase/mcp-server-supabase@latest --project-ref=<YOUR_PROJECT_REF> list-tables
```

AI 에디터에서 테스트:

```
"Show me all tables in my Supabase database"
"Query the users table and show me the first 5 records"
```

---

### 2. MongoDB Atlas MCP

MongoDB Atlas MCP를 사용하여 AI가 MongoDB 데이터베이스에 접근할 수 있습니다.

#### 설치

MongoDB Atlas MCP 서버 설치:

```bash
npm install -g @mongodb/mcp-server-mongodb
```

또는 npx 사용 (설치 불필요):

```bash
npx @mongodb/mcp-server-mongodb
```

#### 설정

`.mcp.json`에 다음 설정 추가:

```json
{
  "mcpServers": {
    "mongodb": {
      "command": "npx",
      "args": [
        "-y",
        "@mongodb/mcp-server-mongodb@latest"
      ],
      "env": {
        "MONGODB_URI": "${env:MONGODB_URI}",
        "MONGODB_DATABASE": "${env:MONGODB_DATABASE}"
      }
    }
  }
}
```

#### MongoDB Atlas 연결 URI 및 API Key 발급

1. **MongoDB Atlas 접속**
   - https://cloud.mongodb.com 접속
   - 로그인

2. **클러스터 생성 (없는 경우)**
   - "Build a Database" 클릭
   - 무료 Shared 클러스터 선택
   - 클라우드 제공자 및 리전 선택
   - "Create" 클릭

3. **데이터베이스 사용자 생성**
   - 좌측 메뉴에서 "Database Access" 선택
   - "Add New Database User" 클릭
   - Authentication Method: Password 선택
   - Username과 Password 입력
   - Database User Privileges: "Atlas admin" 선택 (또는 필요한 권한)
   - "Add User" 클릭

4. **네트워크 접근 설정**
   - 좌측 메뉴에서 "Network Access" 선택
   - "Add IP Address" 클릭
   - "Allow Access from Anywhere" 선택 (0.0.0.0/0)
     - *보안을 위해 실제 운영 환경에서는 특정 IP만 허용 권장*
   - "Confirm" 클릭

5. **연결 문자열 가져오기**
   - 좌측 메뉴에서 "Database" 선택
   - 클러스터의 "Connect" 버튼 클릭
   - "Connect your application" 선택
   - Driver: Node.js 선택
   - Connection string 복사:
     ```
     mongodb+srv://<username>:<password>@cluster0.xxxxx.mongodb.net/?retryWrites=true&w=majority
     ```
   - `<username>`과 `<password>`를 실제 값으로 교체

6. **환경 변수 설정**

`.env` 파일에 추가:

```bash
MONGODB_URI=mongodb+srv://username:password@cluster0.xxxxx.mongodb.net/?retryWrites=true&w=majority
MONGODB_DATABASE=your_database_name
```

#### 테스트

```bash
# 연결 테스트
npx @mongodb/mcp-server-mongodb --test
```

AI 에디터에서 테스트:

```
"List all collections in my MongoDB database"
"Query the products collection and show me 10 documents"
```

---

### 3. Firebase Firestore MCP

Firebase Firestore MCP를 통해 AI가 Firestore 데이터베이스에 접근할 수 있습니다.

#### 설치

Firebase Admin SDK 설치:

```bash
npm install -g firebase-admin
npm install -g @firebase/mcp-server-firestore
```

또는 npx 사용:

```bash
npx @firebase/mcp-server-firestore
```

#### 설정

`.mcp.json`에 다음 설정 추가:

```json
{
  "mcpServers": {
    "firestore": {
      "command": "npx",
      "args": [
        "-y",
        "@firebase/mcp-server-firestore@latest"
      ],
      "env": {
        "FIREBASE_PROJECT_ID": "${env:FIREBASE_PROJECT_ID}",
        "FIREBASE_SERVICE_ACCOUNT": "${env:FIREBASE_SERVICE_ACCOUNT}"
      }
    }
  }
}
```

#### Firebase Service Account Key 발급

1. **Firebase Console 접속**
   - https://console.firebase.google.com 접속
   - 로그인

2. **프로젝트 선택 또는 생성**
   - 기존 프로젝트 선택 또는
   - "Add project" 클릭하여 새 프로젝트 생성

3. **프로젝트 설정 이동**
   - 좌측 상단 톱니바퀴 아이콘 클릭
   - "Project settings" 선택

4. **Service Account 생성**
   - "Service accounts" 탭 선택
   - "Generate new private key" 버튼 클릭
   - 경고 메시지 확인 후 "Generate key" 클릭
   - JSON 파일이 자동으로 다운로드됨

5. **JSON 파일 저장**
   - 다운로드된 JSON 파일을 프로젝트 루트에 저장
   - 파일명 예: `firebase-service-account.json`
   - ⚠️ **주의**: 이 파일은 절대 Git에 커밋하지 마세요!

6. **환경 변수 설정**

방법 1: JSON 파일 경로 사용

`.env` 파일에 추가:

```bash
FIREBASE_PROJECT_ID=your-project-id
FIREBASE_SERVICE_ACCOUNT=/absolute/path/to/firebase-service-account.json
```

방법 2: JSON 내용을 Base64로 인코딩

```bash
# macOS/Linux
cat firebase-service-account.json | base64

# Windows PowerShell
[Convert]::ToBase64String([System.IO.File]::ReadAllBytes("firebase-service-account.json"))
```

`.env` 파일에 추가:

```bash
FIREBASE_PROJECT_ID=your-project-id
FIREBASE_SERVICE_ACCOUNT_BASE64=<base64_encoded_json>
```

7. **.gitignore 업데이트**

```gitignore
# Firebase
firebase-service-account.json
*-service-account.json
```

#### Firestore 데이터베이스 활성화

1. Firebase Console에서 "Firestore Database" 선택
2. "Create database" 클릭
3. 보안 규칙 선택:
   - Production mode (보안 규칙 적용)
   - Test mode (개발용, 30일 후 만료)
4. 위치 선택 (가장 가까운 지역)
5. "Enable" 클릭

#### 테스트

AI 에디터에서 테스트:

```
"Show me all collections in Firestore"
"Query the users collection"
"Create a new document in the posts collection"
```

---

### 4. Convex MCP

Convex MCP를 사용하여 AI가 Convex 백엔드와 상호작용할 수 있습니다.

#### 설치

Convex MCP 서버는 npx를 통해 자동으로 설치됩니다.

#### 설정

`.mcp.json`에 다음 설정 추가:

```json
{
  "mcpServers": {
    "convex": {
      "command": "npx",
      "args": [
        "-y",
        "@convex-dev/mcp-server-convex@latest"
      ],
      "env": {
        "CONVEX_URL": "${env:CONVEX_URL}",
        "CONVEX_DEPLOY_KEY": "${env:CONVEX_DEPLOY_KEY}"
      }
    }
  }
}
```

#### Convex Deploy Key 발급

1. **Convex Dashboard 접속**
   - https://dashboard.convex.dev 접속
   - GitHub 또는 Google 계정으로 로그인

2. **프로젝트 생성 (없는 경우)**
   - "Create a project" 클릭
   - 프로젝트 이름 입력
   - "Create" 클릭

3. **프로젝트 선택**
   - 대시보드에서 해당 프로젝트 선택

4. **Settings 이동**
   - 좌측 메뉴에서 "Settings" 클릭
   - "Deploy Keys" 섹션 찾기

5. **Deploy Key 생성**
   - "Generate a deploy key" 버튼 클릭
   - Key 이름 입력 (예: "MCP Access")
   - 권한 선택:
     - ✅ Read
     - ✅ Write
     - ✅ Deploy
   - "Generate" 클릭
   - **생성된 키를 즉시 복사** (한 번만 표시됨!)

6. **Deployment URL 확인**
   - Settings 페이지 상단의 "Deployment URL" 복사
   - 형식: `https://your-project.convex.cloud`

7. **환경 변수 설정**

`.env` 파일에 추가:

```bash
CONVEX_URL=https://your-project.convex.cloud
CONVEX_DEPLOY_KEY=your_deploy_key_here
```

#### 로컬 개발 설정

Convex 로컬 개발을 위한 추가 설정:

```bash
# Convex CLI 설치
npm install -g convex

# 프로젝트 초기화
npx convex dev
```

`.env.local` 파일에 추가:

```bash
CONVEX_URL=http://localhost:3210
```

#### 테스트

AI 에디터에서 테스트:

```
"Show me all tables in my Convex database"
"Query the messages table"
"List all Convex functions"
```

---

### 5. Clerk MCP

Clerk MCP를 사용하여 AI가 인증 및 사용자 관리 작업을 수행할 수 있습니다.

#### 설치

Clerk MCP 서버는 npx를 통해 자동으로 설치됩니다.

#### 설정

`.mcp.json`에 다음 설정 추가:

```json
{
  "mcpServers": {
    "clerk": {
      "command": "npx",
      "args": [
        "-y",
        "@clerk/mcp-server-clerk@latest"
      ],
      "env": {
        "CLERK_SECRET_KEY": "${env:CLERK_SECRET_KEY}",
        "CLERK_PUBLISHABLE_KEY": "${env:CLERK_PUBLISHABLE_KEY}"
      }
    }
  }
}
```

#### Clerk API Keys 발급

1. **Clerk Dashboard 접속**
   - https://dashboard.clerk.com 접속
   - 로그인 또는 회원가입

2. **애플리케이션 선택 또는 생성**
   - 기존 애플리케이션 선택 또는
   - "Create Application" 클릭
   - 애플리케이션 이름 입력
   - 지원할 인증 방법 선택:
     - Email
     - Google
     - GitHub
     - 기타 소셜 로그인
   - "Create Application" 클릭

3. **API Keys 페이지 이동**
   - 좌측 메뉴에서 "API Keys" 선택
   - 또는 우측 상단 "Configure" → "API Keys"

4. **Keys 확인 및 복사**
   - **Publishable Key**:
     - 프론트엔드에서 사용
     - 공개해도 안전
     - 형식: `pk_test_...` (개발) 또는 `pk_live_...` (프로덕션)

   - **Secret Key**:
     - 백엔드에서 사용
     - ⚠️ **절대 공개하지 마세요!**
     - 형식: `sk_test_...` (개발) 또는 `sk_live_...` (프로덕션)

   - 각 키 옆의 "Copy" 버튼 클릭하여 복사

5. **환경 변수 설정**

`.env` 파일에 추가:

```bash
CLERK_SECRET_KEY=sk_test_your_secret_key_here
CLERK_PUBLISHABLE_KEY=pk_test_your_publishable_key_here
```

#### 프론트엔드 환경 변수 (Next.js 예시)

`.env.local` 파일에 추가:

```bash
NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=pk_test_your_publishable_key_here
CLERK_SECRET_KEY=sk_test_your_secret_key_here
```

#### Webhook 설정 (선택사항)

Clerk 이벤트를 수신하려면:

1. Dashboard에서 "Webhooks" 선택
2. "Add Endpoint" 클릭
3. Endpoint URL 입력 (예: `https://your-domain.com/api/webhooks/clerk`)
4. 수신할 이벤트 선택:
   - user.created
   - user.updated
   - session.created
   - 등등
5. Signing Secret 복사하여 환경 변수에 추가:

```bash
CLERK_WEBHOOK_SECRET=whsec_your_webhook_secret
```

#### 테스트

AI 에디터에서 테스트:

```
"List all users in my Clerk application"
"Show me user details for user ID: user_xxx"
"Get the total number of users"
```

---

### 6. OpenAI API MCP

OpenAI API MCP를 사용하여 AI가 OpenAI의 다양한 모델과 기능에 접근할 수 있습니다.

#### 설치

OpenAI MCP 서버는 npx를 통해 자동으로 설치됩니다.

#### 설정

`.mcp.json`에 다음 설정 추가:

```json
{
  "mcpServers": {
    "openai": {
      "command": "npx",
      "args": [
        "-y",
        "@openai/mcp-server-openai@latest"
      ],
      "env": {
        "OPENAI_API_KEY": "${env:OPENAI_API_KEY}",
        "OPENAI_ORGANIZATION": "${env:OPENAI_ORGANIZATION}"
      }
    }
  }
}
```

#### OpenAI API Key 발급

1. **OpenAI Platform 접속**
   - https://platform.openai.com 접속
   - 로그인 또는 회원가입

2. **계정 설정**
   - 전화번호 인증 완료
   - 결제 방법 등록 (신용카드)
   - 최소 $5 크레딧 충전 권장

3. **API Keys 페이지 이동**
   - 우측 상단 프로필 아이콘 클릭
   - "View API keys" 선택
   - 또는 직접 URL 접속: https://platform.openai.com/api-keys

4. **새 API Key 생성**
   - "Create new secret key" 버튼 클릭
   - Key 이름 입력 (예: "MCP Integration")
   - 권한 설정 (선택사항):
     - All (모든 권한)
     - Custom (세부 권한 설정)
   - "Create secret key" 클릭

5. **API Key 복사**
   - ⚠️ **매우 중요**: 생성된 키는 **한 번만 표시**됩니다!
   - 즉시 안전한 곳에 복사하여 저장
   - 형식: `sk-...` (약 51자)

6. **Organization ID 확인 (선택사항)**
   - 여러 조직에 속한 경우 필요
   - Settings → Organization → Organization ID 복사
   - 형식: `org-...`

7. **사용량 제한 설정 (권장)**
   - Settings → Billing → Usage limits
   - 월별 사용 한도 설정하여 예상치 못한 과금 방지

8. **환경 변수 설정**

`.env` 파일에 추가:

```bash
OPENAI_API_KEY=sk-your_api_key_here
# 조직에 속한 경우만 필요
OPENAI_ORGANIZATION=org-your_organization_id
```

#### 모델별 사용 요금 (2025년 1월 기준)

- **GPT-4 Turbo**: $0.01 / 1K tokens (입력), $0.03 / 1K tokens (출력)
- **GPT-4**: $0.03 / 1K tokens (입력), $0.06 / 1K tokens (출력)
- **GPT-3.5 Turbo**: $0.0015 / 1K tokens (입력), $0.002 / 1K tokens (출력)
- **DALL-E 3**: $0.040 - $0.120 per image
- **Whisper**: $0.006 / minute

#### 테스트

AI 에디터에서 테스트:

```
"Generate an image with DALL-E: a cat wearing a hat"
"Use GPT-4 to summarize this text: [your text]"
"Check my OpenAI API usage and remaining credits"
```

프로그래밍 테스트:

```bash
curl https://api.openai.com/v1/models \
  -H "Authorization: Bearer $OPENAI_API_KEY"
```

---

### 7. GitHub MCP

GitHub MCP를 사용하여 AI가 GitHub 저장소, 이슈, PR 등을 관리할 수 있습니다.

#### 설치

GitHub MCP 서버는 npx를 통해 자동으로 설치됩니다.

#### 설정

`.mcp.json`에 다음 설정 추가:

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": [
        "-y",
        "@github/mcp-server-github@latest"
      ],
      "env": {
        "GITHUB_TOKEN": "${env:GITHUB_TOKEN}"
      }
    }
  }
}
```

#### GitHub Personal Access Token 발급

1. **GitHub 접속 및 로그인**
   - https://github.com 접속
   - 로그인

2. **Settings 이동**
   - 우측 상단 프로필 아이콘 클릭
   - "Settings" 선택

3. **Developer settings 이동**
   - 좌측 하단 "Developer settings" 클릭
   - 또는 직접 URL 접속: https://github.com/settings/apps

4. **Personal access tokens 선택**
   - "Personal access tokens" → "Tokens (classic)" 선택
   - 또는 "Fine-grained tokens" 선택 (더 세밀한 권한 제어)

#### Classic Token 생성

1. **"Generate new token" 클릭**
   - "Generate new token (classic)" 선택

2. **토큰 정보 입력**
   - Note: 토큰 용도 입력 (예: "MCP Integration")
   - Expiration: 만료 기간 선택
     - 30 days
     - 60 days
     - 90 days
     - Custom
     - No expiration (권장하지 않음)

3. **권한(Scopes) 선택**

   **저장소 접근용 (기본)**:
   - ✅ `repo` - 전체 저장소 제어
     - repo:status
     - repo_deployment
     - public_repo
     - repo:invite
     - security_events

   **조직 접근용**:
   - ✅ `admin:org` - 조직 전체 제어
     - write:org
     - read:org

   **사용자 정보**:
   - ✅ `user` - 사용자 정보 접근
     - read:user
     - user:email

   **워크플로우**:
   - ✅ `workflow` - GitHub Actions 워크플로우 제어

   **패키지**:
   - ✅ `write:packages` - 패키지 업로드
   - ✅ `read:packages` - 패키지 다운로드

4. **토큰 생성**
   - 페이지 하단 "Generate token" 클릭
   - ⚠️ **생성된 토큰을 즉시 복사** (한 번만 표시됨!)
   - 형식: `ghp_...` (약 40자)

#### Fine-grained Token 생성 (더 안전)

1. **"Generate new token" 클릭**
   - "Generate new token (fine-grained)" 선택

2. **토큰 정보 입력**
   - Token name: 토큰 이름
   - Expiration: 만료 기간
   - Description: 토큰 설명
   - Resource owner: 본인 또는 조직 선택

3. **Repository access 선택**
   - All repositories (모든 저장소)
   - Only select repositories (특정 저장소만)

4. **Permissions 설정**

   **Repository permissions**:
   - Contents: Read and write
   - Issues: Read and write
   - Pull requests: Read and write
   - Metadata: Read-only (자동 선택)
   - Actions: Read and write
   - Workflows: Read and write

   **Organization permissions** (조직 소유 저장소):
   - Members: Read
   - Administration: Read

5. **토큰 생성**
   - "Generate token" 클릭
   - 생성된 토큰 복사

6. **환경 변수 설정**

`.env` 파일에 추가:

```bash
GITHUB_TOKEN=ghp_your_personal_access_token_here
```

#### GitHub CLI 설정 (선택사항)

GitHub CLI를 함께 사용하면 더 편리합니다:

```bash
# GitHub CLI 설치 (macOS)
brew install gh

# 인증
gh auth login

# 토큰 사용 설정
gh auth setup-git
```

#### 테스트

AI 에디터에서 테스트:

```
"List all my GitHub repositories"
"Show me open issues in repository owner/repo-name"
"Create a new issue in my repository"
"Get the latest commit from the main branch"
```

명령줄 테스트:

```bash
curl -H "Authorization: token $GITHUB_TOKEN" \
  https://api.github.com/user
```

---

### 8. PostgreSQL MCP

PostgreSQL MCP를 사용하여 AI가 PostgreSQL 데이터베이스에 직접 접근할 수 있습니다.

#### 설치

```bash
npm install -g @postgresql/mcp-server-postgresql
```

또는 npx 사용:

```bash
npx @postgresql/mcp-server-postgresql
```

#### 설정

`.mcp.json`에 다음 설정 추가:

```json
{
  "mcpServers": {
    "postgresql": {
      "command": "npx",
      "args": [
        "-y",
        "@postgresql/mcp-server-postgresql@latest"
      ],
      "env": {
        "POSTGRES_URI": "${env:POSTGRES_URI}"
      }
    }
  }
}
```

#### PostgreSQL 연결 URI 구성

PostgreSQL 연결 URI 형식:

```
postgresql://[user[:password]@][host][:port][/dbname][?param1=value1&...]
```

예시:

```bash
# 로컬 데이터베이스
postgresql://postgres:password@localhost:5432/mydb

# 원격 데이터베이스
postgresql://user:pass@db.example.com:5432/production

# SSL 연결
postgresql://user:pass@db.example.com:5432/mydb?sslmode=require

# 연결 풀 설정
postgresql://user:pass@db.example.com:5432/mydb?pool_max=10&pool_min=2
```

#### 환경 변수 설정

`.env` 파일에 추가:

```bash
POSTGRES_URI=postgresql://username:password@host:port/database
```

개별 설정으로 분리 (대안):

```bash
POSTGRES_HOST=localhost
POSTGRES_PORT=5432
POSTGRES_USER=postgres
POSTGRES_PASSWORD=your_password
POSTGRES_DATABASE=your_database
POSTGRES_SSL=false
```

#### 테스트

```bash
# 연결 테스트
psql $POSTGRES_URI -c "SELECT version();"
```

AI 에디터에서 테스트:

```
"Show me all tables in the PostgreSQL database"
"Query the users table and show the schema"
"Count the number of records in each table"
```

---

### 9. Redis MCP

Redis MCP를 사용하여 AI가 Redis 캐시와 상호작용할 수 있습니다.

#### 설치

```bash
npm install -g @redis/mcp-server-redis
```

#### 설정

`.mcp.json`에 다음 설정 추가:

```json
{
  "mcpServers": {
    "redis": {
      "command": "npx",
      "args": [
        "-y",
        "@redis/mcp-server-redis@latest"
      ],
      "env": {
        "REDIS_URL": "${env:REDIS_URL}"
      }
    }
  }
}
```

#### Redis 연결 URL 구성

Redis 연결 URL 형식:

```
redis://[username][:password]@[host][:port][/database]
```

예시:

```bash
# 로컬 Redis (인증 없음)
redis://localhost:6379

# 비밀번호 인증
redis://:password@localhost:6379

# 원격 Redis
redis://:password@redis.example.com:6379/0

# Redis SSL/TLS
rediss://:password@redis.example.com:6380
```

#### 환경 변수 설정

`.env` 파일에 추가:

```bash
REDIS_URL=redis://:your_password@localhost:6379
```

#### Redis Cloud 사용 (Upstash 예시)

1. **Upstash 접속**
   - https://upstash.com 접속
   - 로그인 또는 회원가입

2. **Redis 데이터베이스 생성**
   - "Create Database" 클릭
   - 이름 입력
   - 리전 선택
   - "Create" 클릭

3. **연결 정보 복사**
   - "Redis Connect" 섹션에서 연결 URL 복사
   - 형식: `rediss://default:***@global-***-***.upstash.io:6379`

4. **환경 변수 설정**

```bash
REDIS_URL=rediss://default:your_token@global-xxx.upstash.io:6379
```

#### 테스트

```bash
# Redis CLI 테스트
redis-cli -u $REDIS_URL ping
```

AI 에디터에서 테스트:

```
"Get all keys from Redis"
"Set a value in Redis: key='test', value='hello'"
"Get the value for key 'test' from Redis"
```

---

### 10. Stripe MCP

Stripe MCP를 사용하여 AI가 결제 처리 및 구독 관리를 수행할 수 있습니다.

#### 설치

```bash
npm install -g @stripe/mcp-server-stripe
```

#### 설정

`.mcp.json`에 다음 설정 추가:

```json
{
  "mcpServers": {
    "stripe": {
      "command": "npx",
      "args": [
        "-y",
        "@stripe/mcp-server-stripe@latest"
      ],
      "env": {
        "STRIPE_SECRET_KEY": "${env:STRIPE_SECRET_KEY}",
        "STRIPE_PUBLISHABLE_KEY": "${env:STRIPE_PUBLISHABLE_KEY}"
      }
    }
  }
}
```

#### Stripe API Keys 발급

1. **Stripe Dashboard 접속**
   - https://dashboard.stripe.com 접속
   - 로그인 또는 회원가입

2. **개발자 메뉴 이동**
   - 우측 상단 "Developers" 클릭
   - "API keys" 선택

3. **테스트 모드 vs 라이브 모드**
   - 우측 상단 토글로 모드 전환
   - **Test mode**: 개발 및 테스트용 (실제 결제 없음)
   - **Live mode**: 실제 운영용 (실제 결제 발생)

4. **API Keys 확인**

   **Publishable key** (공개 키):
   - 프론트엔드에서 사용
   - 공개해도 안전
   - Test: `pk_test_...`
   - Live: `pk_live_...`

   **Secret key** (비밀 키):
   - 백엔드에서 사용
   - ⚠️ **절대 공개하지 마세요!**
   - Test: `sk_test_...`
   - Live: `sk_live_...`
   - "Reveal test key token" 클릭하여 확인

5. **Webhook Signing Secret (선택사항)**
   - "Webhooks" 탭 선택
   - "Add endpoint" 클릭
   - Endpoint URL 입력: `https://your-domain.com/api/webhooks/stripe`
   - 수신할 이벤트 선택:
     - payment_intent.succeeded
     - customer.subscription.created
     - customer.subscription.updated
     - invoice.payment_succeeded
     - 등등
   - Signing secret 복사: `whsec_...`

6. **환경 변수 설정**

개발 환경 (`.env.local`):

```bash
STRIPE_SECRET_KEY=sk_test_your_test_secret_key
STRIPE_PUBLISHABLE_KEY=pk_test_your_test_publishable_key
STRIPE_WEBHOOK_SECRET=whsec_your_webhook_secret
```

프로덕션 환경 (`.env.production`):

```bash
STRIPE_SECRET_KEY=sk_live_your_live_secret_key
STRIPE_PUBLISHABLE_KEY=pk_live_your_live_publishable_key
STRIPE_WEBHOOK_SECRET=whsec_your_production_webhook_secret
```

#### 테스트 카드 번호

테스트 모드에서 사용할 수 있는 카드 번호:

```
성공:
4242 4242 4242 4242 (Visa)
5555 5555 5555 4444 (Mastercard)

실패:
4000 0000 0000 0002 (카드 거부)
4000 0000 0000 9995 (자금 부족)

3D Secure 인증 필요:
4000 0027 6000 3184

CVV: 아무 3자리
만료일: 미래의 아무 날짜
우편번호: 아무 5자리
```

#### 테스트

AI 에디터에서 테스트:

```
"List all Stripe customers"
"Show me recent payments"
"Get subscription details for customer cus_xxx"
"Create a test payment intent for $10"
```

Stripe CLI 테스트:

```bash
# Stripe CLI 설치 (macOS)
brew install stripe/stripe-cli/stripe

# 로그인
stripe login

# 웹훅 로컬 테스트
stripe listen --forward-to localhost:3000/api/webhooks/stripe
```

---

## 트러블슈팅

### 일반적인 문제

#### 1. MCP 서버가 시작되지 않음

**증상**: AI 에디터에서 MCP 연결 실패 메시지

**해결 방법**:

```bash
# Node.js 및 npm 버전 확인
node --version  # v18 이상 권장
npm --version   # v9 이상 권장

# npm 캐시 정리
npm cache clean --force

# MCP 서버 수동 테스트
npx @supabase/mcp-server-supabase@latest --help
```

#### 2. 환경 변수가 로드되지 않음

**증상**: "API key not found" 또는 "Unauthorized" 오류

**해결 방법**:

1. `.env` 파일 위치 확인 (프로젝트 루트에 있어야 함)
2. 환경 변수 이름 확인 (대소문자 구분)
3. 따옴표 없이 작성:
   ```bash
   # 올바름
   API_KEY=abc123

   # 잘못됨
   API_KEY="abc123"
   API_KEY='abc123'
   ```

4. IDE 재시작
5. 환경 변수 확인:
   ```bash
   echo $SUPABASE_ACCESS_TOKEN
   ```

#### 3. npx 명령어가 느림

**증상**: MCP 서버 시작이 매우 느림

**해결 방법**:

1. 전역 설치로 전환:
   ```bash
   npm install -g @supabase/mcp-server-supabase
   ```

2. `.mcp.json` 수정:
   ```json
   {
     "mcpServers": {
       "supabase": {
         "command": "mcp-server-supabase",  // npx 제거
         "args": ["--project-ref=xxx"]
       }
     }
   }
   ```

#### 4. 여러 프로젝트에서 다른 환경 변수 사용

**증상**: 프로젝트마다 다른 API 키가 필요

**해결 방법**:

1. 프로젝트별 `.env` 파일 사용
2. 또는 direnv 사용:
   ```bash
   # direnv 설치
   brew install direnv

   # .envrc 파일 생성
   echo 'dotenv' > .envrc

   # 권한 부여
   direnv allow
   ```

#### 5. MCP 서버 충돌

**증상**: 동일한 포트를 사용하는 여러 MCP 서버

**해결 방법**:

각 MCP 서버에 다른 포트 할당:

```json
{
  "mcpServers": {
    "supabase": {
      "command": "npx",
      "args": ["-y", "@supabase/mcp-server-supabase@latest"],
      "env": {
        "PORT": "3001"
      }
    },
    "mongodb": {
      "command": "npx",
      "args": ["-y", "@mongodb/mcp-server-mongodb@latest"],
      "env": {
        "PORT": "3002"
      }
    }
  }
}
```

### 서비스별 문제

#### Supabase

**문제**: "Invalid project reference"

**해결**:
- Project URL 확인: `https://<project-ref>.supabase.co`
- `.mcp.json`의 `--project-ref`에 올바른 값 입력

**문제**: "Insufficient permissions"

**해결**:
- Access Token 재생성
- 필요한 모든 권한 체크
- Service Role Key 대신 Access Token 사용 확인

#### MongoDB Atlas

**문제**: "Connection timeout"

**해결**:
1. Network Access에 IP 추가
2. 데이터베이스 사용자 권한 확인
3. 연결 문자열의 비밀번호에 특수문자가 있다면 URL 인코딩:
   ```bash
   # 예: password! → password%21
   ```

**문제**: "Authentication failed"

**해결**:
- 데이터베이스 사용자 이름/비밀번호 확인
- 사용자가 올바른 데이터베이스에 권한이 있는지 확인

#### Firebase

**문제**: "Service account file not found"

**해결**:
- JSON 파일 경로가 절대 경로인지 확인
- 파일 권한 확인: `chmod 600 firebase-service-account.json`

**문제**: "Permission denied"

**해결**:
- Firebase Console에서 Firestore 활성화 확인
- Service Account에 충분한 권한 부여
- IAM & Admin에서 역할 확인

#### Convex

**문제**: "Deploy key invalid"

**해결**:
- Deploy Key 재생성
- 만료 여부 확인
- 올바른 프로젝트의 키인지 확인

#### GitHub

**문제**: "Bad credentials"

**해결**:
- Personal Access Token 만료 여부 확인
- 토큰 앞에 공백이나 줄바꿈이 없는지 확인
- 필요한 scopes가 모두 선택되었는지 확인

**문제**: "API rate limit exceeded"

**해결**:
- 인증된 요청은 시간당 5,000회 제한
- 제한 확인: `curl -H "Authorization: token $GITHUB_TOKEN" https://api.github.com/rate_limit`
- 대기 후 재시도

#### Stripe

**문제**: "No such customer"

**해결**:
- Test 모드와 Live 모드의 데이터는 별개
- 올바른 모드의 API 키 사용 확인

**문제**: "Webhook signature verification failed"

**해결**:
- Webhook signing secret 확인
- 요청 본문을 수정하지 않고 그대로 검증
- Stripe CLI로 로컬 테스트

### 로그 및 디버깅

#### MCP 서버 로그 확인

```bash
# 개별 MCP 서버 로그 레벨 설정
export DEBUG=mcp:*

# 상세 로그 출력
npx @supabase/mcp-server-supabase@latest --project-ref=xxx --verbose
```

#### Claude Code 로그

```bash
# macOS
tail -f ~/Library/Logs/Claude\ Code/main.log

# Linux
tail -f ~/.config/claude-code/logs/main.log
```

#### Cursor 로그

Help → Toggle Developer Tools → Console 탭

---

## 참고 자료

### 공식 문서

- **MCP 프로토콜**: https://modelcontextprotocol.io
- **Anthropic Claude**: https://docs.anthropic.com/claude
- **Supabase**: https://supabase.com/docs
- **MongoDB Atlas**: https://www.mongodb.com/docs/atlas
- **Firebase**: https://firebase.google.com/docs
- **Convex**: https://docs.convex.dev
- **Clerk**: https://clerk.com/docs
- **OpenAI**: https://platform.openai.com/docs
- **GitHub API**: https://docs.github.com/en/rest
- **Stripe**: https://stripe.com/docs/api

### MCP 서버 저장소

- **Supabase MCP**: https://github.com/supabase/mcp-server-supabase
- **MongoDB MCP**: https://github.com/mongodb/mcp-server-mongodb
- **Firebase MCP**: https://github.com/firebase/mcp-server-firestore
- **Convex MCP**: https://github.com/get-convex/mcp-server-convex
- **GitHub MCP**: https://github.com/github/mcp-server-github
- **Stripe MCP**: https://github.com/stripe/mcp-server-stripe

### 커뮤니티 및 지원

- **Claude Community**: https://community.anthropic.com
- **MCP Discord**: https://discord.gg/modelcontextprotocol
- **GitHub Discussions**: 각 MCP 서버 저장소의 Discussions 섹션

### 추가 리소스

- **환경 변수 관리**: https://github.com/motdotla/dotenv
- **direnv**: https://direnv.net
- **MCP Awesome List**: https://github.com/anthropics/awesome-mcp

### 보안 권장사항

1. **API 키 보호**
   - 절대 Git에 커밋하지 마세요
   - `.gitignore`에 `.env` 추가
   - 키 로테이션 주기적으로 수행

2. **최소 권한 원칙**
   - 필요한 최소한의 권한만 부여
   - Production과 Development 키 분리

3. **모니터링**
   - API 사용량 모니터링
   - 비정상적인 활동 감지
   - 사용량 제한 설정

4. **백업**
   - 중요한 API 키는 안전한 곳에 백업
   - Password Manager 사용 권장 (1Password, Bitwarden 등)

---

## 문서 버전 정보

- **작성일**: 2025년 1월
- **버전**: 1.0.0
- **마지막 업데이트**: 2025-01-17
- **작성자**: Portfolio Project Team

### 변경 이력

- 2025-01-17: 초기 문서 작성
  - 10개 서비스 MCP 설정 가이드 추가
  - 6개 IDE/Editor 설정 방법 추가
  - 트러블슈팅 섹션 추가

---

**참고**: 이 문서의 API 키 발급 절차와 UI는 각 서비스의 업데이트에 따라 변경될 수 있습니다. 최신 정보는 각 서비스의 공식 문서를 참조하세요.
