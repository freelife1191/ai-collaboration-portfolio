# JS 기술스택 정리 

---

## 1. Next.js 기반 가장 인기 좋은 기술 스택 정리

## 
Next.js 기반의 인기 기술 스택은 최근 몇 년간 빠르게 변화했지만, 질문에 언급한 TailwindCSS, shadcn/ui, Lucide, zustand, Supabase, Vercel은 2025년 기준 확실히 많이 선택되는 최신 스택에 속합니다. 다만 각 기술의 정확한 용도와 함께 추가로 자주 쓰이는 기술, 그리고 비슷한 역할의 대체 인기 스택도 있으니 용도별로 아래와 같이 정리할 수 있습니다.[1][2][3]

### 스타일링 & UI 컴포넌트

- **TailwindCSS:** CSS 유틸리티 프레임워크로, 개발자들에게 빠른 스타일링과 일관된 UI 구현을 제공해서 거의 표준처럼 자리잡았습니다.[2][4][1]
- **shadcn/ui:** Tailwind와 궁합이 매우 좋은 오픈소스 컴포넌트 세트로, 커스터마이징이 쉽고 직접 복사해서 쓸 수 있다는 점에서 각광받습니다.[5][1][2]
- **Lucide:** 아이콘 라이브러리로, shadcn/ui와 자주 세트로 사용됩니다. Feather 아이콘 기반의 현대적인 벡터 아이콘을 제공합니다.[2]

동일 분야의 대안/경쟁:
- CSS-in-JS(Emotion, styled-components), 기타 UI 프레임워크(Material UI, Chakra UI)도 일부 프로젝트에서 여전히 선택받고 있습니다.[6]

### 상태 관리

- **zustand:** React 생태계에서 간편하게 쓸 수 있는 경량 상태 관리자이며, recoil, jotai, redux도 상황에 따라 여전히 쓰입니다.[7]
- **React Query:** 서버 State(원격/비동기 데이터) 관리를 위해 최신 프로젝트에서 매우 선호되며, zustand가 로컬/전역 state면 React Query는 API 데이터 캐싱에 특화되어 있습니다.[8]

### 백엔드/데이터 & 인증

- **Supabase:** 오픈소스 Firebase 대체제로, 실시간 DB(PostgreSQL), 인증, 파일 스토리지, 함수 등을 제공하며 full-stack 개발에 많이 사용됩니다.[8][7]
- **Prisma:** DB ORM/쿼리빌더로 Supabase, Postgres, MySQL 등 다양한 데이터베이스와 함께 쓰입니다.
- **Convex:** 실시간 데이터 및 서버리스 함수 중심의 새로운 BaaS, Supabase와 대체/보완 관계입니다.[3][9]
- **NextAuth.js:** OAuth 기반 인증 구현에 널리 사용됩니다.[3]

### 인프라, 빌드, 배포

- **Vercel:** Next.js의 공식 배포 플랫폼(무료/유료), 빠른 배포 경험과 자동 CDN, SSR 등과 잘 어울립니다.[1][3]
- **Netlify, AWS, Cloudflare:** Vercel 외에도 배포 목적에 따라 사용됩니다.

### 기타 트렌드/필수 도구

- **TypeScript:** 타입 안정성 덕분에 사실상 프론트엔드 표준 언어입니다.[1]
- **App Router/React Server Components:** Next.js 13+에서 도입된 아키텍처로 최신 프로젝트의 핵심 경향입니다.[10][11][1]
- **PostHog(애널리틱스)**, **Storybook(컴포넌트 개발)** 등 실무에서 자주 쓰이는 보조도구도 있습니다.[3]

***

## 용도별 인기 기술 스택(2025 기준)

