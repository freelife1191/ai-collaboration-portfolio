# 토스 앱인토스 개발 참고 문서 (Memory Reference)

> **목적**: AI 개발 시 토큰 효율적으로 토스 앱인토스 문서를 참조하기 위한 메모리 문서
> **최종 업데이트**: 2025-10-26

---

## 📚 문서 사용 전략

### 토큰 최적화 원칙
1. **계층적 참조**: 이 문서 → 상세 API Docs → 공식 문서 순으로 참조
2. **컨텍스트 최소화**: 필요한 섹션만 선택적으로 로드
3. **캐싱 전략**: 자주 사용하는 API 패턴은 이 문서에서 직접 참조
4. **동적 fetch**: 상세 스펙 필요 시에만 WebFetch 도구로 공식 문서 조회

### 개발 워크플로우
```
[개발 요청]
  ↓
[이 메모리 문서에서 핵심 개념/API 확인]
  ↓
[필요 시 특정 API 페이지만 WebFetch로 조회]
  ↓
[구현]
```

---

## 🎯 핵심 개념 (Core Concepts)

### 앱인토스란?
- 토스 앱 내 파트너 서비스를 "앱인앱" 형태로 노출하는 플랫폼
- 3,000만 누적 사용자 접근 가능
- WebView 및 React Native 기반 SDK 제공

### 기술 스택
| 구분 | 기술 |
|------|------|
| 프레임워크 | Granite (공통 런타임) |
| SDK 타입 | WebView용, React Native용 |
| 배포 | 토스 CDN (자동 호스팅) |
| 라우팅 | 파일 기반 (Next.js 스타일) |

### 지원 프레임워크 (공식 문서 확인, 2025-10-27)
| 프레임워크 | 지원 여부 | 비고 |
|----------|----------|------|
| **WebView** | ✅ 완전 지원 | HTML/CSS/JavaScript |
| **React Native** | ✅ 완전 지원 | 네이티브 앱 개발 |
| **Unity** | ✅ 지원 | WebView 기반 포팅 |
| **Flutter** | ❌ 미지원 | 공식 SDK 제공 없음 |

**출처**:
- https://developers-apps-in-toss.toss.im/intro/overview.html
- https://developers-apps-in-toss.toss.im/bedrock/reference/framework/시작하기/intro.html

### 시스템 요구사항
- Android: 7.0+
- iOS: 16.0+
- 사용자: 만 19세 이상

### 개발 기간 가이드
- 게임: 2~4주
- 비게임: 2~3개월
- 검수: 영업일 기준 2~3일 (4단계: 운영/디자인/기능/보안)

---

## 🛠️ SDK 핵심 기능

### 초기화 (AppsInToss.registerApp)
```typescript
AppsInToss.registerApp({
  appName: 'my-service'
  // 이것만으로 핵심 기능 활성화
})
```

**자동 제공 기능**:
- ✅ 파일 기반 라우팅 (`/pages/index.ts` → `intoss://my-service`)
- ✅ 쿼리 파라미터 처리
- ✅ 뒤로 가기 제어
- ✅ 화면 가시성 감지

### InitialProps
네이티브에서 전달되는 초기 데이터 (플랫폼별 상이)
- 포함 정보: 플랫폼, 테마, 네트워크 상태, URL 스킴, 폰트 설정

---

## 🔌 API 카테고리 및 우선순위

### Tier 1: 필수 API (로그인/결제)

#### 1. 간편 로그인 (토스 로그인)
**엔드포인트 기반**: `apps-in-toss-api.toss.im`

| API | 용도 | 우선순위 |
|-----|------|---------|
| AccessToken 발급 | 최초 인증 | 🔴 High |
| AccessToken 재발급 | 세션 유지 | 🔴 High |
| 사용자 정보 조회 | 프로필 획득 | 🟡 Medium |
| 로그인 연결 해제 | 계정 연동 해제 | 🟢 Low |

**핵심 플로우**:
```
사용자 진입 → AccessToken 발급 → 사용자 정보 조회 → 세션 관리
```

#### 2. 간편 결제 (토스페이)
**엔드포인트 기반**: `pay-apps-in-toss-api.toss.im`

| API | 용도 | 우선순위 |
|-----|------|---------|
| 결제 생성 | 결제 요청 시작 | 🔴 High |
| 결제 실행 | 최종 결제 처리 | 🔴 High |
| 결제 상태 조회 | 결제 확인 | 🟡 Medium |
| 결제 환불 | 환불 처리 | 🟡 Medium |

**핵심 플로우**:
```
상품 선택 → 결제 생성 → 결제 실행 → 상태 조회/환불
```

### Tier 2: 부가 기능 API

#### 3. 메시지 발송
- 사용자 알림 및 커뮤니케이션

#### 4. 토스 포인트 지급 (프로모션)
- 리워드 지급 키 생성
- 프로모션 리워드 지급
- 지급 결과 조회

