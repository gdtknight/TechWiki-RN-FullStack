---
type: source
source_type: book
title: "Full Stack JavaScript Strategies"
authors:
  - "Milecia McGregor"
publisher: "O'Reilly Media"
year: 2025
category: "H. Information Systems/Full Stack"
pdf_path: "/Users/yoshin/Documents/MarginNote4/H. Information Systems/Full Stack/2025-Full Stack JavaScript Strategies-Milecia McGregor-O'Reilly Media.pdf"
status: partial
tags:
  - full-stack
  - react
  - nestjs
  - state-management
  - tanstack-query
  - error-boundary
  - frontend-security
  - frontend-testing
  - e2e-testing
  - deployment-strategy
  - ci-cd
sources: ["[[2025-Full Stack JavaScript Strategies]]"]
created: 2026-10-23
updated: 2026-10-23
---

# Full Stack JavaScript Strategies (2025)

> **서지 정보**: Milecia McGregor | O'Reilly Media (2025, First Release 2025-01-09, p.4) | 480 Pages
> **분류**: `H. Information Systems / Full Stack`
> **원천 PDF**: [2025-Full Stack JavaScript Strategies-Milecia McGregor-O'Reilly Media.pdf](file:///Users/yoshin/Documents/MarginNote4/H.%20Information%20Systems/Full%20Stack/2025-Full%20Stack%20JavaScript%20Strategies-Milecia%20McGregor-O'Reilly%20Media.pdf)
> **쪽 표기**: 이 페이지의 `p.XX`는 **PDF 쪽 번호**입니다. 인쇄 쪽과 다릅니다 (Part III 이후 PDF 쪽 = 인쇄 쪽 + 20, 예: PDF p.181 = 인쇄 161쪽).
> **투입 범위 (`status: partial`)**: **Part III 프론트엔드(Ch 13–22, p.181–304)** 와 **Part IV의 Ch 24–27(p.319–381)** 을 정독함. Ch 2는 앞부분(p.43–45, p.53–54)만 읽음. 나머지 장(Ch 1, 3–12, 23, 28–31)은 목차로만 요약함.
> **집필 시점**: 예제 버전이 NestJS 9.2.0(p.45), Vite 5.0.5와 Node 20.10.0(p.200), sitespeed.io 실행 로그 2024-02-17(p.266)이다. 출간은 2025년이지만 **내용은 2024년 초 기준**이어서, 그 뒤 바뀐 것은 아래 "공식 문서와 다른 점"에 모았다.

---

## 핵심 요약 (Core Thesis)

주니어에서 시니어로 가려는 개발자를 위해, 클라우드에 배포하는 풀스택 웹 앱 하나를 만들며 **"무엇을 왜 결정하는가"** 를 보여 주는 책이다: "By the time you finish, you'll know how, why, and when to make technical decisions and how the business requirements evolve." (p.16)

특정 도구를 깊이 가르치는 책이 아니라고 스스로 밝힌다: "This book is not a deep dive into any specific set of tools, and it will not teach you general JavaScript programming." (p.16) 그래서 이 위키에서는 **코드 레시피보다 체크리스트와 의사결정 기준**의 출처로 쓴다. 장마다 Ethan Brown 등 다른 엔지니어의 의견 상자가 붙어 있다 (p.17).

예제 앱 구성: 백엔드는 NestJS(Part II), 프론트엔드는 React + TypeScript + Vite(Part III), 배포는 Docker와 CircleCI(Part IV) (p.17, p.192, p.356, p.370).

---

## 주요 챕터 및 구조

### Part I–II: 프로젝트 시작과 백엔드 (Ch 1–12, p.21–178) — 목차 위주

- **Ch 1. Kicking Off the Project (p.23)**: 디자인을 작업 단위로 나누고 Product·DevOps·QA·Support 팀과 일정 조율.
- **Ch 2. Setting Up the Backend (p.43)**: NestJS를 고른 이유를 "basically like a backend architecture in a box"라고 설명한다. 검증·인증·라우팅이 처음부터 있는 대신 "a little less flexible" (p.43). 모놀리스에 서버리스 함수 몇 개를 더한 하이브리드 구조를 택한다 (p.44). REST와 GraphQL의 차이는 "GraphQL uses a single endpoint to handle all requests while REST uses multiple endpoints for requests" (p.53).
- **Ch 3–6 (p.57–100)**: Postgres와 ORM 선택, 마이그레이션, REST API 규약 문서, Stripe 연동, cron 기반 백그라운드 작업.
- **Ch 7–12 (p.101–178)**: 백엔드 테스트, 보안(인증·인가·OWASP Top 10), 디버깅, 성능과 Redis 캐싱, 확장(수직·수평), 모니터링·로깅·장애 대응 플레이북과 비난 없는 사후 분석(blameless postmortem).

