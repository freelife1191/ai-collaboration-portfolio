# AI로 나만의 웹사이트 만들기

- 디자인 레퍼런스 사이트
  - https://dribbble.com/


## 0. 개요

- ChatGPT: 기획 및 화면 설계서 작성
- v0: 웹사이트의 디자인 및 기본 틀 잡기 (웹사이트 초안)
- Cursor: 디자인 세부사항 수정 및 기능 추가 (웹사이트 고도화)
- Notion API: 노션과 연동하여 데이터베이스 활용
- Vercel: 프로젝트 배포 및 추적
- Make: 메일 자동화


## 1. 기획서 작성 with GPT

https://demodev.notion.site/1-with-GPT-1caccaf841aa811b8f51d9f46c27ffa2

기본 프롬프트
```
너는 경력 10년 이상의 베테랑 기획자야. 

내 아이디어를 기반으로 랜딩페이지 제작용 기획서를 작성해줘.
다른 사족은 붙이지 말고 기획서의 내용만을 답변해줘
아래 내용을 중심으로 정리해줘:

- 서비스 이름
- 핵심 기능 요약
- 주요 타겟 사용자
- 사용자에게 어떤 효과나 가치를 주는지
- 가격 정책
- 디자인 컨셉 아이디어 (톤앤매너, 색상, 레이아웃 등)

내 아이디어는 다음과 같아:
{아이디어}
```

변경 프롬프트

```
너는 경력 10년 이상의 베테랑 기획자이자 코인 거래 전문가이자 코인 자동 매매봇 전문가야. 

내 아이디어를 기반으로 랜딩페이지 제작용 기획서를 작성해줘.
다른 사족은 붙이지 말고 기획서의 내용만을 답변해줘
아래 내용을 중심으로 정리해줘:

- 서비스 이름
- 핵심 기능 요약
- 주요 타겟 사용자
- 사용자에게 어떤 효과나 가치를 주는지
- 가격 정책
- 디자인 컨셉 아이디어 (톤앤매너, 색상, 레이아웃 등)

내 아이디어는 다음과 같아:
개인들이 AI로 자동 매매봇을 생성하거나 자동 매매봇 템플릿을 생성해 코인 거래를 할 수 있는 서비스를 기획하고 있어 구체적인 아이디어는 아래와 같아

- AI를 활용해 매매봇을 직접 생성 또는 일반, 프리미엄 매매봇 템플릿을 제공
  - 친절한 AI를 활용한 매매봇 생성 가이드 및 매매봇 템플릿 적용 및 사용 가이드를 제공
  - 제공되는 매매봇들을 자신의 매매봇으로 등록시키고 즉각적용 해볼 수 있도록 구성
  - 매매봇은 전략별로 카테고리로 구분되어 생성됨 전략별, 생성자별, 카테고리별, 이름등으로 검색할 수 있음
  - 프리미엄 매매봇은 별도로 유료 판매 OR 프리미엄 구독자에게만 제공
- 매매봇은 업비트, 빗썸, BitGet 등의 API를 제공하는 거래소와 연동이 가능하도록 구성
  - Bitget API Docs: https://www.bitget.com/api-doc/common/intro
  - Upbit API Docs: https://docs.upbit.com/kr/reference/api-overview
  - bitthumb API Docs: https://apidocs.bithumb.com/
  - 매매봇 쉬운 연동을 위한 편의 기능들을 제공
- AI를 활용한 코인투자 TIP 및 매매봇 가이드 제공
- 각 사용자의 매매봇별 수익 랭킹을 확인할 수 있음
- 자신의 매매봇 소개와 홍보를 통해 많은 구독자들을 유치할 수 있음
- 매매봇 구독 금액은 정해져있음 일부사용자에게는 원하는 만큼 쿠폰을 먹여서 저렴하게 판매할 수 있음
- 매매봇 투자수익률 상위의 매매봇 또는 매매봇을 소개하는 사용자의 매매봇을 구독 하여 구독자의 매매봇의 업데이트 현황을 그대로 이어받으며 투자가능
  - 구독자에게는 매월 일정 비용을 지불해야 함
- 매매봇이 투자한 코인과 수익률, 그외에도 코인 현황 및 거래량 등을 확인하고 분석할 수 있는 트레이딩 차트 제공
  - Tradingview: https://kr.tradingview.com/
    - Tradingview API Docs: https://www.tradingview.com/charting-library-docs/latest/api/
  - Binance: https://www.binance.com/en
    - Binance API Docs: https://developers.binance.com/docs/binance-spot-api-docs/rest-api
  - 알트코인 지수(CoinMarketCap): https://coinmarketcap.com/ko/charts/altcoin-season-index/
    - CoinMarketCap API Docs: https://coinmarketcap.com/api/documentation/v1/
  - Bitget: https://www.bitget.com/asia/spot/BTCUSDT
    - Bitget API Docs: https://www.bitget.com/api-doc/common/intro
- 김치 프리미엄도 확인할 수 있어야됨
  - https://kimpga.com/
- 수익 모델
  - 매매봇을 통해 거래시 일정 수수료를 받음
  - 사용자가 다른 사용자 구독시 구독 금액의 일정 수수료를 받음
  - 프리미엄 구독을 통해 질좋은 서비스를 제공
  - 금액 충전 및 계좌 환급시 일정 수수료 받음
  - 그 외 다양한 수익 모델 연구 필요
```

## 2. 화면 설계서 작성 with GPTs

https://demodev.notion.site/2-with-GPTs-1caccaf841aa81a2adc0c9795cca4b15

프롬프트

