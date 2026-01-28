# Toss Apps-in-Toss 용 React Native 앱 개발

- AI 영어 회화 튜터
  - https://github.com/freelife1191/english-tutor-app
- AI 이력서 작성기
  - https://github.com/freelife1191/ai-resume-builder-app
- AI 회의록 자동 정리기
  - https://github.com/freelife1191/ai-meeting-notes-app

데모 페이지는 Supabase, Google Cloud 만료로 현재 제공되고 있지 않음

Claude Code에서 Spec-Kit 을 테스트 해보고자 개발을 시작했고 React Native APP을 하루만에 개발 완료
간단한 기능의 앱이지만 React Native 개발을 한번도 해보지 않은 입장에서 Spec-Kit을 사용해 큰 어려움 없이 개발이 완료되는 것을 경험함

벌써 오래된 기술이 되버리긴 했지만 스펙 주도 개발을 통해 기존 프롬프트와 PRD 문서에만 의지해 개발할 때보다 AI가 훨씬 더 프로젝트를 잘 이해하고 개발하고 오류 발생율이 줄어드는 것을 체감함

https://github.com/github/spec-kit

### 문제점
- AI에게 명확한 스펙과 함께 지시를 하는것이 중요하긴하지만 너무 과도한 정보는 오히려 독이 되는 것을 깨달음
- 문서와 지침을 간소화하고 Spec 주도 방식으로 진행하며 단위별로 명확한 지침과 지시를 통해 AI가 단계별로 정확하게 업무를 이해하고 수행할 수 있도록 개발 방식으로 바꿨음
- AI의 이해도도 높아져 단계별로 정확하게 지시를 이해하고 업무를 수행해서 프로젝트 개발 시간을 단축시키는 것을 경험함
  - [Spec-kit스터디](./docs/Spec-Kit스터디.md) 내용 참고

### 개발 기간
10시간 (1일)

### AI 활용 범위
- 레퍼런스 문서 및 PRD 생성: Gemini + Perplexity
- SaaS 기획 및 설계: Speckit + Claude 생성 (100%)
- 테스트 코드: Claude 활용 (100%)

### 본인 기여
- 아키텍처 설계: 100%
- 비즈니스 로직 구현: 100%
- 통합 및 배포: 100%