### Part III: 프론트엔드 (Ch 13–22, p.179–304) — 정독

- **Ch 13. Setting Up the Frontend (p.181–190)**: 프레임워크 선택 기준(커뮤니티, 문서, 업데이트 빈도, 라이선스, 보안 패치 속도 등) (p.185–186). 컴포넌트 기반 아키텍처와 아토믹 디자인을 택하고 (p.182), "keep business logic separated from components when possible"의 예로 엔드포인트 호출을 커스텀 훅으로 빼는 것을 든다 (p.183).
- **Ch 14. Building the React App (p.191–205)**: Vite로 시작하고(p.192), Prettier + ESLint + husky를 쓴다. pre-commit에서는 lint·format, pre-push에서는 테스트를 돌린다 (p.193–194). Vitest와 React Testing Library를 설정하고 (p.198), 컴포넌트마다 `index.tsx`·`*.Container.tsx`·`*.test.tsx` 세 파일을 최소 구성으로 둔다 (p.201–202).
- **Ch 15. State Management (p.207–216)**: `useState`·`useReducer`·`useContext`와 상태 끌어올리기 (p.208–211). 외부 상태 관리의 세 접근(reducer, atom, mutable)을 비교하고 (p.212), 예제는 Valtio를 쓴다 (p.213–215). 자세한 내용은 [[TanStack Query and Zustand]] 참고.
- **Ch 16. Data Management (p.217–227)**: 데이터 요청 라이브러리 비교 기준 (p.218), Axios + TanStack Query 커스텀 훅 (p.221–222), 로딩 상태를 컴포넌트별로 나누기 (p.223–224), 프론트엔드 환경 변수는 "simulated and should never contain sensitive info" (p.220), 계산과 데이터 병합이 프론트엔드로 오면 백엔드 이동을 검토할 것 (p.226).
- **Ch 17. Custom Styles (p.229–239)**: 접근성을 맨 앞에 둔다. ARIA 라벨, 대체 텍스트, 시맨틱 요소를 쓰고 (p.230), React Hook Form으로 접근 가능한 검색 폼을 만들며 (p.231–232), Lighthouse와 axe-core로 감사한다 (p.233–235). MUI 테마, 미디어 쿼리 반응형, `srcset`을 다룬다 (p.236–238).
- **Ch 18. Frontend Error Handling (p.241–252)**: Error Boundary를 앱·레이아웃·컴포넌트 세 수준으로 두고 `react-error-boundary`를 쓴다 (p.243–246). 사용자 입력 검증 오류와 API 오류를 처리한다 (p.248–252). → [[Frontend Error Handling and Error Boundaries]]
- **Ch 19. Frontend Security Considerations (p.253–260)**: 비즈니스 로직 검증, 세션 타임아웃(유휴·절대·갱신), 패키지 버전 관리, 입력 검증, 노출 정보 줄이기 (p.254–259). → [[Frontend Security and Session Management]]
- **Ch 20. Frontend Performance (p.261–275)**: Lighthouse·sitespeed.io로 측정하고, 번들 분석, 캐시 설정(TanStack Query `staleTime`·`gcTime`), `lazy`·`Suspense`와 스켈레톤, 프리페치, 이미지·폰트 최적화를 다룬다 (p.262–275). → [[Web Performance Metrics and Core Web Vitals]]
- **Ch 21. Frontend Testing (p.277–286)**: 무엇을 테스트할지 고르는 기준 (p.278), Vitest 모킹 (p.279–281), 테스트하기 쉬운 코드로 리팩터링 (p.282), MSW (p.282–284), Cypress와 Gherkin (p.284–286). → [[Frontend Testing Strategy (Unit, Mocking, and E2E)]]
- **Ch 22. Frontend Debugging (p.287–304)**: 로그 체크리스트 (p.290), `console.log`·중단점·단위 테스트·`git bisect`로 원인 좁히기 (p.291–293), DevTools의 Elements·Sources·Network·Application 탭 (p.294–302), 빌드 산출물 해시와 CDN 캐시처럼 예상 밖의 원인 (p.303–304).

### Part IV: 배포 (Ch 23–31, p.305–452)