| 용도            | 대표 스택         | 비고 및 대체 스택                   |
|----------------|------------------|-----------------------------------|
| 스타일링/컴포넌트  | Tailwind, shadcn, Lucide | Material UI, Chakra UI, Emotion[1][2][12] |
| 상태 관리        | zustand, React Query | recoil, jotai, redux, TanStack Query[8][7]  |
| 데이터/백엔드     | Supabase, Prisma, Convex | Planetscale(Postgres), Firebase, GraphQL[8][3][9]|
| 인증/보안        | NextAuth         | Supabase Auth, Clerk, Auth0[3][7]           |
| 배포/인프라      | Vercel           | Netlify, AWS, Cloudflare[1][3]              |
| 언어/환경        | TypeScript       | JavaScript (점차 줄어드는 추세)[1][8]        |
| 분석/테스팅      | PostHog, Storybook| Google Analytics, Jest, Cypress[3]              |

질문에 언급한 기술은 현재 Next.js 프로젝트에서 ‘핵심’이라 불릴 만큼 인기가 높으며, 잘못 포함된 스택은 없습니다. 오히려 React Query, NextAuth, Prisma, Convex 등도 함께 알아두면 최신 트렌드 흐름에 더 가까워질 수 있습니다.[8][2][1][3]

---

## 2. React 기반 가장 인기 좋은 기술 스택 정리

React 기반 기술 스택은 Next.js 기반과 상당 부분 겹치지만, 범용성과 선택지가 더 폭넓습니다. 2025년 기준으로 인기 있는 주요 스택들과 용도별 대표 라이브러리, 그리고 자주 쓰이는 대체 스택을 용도에 따라 아래처럼 정리할 수 있습니다.[1][2][3][4][5][6][7][8]

***

### UI/스타일링

- **TailwindCSS:** 유틸리티-퍼스트 CSS 프레임워크로, React에서 JSX와 조합하여 빠른 스타일링이 가능합니다.[9][1]
- **Material UI (MUI):** 다양한 컴포넌트와 테마 시스템을 제공하며, 엔터프라이즈 프로젝트에서 많이 선택됩니다.[8]
- **shadcn/ui, Chakra UI:** Tailwind 기반 컴포넌트 세트(shadcn/ui)와 완성도 높은 디자인 시스템(Chakra UI)도 많이 사용됩니다.[1]
- **Lucide:** 벡터 아이콘 라이브러리로 Feather 기반, 다양한 디자인 시스템과 함께 활용됩니다.[10]

### 상태 관리

- **Zustand:** 경량 상태 관리 라이브러리로 빠른 성장세와 뛰어난 성능, 쉬운 API가 특징입니다.[6][7][11]
- **Redux Toolkit:** 대규모 앱에서 여전히 최적의 선택지이며, 불필요한 보일러플레이트를 줄인 최신 방식이 도입되었습니다.[3][5][7][11]
- **Jotai, Recoil, MobX, XState:** 각각 컨텍스트 기반, 원자 상태 관리, 반응형 프로그래밍, Finite State Machine 관리식 등 다양한 접근법을 제공합니다.[5][7][3]
- **React Query (TanStack Query):** 비동기 및 서버 상태를 최적화하여, API 데이터 관리에 거의 표준처럼 자리잡고 있습니다.[7][8]

### 폼 및 입력 관리

- **Formik, React Hook Form:** 대규모/복잡한 폼 처리에 선호되는 라이브러리이며, 다양한 유효성 검증과 컴포넌트 결합이 강점입니다.[8]

### 라우팅/SPA

- **React Router:** 공식 SPA/라우팅 라이브러리로 지속적인 버전업(2025년 기준 V7) 및 개선이 계속되고 있습니다.[12][8]

### 백엔드/데이터 & 인증

- **Node.js + Express.js:** 실무에서 가장 널리 쓰이는 백엔드 조합으로, JavaScript 전체(프런트/백엔드) 통일과 높은 확장성이 장점입니다.[4][13]
- **Prisma:** ORM 툴로 Postgres, MySQL 등 다양한 DB 연동에 사용됩니다.[4][1]
- **Supabase, Firebase, Convex:** BaaS로 인증, 실시간 데이터, 파일 업로드 등을 빠르게 구현할 수 있습니다.[1][4]
- **NextAuth.js, Auth0, Clerk:** 인증 및 OAuth 구현에 널리 쓰이는 라이브러리입니다.[8][1]

