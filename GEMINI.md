# Gemini Context: AI Use History Portfolio

## Project Overview
This workspace (`/Users/freelife/portfolio/ai-use-history`) is a comprehensive portfolio documenting various projects developed using AI-assisted methodologies. It serves as an archive of "Vibe Coding" experiments, Spec-Driven Development (Spec-Kit), and AI-native workflows. The repository includes both personal projects and professional security solution developments, showcasing the evolution of AI utilization in software engineering.

## Directory Structure

### `개인프로젝트` (Personal Projects)
Collections of rapid prototypes and full-stack applications built to test AI development efficiency.

*   **`Shorts-SaaS`** (YouTube Creator Platform)
    *   **Goal:** A SaaS platform for YouTube creators.
    *   **Method:** Developed in **1 day** (6 hours) to test Spec-Kit.
    *   **Tech:** Spec-Kit, Claude Code.
    *   **AI Usage:** Gemini/Perplexity (PRD), Claude (Logic/Tests).

*   **`Notion 기반 자동화 블로그 시스템`** (Notion Automated Blog)
    *   **Goal:** A Next.js blog system using Notion as a Headless CMS.
    *   **Key Feature:** Solves Notion image link expiration using GitHub Actions for hourly sync.
    *   **Method:** "Vibe Coding" approach—detailed PRD generation followed by checklist-driven implementation.
    *   **Tech:** Next.js, Notion API, GitHub Actions.

*   **`토스인앱토스`** (Toss Apps-in-Toss)
    *   **Goal:** Three React Native applications (English Tutor, Resume Builder, Meeting Notes).
    *   **Method:** Developed in **10 hours** total using Spec-Kit.
    *   **Tech:** React Native.

### `회사` (Company)
Professional development projects involving complex migration and security requirements.

*   **`보안솔루션 개발`** (Security Solution)
    *   **Goal:** Rust-based Native Library for DB encryption/decryption to replace legacy Java solutions.
    *   **Outcomes:** Cross-platform support (Windows/Linux/macOS), JNI integration for SpringBoot.
    *   **Method:** Used AI (Windsurf, Cursor) to learn Rust and refactor Java logic into Rust.
    *   **Tech:** Rust, JNI, Docker (Cross-compile), SpringBoot.

## Development Methodologies & AI Usage

The projects in this portfolio strongly emphasize **Spec-Driven Development** and **AI Collaboration**:

1.  **Spec-Kit & PRD First:**
    *   Projects often start with generating comprehensive PRDs using Gemini or Perplexity.
    *   "Spec-Kit" is used to feed context to coding agents (Claude Code), reducing hallucination and improving logic.

2.  **Vibe Coding:**
    *   A workflow defined by: `Detailed Spec -> Checklist Generation -> Step-by-Step Implementation -> Verification`.
    *   Used extensively in the Notion Blog project to ensure architectural integrity.

3.  **Tools:**
    *   **Planning:** Gemini, Perplexity.
    *   **Coding:** Claude Code, Cursor, Windsurf.
    *   **Verification:** Gemini CLI, Codex CLI, Qwen Coder.

## Key Configuration Files
*   **Documentation:** Most projects contain a `docs/` folder with PRDs (`PROMPT.md`, `STEP*.md`) and technical references.
*   **Build/CI:** `gh-pages.yml` (in Notion Blog) for automation.
*   **Specs:** Look for `Spec-Kit` related docs or structured PRD markdown files in `docs/` to understand the intent of each project.
