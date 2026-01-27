계획전에 @docs 에 생성한 포트폴리오 프로젝트들에 맞게 @UIUX_PLAN.md 의 레이아웃 패턴, 스타일 시스템, UI 트렌드 2025 를 **웹검색**을 통해 최신트랜드를 조사해보고 추가해줘

@UIUX_LAYOUT_GUIDE.md 도 **웹검색**을 통해 포트폴리오 프로젝트들에 맞게 조사해보고 추가해줘

**작업이 다되면 포트폴리오 프로젝트별로 디렉토리를 만들면서 아래의 작업들 해당 프로젝트 폴더 내에서 각각 진행해줘**

## 프로젝트 진행룰

모든 프로젝트들은 체계적인 문서화를 통해 관리된다:

@PLAN.md    : 전체 프로젝트 구현 계획 및 단계별 로드맵, @format/PLAN-TEMPLATE.md 포멧으로 생성
@PRD.md     : 제품 요구사항 명세서 (Product Requirements Document), @format/PRD-TEMPLATE.md 포멧으로 생성
@LLD.md     : 저수준 설계 문서 (Low Level Design) - 기술적 구현 상세, 개발 전략, @format/LLD-TEMPLATE.md 포멧으로 생성
@ARCHITECTURE.md: 아키텍처 설계 문서, mermaid 다이어그램과 ascii 코드로 그림을 그려 아키텍처를 디테일하게 표현, @format/ARCHITECTURE-TEMPLATE.md 포멧으로 생성
@API-DESIGN.md: API 상세 설계 문서, @format/API-DESIGN-TEMPLATE.md 포멧으로 생성
@UI-DEGIGN.md: UI/UX 디자인 및 상세 화면설계 문서, 화면 구성, 레이아웃, 디자인, 컴포넌트등 디테일하게 설계, @format/UI-DESIGN-TEMPLATE.md 포멧으로 생성
@TEST-VERIFY.md: 단위 테스트, 통합 테스트, E2E 테스트, 코드 리뷰, 성능 테스트, 보안 검토등 높은 품질의 서비스 개발을 위한 단계별 테스트와 검증을 위한 모든 계획과 정의, @format/TEST-VERIFY-TEMPLATE.md 포멧으로 생성
@DEPLOY.md: 배포 전략문서, @format/DEPLOY-TEMPLATE.md 포멧으로 생성
@README.md: 모든 문서들의 명세, @format/README-TEMLPLATE.md 포멧으로 생성

모든 개발은 이 문서들을 기반으로 진행되며, 변경 사항 시 문서도 함께 업데이트된다.


## UI/UX 디자인

각 포트폴리오 프로젝트의 모든 UI/UX 디자인 및 상세 화면을 설계한다

UI/UX 디자인은 @UIUX_PLAN.md 와 @UIUX_LAYOUT_GUIDE.md 를 꼼꼼하게 읽어보고 적용해서 디자인한다

UI는 **TailwindCSS**, **shadcn/ui**, **Lucide** 로만 구성한다    
그외 애니메이션, 이미지, 일러스트 및 그래픽, UI패턴 및 백그라운드는 최대한 아래의 서비스들만 이용해서 만든다  

- 애니메이션
  - framer motion
- 이미지
  - https://unsplash.com/
- 일러스트 및 그래픽
  - https://undraw.co/illustrations
- UI패턴 및 백그라운드
  - https://heropatterns.com/


## Supabase 설정

Supabase TOKEN 각 프로젝트에 .mcp.json 에 아래와 같이 mcp 셋팅 해야돼

Project URL과 데이터베이스 패스워드는 내가 별도로 셋팅할게

```json
{
"mcpServers": {
    "supabase": {
      "command": "npx",
      "args": [
        "-y",
        "@supabase/mcp-server-supabase@latest",
        "--project-ref=<>"
      ],
      "env": {
        "SUPABASE_ACCESS_TOKEN": "${env:SUPABASE_ACCESS_TOKEN}"
      }
    }
  }
}
```

## 그 외 다른 서비스들 MCP 설정 및 ACCESS_TOKEN 설정

Supabase 외에도 프로젝트별로 각각 필요한 MCP 설정과 ACCESS_TOKEN 발급이 필요한 서비스들은 MCP 설정 방법과 토큰 발급 및 설정 방법을 문서에 자세히 안내해줘

MCP 추가나 설정 방법은 Claude Code, Cursor, Gemini, Kiro, Warp, Windsurf, Codex CLI, Qwen Coder 등에 어떻게 추가하고 설정해야되는지 자세히 알려줘

## 깃허브를 위한 지침
1. GitHub MCP 연결되어있고 gh 명령어 사용 가능해. 이걸로 github 에 작업진행하도록 모든 프로젝트 셋팅해줘 
2. 원격 저장소에 푸시할 때, 먼저 HTTP 버퍼 크기를 늘리고 조금 씩 나누어 푸시할 것. 에러 시 작은 변경사항만 포함하는 새커밋을 만들어 푸시할 것
3. PLAN.md 파일의 작업이 한단계 진행될때마다 PLAN.md 파일에 진행상황 체크하고, 깃허브에 반영할 것