### 배포/인프라

- **Vercel, Netlify, Heroku, AWS:** 다양한 목적과 가격대별로 맞춤형 선택지가 많으며, Vercel은 Next.js의 공식 서비스이기도 합니다.[4][1][8]

### 타입/코드 품질

- **TypeScript:** React와 결합할 때 타입 안정성과 유지보수에 크게 기여해, 사실상 표준 언어로 자리잡았습니다.[12][8]
- **ESLint, Prettier:** 코드 품질 및 스타일을 위한 필수 도구입니다.[9]

### AI/애널리틱스/서드파티 연동

- **PostHog, Storybook, Framer Motion:** 애널리틱스, 컴포넌트 별개 개발, 고급 애니메이션 등을 위한 보조 도구로 매우 많이 활용됩니다.[1][8]
- **AI Plugins:** GPT-4 기반의 코드 자동화, LLM 통합 기능들도 점점 도입 중입니다.[12]

***

## 용도별 인기 React 기술 스택 (2025 기준)

| 용도            | 대표 기술 스택                            | 대체/보완 기술                                            |
|----------------|-----------------------------------------|--------------------------------------------------|
| UI 컴포넌트/스타일 | TailwindCSS, MUI, shadcn/ui, Chakra UI, Lucide | Ant Design, Emotion, Styled-components[1][8] |
| 상태 관리        | Zustand, Redux Toolkit, Jotai, Recoil, React Query | MobX, XState, Context API[6][7][8]      |
| 폼 처리          | Formik, React Hook Form                 | Final Form[8]                            |
| 라우팅/SPA       | React Router, Next.js, Remix            | Tanstack Start[12][8]                  |
| 백엔드/DB        | Node.js+Express, Prisma, Supabase, Firebase | Convex, Django, Laravel[1][4][13]      |
| 인증/보안        | NextAuth.js, Auth0, Clerk, Supabase Auth | Passport.js[1][8]                             |
| 배포/인프라      | Vercel, Netlify, Heroku, AWS            | Cloudflare, Digital Ocean[1][4][8]     |
| 타입/품질        | TypeScript, ESLint, Prettier            | JavaScript(점차 감소), SonarLint[12][8]        |
| 기타 도구/AI     | PostHog, Storybook, Framer Motion, AI Plugins | Analytics.js, Cypress, Jest[1][12][8]    |

각 분야별로 인기 있는 스택들은 대부분 실제 기업/커뮤니티/오픈소스 프로젝트에서 많이 채택되고 있습니다. Next.js가 대세 프레임워크라면, React의 뎁스(Depth)는 훨씬 방대하므로 상태 관리, 백엔드, 폼 등에서 취향과 프로젝트 성격에 따라 세부 선택지가 다양하게 존재합니다.2025년 기준 React 기반 인기 기술 스택은 UI, 상태관리, 라우팅, 백엔드, 배포 등 용도별로 다양한 선택지가 있습니다. 아래 표와 설명은 최신 트렌드와 함께 각 분야별 대표 라이브러리를 정리한 것입니다.[3][5][6][7][12][4][8][1]

***

### UI/스타일링

- TailwindCSS: 유틸리티 기반 CSS 프레임워크로 빠르고 일관된 스타일링에 탁월합니다.[12][8]
- Material UI (MUI): 강력한 디자인 시스템과 컴포넌트로 엔터프라이즈 프로젝트에서 인기가 많습니다.[8]
- shadcn/ui, Chakra UI: 커스터마이징 가능한 신세대 컴포넌트 라이브러리들도 높은 평가를 받고 있습니다.[12]
- Lucide: 최신 벡터 아이콘 라이브러리입니다.[10]

