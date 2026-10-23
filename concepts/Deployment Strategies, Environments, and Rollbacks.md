---
type: concept
title: "Deployment Strategies, Environments, and Rollbacks"
category: "D. Software Engineering/DevOps-CI"
sources:
  - "[[2025-Full Stack JavaScript Strategies]]"
tags:
  - deployment
  - blue-green
  - canary
  - feature-flags
  - rollback
  - hotfix
  - semantic-versioning
  - ci-cd
  - environments
  - git-hooks
created: 2026-10-23
updated: 2026-10-23
---

# Deployment Strategies, Environments, and Rollbacks

## 정의 (Definition)

배포 전략은 새 코드를 **언제, 누구에게, 얼마나** 내보낼지, 그리고 문제가 생기면 **어떻게 되돌릴지**를 정하는 일이다. [[2025-Full Stack JavaScript Strategies]]는 배포를 코드 작업의 끝이 아니라 시작으로 본다: "It can be easy to consider your job done as soon as all of the code changes have been merged, but this is the beginning of one of your more visible jobs." (p.335) 그리고 이것을 "the balance between speed and risk"를 찾는 일이라고 한다 (p.335).

컨테이너·Kubernetes·GitHub Actions 쪽 구현은 [[Container Infrastructure, Kubernetes, and CI-CD Pipelines]]. 이 페이지는 **전략과 절차**를 다룬다.

---

## 1. 프론트엔드와 백엔드를 따로 배포할 때

책 p.336–337, p.350:

- 백엔드 변경은 프론트엔드·서드파티 서비스·DB 같은 다른 시스템에 영향을 주는지 먼저 확인한다 (p.336).
- 프론트엔드는 보통 단독으로 배포할 수 있지만, **백엔드가 먼저 준비되어 있어야** 한다: "If you deploy a frontend change before the backend is deployed, then you risk breaking part of the app for the users." (p.336)
- 순서: "Deploying the backend first is often a good move—unless there are breaking changes in the API." 깨지는 변경이면 두 쪽을 맞춰 배포하거나 사용량이 적은 시간에 한다 (p.350).
- 프론트엔드와 백엔드에 버전을 따로 붙이고, 프론트엔드가 의존하는 백엔드 버전을 적어 둔다 (p.337).

---

## 2. 버전 관리

- **백엔드 API 버전**: 새 버전을 내도 이전 버전을 한동안 유지해, 소비하는 앱이 옮겨 갈 시간을 준다. 책은 이것을 "a form of graceful degradation"이라고 부른다 (p.340).
- **시맨틱 버저닝**: 번호만 보고 깨지는 변경·호환되는 기능·버그 수정을 구별한다 (p.340). release-please, commit-and-tag-version, release-it, `npm version`, Git 태그를 쓸 수 있다 (p.340). 책은 Conventional Commits 형식의 커밋 메시지(4.2절의 `commit-msg` 훅)가 시맨틱 버저닝으로 넘어가는 기반이 된다고 한다 (p.340).
- **UI에 버전 표시**: 배포가 조용히 실패했는지 바로 알 수 있고, 고객 지원에도 쓴다. 로그인하지 않아도 보이는 곳(내비게이션 바, 푸터)에 둔다 (p.340–341).

---

## 3. 배포 전략: 블루-그린과 카나리

### 3.1 책의 정의

| 전략 | 책의 설명 | 장점 (책) | 비용·위험 (책) |
|---|---|---|---|
| **블루-그린** | 두 개의 운영 환경에 구 버전과 새 버전을 띄우고, 트래픽을 10%부터 점차 옮김. 다 옮긴 뒤에도 구 환경을 대기시켜 바로 되돌릴 수 있음 (p.341) | 무중단, A/B 테스트, 운영 환경에서 최종 확인 (p.342) | 운영급 환경 두 벌의 비용, 두 환경의 데이터·스키마 동기화, DNS 캐시로 블루 프론트엔드가 그린 백엔드를 가리키는 문제 (p.342) |
| **카나리** | "you don't need two full production environments". 기능 플래그로 새 기능을 운영 환경에 배포해 두고 사용자 그룹별로 연다 (p.342) | 운영 환경 하나로 점진 공개 (p.342) | 플래그를 켜는 것을 잊거나 너무 일찍 지우는 문제. 플래그 목록·상태·대상 그룹을 문서화해야 함 (p.343) |

