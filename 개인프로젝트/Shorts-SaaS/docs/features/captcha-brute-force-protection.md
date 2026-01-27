# CAPTCHA 및 Brute-Force 방어 기능

## 개요

YouTube Creator SaaS의 보안을 강화하기 위한 CAPTCHA 및 무차별 대입 공격(Brute-Force) 방어 시스템입니다.

**구현 날짜**: 2025년 1월 (Phase 7)
**작성자**: Claude Code
**버전**: 1.0

---

## 주요 기능

### 1. 로그인 시도 추적

- **실패 카운터**: IP 주소 기반으로 연속 실패 횟수 추적
- **임계값**: 5회 연속 실패 시 CAPTCHA 요구
- **자동 리셋**: 1시간 경과 또는 로그인 성공 시 카운터 초기화
- **데이터베이스 저장**: `AuthAttempt` 테이블에 시도 기록 저장

### 2. Cloudflare Turnstile CAPTCHA

- **제공자**: Cloudflare Turnstile (Google reCAPTCHA 대안)
- **장점**:
  - 무료 플랜 제공
  - 개인정보 보호 친화적
  - 빠른 검증 속도
  - 모바일 최적화
- **검증 플로우**:
  1. 클라이언트에서 위젯 렌더링
  2. 사용자가 챌린지 완료
  3. 클라이언트가 토큰을 서버로 전송
  4. 서버가 Cloudflare API로 검증
  5. 검증 성공 시 로그인 허용

### 3. 보안 로그

- **AuthLog 테이블**: 모든 CAPTCHA 이벤트 기록
  - `CAPTCHA_CHALLENGE`: CAPTCHA 요구 이벤트 (보안 이벤트, 1년 보관)
  - `CAPTCHA_SUCCESS`: CAPTCHA 검증 성공
  - `CAPTCHA_FAILURE`: CAPTCHA 검증 실패 (보안 이벤트, 1년 보관)
- **GDPR 준수**: 일반 로그는 90일, 보안 이벤트는 1년 보관

---

## 아키텍처

```
┌─────────────────────────────────────────────────────────────────┐
│                          사용자 브라우저                           │
├─────────────────────────────────────────────────────────────────┤
│  1. 로그인 페이지 접근                                             │
│  2. GET /api/auth/check-attempts → requiresCaptcha 확인          │
│  3. 5회 이상 실패 시 CAPTCHA 위젯 렌더링                           │
│  4. CAPTCHA 완료 → POST /api/auth/verify-captcha                 │
│  5. 검증 성공 → 로그인 버튼 활성화                                  │
└─────────────────────────────────────────────────────────────────┘
                                ↓
┌─────────────────────────────────────────────────────────────────┐
│                         Next.js API Routes                       │
├─────────────────────────────────────────────────────────────────┤
│  • /api/auth/check-attempts (GET)                               │
│    - IP 기반 실패 횟수 조회                                        │
│    - 5회 이상 → requiresCaptcha: true                            │
│                                                                  │
│  • /api/auth/verify-captcha (POST)                              │
│    - Cloudflare Turnstile API 호출                               │
│    - 검증 성공 → AuthLog에 CAPTCHA_SUCCESS 기록                   │
│    - 검증 실패 → AuthLog에 CAPTCHA_FAILURE 기록                   │
│                                                                  │
│  • /api/auth/callback (POST)                                    │
│    - OAuth 성공 → recordAuthAttempt(success: true)               │
│    - OAuth 실패 → recordAuthAttempt(success: false)              │
└─────────────────────────────────────────────────────────────────┘
                                ↓
┌─────────────────────────────────────────────────────────────────┐
│                    lib/auth/attempt-tracker.ts                   │
├─────────────────────────────────────────────────────────────────┤
│  • recordAuthAttempt()       - 시도 기록                          │
│  • getFailedAttemptCount()   - 실패 횟수 조회                      │
│  • shouldShowCaptcha()       - CAPTCHA 표시 여부                  │
│  • resetAttemptCounter()     - 카운터 리셋                         │
│  • recordCaptchaSuccess()    - CAPTCHA 성공 로그                  │
│  • recordCaptchaFailure()    - CAPTCHA 실패 로그                  │
└─────────────────────────────────────────────────────────────────┘
                                ↓
┌─────────────────────────────────────────────────────────────────┐
│                       Supabase PostgreSQL                        │
├─────────────────────────────────────────────────────────────────┤
│  • AuthAttempt 테이블                                             │
│    - identifier (email/IP), failureCount, captchaRequired       │
│                                                                  │
│  • AuthLog 테이블                                                 │
│    - eventType, timestamp, ipAddress, isSecurityEvent           │
└─────────────────────────────────────────────────────────────────┘
```

