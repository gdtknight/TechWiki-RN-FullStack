---
type: source
title: "Hands-on GitHub Actions: Implement CI/CD with GitHub Action Workflows for Your Applications"
authors: ["Chaminda Chandrasekara", "Pushpa Herath"]
publisher: "Apress"
year: 2021
category: "Apple Books/DevOps & Cloud"
epub_path: "/Users/yoshin/Library/Mobile Documents/iCloud~com~apple~iBooks/Documents/Hands-on GitHub Actions.epub"
status: ingested
tags: [github-actions, ci-cd, devops, workflow, automation, docker, mobile-build, testing]
sources: ["[[2021-Hands-on GitHub Actions]]"]
created: 2026-10-19
updated: 2026-10-19
---

# Hands-on GitHub Actions

## 1. 핵심 테제 (Core Thesis)
별도의 외부 CI 서버(Jenkins 등)를 직접 구축·유지보수하지 않고도 소스 코드 저장소(`.github/workflows/*.yml`) 내부에서 이벤트 기반(Push, Pull Request, Release, Schedule, Webhook)으로 **빌드·테스트·패키지 배포·컨테이너 서비스 통합·모바일 앱 빌드**를 완전 자동화하는 **GitHub Actions**의 아키텍처와 실무 워크플로우 설계법을 다룬다.

---

## 2. 챕터별 핵심 요약 (Chapter-by-Chapter Summary)

- **Ch 1–2. Introduction & Getting Started with GitHub Actions Workflows**:
  - GitHub Actions의 핵심 구성요소: **Event**(트리거), **Workflow**(`.yml`), **Job**(병렬 또는 `needs`로 순차 실행되는 작업 단위), **Runner**(Ubuntu, Windows, macOS 가상 환경), **Step** 및 **Action**(재사용 가능한 작업 모듈).
  - 매트릭스 전략(`strategy.matrix`)을 활용해 여러 OS와 여러 Node.js 런타임 버전에서 동시에 테스트를 실행하는 기법.
- **Ch 3–4. Variables, Secrets, and Tokens**:
  - 기본 환경 변수(`GITHUB_*`), 워크플로우/잡/스텝 스코프 커스텀 `env` 변수, 스텝 간 데이터 전달(`GITHUB_OUTPUT` / `GITHUB_ENV`)(Ch 3).
  - 암호화된 **Secrets**(저장소·환경·조직 단위 시크릿) 관리와 워크플로우 실행 시 자동 발급되는 단기 권한 토큰인 **`GITHUB_TOKEN`** 의 최소 권한 제어(Ch 4).
- **Ch 5. Artifacts and Caching Dependencies**:
  - `actions/upload-artifact`와 `actions/download-artifact`를 사용해 빌드 산출물, 테스트 커버리지 리포트, 바이너리를 잡(Job) 간에 공유하거나 보관하는 방법.
  - **`actions/cache`** 로 `node_modules`(npm/yarn/pnpm), Gradle, CocoaPods 등 패키지 의존성을 해시 키(`hashFiles('**/package-lock.json')`) 기반으로 캐싱하여 CI 빌드 시간을 획기적으로 단축하는 기법.
- **Ch 6. Using Self-Hosted Runners**:
  - GitHub 호스팅 러너 대신 사내 보안 네트워크나 특수 하드웨어(GPU, macOS 빌드 머신)에 **Self-Hosted Runner**를 구축·그룹화·보안 격리하는 방법.
- **Ch 7. Package Management**:
  - GitHub Packages 및 외부 레지스트리(npm, NuGet, Docker Hub, GitHub Container Registry)에 버전 태그와 함께 패키지와 컨테이너 이미지를 자동 퍼블리시하는 파이프라인.
- **Ch 8. Service Containers**:
  - 백엔드 통합 테스트나 E2E 테스트 실행 시 워크플로우 잡 내부에 **Service Containers**(PostgreSQL, Redis, MongoDB 등 Docker 컨테이너)를 사이드카 형태로 띄워 실제 DB와 연동 테스트를 수행하는 방법.
- **Ch 9. Creating Custom Actions**:
  - 반복되는 CI/CD 로직을 모듈화하는 3가지 커스텀 액션 제작법: **Docker Container Action**, **JavaScript/TypeScript Action**(`@actions/core`, `@actions/github`), **Composite Run Steps Action**(`action.yml`).
- **Ch 10. A Few Tips and a Mobile Build Example**:
  - 배지(Status Badges), 수동 승인 게이트(Environments & Required Reviewers), 조건부 스텝 실행(`if:`), 그리고 **Android/iOS 모바일 애플리케이션 빌드 및 서명(Signing) 자동화** 실전 예제.

---

## 3. 연결된 위키 페이지 (Extracted Nodes)

### Concepts
- [[Container Infrastructure, Kubernetes, and CI-CD Pipelines]] — GitHub Actions 워크플로우 아키텍처, `actions/cache` 의존성 캐싱, 통합 테스트용 Service Containers, 커스텀 액션 및 모바일/웹 자동 배포 파이프라인 (Ch 1–10)

### Entities
- [[TypeScript]] / [[JavaScript (ECMAScript)]] — JavaScript/TypeScript 기반 Custom GitHub Action 개발 및 Node.js 매트릭스 CI 빌드 (Ch 2, Ch 9)
- [[React Native]] — 모바일 앱(Android Gradle / iOS Xcode) CI/CD 빌드 및 코드 서명 자동화 (Ch 10)
