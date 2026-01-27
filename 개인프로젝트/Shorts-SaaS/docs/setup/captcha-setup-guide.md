# CAPTCHA 설정 가이드 (빠른 시작)

## 1. Cloudflare Turnstile 키 발급 (5분)

### 단계 1: Cloudflare 계정 생성/로그인

1. https://dash.cloudflare.com 접속
2. 계정이 없다면 무료 가입 (이메일 인증 필요)

### 단계 2: Turnstile Site 생성

1. 대시보드에서 **Turnstile** 메뉴 클릭
2. **Add Site** 버튼 클릭
3. 사이트 정보 입력:

   ```
   Site Name: YouTube Creator SaaS
   Domain: localhost (개발용)
   Widget Mode: Managed (권장)
   ```

4. **Create** 버튼 클릭

### 단계 3: 키 복사

생성 후 다음 정보 복사:

- **Site Key**: `0x4AAAAAAA...` (Public, 클라이언트 노출 가능)
- **Secret Key**: `0x4AAAAAAA...` (Private, 서버 전용)

---

## 2. 환경 변수 설정 (1분)

### .env.local 파일 수정

프로젝트 루트의 `.env.local` 파일에 다음 추가:

```bash
# Cloudflare Turnstile (CAPTCHA)
NEXT_PUBLIC_TURNSTILE_SITE_KEY="0x4AAAAAAA..."  # 위에서 복사한 Site Key
TURNSTILE_SECRET_KEY="0x4AAAAAAA..."            # 위에서 복사한 Secret Key
```

**주의**: `.env.local`은 Git에 커밋되지 않습니다. 팀원과 공유 시 Slack/이메일 등 안전한 채널 사용.

---

## 3. 개발 서버 실행 (1분)

```bash
npm run dev
```

서버가 시작되면 http://localhost:3000/sign-in 접속

---

## 4. CAPTCHA 테스트 (5분)

### 방법 1: Prisma Studio 사용 (권장)

1. **Prisma Studio 열기**:
   ```bash
   npx prisma studio
   ```

2. **AuthAttempt 테이블 선택**

3. **새 레코드 추가**:
   ```
   identifier: 127.0.0.1
   ipAddress: 127.0.0.1
   failureCount: 5
   captchaRequired: true
   attemptedAt: 현재 시간 (예: 2025-01-04T12:00:00.000Z)
   success: false
   ```

4. **Save** 클릭

5. **로그인 페이지 새로고침**:
   - http://localhost:3000/sign-in 접속
   - CAPTCHA 위젯이 표시되어야 함

6. **CAPTCHA 완료**:
   - 체크박스 클릭
   - "Google로 로그인" 버튼이 활성화되면 성공!

### 방법 2: 실제 실패 시도 (번거로움)

1. 로그인 페이지에서 **5회 연속 로그인 실패** 시도
   - 단, Google OAuth는 실패시키기 어려우므로 방법 1 권장

---

## 5. 로그 확인 (선택)

### AuthLog 테이블 확인

1. Prisma Studio에서 **AuthLog** 테이블 선택
2. 다음 이벤트 확인:
   - `CAPTCHA_CHALLENGE`: CAPTCHA 요구됨
   - `CAPTCHA_SUCCESS`: CAPTCHA 검증 성공
   - `CAPTCHA_FAILURE`: CAPTCHA 검증 실패 (있다면)

---

## 6. 프로덕션 배포 준비

### Cloudflare Turnstile 프로덕션 Site 생성

1. Cloudflare Dashboard → **Turnstile** → **Add Site**
2. 사이트 정보 입력:
   ```
   Site Name: YouTube Creator SaaS (Production)
   Domain: yourdomain.com (실제 도메인)
   Widget Mode: Managed
   ```
3. 생성된 **Site Key**와 **Secret Key**를 Vercel 환경 변수에 추가:
   - Vercel Dashboard → Project → Settings → Environment Variables
   - `NEXT_PUBLIC_TURNSTILE_SITE_KEY`: [프로덕션 Site Key]
   - `TURNSTILE_SECRET_KEY`: [프로덕션 Secret Key]

---

## 문제 해결

### CAPTCHA 위젯이 표시되지 않아요

**원인 1**: 환경 변수 미설정
```bash
# 확인
echo $NEXT_PUBLIC_TURNSTILE_SITE_KEY

# 없다면 .env.local 파일 확인
```

**원인 2**: 개발 서버 재시작 필요
```bash
# Ctrl+C로 서버 종료 후 재시작
npm run dev
```

**원인 3**: 실패 횟수 부족
- Prisma Studio에서 `failureCount`를 5 이상으로 설정했는지 확인

### CAPTCHA 검증이 실패해요

**원인**: Secret Key 불일치
- Cloudflare Dashboard에서 **Secret Key**를 다시 확인
- `.env.local`의 `TURNSTILE_SECRET_KEY`와 일치하는지 확인

### 브라우저 콘솔 에러

```
Failed to load Turnstile script
```

**해결**: 인터넷 연결 확인 또는 Cloudflare CDN 차단 여부 확인

---

## 다음 단계

- [CAPTCHA 및 Brute-Force 방어 문서](../features/captcha-brute-force-protection.md) 읽기
- Phase 8 세션 관리 개선 구현
- Sentry 통합하여 CAPTCHA 실패 모니터링

---

**소요 시간**: 약 10-15분
**난이도**: 쉬움
**마지막 업데이트**: 2025-01-04