책은 **카나리 배포**와 **카나리 릴리스**(Chrome Canary처럼 얼리 어답터용 불안정 버전)를 구별한다 (p.343).

### 3.2 공식 문서의 정의와 다른 점

Kubernetes 문서의 카나리는 **기능 플래그가 아니라 새 버전을 기존 버전 옆에 함께 띄우는 것**이다: "It is common practice to deploy a *canary* of a new application release (specified via image tag in the pod template) side by side with the previous release so that the new release can receive live production traffic before fully rolling it out." (*Managing Workloads*, "Canary deployments") 예시에서는 `track: stable`과 `track: canary` 라벨로 두 Deployment를 나누고, 서비스는 `track`을 뺀 공통 라벨로 두 쪽을 모두 선택한다. 비율은 복제본 수로 정한다: "You can tweak the number of replicas of the stable and canary releases to determine the ratio of each release that will receive live production traffic (in this case, 3:1)."

정리 (판단):
- 책이 블루-그린에 넣은 "트래픽을 10%씩 옮기기"는 Kubernetes 문서의 카나리 설명(새 버전이 운영 트래픽 일부를 먼저 받음)과 겹친다.
- 책의 "카나리 = 기능 플래그"는 **코드 수준의 점진 공개**이고, Kubernetes의 카나리는 **배포 단위(파드·버전) 수준의 점진 공개**다.
- 문서마다 용어의 경계가 다르므로, 팀에서는 "몇 개의 버전이 동시에 운영되는가"와 "트래픽을 무엇으로 나누는가(라우팅 vs 플래그)"를 명시하는 편이 덜 헷갈린다.

---

## 4. CI/CD 파이프라인과 환경

### 4.1 단계

책 p.362–363의 최소 구성:

| 단계 | 흔한 단계(step) |
|---|---|
| **build** | 환경 준비, 환경 변수, 코드 체크아웃, 패키지 설치, 빌드 |
| **test** | 코드 품질, 단위·통합 테스트, 보안 테스트, 결과 보고 |
| **deploy** | 클라우드 비밀·연결 설정, 산출물 업로드, 서버로 이동, 캐시 초기화, 성공·실패 알림 |

원칙: "One sign of a good pipeline is that you get failures as early in the pipeline as possible." (p.363) 그래서 검사를 앞쪽으로 당긴다: Git 훅 → PR 검사 → 파이프라인. 책의 CircleCI 예제는 lint·단위 테스트, Snyk 보안 스캔, Cypress 통합 테스트를 작업(job)으로 나눈다 (p.375–376).

**책 예제의 실수** (판단): `integration-test` 작업의 단계가 `security-scan`과 똑같이 `snyk/scan`이다 (p.375). 통합 테스트는 아래 `cypress/run` 워크플로가 대신 맡는 것으로 보인다 (p.376).

### 4.2 Git 훅과 브랜치 보호

- husky로 pre-commit에서 lint·format, pre-push에서 테스트 (p.194, p.364–365). `commit-msg` 훅에서 Conventional Commits 형식(`feat`, `fix` 등)을 정규식으로 검사하는 예가 있다 (p.364).
- 훅이 너무 무거우면 개발자가 훅을 끈다: "This could lead to a situation where the devs comment out all the pre-push checks" (p.365).
- GitHub에서 `main`·`develop` 직접 푸시 금지, 병합 전 PR 승인 1개 이상, PR에서 테스트·빌드 실행 (p.368–370).

### 4.3 환경

| 환경 | 용도 (책 p.378–379) |
|---|---|
| **feature** | 큰 프론트엔드 변경을 따로 시험. 보통 백엔드 development에 연결 |
| **development** | 개발 팀의 수시 배포, 기능 플래그 확인 |
| **staging** | QA의 기능·회귀 테스트, Product·Design의 인수 테스트. "It should be as close to production as possible." |
| **production** | 실제 사용자 |

환경별 값은 CI 도구의 프로젝트 설정에 두고, 브랜치에 따라 배포 작업을 나눈다 (p.379–380). 환경별 `.env` 파일을 두는 방식은 "This is a risker approach"(원문 철자 그대로)라고 하며, 비공개 저장소나 파이프라인에서 값을 메모리에 넣는 스크립트로만 하라고 한다 (p.381). 프론트엔드에 들어가는 값은 비밀이 아니라는 점은 [[Frontend Security and Session Management]] 3.1절.

---