### 상태 관리

- Zustand: 경량·고성능 상태 관리로 빠른 성장세를 보이고 있습니다.[6][7]
- Redux Toolkit: 대형 프로젝트의 글로벌 상태 관리에 여전히 강세입니다.[5][3]
- Jotai, Recoil, MobX: 다양한 크기의 앱에 맞춘 특화된 관리 방식도 다수 존재합니다.[7][3]
- React Query(TanStack Query): 원격 데이터 및 서버 상태 관리 표준입니다.[8]

### 폼 처리

- Formik, React Hook Form: 복잡한 폼과 유효성 검증에 최적화된 라이브러리들입니다.[8]

### 라우팅

- React Router: SPA 및 멀티페이지 라우팅의 공식 라이브러리로 지속적으로 개선되고 있습니다.[12][8]
- Next.js, Remix: 서버 사이드 및 풀스택 개발까지 확장되는 프레임워크입니다.[14][12]

### 백엔드/DB/인증

- Node.js + Express: 범용적이고 빠른 API 개발에 가장 널리 쓰입니다.[13][4]
- Prisma: 다양한 DB와 연결 가능한 ORM 툴입니다.[4][1]
- Supabase, Firebase, Convex: BaaS(Backend as a Service)의 주요 솔루션입니다.[1][4]
- NextAuth.js, Auth0, Clerk: 인증과 OAuth 구현에 표준처럼 활용됩니다.[1][8]

### 배포·인프라

- Vercel, Netlify, Heroku, AWS: 다양한 가격대와 확장성을 가진 배포 옵션입니다.[4][1][8]

### 기타 도구

- TypeScript: 사실상 표준 타입 시스템.[12][8]
- ESLint, Prettier: 코드 품질·스타일 유지 보조 도구.[9]
- Storybook, Framer Motion: 컴포넌트 테스트·애니메이션 툴도 광범위하게 쓰입니다.[8]

***

## 용도별 인기 React 기술 스택 표

| 용도            | 대표 기술 스택                            | 대체/보완 기술                   |
|----------------|-------------------------------------------|-------------------------------|
| UI/컴포넌트      | Tailwind, MUI, shadcn/ui, Chakra UI, Lucide| Ant Design, Emotion, Styled-components[8] |
| 상태 관리        | Zustand, Redux Toolkit, Jotai, Recoil, React Query | MobX, XState, Context API[7]           |
| 폼/입력           | Formik, React Hook Form                   | Final Form[8]              |
| 라우팅           | React Router, Next.js, Remix               | TanStack Start[12]         |
| DB/백엔드        | Node.js+Express, Prisma, Supabase, Firebase | Convex, Django, Laravel[1][4]   |
| 인증/보안        | NextAuth.js, Auth0, Clerk, Supabase Auth      | Passport.js[8]                 |
| 배포/인프라      | Vercel, Netlify, Heroku, AWS               | Cloudflare, Digital Ocean[1]   |
| 타입/품질        | TypeScript, ESLint, Prettier               | SonarLint[12][8]            |
| 기타 도구        | Storybook, Framer Motion, AI Plugins        | Analytics.js, Cypress, Jest[8]        |

업계에서 쓰이는 React 기술 스택은 워낙 다양해서 프로젝트 목적, 규모, 팀 구성에 따라 세부 선택지는 계속 진화하고 있습니다.[3][6][12][8]

---

## 3. Nest.js 기반 가장 인기 좋은 기술 스택 정리

Nest.js 기반의 2025년 최신 인기 기술 스택을 용도별로 정리하면 다음과 같습니다. Nest.js는 엔터프라이즈급 TypeScript 백엔드 프레임워크로, 확장성과 구조적 아키텍처가 강점이며, 아래 각 영역에서 대표 라이브러리와 대안 스택들이 혼합 사용되고 있습니다.[1][2][3][4][5]

***

### 언어, 엔진, 아키텍처

