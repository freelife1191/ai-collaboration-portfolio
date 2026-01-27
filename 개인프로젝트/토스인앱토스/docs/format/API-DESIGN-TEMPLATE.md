# API 상세 설계 문서
## [프로젝트 이름] - [프로젝트 설명]

**문서 버전**: 1.0
**작성일**: [작성일]
**작성자**: [작성자]
**API 버전**: v1

---

## 📋 목차

1. [API 개요](#api-개요)
2. [인증 및 권한](#인증-및-권한)
3. [Server Actions API](#server-actions-api)
4. [REST API Endpoints](#rest-api-endpoints)
5. [에러 코드 및 메시지](#에러-코드-및-메시지)
6. [Rate Limiting](#rate-limiting)
7. [Webhook 명세](#webhook-명세)
8. [API 버전 관리](#api-버전-관리)

---

## API 개요

### 기술 스택

| 기술 | 설명 | 사용처 |
|------|-----|--------|
| [프레임워크] | [설명] | [사용처] |
| [인증 시스템] | [설명] | [사용처] |
| [데이터베이스] | [설명] | [사용처] |
| [검증 라이브러리] | [설명] | [사용처] |

### Base URLs

```
Development:  [개발 환경 URL]
Production:   [프로덕션 환경 URL]
```

### 공통 Response 형식

**성공 응답**:
```typescript
interface SuccessResponse<T> {
  success: true
  data: T
}
```

**에러 응답**:
```typescript
interface ErrorResponse {
  success: false
  error: {
    code: string
    message: string
    details?: Record<string, any>
  }
}
```

---

## 인증 및 권한

### Authentication Flow

**1. JWT Token ([인증 제공자])**
```typescript
// Header에 포함
Authorization: Bearer <JWT_TOKEN>

// Token Payload
{
  sub: "[사용자 ID]",
  email: "[사용자 이메일]",
  iat: [발급 시간],
  exp: [만료 시간]
}
```

**2. Server Actions에서 인증**
```typescript
import { auth } from '[인증 라이브러리]'

export async function protectedAction() {
  const { userId } = await auth()

  if (!userId) {
    throw new Error('Unauthorized')
  }

  // 비즈니스 로직
}
```

**3. API Routes에서 인증**
```typescript
import { auth } from '[인증 라이브러리]'

export async function GET(request: Request) {
  const { userId } = await auth()

  if (!userId) {
    return new Response('Unauthorized', { status: 401 })
  }

  // 비즈니스 로직
}
```

### Authorization Levels

| Role | Permissions |
|------|------------|
| **[역할 1]** | [권한 설명] |
| **[역할 2]** | [권한 설명] |
| **[역할 3]** | [권한 설명] |
| **[역할 4]** | [권한 설명] |

---

## Server Actions API

### [리소스 1] Actions

#### `[함수명1]`

[함수 설명]

**Signature**:
```typescript
export async function [함수명](
  [파라미터]: [타입]
): Promise<[반환 타입]>
```

**Input ([입력 형식])**:
```typescript
{
  [필드명]: [타입] // [설명]
}
```

**Output**:
```typescript
interface [인터페이스명] {
  [필드명]: [타입]
  [필드명]: [타입]
  [필드명]: [타입]
}
```

**Validation Rules**:
```typescript
const [스키마명] = z.object({
  [필드명]: z.[타입]()
    .[검증 규칙]('[에러 메시지]'),
})
```

**Errors**:
- `[에러 코드]`: [에러 설명]
- `[에러 코드]`: [에러 설명]

**Example**:
```typescript
const formData = new FormData()
formData.append('[필드명]', '[값]')

try {
  const result = await [함수명](formData)
  console.log(result)
} catch (error) {
  console.error(error.message)
}
```

---

#### `[함수명2]`

[함수 설명]

**Signature**:
```typescript
export async function [함수명](): Promise<[반환 타입][]>
```

**Output**:
```typescript
[타입][]
```

**Errors**:
- `[에러 코드]`: [에러 설명]

**Example**:
```typescript
const results = await [함수명]()
console.log(results)
```

---

### [리소스 2] Actions

#### `[함수명3]`

[함수 설명]

**Signature**:
```typescript
export async function [함수명](
  [파라미터1]: [타입],
  [파라미터2]: [타입]
): Promise<[반환 타입]>
```

**Input**:
```typescript
{
  [파라미터1]: [타입],
  [파라미터2]: {
    [필드명]?: [타입],
    [필드명]?: [타입]
  }
}
```

**Validation**:
- `[필드명]`: [검증 규칙 설명]
- `[필드명]`: [검증 규칙 설명]

**Errors**:
- `[에러 코드]`: [에러 설명]

---

## REST API Endpoints

### Webhook Endpoints

#### POST `/api/webhooks/[서비스명]`

[서비스명] 이벤트 수신

**Events**:
- `[이벤트 타입 1]`
- `[이벤트 타입 2]`
- `[이벤트 타입 3]`

**Request Body**:
```json
{
  "type": "[이벤트 타입]",
  "data": {
    "id": "[ID]",
    "[필드명]": "[값]"
  }
}
```

**Response**:
```json
{
  "received": true
}
```

**Verification**:
```typescript
import { [라이브러리] } from '[패키지명]'

const [변수명] = new [클래스명](process.env.[ENV_VAR]!)
const payload = [변수명].verify(body, headers)
```

---

### Cron Endpoints

#### GET `/api/cron/[작업명]`

[작업 설명]

**Trigger**: [트리거 설명]

**Process**:
1. [단계 1 설명]
2. [단계 2 설명]
3. [단계 3 설명]

**Authorization**:
```typescript
if (request.headers.get('authorization') !== `Bearer ${process.env.CRON_SECRET}`) {
  return new Response('Unauthorized', { status: 401 })
}
```

---

## 에러 코드 및 메시지

### HTTP Status Codes

| Code | Description | Usage |
|------|-------------|-------|
| 200 | OK | 성공적인 요청 |
| 201 | Created | 리소스 생성 성공 |
| 400 | Bad Request | 잘못된 입력 |
| 401 | Unauthorized | 인증 실패 |
| 403 | Forbidden | 권한 없음 |
| 404 | Not Found | 리소스 없음 |
| 422 | Unprocessable Entity | 처리 불가 |
| 429 | Too Many Requests | Rate limit 초과 |
| 500 | Internal Server Error | 서버 에러 |
| 503 | Service Unavailable | 외부 서비스 에러 |

### Custom Error Codes

```typescript
export const ERROR_CODES = {
  // Authentication
  AUTH_ERROR: {
    code: 'AUTH_ERROR',
    message: '[에러 메시지]',
    statusCode: 401,
  },
  PERMISSION_DENIED: {
    code: 'PERMISSION_DENIED',
    message: '[에러 메시지]',
    statusCode: 403,
  },

  // Validation
  VALIDATION_ERROR: {
    code: 'VALIDATION_ERROR',
    message: '[에러 메시지]',
    statusCode: 400,
  },

  // Resources
  NOT_FOUND: {
    code: 'NOT_FOUND',
    message: '[에러 메시지]',
    statusCode: 404,
  },

  // Processing
  PROCESSING_ERROR: {
    code: 'PROCESSING_ERROR',
    message: '[에러 메시지]',
    statusCode: 422,
  },

  // External Services
  EXTERNAL_SERVICE_ERROR: {
    code: 'EXTERNAL_SERVICE_ERROR',
    message: '[에러 메시지]',
    statusCode: 503,
  },

  // Rate Limiting
  RATE_LIMIT_EXCEEDED: {
    code: 'RATE_LIMIT_EXCEEDED',
    message: '[에러 메시지]',
    statusCode: 429,
  },
}
```

---

## Rate Limiting

### Limits

| Endpoint | [플랜 1] | [플랜 2] | [플랜 3] |
|----------|---------|---------|---------|
| [엔드포인트 1] | [제한] | [제한] | [제한] |
| [엔드포인트 2] | [제한] | [제한] | [제한] |
| [엔드포인트 3] | [제한] | [제한] | [제한] |
| API Calls (general) | [제한] | [제한] | [제한] |

### Implementation

```typescript
import { Ratelimit } from '@upstash/ratelimit'
import { Redis } from '@upstash/redis'

const redis = new Redis({
  url: process.env.UPSTASH_REDIS_REST_URL!,
  token: process.env.UPSTASH_REDIS_REST_TOKEN!,
})

export const [rateLimiterName] = new Ratelimit({
  redis,
  limiter: Ratelimit.slidingWindow([횟수], '[시간 단위]'),
  analytics: true,
})

export async function [함수명]([파라미터]) {
  const { userId } = await auth()
  const { success } = await [rateLimiterName].limit(userId)

  if (!success) {
    throw new Error('RATE_LIMIT_EXCEEDED')
  }

  // 로직
}
```

### Headers

**Response Headers**:
```
X-RateLimit-Limit: [최대 횟수]
X-RateLimit-Remaining: [남은 횟수]
X-RateLimit-Reset: [리셋 시간]
```

---

## Webhook 명세

### [서비스 1] Webhooks

**Endpoint**: `POST /api/webhooks/[서비스명]`

**Events**:

1. **[이벤트 타입 1]**
```json
{
  "type": "[이벤트 타입]",
  "data": {
    "id": "[ID]",
    "[필드명]": "[값]"
  }
}
```

2. **[이벤트 타입 2]**
```json
{
  "type": "[이벤트 타입]",
  "data": {
    "id": "[ID]",
    "[필드명]": "[값]"
  }
}
```

---

## API 버전 관리

### Versioning Strategy

현재는 v1만 지원. 향후 변경 시:

**URL Versioning**:
```
/api/v1/...
/api/v2/...
```

**Header Versioning**:
```
X-API-Version: 1
```

### Deprecation Policy

1. 새 버전 출시 6개월 전 공지
2. 3개월 전 deprecated 마킹
3. 6개월 후 제거

### Changelog

**v1.0.0** ([날짜])
- Initial release
- [주요 기능 1]
- [주요 기능 2]
- [주요 기능 3]

---

이 API 설계는 확장 가능하고 안전하며 개발자 친화적인 인터페이스를 제공합니다.