**사용 시나리오**: 사용자 인센티브, 이벤트 보상

#### 5. 인앱 결제
- 결제 상태 조회 (모바일 앱 내 구매 관리)

---

## 🔐 보안 요구사항

### mTLS 인증서
- **필수**: API 호출 시 mTLS 인증서 설정 필요
- 콘솔에서 사전 등록 필요

### 방화벽 설정
- IP 화이트리스트 등록 필수
- 허용 도메인:
  - `apps-in-toss-api.toss.im`
  - `pay-apps-in-toss-api.toss.im`

---

## 💰 수익화 옵션

1. **인앱 광고** (AdMob 통합)
2. **토스페이 결제**
3. **인앱 결제**
4. **토스 포인트 프로모션**

---

## 📋 API 공통 응답 형식

모든 API는 일관된 SUCCESS/FAIL 형식 제공
```json
{
  "resultType": "SUCCESS" | "FAIL",
  "data": { ... },
  "error": { ... }
}
```

---

## 🚀 빠른 시작 체크리스트

### 개발 준비
- [ ] SDK 선택 (WebView / React Native)
- [ ] 개발 환경 설정
- [ ] mTLS 인증서 발급 및 등록
- [ ] IP 화이트리스트 등록

### 필수 구현
- [ ] `AppsInToss.registerApp` 초기화
- [ ] 로그인 플로우 (AccessToken 발급/재발급)
- [ ] 파일 기반 라우팅 구조 설계
- [ ] 결제 플로우 (필요 시)

### 배포 전
- [ ] 빌드 결과물 생성
- [ ] 콘솔에 업로드
- [ ] 4단계 검수 통과 (운영/디자인/기능/보안)

---

## 📖 상세 문서 참조 가이드

### 언제 공식 문서를 WebFetch로 조회해야 하나?

#### 즉시 조회 필요 (이 문서로 불충분)
- ❌ API 요청/응답 상세 스키마
- ❌ 에러 코드 및 처리 방법
- ❌ 특정 SDK 메서드 파라미터 상세
- ❌ 고급 설정 옵션

#### 이 문서로 충분 (조회 불필요)
- ✅ 앱인토스 개념 이해
- ✅ 개발 프로세스 파악
- ✅ API 카테고리 및 용도 확인
- ✅ 프로젝트 초기 구조 설계

### 추천 조회 패턴
```typescript
// 개발 시작 단계
1. 이 메모리 문서 전체 스캔 (전체 구조 파악)
2. 해당 기능 섹션 집중 참조 (예: "로그인 API")
3. 구현 중 상세 스펙 필요 시 WebFetch 사용

// 예시: 로그인 구현 시
WebFetch("https://developers-apps-in-toss.toss.im/api/토스로그인.html",
  "AccessToken 발급 API의 요청 헤더, 바디, 응답 형식, 에러 코드를 추출해주세요")
```

---

## 🎓 개발 시 참조 순서

### Level 1: 프로젝트 초기화 (이 문서 100% 활용)
- 핵심 개념 이해
- 기술 스택 선택
- 개발 계획 수립
- 필수 API 식별

### Level 2: 기능 구현 (이 문서 70% + WebFetch 30%)
- 이 문서에서 API 카테고리/플로우 확인
- 상세 스펙 필요 시 해당 API 문서만 WebFetch

### Level 3: 디버깅/최적화 (WebFetch 중심)
- 에러 코드 조회
- 고급 옵션 확인
- 베스트 프랙티스 검색

---

## 📌 중요 링크 (필요 시 WebFetch 활용)

- **공식 가이드**: https://developers-apps-in-toss.toss.im/intro/overview.html
- **API Reference**: https://developers-apps-in-toss.toss.im/api/overview.html
- **SDK Reference**: https://developers-apps-in-toss.toss.im/bedrock/reference/framework/시작하기/intro.html
- **앱인토스 홈**: https://toss.im/apps-in-toss

---

## 🧠 AI 개발 최적화 팁

### 컨텍스트 관리
- 이 문서는 약 2,000 토큰 이하로 설계됨
- 전체 로드해도 컨텍스트 부담 최소화
- 개발 세션마다 초반에 한 번만 로드 권장

### 동적 학습 패턴
```
[세션 시작]
  ↓
[메모리 문서 로드] ← 이 파일
  ↓
[기능 구현 중 상세 정보 필요]
  ↓
[WebFetch로 특정 섹션만 조회]
  ↓
[구현 완료 후 새로운 인사이트 발견 시]
  ↓
[이 메모리 문서에 추가/업데이트] ← 지속적 개선
```

### 효율적인 질문 패턴
❌ 비효율적: "토스 앱인토스 전체 문서를 분석해줘"
✅ 효율적: "이 메모리 문서 기준으로 로그인 구현 방법 알려줘. 필요하면 상세 API 스펙만 조회해줘"

---

**END OF MEMORY REFERENCE**
