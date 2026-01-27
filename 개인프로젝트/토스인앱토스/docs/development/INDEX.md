# development/ 디렉토리 인덱스

> **목적**: 토스 앱인토스 앱 개발을 위한 기술 문서 및 개발 가이드
> **사용 방법**: 기획 및 개발 시 기술 스택, 아키텍처, 최적화 기법 참조

---

## 📋 문서 목록 (총 11개)

### 1️⃣ 기술 스택 & 아키텍처

#### TECH_STACK.md ⭐ 핵심 참조
**파일 경로**: `development/TECH_STACK.md`

**설명**: 토스 앱인토스 개발을 위한 전체 기술 스택 가이드

**주요 내용**:
- 필수 기술 스택 (React Native, Next.js, Nest.js, Fastify)
- 프론트엔드/백엔드/인프라 기술 선택 가이드
- 데이터베이스 선택 (PostgreSQL, Firestore, D1)
- AI 서비스 통합 (Gemini, Groq, Llama)
- 배포 플랫폼 (Vercel, Railway, Fly.io)

**참조 시점**:
- ✅ 새로운 앱 기획 시작 시
- ✅ 기술 스택 선택 시
- ✅ 아키텍처 설계 시

---

#### REACT_NATIVE_STACK.md
**파일 경로**: `development/REACT_NATIVE_STACK.md`

**설명**: React Native 앱 개발 완벽 가이드

**주요 내용**:
- React Native 프로젝트 구조
- 필수 라이브러리 (Navigation, State Management, UI)
- 성능 최적화 기법
- 네이티브 모듈 통합
- 빌드 및 배포 전략

**참조 시점**:
- ✅ React Native 앱 개발 시
- ✅ 네이티브 기능 구현 시
- ✅ 성능 최적화 필요 시

---

#### PWA_GUIDE.md
**파일 경로**: `development/PWA_GUIDE.md`

**설명**: Next.js PWA 개발 가이드 (앱스토어 없이 배포)

**주요 내용**:
- Next.js 공식 PWA 지원 (2024년 가을~)
- Manifest 파일 설정
- 서비스 워커 등록
- 오프라인 지원
- 푸시 알림 통합
- PWA vs 네이티브 앱 비교

**참조 시점**:
- ✅ 웹 앱 개발 시
- ✅ 앱스토어 수수료 절감 필요 시
- ✅ 빠른 배포 필요 시

---

### 2️⃣ UI/UX 디자인

#### UIUX_PLAN.md
**파일 경로**: `development/UIUX_PLAN.md`

**설명**: UI/UX 디자인 전략 및 가이드

**주요 내용**:
- 디자인 시스템 구축
- 컴포넌트 라이브러리 선택
- 반응형 디자인 전략
- 접근성 (Accessibility)
- 사용자 경험 최적화

**참조 시점**:
- ✅ 디자인 시스템 구축 시
- ✅ UI 컴포넌트 선택 시

---

#### UIUX_LAYOUT_GUIDE.md
**파일 경로**: `development/UIUX_LAYOUT_GUIDE.md`

**설명**: 레이아웃 및 화면 구성 가이드

**주요 내용**:
- 레이아웃 패턴 (Grid, Flexbox)
- 화면 구성 베스트 프랙티스
- 토스 앱인토스 디자인 가이드라인 준수
- 반응형 레이아웃

**참조 시점**:
- ✅ 화면 레이아웃 설계 시
- ✅ 반응형 디자인 구현 시

---

### 3️⃣ 성능 최적화

#### CACHEING-STRATEGY.md
**파일 경로**: `development/CACHEING-STRATEGY.md`

**설명**: 캐싱 전략 및 성능 최적화

**주요 내용**:
- 클라이언트 사이드 캐싱 (React Query, SWR)
- 서버 사이드 캐싱 (Redis, CDN)
- 이미지 최적화 (Next Image, FastImage)
- API 응답 캐싱
- 캐시 무효화 전략

**참조 시점**:
- ✅ 성능 최적화 필요 시
- ✅ API 요청 최소화 필요 시
- ✅ 사용자 경험 개선 시

---

### 4️⃣ 배포 & 인프라

#### DEPLOY_STACK.md
**파일 경로**: `development/DEPLOY_STACK.md`

**설명**: 배포 전략 및 CI/CD 구축

**주요 내용**:
- Vercel 배포 (웹앱, PWA)
- EAS 배포 (React Native)
- CI/CD 파이프라인 (GitHub Actions)
- 환경 변수 관리
- 도메인 설정
- 모니터링 설정

**참조 시점**:
- ✅ 앱 배포 준비 시
- ✅ CI/CD 자동화 필요 시
- ✅ 인프라 설계 시

---

### 5️⃣ AI 통합

#### AI_REFERENCE.md
**파일 경로**: `development/AI_REFERENCE.md`

**설명**: AI 서비스 통합 레퍼런스

**주요 내용**:
- 무료 AI API (Google Gemini 2.5, Groq, Llama)
- AI API 통합 방법
- 프롬프트 엔지니어링
- 비용 최적화 (무료 티어 활용)
- 에러 핸들링
- 레이트 리미팅