- **TypeScript:** Nest.js의 핵심 언어, 타입 안정성과 규모 확장에 필수적입니다.[2][1]
- **Node.js:** 런타임으로 대다수 Nest.js 프로젝트의 기반입니다.[3]

### DB/ORM

- **TypeORM:** Nest.js 공식 예제 및 기본 ORM. 데코레이터, 리포지터리 패턴 등 TypeScript와 깊게 연동됩니다.[4]
- **Prisma:** 최근 빠른 성장세로, 타입 안전성, 자동 쿼리 생성을 강조하며 TypeORM 대체하거나 함께 사용됩니다.[6]
- **Mongoose:** MongoDB 연동 시 표준 ORM입니다.[6]

### 인증, 사용자 관리

- **Passport.js:** 다양한 인증 전략(JWT, Local, OAuth)을 지원해 인증 모듈 구축에 표준처럼 쓰입니다.[7][8][9][10]
- **@nestjs/jwt:** JWT 기반 인증에서 사용되는 공식 Nest.js 패키지입니다.[7]
- **Redis:** 세션 캐싱, 토큰 관리 등에 활용되며 Passport와 조합 경우가 많습니다.[8][9]

### 서버/테스팅/실시간 통신

- **GraphQL (Apollo Server):** REST API 외에 GraphQL API를 만들 때 공식적으로 지원하며, Resolver 패턴/데코레이터 기반입니다.[4]
- **WebSockets (Socket.io, @nestjs/websockets):** 실시간 메시징, 알림, 채팅 기능 구현에 필수입니다.[4][6]
- **Jest:** 공식 테스트 프레임워크입니다.[4]

### 배포/인프라

- **Docker:** 컨테이너화와 인프라 자동화의 표준입니다.[11][1]
- **Leapcell, Vercel, AWS:** Leapcell은 최근 Node.js/Nest.js 기반 서버리스 배포에 각광받고 있고, AWS와 Vercel도 많이 채택하고 있습니다.[1][3]
- **PM2:** 프로덕션 Node 프로세스 매니저, 서비스 안정화에 사용됩니다.[3][4]

### 기타 보조/유틸리티

- **Swagger (OpenAPI):** Nest.js는 데코레이터 기반 문서 자동화 모듈(@nestjs/swagger)을 제공합니다.
- **Class-validator, class-transformer:** 파라미터 및 DTO 유효성 검증 표준.[1]
- **CORS, Helmet:** 보안 미들웨어 설정에 자주 사용됩니다.[4]

---

## 4. 용도별 인기 Nest.js 기술 스택 정리(2025 기준)

| 용도            | 대표 기술 스택                                    | 대체/보완 스택                       |
|----------------|-------------------------------------------------|------------------------------------|
| 핵심 언어/엔진   | TypeScript, Node.js                             | JavaScript(점차 감소)[2][4]     |
| DB/ORM         | TypeORM, Prisma, Mongoose                        | Sequelize, MikroORM[6]        |
| 인증/세션 관리   | Passport.js, @nestjs/jwt, Redis                 | Auth0, Clerk[7][8][9][10] |
| API            | REST, GraphQL(Apollo), Websockets(Socket.io)     | gRPC, RPC[4][6]           |
| 배포/인프라      | Docker, Leapcell, AWS, Vercel, PM2              | Heroku, Netlify[1][3][11] |
| 문서/유효성검증   | Swagger, class-validator, class-transformer      | Ajv, Joi[1][4]           |
| 테스팅/보안      | Jest, CORS, Helmet                              | Mocha, Supertest[3][4]   |

Nest.js의 강점은 TypeScript 기반 설계, 구조적 아키텍처, 빠른 생산성과 유지보수성, 그리고 Passport/JWT 조합을 통한 강력한 인증 및 다양한 DB 연동 등으로, 2025년에도 TypeORM, Prisma, Passport-JWT, Docker-CI/CD, GraphQL-REST-WebSocket 구조가 기술 스택의 표준으로 자리잡고 있습니다.[5][2][3][1][4]