- **Ch 24. Integration Testing (p.319–333)** — 정독: 같은 E2E 테스트 세 개를 Cypress, Playwright, Nightwatch로 각각 써서 비교한다 (p.320–331). 고르는 기준은 작성·유지 시간, CI 실행 시간, 문서와 커뮤니티다 (p.331–332).
- **Ch 25. Making Deployments (p.335–348)** — 정독: 프론트엔드와 백엔드를 따로 배포할 때 주의할 점 (p.336–337), 릴리스 날짜와 동결 기간 (p.337–340), 시맨틱 버저닝과 UI에 버전 표시 (p.340–341), 블루-그린과 카나리 (p.341–343), 롤백(이전 산출물 재배포, revert/reset, 핫픽스) (p.343–347). → [[Deployment Strategies, Environments, and Rollbacks]]
- **Ch 26. Integration Concerns (p.349–359)** — 정독: UTC 사용 권고 (p.351), 서드파티 서비스의 운영 모드 차이 (p.351–353), 운영 환경에서 권한·토큰 조작 시험 (p.353–354), Docker로 프론트엔드·백엔드 이미지 만들기 (p.354–358).
- **Ch 27. Building a CI/CD Pipeline (p.361–382)** — 정독: build·test·deploy 단계 (p.362–363), "you get failures as early in the pipeline as possible" (p.363), Conventional Commits를 검사하는 `commit-msg` 훅 (p.364), 브랜치 보호 (p.368–370), CircleCI 설정 (p.375–376), feature·development·staging·production 환경 (p.378–379).
- **Ch 23, 28–31** (목차만): 배포 준비, Git 브랜치 전략과 충돌 해결, 스프린트·추정, 도메인 이해(DDD, C4), 경력.

---

## 공식 문서와 다른 점 (2026-10-23 대조)

책의 코드와 설명 중 **공식 문서와 다르거나 지금은 바뀐 것**이다. 위키의 다른 페이지에서는 공식 문서 쪽을 따른다.

| 책 (PDF 쪽) | 공식 문서 | 판정 |
|---|---|---|
| Core Web Vitals의 예로 "LCP, first input delay (FID), and cumulative layout shift (CLS)"를 들고 FID 기준(100 ms 이하)을 제시 (p.262) | web.dev (2024-01-31): "INP will officially become a Core Web Vital and replace FID on March 12 of this year, and that FID will be deprecated in this transition." | **바뀜**. 지금은 FID 대신 INP |
| "the React team deprecated CRA in early 2023" (p.279) | react.dev 블로그 *Sunsetting Create React App* (2025-02-14): "Today, we're deprecating Create React App for new apps" | **날짜가 다름**. 공식 지원 중단은 2025-02-14 |
| Valtio: `useSnapshot`이 돌려준 스냅샷에 `orderSnap.orders = orderResponseData`로 값을 씀 (p.214–215) | Valtio *useSnapshot*: "Snapshots are read-only to render the JSX from their consistent view of the data. Mutations, and also any reads in callbacks that make mutations, need to be made via the proxy" | **코드 오류**. 값은 프록시(`orderStore`)에 써야 함. 책도 뒤(p.222)에서는 `orderStore.orders = orderData`로 씀 |
| 캐시 예제에서 `import.meta.env.API_URL`을 씀 (p.271) | Vite *Env Variables and Modes*: "Variables prefixed with `VITE_` will be exposed in client-side source code after Vite bundling." | **코드 오류**. 접두사가 없으면 클라이언트에 노출되지 않음. 책의 다른 예제(p.221)는 `VITE_API_URL` |
| `shouldEnableMocking()`을 호출만 하고, 결과를 기다리지 않고 바로 `render` (p.283) | MSW *Browser integration*: "Make sure to await the `worker.start()` Promise! Service Worker registration is asynchronous and failing to await it may result in a race condition between the worker registration and the initial requests your application makes." | **코드 오류**. 공식 예제는 `enableMocking().then(() => render)` |
| `git revert`는 "removes only the changes in your workspace, not the Git history. It's like hitting an undo button locally." (p.344) | git 2.55 `git-revert(1)`: "Given one or more existing commits, revert the changes that the related patches introduce, and record some new commits that record them." | **설명 오류**. revert는 되돌리는 **새 커밋**을 만든다. 작업 디렉터리만 되돌리는 것은 `git reset --hard`나 `git restore` (같은 문서) |
| 카나리 배포는 "take advantage of a feature-flagged approach", 두 버전을 다른 환경에 두지 않음 (p.342) | Kubernetes *Managing Workloads* "Canary deployments": "deploy a *canary* of a new application release ... side by side with the previous release so that the new release can receive live production traffic before fully rolling it out." | **정의가 다름**. Kubernetes 문서의 카나리는 새 버전을 기존 버전 옆에 **함께 배포**하는 것. 기능 플래그는 책이 고른 구현 방법 (판단) |
| 버그를 추적하려면 로그에 "the user's auth token"을 넣을 수 있다 (p.289) | OWASP *Logging Cheat Sheet*: 다음은 "should usually not be recorded directly in the logs, but instead should be removed, masked, sanitized, hashed, or encrypted" — 목록에 "Access tokens"와 "Session identification values"가 있음 | **보안상 권장되지 않음** |
| 민감 정보를 평문으로 보내지 않으려면 "This should be handled through JWTs or by simply having HTTPS" (p.354) | RFC 7519 §1: JWT의 클레임은 JWS 구조의 페이로드나 JWE 구조의 평문이 되며, 그래서 "digitally signed or integrity protected ... and/or encrypted" 될 수 있음 | **불완전**. 서명만 한 JWT(JWS)는 암호화되지 않음. 책도 같은 쪽에서 "Anyone can decode your JWTs"라고 씀 |
| SSR 컴포넌트는 "never rerender", `useState`·`useEffect`로 갱신되지 않는다 (p.274, p.303) | react.dev *Server Components*: "Server Components are not sent to the browser, so they cannot use interactive APIs like `useState`." | **SSR과 RSC를 섞어 씀** (판단). 공식 문서에서 상호작용 API를 못 쓰는 것은 **Server Components**. SSR로 HTML을 받은 클라이언트 컴포넌트는 하이드레이션 뒤 다시 렌더링된다 ([[Next.js Rendering Strategies (SSR, SSG, ISR, and RSC)]]) |
| WebP·AVIF는 "there isn't wide browser support for these formats yet" (p.274–275) | MDN *Image file type and format guide*: "AVIF is supported in Chrome, Edge, Opera, Safari and Firefox." 다만 AVIF를 쓸 때는 "you should include fallbacks" | **부분적으로 낡음**. 주요 브라우저는 지원하고, MDN은 여전히 대체 포맷을 함께 두라고 권함 |