## 5. 롤백

책은 쉬운 순서로 이렇게 정리한다: "The easiest and simplest approaches are to redeploy an older artifact, switch traffic to your blue or green environment, or rerelease a previous tagged version." (p.347)

| 방법 | 설명 |
|---|---|
| **이전 산출물 재배포** | Git 태그나 클라우드에 보관한 이전 빌드를 다시 배포. 코드 변경이나 새 PR이 필요 없음 (p.343–344) |
| **revert / reset** | 아래 5.1 |
| **핫픽스 (roll-forward)** | 되돌리지 않고 고쳐서 내보냄. 정상 QA를 건너뛰므로 여러 사람이 함께 확인 (p.347) |

### 5.1 `git revert`와 `git reset` — 책의 설명 바로잡기

책은 `git revert`가 "removes only the changes in your workspace, not the Git history. It's like hitting an undo button locally."라고 설명한다 (p.344). **공식 문서와 다르다.** git 2.55 `git-revert(1)`:

- "Given one or more existing commits, revert the changes that the related patches introduce, and record some new commits that record them."
- "Note: git revert is used to record some new commits to reverse the effect of some earlier commits (often only a faulty one). If you want to throw away all uncommitted changes in your working directory, you should see git-reset(1), particularly the --hard option."

| 명령 | 실제 동작 (git 문서) | 공유 브랜치에서 (판단) |
|---|---|---|
| `git revert <커밋>` | 되돌리는 **새 커밋**을 만듦. 기존 히스토리는 그대로 | 히스토리를 바꾸지 않으므로 공유 브랜치를 되돌리는 기본 방법 |
| `git reset --soft/--mixed/--hard <커밋>` | 브랜치를 그 커밋으로 옮김. `--soft`: "Leave your working tree files and the index unchanged." `--mixed`: "Leave your working directory unchanged. Update the index to match the new HEAD, so nothing will be staged." `--hard`: 작업 트리까지 그 커밋 상태로 덮어씀 (`git-reset(1)`; 책 p.345의 설명도 같음) | 이미 푸시한 커밋을 지우므로 다시 올리려면 강제 푸시가 필요하고, 다른 사람의 클론과 갈라짐 |

책은 revert를 "unless you're a Git expert, it's probably best to avoid this command"라며 피하라고 하고 reset을 권하는데 (p.344), 위 문서 설명대로라면 공유되는 `main`을 되돌릴 때는 오히려 revert 쪽이 히스토리를 보존한다 (판단).

### 5.2 핫픽스 절차

책에 실린 Ethan Brown의 의견: 위기 때는 리뷰·테스트를 줄인 "hotfix protocol"을 두되, 마지막 단계를 "do all the review, test, and check that are part of the normal process as soon as the crisis is over"로 하라 (p.347). 장애 대응 플레이북과 사후 분석은 책 Ch 12 (p.172–176, 목차).

---

## 6. React Native 앱과의 차이 (판단)

이 책의 전략은 서버에서 제공하는 웹 앱 기준이다. 웹 프론트엔드는 "there should be only one version of the UI for all users" (p.341)이지만, [[React Native]] 앱은 사용자가 스토어 업데이트를 받기 전까지 **여러 버전이 동시에 쓰인다**. 그래서 1절의 "백엔드가 이전 버전 API를 한동안 유지"하는 원칙이 모바일에서는 더 오래, 더 엄격하게 필요하다. [[Expo]]의 OTA 업데이트 같은 수단은 이 위키에서 아직 확인하지 않았다 (미확인: docs.expo.dev의 EAS Update 문서로 확인).

---

## 관련 개념 및 엔티티

- **상위/연관 개념**: [[Container Infrastructure, Kubernetes, and CI-CD Pipelines]], [[Frontend Testing Strategy (Unit, Mocking, and E2E)]], [[Frontend Security and Session Management]], [[Full-Stack Authentication, Token Refresh, and Observability]]
- **구현 기술/엔티티**: [[React]], [[React Native]], [[Expo]]
- **출처 문헌**: [[2025-Full Stack JavaScript Strategies]] (Ch 25 p.335–348, Ch 26 p.349–359, Ch 27 p.361–382), Kubernetes [*Managing Workloads*](https://kubernetes.io/docs/concepts/workloads/management/), Git [*git-revert*](https://git-scm.com/docs/git-revert), [*git-reset*](https://git-scm.com/docs/git-reset)
