# AI Collaboration Portfolio

이 포트폴리오는 **AI(LLM)와 협업하여 소프트웨어 엔지니어링의 생산성을 극대화한 경험**을 담고 있습니다.  
단순한 코드 생성을 넘어, **Spec-Driven Development(스펙 주도 개발)**, **Vibe Coding(바이브 코딩)** 등의 방법론을 정립하고 적용하여, 기획부터 배포까지의 전 과정에서 AI를 활용했습니다.

---

## 📂 목차

1.  [**개인 프로젝트: Notion 기반 자동화 블로그 시스템**](#1-notion-기반-자동화-블로그-시스템)  
    *AI를 활용한 기획 구체화 및 CI/CD 자동화*
2.  [**개인 프로젝트: YouTube Shorts SaaS**](#2-youtube-shorts-saas)  
    *Spec-Kit을 활용한 1일 완성 초고속 프로토타이핑*
3.  [**개인 프로젝트: Toss Apps-in-Toss**](#3-toss-apps-in-toss)  
    *AI와 함께하는 낯선 기술 스택(React Native) 정복*
4.  [**회사 프로젝트: 데이터베이스 암복호화 솔루션**](#4-데이터베이스-암복호화-솔루션)  
    *Legacy Java 코드를 Rust로 리팩토링 및 현대화*

---

## 1. Notion 기반 자동화 블로그 시스템

Notion을 Headless CMS로 활용하는 Next.js 블로그 시스템입니다. Notion의 미디어 링크 만료 문제를 GitHub Actions 자동화로 해결했습니다.

*   **개발 기간:** 3일 (2025.10.24 ~ 2025.10.27)
*   **기술 스택:** Next.js, Notion API, GitHub Actions
*   **데모:** [Sample Blog](https://notionblogsample.github.io/) / [GitHub](https://github.com/freelife1191/nextjs-notion-blog)

### 🤖 AI 활용 및 문제 해결

*   **Vibe Coding 방법론 적용:** "구체적인 PRD → 단위 체크리스트 생성 → 단계별 구현 및 검증" 프로세스를 통해 AI 할루시네이션 최소화.
*   **자동화 이슈 해결:** Notion 이미지 링크가 1시간마다 만료되는 문제를 해결하기 위해, AI와 협업하여 GitHub Actions 기반의 주기적(1시간) 정적 빌드/배포 파이프라인 구축.

### 📊 기여도 및 AI 사용 범위

| 항목 | 기여도 | 사용 도구 | 비고 |
| :--- | :--- | :--- | :--- |
| **기획/설계** | 50% (AI 50%) | Gemini, Perplexity | 레퍼런스 분석 및 PRD 고도화 |
| **코드 구현** | 10% (AI 90%) | Claude Code, Cursor | 체크리스트 기반 구현 |
| **검증/배포** | 100% | Gemini CLI | 코드 리뷰 및 트러블 슈팅 |

### 🔗 관련 문서
- [📂 프로젝트 폴더 바로가기](./개인프로젝트/Notion%20기반%20자동화%20블로그%20시스템)
- [📝 PRD(제품 요구사항 문서) v2](./개인프로젝트/Notion%20기반%20자동화%20블로그%20시스템/프롬프트/PRD_v2.md)
- [📝 프롬프트 히스토리](./개인프로젝트/Notion%20기반%20자동화%20블로그%20시스템/프롬프트/프롬프트%20히스토리.md)
- [✅ 개발 체크리스트](./개인프로젝트/Notion%20기반%20자동화%20블로그%20시스템/체크리스트)

---

## 2. YouTube Shorts SaaS

YouTube 크리에이터를 위한 SaaS 플랫폼 프로토타입입니다. **Spec-Kit**을 활용하여 기획부터 개발까지 단 **6시간(1일)** 만에 완료했습니다.

*   **개발 기간:** 6시간 (1일)
*   **기술 스택:** Spec-Kit, AI-Generated Stack
*   **GitHub:** [shorts-saas](https://github.com/freelife1191/shorts-saas)

### 🤖 AI 활용 및 문제 해결

*   **Spec-Driven Development:** 프롬프트만으로 설명하기 힘든 복잡한 요구사항을 구조화된 Spec 문서로 정의하여 Claude Code에 주입, 개발 정확도 향상.
*   **Rapid Prototyping:** 비즈니스 로직 구현과 테스트 코드 작성을 AI에게 100% 위임하고, 아키텍처 설계와 통합에 집중.

### 📊 기여도 및 AI 사용 범위

| 항목 | 기여도 | 사용 도구 | 비고 |
| :--- | :--- | :--- | :--- |
| **기획/설계** | 0% (AI 100%) | Spec-Kit, Claude | SaaS 기획 및 설계 자동화 |
| **코드 구현** | 0% (AI 100%) | Claude Code | 비즈니스 로직 및 테스트 코드 |
| **아키텍처/통합** | 100% | - | 전체 설계 및 배포 |

### 🔗 관련 문서
- [📂 프로젝트 폴더 바로가기](./개인프로젝트/Shorts-SaaS)
- [📝 기능 명세서(Feature Specs)](./개인프로젝트/Shorts-SaaS/docs/features/feature-specifications.md)
- [📝 PRD(Overview)](./개인프로젝트/Shorts-SaaS/docs/prd/01-overview.md)

---

## 3. Toss Apps-in-Toss

토스 인앱 환경을 타겟팅한 3가지 앱(영어 회화 튜터, 이력서 작성기, 회의록 정리기)을 개발했습니다. React Native 경험이 전무한 상태에서 AI의 도움을 받아 하루 만에 앱 3개를 완성했습니다.

*   **개발 기간:** 10시간 (1일)
*   **기술 스택:** React Native
*   **GitHub:** [English Tutor](https://github.com/freelife1191/english-tutor-app) 등

### 🤖 AI 활용 및 문제 해결

*   **Tech Stack Adaptation:** 익숙하지 않은 React Native 프레임워크의 학습 비용을 AI 페어 프로그래밍으로 획기적으로 단축.
*   **Modular Development:** 3개의 앱을 동시에 개발하기 위해 공통 모듈을 식별하고 AI를 통해 재사용성 높은 코드를 생성.

### 📊 기여도 및 AI 사용 범위

| 항목 | 기여도 | 사용 도구 | 비고 |
| :--- | :--- | :--- | :--- |
| **기획/설계** | 0% (AI 100%) | Gemini, Perplexity | 아이디어 발굴 및 스펙 정의 |
| **코드 구현** | 0% (AI 100%) | Claude Code | React Native 구현 |
| **통합/검증** | 100% | - | 빌드 테스트 및 배포 |

### 🔗 관련 문서
- [📂 프로젝트 폴더 바로가기](./개인프로젝트/토스인앱토스)
- [📝 개발 팁 및 가이드](./개인프로젝트/토스인앱토스/docs/development)
- [📝 추천 앱 Top 3 기획서](./개인프로젝트/토스인앱토스/docs/top3-recommended)

---

## 4. 데이터베이스 암복호화 솔루션

기존 Java 기반의 Legacy 암복호화 솔루션을 **Rust** 언어로 리팩토링하고 단일 라이브러리로 통합하여 유지보수성을 높이고 매해 1억이 넘는 라이선스 비용을 절감했습니다.

*   **개발 기간:** 1년 (2024.07 ~ 2025.07)
*   **기술 스택:** Rust, JNI, Docker, SpringBoot
*   **성과:** Windows/Linux/macOS 멀티 플랫폼 지원 및 빌드 시스템 구축

### 🤖 AI 활용 및 문제 해결

*   **Language Migration:** Java 코드를 Rust로 변환하는 과정에서 AI를 활용하여 Rust를 빠르게 학습하고 JNI 라이브러리 개발.
*   **Legacy Analysis:** 문서화되지 않은 레거시 코드의 로직을 AI로 분석하여 Rust로 정확하게 포팅.
*   **Cross-Compilation:** 다양한 OS 지원을 위한 Docker 기반 크로스 컴파일 환경 구성을 AI와 함께 구축.

### 📊 기여도 및 AI 사용 범위

| 항목 | 기여도 | 사용 도구 | 비고 |
| :--- | :--- | :--- | :--- |
| **솔루션 개발** | 100% | Windsurf, Cursor | Rust 구현 및 전체 프로젝트 리딩 |
| **프로토타입** | - | - | Java 기반 프로토타입은 타 팀원 담당 |
| **AI 활용** | - | ChatGPT, Gemini | Rust 학습 및 코드 변환, 문서화 |

### 🔗 관련 문서
- [📂 프로젝트 폴더 바로가기](./회사/보안솔루션%20개발)
- [📝 프로젝트 전체 구조](./회사/보안솔루션%20개발/docs/프로젝트전체구조문서.md)
- [📝 PRD (Legacy 분석)](./회사/보안솔루션%20개발/docs/PRD(enigma-legacy).md)
