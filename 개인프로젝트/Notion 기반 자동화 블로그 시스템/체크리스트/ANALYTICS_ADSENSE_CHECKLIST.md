# Google Analytics & Google AdSense 통합 가이드

이 문서는 Google Analytics 4 (GA4)와 Google AdSense를 Next.js 15 Static Export 블로그에 통합하는 가이드입니다.

---

## 📊 Google Analytics 4 (GA4) 개요

### 목적
- **사용자 행동 분석**: 페이지뷰, 이벤트, 사용자 유입 경로 추적
- **데이터 기반 의사결정**: 어떤 콘텐츠가 인기 있는지, 사용자가 어디서 오는지 파악
- **성능 모니터링**: Core Web Vitals, 페이지 로딩 속도 측정

### 주요 기능
1. **자동 이벤트 추적**
   - 페이지뷰 (page_view)
   - 스크롤 (scroll)
   - 아웃바운드 클릭 (outbound_click)
   - 사이트 검색 (site_search)
   - 파일 다운로드 (file_download)

2. **맞춤 이벤트**
   - 포스트 읽기 완료
   - 태그 클릭
   - 소셜 링크 클릭
   - 다크모드 토글

3. **사용자 속성**
   - 국가, 도시, 언어
   - 기기 종류 (모바일/데스크톱)
   - 브라우저 종류

### 설정 방법
1. Google Analytics 계정 생성
2. GA4 속성 생성 (측정 ID 발급: `G-XXXXXXXXXX`)
3. Next.js에 gtag.js 스크립트 삽입
4. Notion에서 측정 ID 관리

---

## 💰 Google AdSense 개요

### 목적
- **수익 창출**: 블로그 콘텐츠로부터 광고 수익 발생
- **자동 광고 최적화**: Google이 자동으로 최적의 광고 위치 선택
- **사용자 경험 유지**: 관련성 높은 광고만 표시

### 주요 광고 유형
1. **디스플레이 광고**
   - 배너 광고 (상단, 사이드바, 하단)
   - 반응형 광고 (모든 화면 크기에 자동 조정)

2. **피드 내 광고** (In-feed Ads)
   - 포스트 목록 사이에 자연스럽게 삽입
   - 네이티브 광고 형식

3. **문서 내 광고** (In-article Ads)
   - 포스트 본문 사이에 삽입
   - 콘텐츠와 유사한 스타일로 표시

4. **자동 광고** (Auto Ads)
   - Google이 자동으로 최적 위치 선택
   - 설정 한 번으로 모든 페이지에 적용

### 승인 요구사항
- 최소 콘텐츠 양: 20-30개 이상의 고품질 포스트
- 충분한 트래픽: 최소 월 1,000 페이지뷰 권장
- 독자적 콘텐츠: 복사/붙여넣기 금지
- 정책 준수: Google 게시자 정책 준수 필요

### 설정 방법
1. Google AdSense 계정 생성
2. 사이트 등록 및 승인 대기 (1-2주 소요)
3. 광고 코드 발급 (ca-pub-XXXXXXXXXXXXXXXX)
4. Next.js에 AdSense 스크립트 삽입
5. Notion에서 Publisher ID 관리

---

## 🔧 Notion 데이터베이스 스키마 확장

### Settings 데이터베이스에 추가할 속성

| 속성 이름 | 타입 | 설명 | 예시 |
|----------|------|------|------|
| **GA4 Measurement ID** | Text | Google Analytics 4 측정 ID | `G-XXXXXXXXXX` |
| **AdSense Publisher ID** | Text | Google AdSense 게시자 ID | `ca-pub-XXXXXXXXXXXXXXXX` |
| **Enable Analytics** | Checkbox | Analytics 활성화 여부 | ✅ |
| **Enable AdSense** | Checkbox | AdSense 활성화 여부 | ✅ |
| **AdSense Auto Ads** | Checkbox | AdSense 자동 광고 활성화 | ✅ |

### 설정 예시
```
Settings 페이지 (Notion):
┌─────────────────────────────────────────┐
│ Name: Blog Settings                     │
│ GA4 Measurement ID: G-ABC123XYZ         │
│ AdSense Publisher ID: ca-pub-1234567... │
│ Enable Analytics: ✅                    │
│ Enable AdSense: ✅                      │
│ AdSense Auto Ads: ✅                    │
└─────────────────────────────────────────┘
```

---

## ✅ 구현 체크리스트

### Phase 1: Google Analytics 4 (GA4) 통합

- [ ] **1-1. SiteSettings 타입 확장**
  - [ ] `ga4MeasurementId?: string` 속성 추가
  - [ ] `enableAnalytics?: boolean` 속성 추가