---

## 5. Node.js fastify 기반 가장 인기 좋은 기술 스택 정리

Fastify는 2025년 Node.js 백엔드 개발에서 고성능과 낮은 오버헤드로 주목받는 프레임워크로, Express보다 거의 2배 빠른 성능을 자랑합니다. 초당 최대 30,000개의 요청을 처리할 수 있으며, TypeScript 지원과 플러그인 기반 아키텍처로 확장성이 뛰어납니다.[1][2][3][4]

## 핵심 기술 스택

### 프레임워크 & 언어
**Fastify + TypeScript**가 2025년 표준 조합으로 자리 잡았습니다. TypeScript의 정적 타이핑이 런타임 에러를 줄이고 코드 유지보수성을 높여주며, Fastify는 기본적으로 TypeScript를 완벽하게 지원합니다.[5][2][4][1]

### ORM/데이터베이스
**Prisma**는 Fastify와 함께 가장 많이 사용되는 ORM으로, 직관적인 API와 타입 안정성을 제공합니다. **Drizzle ORM**은 최근 급부상하는 대안으로, SQL에 가까운 문법과 경량화된 번들 크기로 서버리스 환경에서 특히 유리합니다. **TypeORM**도 여전히 인기 있는 선택지입니다.[6][7][8][9][10]

### 인증 & 보안
**@fastify/jwt**는 JWT 기반 인증의 표준 플러그인입니다. **@fastify/passport**를 통해 다양한 Passport 전략(Google OAuth, Local 등)을 Fastify 생태계에서 사용할 수 있습니다.[11][12][13][5]

### 유효성 검증
Fastify는 **JSON Schema 기반 유효성 검증**을 내장하고 있으며, **AJV**를 사용해 스키마를 고성능 함수로 컴파일합니다. 요청 본문, URL 파라미터, 헤더, 쿼리 스트링 모두 검증 가능하며, 응답 직렬화도 최적화됩니다.[4][14][15][1]

### 로깅
**Pino**는 Fastify의 기본 로거로, 최고 성능의 로깅을 제공하며 로깅 비용을 거의 제로에 가깝게 만듭니다.[3][1]

### 데이터베이스
**PostgreSQL**이 가장 널리 사용되며, MySQL, SQLite, MongoDB도 지원됩니다. 실시간 기능이 필요한 경우 **Supabase**(PostgreSQL 기반)도 고려할 수 있습니다.[16][8][6]

### 배포 & 인프라
**Docker**를 활용한 컨테이너화가 표준이며, **Railway**, **Fly.io**, **Vercel**이 인기 있는 배포 플랫폼입니다. **Nginx**나 **HAProxy**를 리버스 프록시로 사용하는 것이 프로덕션 환경에서 권장됩니다.[17][18][19]

### CI/CD
**GitHub Actions**가 가장 널리 사용되며, GitLab CI, CircleCI도 대안입니다.[5][17]

### 추가 도구
**ESLint**와 **Prettier**는 코드 품질 관리의 필수 도구이며, **Vite**를 Fastify와 결합한 **@fastify/vite**는 프론트엔드 통합에 유용합니다.[20][21][5]

## 플러그인 생태계

Fastify의 강점은 풍부한 플러그인 생태계입니다. 훅, 플러그인, 데코레이터를 통해 기능을 유연하게 확장할 수 있으며, 커뮤니티에서 제공하는 다양한 플러그인을 활용할 수 있습니다.[22][23][1][4]

---

## 6. React Native 기반 가장 인기 좋은 기술 스택 정리

2025년 React Native 개발에서 가장 인기 있고 실전성이 높은 기술 스택은 다음과 같습니다. Expo 중심의 프레임워크 환경과 Modern UI/UX, 클라우드 백엔드 및 최신 상태 관리 도구가 강세입니다.[1][2][3]