**참조 시점**:
- ✅ AI 기능 구현 시
- ✅ AI API 선택 시
- ✅ 비용 최적화 필요 시

---

### 6️⃣ 개발 도구 & 팁

#### MCP_SETUP_GUIDE.md
**파일 경로**: `development/MCP_SETUP_GUIDE.md`

**설명**: MCP (Model Context Protocol) 서버 설정 가이드

**주요 내용**:
- MCP 서버 개념
- 설정 방법
- Claude Code와 통합
- 사용 사례

**참조 시점**:
- ✅ MCP 서버 설정 필요 시
- ✅ Claude Code 통합 시

---

#### APP_DEVELOPMENT_TIPS.md ⭐ 개발 꿀팁
**파일 경로**: `development/APP_DEVELOPMENT_TIPS.md`

**설명**: 앱 개발 리소스 출처 및 2025년 트렌드 (웹 검색 기반)

**주요 내용**:
- 무료 디자인 리소스 사이트 (Flaticon, Icons8, Freepik)
- React Native UI 라이브러리 (Paper, Gluestack-UI, Tamagui)
- Next.js PWA 구축 (2025년 최신)
- 게임 개발 리소스 (Unity Asset Store, 사운드 사이트)
- 2025년 UI/UX 트렌드 (AI 디자인, 제로 UI, 다크모드)
- 성능 최적화 기법
- 개발 워크플로우 최적화

**참조 시점**:
- ✅ 리소스 출처 확인 필요 시
- ✅ 최신 트렌드 파악 시
- ✅ 개발 효율성 향상 필요 시

---

### 7️⃣ 버전 관리

#### VERSION_UPDATE_REPORT.md
**파일 경로**: `development/VERSION_UPDATE_REPORT.md`

**설명**: 기술 스택 버전 업데이트 이력

**주요 내용**:
- 주요 라이브러리 버전 변경 사항
- Breaking Changes
- 마이그레이션 가이드
- 업데이트 권장사항

**참조 시점**:
- ✅ 라이브러리 업데이트 시
- ✅ 버전 호환성 확인 시

---

## 🎯 사용 가이드

### 문서 우선순위 (기술 스택)

```
1순위: TECH_STACK.md
       ↓ (기술 선택 기준)
2순위: 플랫폼별 가이드
       - React Native: REACT_NATIVE_STACK.md
       - 웹앱: PWA_GUIDE.md
       ↓
3순위: 기능별 가이드
       - UI/UX: UIUX_PLAN.md, UIUX_LAYOUT_GUIDE.md
       - 성능: CACHEING-STRATEGY.md
       - AI: AI_REFERENCE.md
       ↓
4순위: 개발 팁 & 리소스
       - APP_DEVELOPMENT_TIPS.md (최신 트렌드)
```

### 기술 스택 선택 프로세스

```
1. TECH_STACK.md 읽기
   ↓ (필수 기술 스택 파악)
2. 앱 타입에 따라 선택
   - 네이티브: REACT_NATIVE_STACK.md
   - 웹/PWA: PWA_GUIDE.md
   ↓
3. 기능별 가이드 참조
   - AI 기능: AI_REFERENCE.md
   - 성능 최적화: CACHEING-STRATEGY.md
   ↓
4. 배포 전략 수립
   - DEPLOY_STACK.md
```

---

## 📊 문서 카테고리별 분류

### 기술 선택
- TECH_STACK.md
- REACT_NATIVE_STACK.md
- PWA_GUIDE.md

### 디자인
- UIUX_PLAN.md
- UIUX_LAYOUT_GUIDE.md

### 최적화
- CACHEING-STRATEGY.md
- APP_DEVELOPMENT_TIPS.md (성능 섹션)

### AI 통합
- AI_REFERENCE.md

### 배포 & 인프라
- DEPLOY_STACK.md
- MCP_SETUP_GUIDE.md

### 게임 개발
- APP_DEVELOPMENT_TIPS.md (섹션 4)
- ⚠️ 게임 개발 종합 가이드: [reports/GAME_APP_DEVELOPMENT_ANALYSIS_20251027.md](../reports/GAME_APP_DEVELOPMENT_ANALYSIS_20251027.md)

### 참고 자료
- APP_DEVELOPMENT_TIPS.md (리소스, 트렌드)
- VERSION_UPDATE_REPORT.md

---

## 🔗 관련 디렉토리

- **memories/**: 토스 플랫폼 핵심 지식
- **plans/**: AI 앱 기획 및 비즈니스 전략
- **gameplans/**: 게임 기획 및 전략
- **docs/**: 프로젝트 요구사항

---

## 📝 업데이트 이력

| 날짜 | 내용 |
|------|------|
| 2025-10-27 | INDEX.md 생성, APP_DEVELOPMENT_TIPS.md 추가 |
| 2025-10-27 | ~~analysis/ 서브디렉토리 생성~~ (규칙 위반, reports/로 이동) |

---

**[← 메인 INDEX로 돌아가기](../INDEX.md)**
