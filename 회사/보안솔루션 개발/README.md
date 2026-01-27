## 문제 상황
유지보수가 어려운 상황에 있는 암복호화 솔루션 3개를 매년 라이센스 비용을 지불하면서 사용중인 상황 이에 유지보수의 안정성과 라이센스 비용을 절감하고자 사내에서 하나의 암복호화 라이브러리로 통합해 관리하기로 결정하고 분석과 개발을 시작

## AI 활용 방법

- 초기 프로젝트 방향성과 개발 검토시 ChatGPT, Gemini, Perplxity 를 활용
  - Java로 개발된 프로토타입 프로젝트를 바탕으로 Rust 기반의 프로젝트를 개발하기로 결정함
- 1달정도 Rust 기본기 학습과 병행하며 Windsurf와 함께 프로젝트 개발을 위한 작은 조각단위의 Rust Snippet 프로젝트들을 개발
- Rust 프로젝트를 본격적으로 시작하면서 초기에는 Windsurf 무료 버전을 사용하다가 한계를 느끼고 Cursor 무료 버전을 사용해 개발을 계속 이어서 진행함

## AI 사용 효과

- 개발 생산성 향상: 3개월만에 Rust 프로젝트 초기버전(JNI) 개발완료
- 다양한 DB(MySQL, Oracle11G) 및 OS(CentOS 6, Windows, Linux)지원을 위한 다양한 패키징 및 Docker 를 활용한 멀티플랫폼 빌드
- SpringBoot 예제 프로젝트 및 다양한 테스트 프로젝트와 테스트 코드
- 이후 가이드 문서 작성까지 AI 도움을 받아 혼자 단독으로 진행함에도 빠른 속도로 개발을 완료 후 전사 적용

## 프롬프트

초기 프로젝트 진행시에는 PRD라는 개념을 모르고 snippet 형태로 작게작게 개발을 진행했기 때문에 프롬프트를 기록해두진 않았음

이후에 CentOS 6 대응으로 인해 Rust 1.63 버전으로 Legacy 버전을 개발을 해야되는 케이스가 생겨서 PRD 문서를 시작으로 1개월안에 개발을 완료 하였음

- [PRD(enigma-legacy)](./docs/PRD(enigma-legacy).md)


## 결과물

코어 프로젝트는 보안문제로 공개가 어려움

- 발표자료: https://www.slideshare.net/slideshow/rust-db/282367550
- Sample GitHub: https://github.com/freelife1191/crypto-springboot-example
- 데이터 암복호화 SpringBoot 예제
  - https://github.com/freelife1191/crypto-springboot-example

**요약**

- Rust 기반 Native Library 형태의 DB 데이터 암복호화 솔루션 개발

**역할**

- 총 2명이 함께 진행 (본인, IT본부장)
- Java 기반 DB 암복호화 프로토타입 개발 및 서비스 기획(IT본부장)
- Rust 기반 DB 암복호화 솔루션 내재화 프로젝트 개발(기여도 100%)

**성과**

- Native Library 형태로 개발하고 다양한 OS지원(Windows, MacOS, Linux)
- Gradle 과 Docker 를 활용한 크로스컴파일 환경을 구성하고 멀티플랫폼OS를 지원
- 다양한 데이터베이스를 지원(Oracle, MySQL, MariaDB)
- JNI 형태로 빌드하여 SpringBoot 및 Java 지원
- 사내 개발자들이 쉽게 적용할 수 있도록 SpringBoot 예제프로젝트와 가이드 문서 제공
- AWS Repository Publish 를 통해 Maven, Gradle 에서 라이브러리 관리가 용이하도록 제공

**시기**

- 2024.7 ~ 2025.7