### 핵심 개발 환경
- **Expo**: 공식적으로 권장되는 프레임워크로, 빠른 개발·배포·빌드, OTA 업데이트 등 강력한 개발 경험을 제공합니다. CLI보다 Expo 기반 프로젝트가 압도적으로 많아졌습니다.[2][3][1]
- **TypeScript**: 정적 타입 체크로 유지보수성과 팀 협업에 필수적입니다.[1][2]

### 네비게이션
- **Expo Router**: 파일 기반 자동 라우팅, React Server Components 지원.[3][2][1]
- **React Navigation**: 전통적인 네비게이션 시스템, 여전히 산업 표준으로 사용 가능.[4][3]

### UI 라이브러리 & 스타일링
- **NativeWind**: TailwindCSS를 React Native에 적용, 빠르고 일관된 스타일링.[5][3]
- **Tamagui**: 웹-모바일 통합, 퍼포먼스와 커스텀 토큰 지원.[5]
- **Gluestack**: Tailwind 연동, 가볍고 최신 트렌드 맞춤형 컴포넌트 킷.[6][5]

### 상태 관리
- **Redux Toolkit**: 대형 앱, 복잡한 상태 관리에 최적.[7][8]
- **TanStack Query**: 서버 상태 관리, 캐싱·비동기 데이터 처리에 도입 확대.[3]
- **Zustand**: 경량·심플한 글로벌 상태(Redux 대체 수요 증가).[3]
- **React Context API**: 가벼운 상태 공유, 프로젝트 규모에 따라 적합.[8]
- **Jotai/Recoil**: 간결하고 최적화된 글로벌 상태 관리.[7]

### 백엔드 & BaaS
- **Firebase**: 인증, 실시간 DB, 스토리지, 푸시 모두 커버하는 No.1 MBaaS.[9][10]
- **AWS Amplify**: GraphQL API, 대규모 클라우드 서비스 연동 시 강력.[11][12][9]
- **Supabase**: Postgres 기반, 오픈소스 실시간 DB와 인증 및 Storage 지원.[13]
- **Node.js Fastify/Express**: 커스텀 백엔드 직접 구축 시 여전히 견고한 선택.[9]

### 플러그인 & 기능 도구
- **React Native Vision Camera**: 카메라, 이미지 분석 등 멀티미디어 앱 필수.[3]
- **Expo Image**: 네이티브 성능 이미지 렌더링, 최적화된 UX 제공.[3]
- **Clerk/Auth**: 인증 기능에 강력한 클라우드 인증 서비스 등장.[9][3]

### Dev Tools, CI/CD
- **Expo EAS**: 빌드, 배포, OTA 업데이트 자동화.[3]
- **VS Code**: 시장 점유율 1위 코드 에디터, React Native 플러그인 풍부.[14][4]
- **Prettier/ESLint**: 코드 포맷·정적 분석 표준.[4][14]

### 기타
- **Flipper**: 디버그·성능 분석, 리액트 네이티브 공식 지원 Devtool.[4]
- **Cursor/Trae**: AI 기반 코드 생산성 도구(Claude-3.5 지원).[3]

***

#### 기술 조합 예시 (2025년 기준)

| 카테고리     | 대표 스택                        |
|:---------:|:------------------------------:|
| 프레임워크  | Expo + TypeScript             |
| UI 스타일   | NativeWind, Tamagui, Gluestack|
| 상태 관리   | Redux Toolkit, Zustand, TanStack Query |
| 네비게이션  | Expo Router, React Navigation |
| 백엔드      | Firebase, AWS Amplify, Supabase, Fastify(Node.js) |
| DevTools    | Expo EAS, VS Code, Flipper    |

최신 React Native 앱 개발의 표준은 Expo+TypeScript를 기반으로 Modern UI Kit과 BaaS 결합이라 할 수 있습니다. 특별한 커스텀이 요구되는 경우 Fastify 같은 경량 Node.js 백엔드가 활용됩니다.[10][2][13][1][5][3]