**맞는 서술의 예**:
- Error Boundary가 이벤트 처리기, 비동기 코드, 경계 자신이 던진 오류를 잡지 못한다는 설명(p.243)은 react.dev *Component*의 목록과 같다.
- 세션의 유휴·절대·갱신 타임아웃(p.255)은 OWASP *Session Management Cheat Sheet*의 세 항목과 같다.

---

## 추출된 핵심 개념 & 엔티티

- **Concepts (신규)**: [[Frontend Testing Strategy (Unit, Mocking, and E2E)]], [[Frontend Error Handling and Error Boundaries]], [[Frontend Security and Session Management]], [[Deployment Strategies, Environments, and Rollbacks]]
- **Concepts (보강)**: [[Web Performance Metrics and Core Web Vitals]], [[Container Infrastructure, Kubernetes, and CI-CD Pipelines]]
- **Entities (보강)**: [[TanStack Query and Zustand]]

## 관련 도서

- [[2026-Full-Stack React TypeScript and Node 2nd Edition]] — 같은 풀스택 주제를 Express/Hono + Prisma로, 더 최신 버전 기준으로 다룸
- [[2025-Advanced Front-End Development]] — 프론트엔드 성능·테스트·보안
- [[2026-Web Performance Engineering in the Age of AI]] — Ch 20 성능 지표의 최신 기준(INP)

## 참고 출처 (공식 문서)

- web.dev [*Interaction to Next Paint becomes a Core Web Vital on March 12*](https://web.dev/blog/inp-cwv-march-12)
- react.dev [*Sunsetting Create React App*](https://react.dev/blog/2025/02/14/sunsetting-create-react-app), [*Server Components*](https://react.dev/reference/rsc/server-components)
- Valtio [*useSnapshot*](https://valtio.dev/docs/api/basic/useSnapshot)
- Vite [*Env Variables and Modes*](https://vite.dev/guide/env-and-mode)
- MSW [*Browser integration*](https://mswjs.io/docs/integrations/browser)
- Git [*git-revert*](https://git-scm.com/docs/git-revert)
- Kubernetes [*Managing Workloads*](https://kubernetes.io/docs/concepts/workloads/management/)
- OWASP [*Logging Cheat Sheet*](https://cheatsheetseries.owasp.org/cheatsheets/Logging_Cheat_Sheet.html)
- IETF [*RFC 7519 JSON Web Token*](https://www.rfc-editor.org/rfc/rfc7519)
- MDN [*Image file type and format guide*](https://developer.mozilla.org/en-US/docs/Web/Media/Guides/Formats/Image_types)