https://chatgpt.com/g/g-x1eLXooxe-sangsepeiji-gihoegjaj-2030/c/68a31b8f-5240-832d-ae32-7cff9a903ce3

```markdown
이 서비스를 페이크 도어 테스트하기 위해서 랜딩 페이지를 제작하려 해.
다른 사족은 붙이지 말고 화면 설계서의 내용만을 답변해줘

- 버튼 클릭 시 이름/이메일 입력 폼 → 노션 API로 웨이팅 리스트 등록
- 전환 흐름:  
   1. CTA 클릭
   2. 간단한 폼 (이름, 이메일)
   3. 등록 완료 후 메시지: “🎉 웨이팅 리스트 등록 완료! 출시되면 가장 먼저 알려드릴게요.”
- 원페이지 스크롤형 랜딩 페이지
- 사용 시나리오 예시 포함
- 요약 데모는 실제로 동작하지 않고 예시 UI로만 구성

아래 기획서를 바탕으로 화면 설계서를 만들어줘:
{기획서}
```

v0에 디자인 레퍼런스와 함께 UI 개발 요청

https://demodev.notion.site/3-1caccaf841aa813ebb35c35ab8026d9b

https://demodev.notion.site/4-with-v0-1caccaf841aa81668507f06e7b20eedd

### 아이디어

- AI를 활용해 매매봇을 직접 생성 또는 일반, 프리미엄 매매봇 템플릿을 제공
  - 친절한 AI를 활용한 매매봇 생성 가이드 및 매매봇 템플릿 적용 및 사용 가이드를 제공
  - 제공되는 매매봇들을 자신의 매매봇으로 등록시키고 즉각적용 해볼 수 있도록 구성
  - 매매봇은 전략별로 카테고리로 구분되어 생성됨 전략별, 생성자별, 카테고리별, 이름등으로 검색할 수 있음
  - 프리미엄 매매봇은 별도로 유료 판매 OR 프리미엄 구독자에게만 제공
- 매매봇은 업비트, 빗썸, BitGet 등의 API를 제공하는 거래소와 연동이 가능하도록 구성
  - Bitget API Docs: https://www.bitget.com/api-doc/common/intro
  - Upbit API Docs: https://docs.upbit.com/kr/reference/api-overview
  - bitthumb API Docs: https://apidocs.bithumb.com/
  - 매매봇 쉬운 연동을 위한 편의 기능들을 제공
- AI를 활용한 코인투자 TIP 및 매매봇 가이드 제공
- 각 사용자의 매매봇별 수익 랭킹을 확인할 수 있음
- 자신의 매매봇 소개와 홍보를 통해 많은 구독자들을 유치할 수 있음
- 매매봇 구독 금액은 정해져있음 일부사용자에게는 원하는 만큼 쿠폰을 먹여서 저렴하게 판매할 수 있음
- 매매봇 투자수익률 상위의 매매봇 또는 매매봇을 소개하는 사용자의 매매봇을 구독 하여 구독자의 매매봇의 업데이트 현황을 그대로 이어받으며 투자가능
  - 구독자에게는 매월 일정 비용을 지불해야 함
- 매매봇이 투자한 코인과 수익률, 그외에도 코인 현황 및 거래량 등을 확인하고 분석할 수 있는 트레이딩 차트 제공
  - Tradingview: https://kr.tradingview.com/
    - Tradingview API Docs: https://www.tradingview.com/charting-library-docs/latest/api/
  - Binance: https://www.binance.com/en
    - Binance API Docs: https://developers.binance.com/docs/binance-spot-api-docs/rest-api
  - 알트코인 지수(CoinMarketCap): https://coinmarketcap.com/ko/charts/altcoin-season-index/
    - CoinMarketCap API Docs: https://coinmarketcap.com/api/documentation/v1/
  - Bitget: https://www.bitget.com/asia/spot/BTCUSDT
    - Bitget API Docs: https://www.bitget.com/api-doc/common/intro
- 김치 프리미엄도 확인할 수 있어야됨
  - https://kimpga.com/
- 수익 모델
  - 매매봇을 통해 거래시 일정 수수료를 받음
  - 사용자가 다른 사용자 구독시 구독 금액의 일정 수수료를 받음
  - 프리미엄 구독을 통해 질좋은 서비스를 제공
  - 금액 충전 및 계좌 환급시 일정 수수료 받음
  - 그 외 다양한 수익 모델 연구 필요

### 서브 기능

- 각종 코인 관련 뉴스, 코인에 영향을 미치는 유명인들의 발언, 세계 경제 뉴스, 각종 코인 및 거래소 정보, 코인 매매기법등 유용한 정보들을 끊임없이 제공함
  - 회원들끼리 소통할 수 있는 커뮤니티도 제공, 회원들은 코인 정보 뿐만아니라 각각의 매매봇과 수익률도 자랑하고 자신의 포스팅을 올릴 수 있음
    - 개인 회원도 자신의 포스팅을 프리미엄과 일반 정보로 구분 설정할 수 있으며 프리미엄은 유료 구독으로 팔 수도 있음
    - 팔로워 기능이 있어 팔로워들이 생겨나고 자신의 팔로워들에게 새로운 포스팅이나 거래소식, 매매봇 업데이트 및 추가에 대한 푸쉬알람이 발송됨, 팔로워들은 푸쉬알람을 통해 자신의 메세지함에서 팔로잉한 회원의 소식을 확인할 수 있음
  - 일반 사용자들이 볼 수 있는 정보와 프리미엄 구독자들이 볼 수 있는 정보로 구분