- [ ] **1-2. Notion Client 수정**
  - [ ] `getSiteSettings()` 함수에서 GA4 설정 가져오기
  - [ ] Settings 페이지에서 새 속성 파싱

- [ ] **1-3. Google Analytics 컴포넌트 생성**
  - [ ] `src/components/GoogleAnalytics.tsx` 생성
  - [ ] GA4 gtag.js 스크립트 로딩
  - [ ] 환경별 처리 (개발/프로덕션)
  - [ ] 페이지뷰 자동 추적

- [ ] **1-4. Layout 통합**
  - [ ] `src/app/layout.tsx`에 GoogleAnalytics 컴포넌트 추가
  - [ ] Notion 설정에 따라 조건부 렌더링

- [ ] **1-5. 맞춤 이벤트 추적 (선택 사항)**
  - [ ] 태그 클릭 이벤트
  - [ ] 소셜 링크 클릭 이벤트
  - [ ] 다크모드 토글 이벤트
  - [ ] 포스트 읽기 완료 이벤트

### Phase 2: Google AdSense 통합

- [ ] **2-1. SiteSettings 타입 확장**
  - [ ] `adsensePublisherId?: string` 속성 추가
  - [ ] `enableAdsense?: boolean` 속성 추가
  - [ ] `adsenseAutoAds?: boolean` 속성 추가

- [ ] **2-2. Notion Client 수정**
  - [ ] `getSiteSettings()` 함수에서 AdSense 설정 가져오기

- [ ] **2-3. Google AdSense 컴포넌트 생성**
  - [ ] `src/components/GoogleAdSense.tsx` 생성
  - [ ] AdSense 스크립트 로딩
  - [ ] Auto Ads 지원

- [ ] **2-4. 광고 배치 컴포넌트 (선택 사항)**
  - [ ] `src/components/ads/DisplayAd.tsx` - 디스플레이 광고
  - [ ] `src/components/ads/InFeedAd.tsx` - 피드 내 광고
  - [ ] `src/components/ads/InArticleAd.tsx` - 문서 내 광고

- [ ] **2-5. Layout 통합**
  - [ ] `src/app/layout.tsx`에 GoogleAdSense 컴포넌트 추가
  - [ ] Notion 설정에 따라 조건부 렌더링

- [ ] **2-6. 광고 배치**
  - [ ] 홈페이지: 포스트 목록 사이 (In-feed)
  - [ ] 포스트 페이지: 본문 상단/중간/하단 (In-article)
  - [ ] 사이드바: 디스플레이 광고 (선택 사항)

### Phase 3: 테스트 및 검증

- [ ] **3-1. Google Analytics 테스트**
  - [ ] 개발 환경에서 Analytics 비활성화 확인
  - [ ] 프로덕션 빌드에서 gtag.js 로딩 확인
  - [ ] Google Analytics Realtime 보고서에서 페이지뷰 확인
  - [ ] Chrome DevTools로 gtag 이벤트 확인

- [ ] **3-2. Google AdSense 테스트**
  - [ ] AdSense 스크립트 로딩 확인
  - [ ] 광고 코드 올바르게 삽입 확인
  - [ ] 테스트 광고 표시 확인 (승인 전)
  - [ ] CLS (Cumulative Layout Shift) 영향 최소화 확인

- [ ] **3-3. 성능 테스트**
  - [ ] Lighthouse 점수 확인 (Analytics/AdSense 추가 전후 비교)
  - [ ] Core Web Vitals 측정
  - [ ] 번들 크기 증가량 확인

### Phase 4: 문서화

- [ ] **4-1. Notion 설정 가이드 작성**
  - [ ] Settings 데이터베이스 속성 추가 방법
  - [ ] GA4 측정 ID 발급 방법
  - [ ] AdSense Publisher ID 발급 방법

- [ ] **4-2. 코드 문서화**
  - [ ] README.md 업데이트
  - [ ] 환경 변수 가이드
  - [ ] 커스텀 이벤트 추적 가이드

---

## 🎯 우선순위

### Priority 1: Google Analytics 4 (필수)
- 사용자 행동 분석 및 데이터 수집은 블로그 성장에 필수적
- 무료이며 설정이 간단함
- 개인정보 보호 정책 준수 가능

**예상 작업 시간**: 1-2시간

### Priority 2: Google AdSense (선택 사항)
- 수익 창출이 목표인 경우에만 구현
- 승인 과정이 필요하며 시간이 소요됨
- 광고로 인한 UX 저하 가능성 고려 필요

**예상 작업 시간**: 2-3시간 (승인 대기 시간 제외)

---

## 📝 Notion Settings 데이터베이스 구조 예시

