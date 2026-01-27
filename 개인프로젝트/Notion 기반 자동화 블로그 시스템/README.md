# Notion 기반 자동화 블로그 시스템

링크드인 포스팅을 보던 중 홍민지님의 포스팅을 보고 영감을 받아 개발
https://www.linkedin.com/feed/update/urn:li:activity:7381246347204624385/

홍민지님의 Notion CMS 블로그
https://hong-minji.github.io/works/

홍민지님은 Notion CMS + Gatsby 였지만 바이브코딩 스터디겸 Notion CMS + Next.js 기술로 2025년10월24일 개발을 시작해서 2025년10월27일 개발을 완료

## 프로젝트 링크

- Notion 포트폴리오 링크
  - https://www.notion.so/freejava/Notion-2999b0db66b280679a23c79154cbd8d7
- GitHub Repository
  - https://github.com/freelife1191/nextjs-notion-blog
- Sample Blog
  - https://notionblogsample.github.io/

## PRD 개선

### 초안 

[PRD초안](./프롬프트/PRD초안.md)

레퍼런스 블로그에서 UI만을 참고해 PRD초안 생성

### 개선 후 

개발 레퍼런스 문서를 참고해서 [PRD V2](./프롬프트/PRD_v2.md) 생성

- 개발 레퍼런스 문서
  - [Description](./개발%20레퍼런스%20문서/Description.md)
  - [NOTION_BLOCK_SUPPORT_ANALYSIS](./개발%20레퍼런스%20문서/NOTION_BLOCK_SUPPORT_ANALYSIS.md)
  - [REFACTORING_PLAN](./개발%20레퍼런스%20문서/REFACTORING_PLAN.md)
  - [STYLE_COMPARISON_ANALYSIS](./개발%20레퍼런스%20문서/STYLE_COMPARISON_ANALYSIS.md)
  - [TECH_STACK](./개발%20레퍼런스%20문서/TECH_STACK.md)
  - [UIUX_LAYOUT_GUIDE](./개발%20레퍼런스%20문서/UIUX_LAYOUT_GUIDE.md)
  - [UIUX_PLAN](./개발%20레퍼런스%20문서/UIUX_PLAN.md)

- 개발 레퍼런스 문서를 참고해 구체화한 PRD 문서
  - 프로젝트 개요
  - 기술 스택
  - 시스템 아키텍처
  - Notion 데이터 구조
  - 기능 요구사항
  - 성능 최적화
  - 환경 변수
  - 개발 명령어
  - 제약사항 및 고려사항
  - 성공 기준
  - 참고문서

### 개선 효과

UI와 대략적인 컨셉만 갖춘 PRD 문서에서 구체적인 기술스텍, 아키텍처, 세부 개발사항들을 명확히 지시해서 바이브코딩의 구현 정확도를 높임

## AI 활용 범위

- 개발 레퍼런스 문서 및 PRD 문서 생성: Gemini (50%) + Perplexity (50%)
- 체크리스트 문서 생성 및 개발: ClaudeCode 주 개발 (90%) + Cursor 트러블 슈팅 (10%)
- 코드 검증: Gemini CLI, Codex CLI, Qwen Coder

## 바이브코딩

### 개발 방식

- 할루시네이션 최소화 
  - PRD 문서에 기술과 아키텍처를 최대한 상세히 명시
  - ClaudeCode 를 통해 단위별 체크리스트 문서를 생성
  - 이후 체크리스트를 따라 진행하면서 단위별로 완성될때마다 테스트
  - 테스트가 완료 되어야만 다음 단계를 진행
  - GitHub Workflow 는 디버깅 로그를 통해 문제를 명확히 파악하며 개발해나감

## 문제점

- Notion CMS 방식은 1시간마다 이미지나 동영상 링크가 만료되는 문제가 있어 GitHub Action을 통해 최소 1시간마다 동기화를 진행해야지만 이미지나 동영상이 깨지지 않음
- 너무 자주 동기화를 진행하면 GitHub Action 비용 문제가 발생할 수 있어 1시간으로 적용
- Notion 포스팅 실시간 동기화 처리와 이미지, 동영상 링크 영구 보존 처리를 위해서는 외부 구성이나 별도의 처리로 인해 구현 복잡도가 매우 높아짐
  - 아래 블로그에서 그와 관련된 해결책을 제시하고 있음
  - https://www.sxungchxn.dev/blog/47eb7d5f-0c8b-4e45-8c6e-4c8a103f9c10
- Notion 포스팅이 많아질 수록 GitHub Action 동기화 처리 시간이 늘어나는 구조 너무 많은 포스팅을 감당하기는 어려움
- Notion 블록 형태를 최대한 비슷하게 구현하기 위해 공을 많이 들였는데 이후 뒤늦게 `react-notion-x` 라는 라이브러리가 있다는 사실을 뒤늦게 알게됨. 미리 알았더라면 시간을 많이 세이브할 수 있었을 것 같음