---

## 파일 구조

```
youtube-creator-saas/
├── lib/auth/
│   └── attempt-tracker.ts              # 시도 추적 유틸리티 (신규)
│
├── components/auth/
│   └── captcha-challenge.tsx           # CAPTCHA 컴포넌트 (신규)
│
├── app/api/auth/
│   ├── check-attempts/
│   │   └── route.ts                    # 시도 횟수 확인 API (신규)
│   ├── verify-captcha/
│   │   └── route.ts                    # CAPTCHA 검증 API (신규)
│   └── callback/
│       └── route.ts                    # OAuth 콜백 (수정: 시도 추적 추가)
│
├── app/(auth)/sign-in/
│   └── page.tsx                        # 로그인 페이지 (수정: CAPTCHA UI 추가)
│
└── prisma/
    └── schema.prisma                   # 스키마 (이미 생성됨)
```

---

## 환경 변수 설정

### 1. Cloudflare Turnstile 키 발급

1. [Cloudflare Dashboard](https://dash.cloudflare.com) 접속
2. **Account Home** → **Turnstile** 메뉴 이동
3. **Add Site** 클릭
4. 사이트 정보 입력:
   - **Site Name**: `YouTube Creator SaaS`
   - **Domain**: `localhost` (개발) / `yourdomain.com` (프로덕션)
   - **Widget Mode**: `Managed` (권장)
5. 생성 후 **Site Key**와 **Secret Key** 복사

### 2. .env.local 파일 설정

```bash
# Cloudflare Turnstile (CAPTCHA)
NEXT_PUBLIC_TURNSTILE_SITE_KEY="0x4AAAAAAA..."  # Public site key
TURNSTILE_SECRET_KEY="0x4AAAAAAA..."            # Secret key (서버 전용)
```

**주의사항**:
- `NEXT_PUBLIC_*` 접두사는 클라이언트 노출용 (Site Key)
- `TURNSTILE_SECRET_KEY`는 서버 전용 (절대 클라이언트 노출 금지)
- `.env.local`은 `.gitignore`에 포함되어 있음

---

## API 명세

### 1. GET /api/auth/check-attempts

**설명**: 로그인 시도 횟수 확인 및 CAPTCHA 표시 여부 반환

**요청**:
```bash
GET /api/auth/check-attempts
```

**응답**:
```json
{
  "requiresCaptcha": true,
  "message": "보안을 위해 CAPTCHA 인증이 필요합니다."
}
```

**로직**:
- IP 주소 기반으로 최근 1시간 내 실패 횟수 조회
- 5회 이상 → `requiresCaptcha: true`
- 5회 미만 → `requiresCaptcha: false`

---

### 2. POST /api/auth/verify-captcha

**설명**: Cloudflare Turnstile CAPTCHA 서버 검증

**요청**:
```json
{
  "token": "0x4AAAAAAA..."
}
```

**응답 (성공)**:
```json
{
  "success": true,
  "message": "CAPTCHA 검증이 완료되었습니다."
}
```

**응답 (실패)**:
```json
{
  "success": false,
  "error": "CAPTCHA_VERIFICATION_FAILED",
  "message": "CAPTCHA 토큰이 유효하지 않거나 만료되었습니다.",
  "errorCodes": ["invalid-input-response"]
}
```

**Turnstile 에러 코드**:
- `missing-input-secret`: 서버 설정 오류 (비밀 키 누락)
- `invalid-input-secret`: 서버 설정 오류 (비밀 키 유효하지 않음)
- `missing-input-response`: CAPTCHA 토큰 누락
- `invalid-input-response`: CAPTCHA 토큰 유효하지 않거나 만료
- `timeout-or-duplicate`: CAPTCHA 토큰 이미 사용됨 또는 만료
- `internal-error`: Cloudflare 서버 오류

---

## 컴포넌트 사용법

### CaptchaChallenge 컴포넌트

```tsx
import { CaptchaChallenge } from '@/components/auth/captcha-challenge';

function MyComponent() {
  const handleVerified = (token: string) => {
    console.log('CAPTCHA 완료:', token);
    // 로그인 버튼 활성화
  };

  const handleError = (error: string) => {
    console.error('CAPTCHA 실패:', error);
    // 에러 메시지 표시
  };

  return (
    <CaptchaChallenge
      onVerified={handleVerified}
      onError={handleError}
      className="my-4"
    />
  );
}
```

**Props**:
- `onVerified(token: string)`: CAPTCHA 검증 완료 콜백
- `onError(error: string)`: CAPTCHA 에러 콜백
- `className?: string`: 커스텀 스타일 클래스

---

## 유틸리티 함수

### lib/auth/attempt-tracker.ts

```typescript
import { recordAuthAttempt, shouldShowCaptcha } from '@/lib/auth/attempt-tracker';

// 로그인 성공
await recordAuthAttempt({
  identifier: 'user@example.com',
  success: true,
  ipAddress: '127.0.0.1',
  userAgent: 'Mozilla/5.0...'
});

// 로그인 실패
await recordAuthAttempt({
  identifier: 'user@example.com',
  success: false,
  ipAddress: '127.0.0.1'
});

// CAPTCHA 표시 여부 확인
const needsCaptcha = await shouldShowCaptcha('user@example.com'); // true/false
```

---

## 테스트 방법

### 개발 환경에서 CAPTCHA 테스트

1. **실패 횟수 트리거**:
   - Prisma Studio를 열어 `AuthAttempt` 테이블에 수동으로 실패 기록 추가:
     ```bash
     npx prisma studio
     ```
   - `AuthAttempt` 테이블에 다음 레코드 추가:
     - `identifier`: `127.0.0.1`
     - `ipAddress`: `127.0.0.1`
     - `failureCount`: `5`
     - `captchaRequired`: `true`
     - `attemptedAt`: 현재 시간

2. **로그인 페이지 접속**:
   ```bash
   npm run dev
   ```
   - http://localhost:3000/sign-in 접속
   - CAPTCHA 위젯이 표시되는지 확인

3. **CAPTCHA 완료**:
   - 체크박스 클릭하여 챌린지 완료
   - 로그인 버튼이 활성화되는지 확인

4. **로그 확인**:
   ```bash
   npx prisma studio
   ```
   - `AuthLog` 테이블에서 다음 이벤트 확인:
     - `CAPTCHA_CHALLENGE`
     - `CAPTCHA_SUCCESS`

### 프로덕션 테스트

- Cloudflare Turnstile은 프로덕션 도메인에서도 동일하게 작동
- Site Key를 프로덕션 도메인으로 발급받아 환경 변수 설정

---

## 보안 고려사항

### 1. IP 주소 기반 추적

**장점**:
- 이메일 열거 공격(Email Enumeration) 방지
- 분산 공격 탐지 가능

**단점**:
- VPN/프록시 사용자는 동일 IP로 보일 수 있음
- 공용 네트워크(카페, 공항)에서 오탐 가능

**개선 방안** (Phase 8+):
- 이메일 + IP 조합으로 추적
- Device Fingerprinting 추가
- 지리적 위치 기반 이상 탐지

### 2. CAPTCHA 우회 방지

- Turnstile 토큰은 1회만 사용 가능 (중복 제출 방지)
- 토큰 만료 시간: 5분
- 서버 측 검증 필수 (클라이언트 검증은 우회 가능)

### 3. Rate Limiting

**현재 구현**:
- 5회 연속 실패 → CAPTCHA 요구
- 1시간 후 자동 리셋

**미래 개선 사항**:
- Vercel Edge Middleware에서 Rate Limiting 적용
- Redis를 사용한 분산 Rate Limiting
- 10회 이상 실패 → 임시 계정 잠금 (1시간)

---

## 성능 요구사항

- **CAPTCHA 포함 로그인**: 60초 이내 완료 (SC-009)
- **API 응답 시간**: p95 < 500ms
- **데이터베이스 쿼리**: 인덱스 최적화
  - `AuthAttempt.identifier` 인덱스
  - `AuthAttempt.ipAddress` 인덱스
  - `AuthAttempt.attemptedAt` 인덱스

---

## 데이터 정리 (GDPR 준수)

### 자동 정리 (크론잡 필요)

```typescript
import { cleanupOldAttempts } from '@/lib/auth/attempt-tracker';

// 1시간 이상 경과한 실패 기록 삭제
const deletedCount = await cleanupOldAttempts();
console.log(`${deletedCount}개의 오래된 실패 기록 삭제됨`);
```

**권장 스케줄**:
- 매일 새벽 3시 실행
- Vercel Cron Jobs 사용:
  ```typescript
  // app/api/cron/cleanup-auth-attempts/route.ts
  export async function GET(request: Request) {
    const authHeader = request.headers.get('authorization');
    if (authHeader !== `Bearer ${process.env.CRON_SECRET}`) {
      return new Response('Unauthorized', { status: 401 });
    }

    const count = await cleanupOldAttempts();
    return Response.json({ deleted: count });
  }
  ```

---

## 모니터링 및 알림

### Sentry 통합 (Phase 9+)

```typescript
import * as Sentry from '@sentry/nextjs';

// CAPTCHA 검증 실패 추적
Sentry.captureMessage('CAPTCHA verification failed', {
  level: 'warning',
  extra: {
    ipAddress: '127.0.0.1',
    errorCodes: ['invalid-input-response'],
  },
});
```

### 알림 트리거

- **일일 CAPTCHA 요구 횟수 > 100회**: 관리자 이메일 알림
- **시간당 CAPTCHA 실패 > 50회**: Slack 알림
- **특정 IP에서 10회 이상 연속 실패**: 보안 팀 알림

---

## FAQ

### Q1. CAPTCHA가 표시되지 않아요.

**A**: 다음 사항을 확인하세요:
1. `NEXT_PUBLIC_TURNSTILE_SITE_KEY` 환경 변수가 설정되어 있는지
2. Cloudflare Dashboard에서 Site Key가 활성화되어 있는지
3. 브라우저 콘솔에 에러가 있는지 확인
4. 로그인 시도를 5회 이상 실패했는지 확인

### Q2. CAPTCHA를 통과했는데도 로그인이 안 돼요.

**A**: CAPTCHA 통과는 로그인 버튼을 활성화하는 것뿐이며, 실제 로그인은 Google OAuth를 통해 이루어집니다. CAPTCHA 통과 후 "Google로 로그인" 버튼을 클릭하세요.

### Q3. 1시간 이내에 카운터를 리셋하고 싶어요.

**A**: Prisma Studio에서 해당 `AuthAttempt` 레코드를 삭제하거나, `resetAttemptCounter()` 함수를 호출하세요.

### Q4. 프로덕션에서 CAPTCHA가 작동하지 않아요.

**A**: Cloudflare Turnstile Site Key를 발급받을 때 프로덕션 도메인을 입력했는지 확인하세요. `localhost`용 키는 프로덕션에서 작동하지 않습니다.

### Q5. CAPTCHA가 너무 자주 표시돼요.

**A**: `lib/auth/attempt-tracker.ts`의 `MAX_FAILED_ATTEMPTS` 상수를 5에서 더 높은 값(예: 10)으로 변경하세요. 단, 보안 위험이 증가합니다.

---

## 다음 단계

### Phase 8: 세션 관리 개선

- **세션 자동 연장**: 활동 중인 사용자의 세션 자동 연장
- **디바이스 관리**: 로그인된 디바이스 목록 및 원격 로그아웃
- **세션 모니터링**: 동시 세션 수 제한

### Phase 9: 고급 보안 기능

- **2단계 인증 (2FA)**: TOTP 기반 2FA
- **지리적 위치 추적**: IP 기반 로그인 위치 표시
- **이상 탐지**: 머신러닝 기반 비정상 로그인 패턴 탐지

---

## 참고 자료

- [Cloudflare Turnstile 문서](https://developers.cloudflare.com/turnstile/)
- [Turnstile Server-Side Validation](https://developers.cloudflare.com/turnstile/get-started/server-side-validation/)
- [OWASP Brute Force Attack Prevention](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html#brute-force-prevention)
- [Prisma Client API](https://www.prisma.io/docs/concepts/components/prisma-client)

---

**문서 버전**: 1.0
**마지막 업데이트**: 2025-01-04
**작성자**: Claude Code