### 기존 속성
```
Name: 사이트 이름
Profile Image: 프로필 이미지
Job Title: 직업
Bio: 소개
Home Title: 홈 제목
Home Description: 홈 설명
Copyright Text: 저작권 문구
Footer Text: 푸터 문구
Social Links (소셜 링크들...): kakaoChannel, kakao, instagram, blog, email, etc.
```

### 추가할 속성 (Analytics & AdSense)
```
GA4 Measurement ID: G-XXXXXXXXXX
AdSense Publisher ID: ca-pub-XXXXXXXXXXXXXXXX
Enable Analytics: ✅
Enable AdSense: ✅
AdSense Auto Ads: ✅
```

---

## 🔒 개인정보 보호 고려사항

### Google Analytics
- **쿠키 사용**: GA4는 사용자 추적을 위해 쿠키 사용
- **쿠키 동의 배너**: GDPR/CCPA 준수를 위해 필요 (한국은 선택 사항)
- **IP 익명화**: GA4는 기본적으로 IP 익명화 적용
- **데이터 보존 기간**: 2개월 또는 14개월 선택 가능

### Google AdSense
- **맞춤 광고**: 사용자 관심사 기반 광고 표시
- **광고 설정**: 사용자가 광고 맞춤 설정 비활성화 가능
- **개인정보 처리방침**: 사이트에 개인정보 처리방침 페이지 필요

### 권장 사항
- 개인정보 처리방침 페이지 추가
- 쿠키 사용 안내 추가 (선택 사항)
- Google Analytics 및 AdSense 사용 명시

---

## 📚 참고 자료

### Google Analytics 4
- [GA4 공식 문서](https://support.google.com/analytics/answer/10089681)
- [Next.js에서 GA4 사용하기](https://nextjs.org/docs/app/building-your-application/optimizing/analytics)
- [gtag.js 참조](https://developers.google.com/analytics/devguides/collection/gtagjs)

### Google AdSense
- [AdSense 시작 가이드](https://support.google.com/adsense/answer/10162)
- [AdSense 정책](https://support.google.com/adsense/answer/48182)
- [Auto Ads 가이드](https://support.google.com/adsense/answer/9274025)

### Next.js Script Component
- [Next.js Script 컴포넌트](https://nextjs.org/docs/app/api-reference/components/script)
- [Third-Party Scripts Optimization](https://nextjs.org/docs/app/building-your-application/optimizing/scripts)

---

## 🎨 구현 예시 코드 (미리보기)

### GoogleAnalytics.tsx
```typescript
'use client'

import Script from 'next/script'

interface GoogleAnalyticsProps {
  measurementId: string
}

export function GoogleAnalytics({ measurementId }: GoogleAnalyticsProps) {
  return (
    <>
      <Script
        src={`https://www.googletagmanager.com/gtag/js?id=${measurementId}`}
        strategy="afterInteractive"
      />
      <Script id="google-analytics" strategy="afterInteractive">
        {`
          window.dataLayer = window.dataLayer || [];
          function gtag(){dataLayer.push(arguments);}
          gtag('js', new Date());
          gtag('config', '${measurementId}');
        `}
      </Script>
    </>
  )
}
```

### GoogleAdSense.tsx
```typescript
'use client'

import Script from 'next/script'

interface GoogleAdSenseProps {
  publisherId: string
  autoAds?: boolean
}

export function GoogleAdSense({ publisherId, autoAds = true }: GoogleAdSenseProps) {
  return (
    <Script
      async
      src={`https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=${publisherId}`}
      crossOrigin="anonymous"
      strategy="afterInteractive"
    />
  )
}
```

### layout.tsx 통합
```typescript
import { GoogleAnalytics } from '@/components/GoogleAnalytics'
import { GoogleAdSense } from '@/components/GoogleAdSense'

export default async function RootLayout({ children }) {
  const settings = await notionClient.getSiteSettings()

  return (
    <html>
      <body>
        {/* Google Analytics */}
        {settings.enableAnalytics && settings.ga4MeasurementId && (
          <GoogleAnalytics measurementId={settings.ga4MeasurementId} />
        )}

        {/* Google AdSense */}
        {settings.enableAdsense && settings.adsensePublisherId && (
          <GoogleAdSense
            publisherId={settings.adsensePublisherId}
            autoAds={settings.adsenseAutoAds}
          />
        )}

        {children}
      </body>
    </html>
  )
}
```

---

## 💡 다음 단계

1. **조사 완료 확인**: 이 문서 리뷰
2. **Notion 데이터베이스 준비**: Settings 페이지에 새 속성 추가
3. **코드 구현**: Google Analytics 먼저, AdSense는 선택 사항
4. **테스트**: 로컬 및 프로덕션 환경에서 검증
5. **배포**: GitHub Pages에 배포 후 실제 데이터 확인
