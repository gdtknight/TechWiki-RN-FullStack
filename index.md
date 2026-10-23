# TechWiki-RN-FullStack Index

이 파일은 **TechWiki-RN-FullStack**(React Native 모바일 크로스플랫폼 앱 & 풀스택 엔지니어링 지식 베이스)의 전체 문서를 안내하는 콘텐츠 중심 카탈로그(`index.md`)입니다.
LLM 에이전트는 **Ingest / Query / Lint** 작업 시 항상 이 인덱스를 먼저 읽고 최신 상태로 유지합니다.

- **원천 라이브러리 (Raw Sources)**:
  - **MarginNote 4 PDF**: `/Users/yoshin/Documents/MarginNote4` (약 670권 PDF)
  - **Apple Books EPUB**: `/Users/yoshin/Library/Mobile Documents/iCloud~com~apple~iBooks/Documents` (86권 로컬 EPUB 패키지)
  - **공식 웹 기술 문서**: `react.dev` (React 19.0–19.3), `reactnative.dev` (RN 0.76–0.88, 0.82부터 New Architecture 전용), `docs.expo.dev` (Expo SDK 52–58 / CNG)
- **운영 스키마**: [[CLAUDE|CLAUDE.md]]
- **작업 이력 로그**: [[log|log.md]]

---

## Sources (원천 도서 요약)

- [[2026-React Official Documentation]] — React Team (React Foundation, 2026). React 19.0–19.3 코어 명세, 19.2(`<Activity>`, `useEffectEvent`)·19.3(`<ViewTransition>`, Fragment Refs) 릴리스 이력, React Compiler v1.0, 2025-12 RSC 보안 공지, 컴포넌트 순수성과 트리 위치 기반 상태 보존/리셋 규칙, `ref` 일반 Prop 전달, 비동기 Actions(`useActionState`, `useFormStatus`, `useOptimistic`), `use()` API, React Server Components 및 React Compiler 자동 최적화. `[Official Documentation/Frontend]`
- [[2026-React Native Official Documentation]] — React Native Team (React Foundation, 2026). React Native 0.76–0.88 코어 명세, New Architecture 기본화(0.76) → 전용화(0.82) → 레거시 코드 제거(0.84+), JSI 직접 메모리 바인딩, Fabric C++ Shadow Tree & Yoga 동기 레이아웃, TurboModules & Codegen, Hermes V1 기본(0.84), 0.79–0.88 릴리스 이력, "Use a Framework" (Expo & CNG) 권장. `[Official Documentation/Mobile]`
- [[2026-Expo Official Documentation]] — Expo Team (650 Industries, 2026). Expo SDK 52–58 코어 명세, SDK 55부터 New Architecture 전용, SDK 53–58 릴리스 이력(SDK 58 = RN 0.88), Continuous Native Generation (CNG: `npx expo prebuild`) 및 Config Plugins, Development Builds(`expo-dev-client`), Expo Router 파일 기반 라우팅, EAS(Build, Submit, Update) 엔터프라이즈 인프라 및 Expo Modules API. `[Official Documentation/Mobile]`
- [[2026-AI 에이전트를 깊이 이해하기]] — Bojie Li, 역: JeongJaeSoon (2026). `현대 에이전트 = LLM + 컨텍스트 + 도구` 공식, 하네스 엔지니어링 5요소, KV Cache 친화적 접두사 불변 설계, [[Model Context Protocol (MCP)]] 계층형 도구 발견, 코딩 에이전트, 통계적 평가 및 사후 학습(SFT/RL)/지속적 진화 폐루프 총론. `[Apple Books/AI & Software Engineering]`
- [[2026-Build AI-Enhanced Web Apps]] — Theo Despoudis (Manning, 2026). Vercel AI SDK(`useChat`, `streamText`, Generative UI), 대화 상태/슬라이딩 윈도우 메모리, LangChain.js 멀티스텝 워크플로우 및 웹 RAG 파이프라인, 프롬프트 인젝션 방어, [[Model Context Protocol (MCP)]] 웹 통합 실전서. `[H. Information Systems/Web Infrastructure]`
- [[2026-Clean Code with TypeScript]] — Rukevwe Ojigbo, Dr. Sanjay Krishna Anbalagan (Packt, 2026). 클린 함수와 합성 원칙, TDD/비동기 에러/성능 프로파일링, Nx 모노레포, OpenAPI + Orval + React Query 계약 우선 개발 및 JSON 런타임 검증 게이트(Runtime Gate). `[E. Programming Languages/JavaScript-TypeScript]`
- [[2026-Full-Stack React TypeScript and Node 2nd Edition]] — David Choi, Cihan Yakar (Packt, 2026). React 19 + TypeScript + Express/Hono + Prisma 7(PostgreSQL) 풀스택 아키텍처, Argon2 해싱 및 `jose` JWT 이중 토큰 순환, 401 동시 갱신 경쟁 상태(Single-Flight Mutex) 해결, OpenTelemetry 관측성, `CLAUDE.md` 스펙 주도 AI 개발. `[H. Information Systems/Full Stack]`
- [[2026-React Application Architecture for Production Second Edition]] — Alan Alickovic, Anthony Alicea (Packt, 2026). Feature 기반 폴더 구조, OpenAPI 코드 생성 + React Query, 5대 상태 분리, 상태 코로케이션, 다국어/접근성/테스팅/CI·CD, 웹·모바일 코드 공유를 위한 Monorepo 및 BFF 아키텍처. `[H. Information Systems/React]`
- [[2026-Web Performance Engineering in the Age of AI]] — Addy Osmani (O'Reilly, 2026). Core Web Vitals(LCP·INP·CLS)와 실험실/현장 측정, AI가 생성한 React 코드의 성능 함정, Chrome 렌더러 구조와 렌더링 파이프라인 6단계, 메인 스레드 스케줄러와 양보, SSR 하이드레이션 비용, JavaScript의 비용, 서드파티 스크립트 관리. `[H. Information Systems/Web Infrastructure]` (부분 투입)
- [[2025-Advanced Front-End Development]] — Nitesh Upadhyaya (Apress, 2025). Vite, 스타일링 전략(CSS Modules, CSS-in-JS, Tailwind), Context vs Redux Toolkit, 성능/테스팅, 웹 보안(XSS/CSRF/CSP), 접근성(a11y)/국제화(i18n), CI/CD, Stripe 결제 연동 이커머스 풀 프로젝트. `[H. Information Systems/React]`
- [[2025-Building Applications with AI Agents]] — Michael Albada (O'Reilly, 2025). 프로덕션 AI 에이전트 시스템의 4대 구성요소(Model, Tools, Memory, Orchestration), Context Engineering, GraphRAG, 멀티에이전트 아키텍처, 운영/보안 총론. `[G. Artificial Intelligence/Agents]`
- [[2025-Full Stack JavaScript Strategies]] — Milecia McGregor (O'Reilly, 2025). 시니어 관점의 풀스택 의사결정 체크리스트: NestJS 백엔드, React 프론트엔드의 상태(reducer·atom·mutable)·데이터(TanStack Query)·접근성·Error Boundary·세션 보안·성능·테스트(Vitest·MSW·Cypress/Playwright/Nightwatch), 블루-그린·카나리 배포와 롤백, CircleCI 파이프라인. 2024년 초 기준이라 FID·CRA·`git revert` 등 공식 문서와 다른 서술 11건을 대조 기록 (Part III·Ch 24–27 정독, partial).
- [[2025-MongoDB 8.0 in Action, Third Edition]] — Arek Borucki (Manning, 2025). BSON 도큐먼트 모델링(Embedding vs Referencing), 5대 스키마 디자인 패턴, 집계 파이프라인(`$lookup`, `$unwind`, `$group`), ESR 복합 인덱스 규칙, WiredTiger 멀티 도큐먼트 ACID 트랜잭션 및 Atlas Vector Search(`$vectorSearch`) RAG 챗봇 구축. `[Apple Books/Database & Backend]`
- [[2025-Open Source Observability]] — Peter Corless, Neha Pawar (O'Reilly, 2025). 모놀리식 APM을 넘어선 분리형 오픈소스 관측성 스택(Observability 2.0: OpenTelemetry + Kafka + 고카디널리티 OLAP 스토리지 + Grafana) 및 AI/LLM 관측성 가이드. `[Apple Books/DevOps & Cloud]`
- [[2025-TypeScript 5 Design Patterns and Best Practices 2nd Edition]] — Theofanis Despoudis (Packt, 2025). TypeScript 5 신기능(`satisfies`, `const` 제네릭, 표준 데코레이터), Branded Types, 판별 유니온, GoF 23개 디자인 패턴, 함수형 모나드(`Option`/`Either`), SOLID/DDD, Apollo Client 및 tRPC 내부 아키텍처. `[E. Programming Languages/JavaScript-TypeScript]`
- [[2025-Web Browser Engineering]] — Pavel Panchekha, Chris Harrelson (Oxford University Press, 2025). Python으로 브라우저를 직접 만들며 배우는 HTML 트리 파싱, 레이아웃 트리, CSS, JavaScript 실행, 작업 큐와 렌더링 주기, `requestAnimationFrame`, 메인/브라우저(컴포지터) 스레드와 commit, GPU 래스터와 합성, 증분 레이아웃. `[H. Information Systems/Full Stack]` (부분 투입)
- [[2025-비욘드 바이브 코딩 (Beyond Vibe Coding)]] — Addy Osmani (O'Reilly, 2025). 바이브 코딩에서 AI 보조 엔지니어링으로의 전환, '70%의 문제(The 70% Problem)'와 '다수결의 함정(Majority Problem)' 돌파, AI 생성 코드 리뷰·보안 감사·테스팅 및 자율형 백그라운드 코딩 에이전트 실전 가이드. `[Apple Books/AI & Software Engineering]`
- [[2024-Build a Frontend Web Framework (From Scratch)]] — Ángel Sola Orbaiceta (Manning, 2024). 외부 라이브러리 없이 순수 JS로 Virtual DOM(`h`, `mountDOM`, `destroyDOM`), 재조정 Diff 알고리즘(`objectsDiff`, `arraysDiffSequence`, `patchDOM`, `key` Prop), 컴포넌트 상태/생명주기, SPA 라우터를 밑바닥부터 구현. `[H. Information Systems/Full Stack]`
- [[2024-Fluent React]] — Tejas Kumar (O'Reilly, 2024). React 내부 작동 원리(JSX 컴파일, Virtual DOM, Stack vs Fiber Reconciler, Render/Commit 단계, 호스트 렌더러 분리, Concurrent React & Render Lanes, SSR/Hydration, React Server Components, React Compiler) 심층 해부. `[H. Information Systems/React]`
- [[2024-JavaScript Design Patterns]] — Hugo Di Francesco (Packt, 2024). 모던 JS 디자인 패턴(`Proxy`/`Reflect`, Observer, Singleton), React 뷰 패턴 진화(HOC → Hooks → Provider), 고성능 비동기 제어(`Promise.allSettled`, `AbortController`, Throttling/Debouncing/Batching) 및 WebView-Native 브릿지 메시징 보안. `[E. Programming Languages/JavaScript-TypeScript]`
- [[2024-Prompt Engineering for LLMs]] — John Berryman, Albert Ziegler (O'Reilly, 2024). GitHub Copilot 초기 설계자가 밝히는 LLM의 자기회귀 문서 완성기 본질, 토큰 예산에 맞춘 **탄력적 스니펫(Elastic Snippets)** 동적 프롬프트 조립, `logprobs` 활용, 대화형 에이전트 및 오프라인/온라인 평가. `[Apple Books/AI & Software Engineering]`
- [[2024-React in Depth]] — Morten Barklund (Manning, 2024). 시니어급 컴포넌트 디자인 패턴(Provider, Composite/Compound, Summary), 리렌더링 3대 조건과 의존성 배열 최적화, TypeScript 제네릭 훅, 모던 상태 관리(Zustand, Redux Toolkit, XState) 및 원격 데이터 캐싱(TanStack Query). `[H. Information Systems/React]`
- [[2023-Do it! 리액트 모던 웹 개발 with 타입스크립트]] — 전예홍 (이지스퍼블리싱, 2023). `2021-Do it! RN`과 1:1 대응되는 쌍둥이 웹 편. Virtual DOM, JSX, TailwindCSS/Flexbox, 7대 React Hooks, Redux(Trello 클론), React Router, Express/MongoDB/JWT 풀스택 실습. `[E. Programming Languages/JavaScript-TypeScript]`
- [[2023-어떤 개발자가 되고 싶니]] — 박동기 (슬로디미디어, 2023). 25년 차 엔지니어가 전하는 '프로그래밍 노동자(코더)'에서 '소프트웨어 개발자'로의 도약, 요구사항 분석·설계력, 막힌 문제를 해결하는 돌파력, 기술 블로그와 공유의 힘, 엔지니어링 책임감. `[Apple Books/Career & Mindset]`
- [[2022-React and React Native, Fourth Edition]] — Adam Boduch, Roy Derks, Mikhail Sakhniuk (Packt, 2022). React 18 웹 기초와 모던 React Native(Hermes, JSI, Fabric, TurboModules, Codegen, Reanimated, Expo, 오프라인 동기화, GraphQL/Apollo Client) 통합 가이드. `[E. Programming Languages/JavaScript-TypeScript]`
- [[2022-Real-World Next.js]] — Michele Riva (Packt, 2022). Next.js 하이브리드 렌더링(SSR, CSR, SSG, ISR), 내장 `<Image>`/`<Link>` 최적화, 상태 관리, 커스텀 서버, Cypress E2E 테스팅, SEO/Core Web Vitals, Vercel/Docker 컨테이너 배포, JWT/Auth0 인증 및 GraphCMS/Stripe 이커머스 구축. `[Apple Books/Frontend & Full-Stack]`
- [[2022-자바스크립트로 배우는 SICP]] — Harold Abelson, Gerald Jay Sussman 외 (한빛미디어, 2022). 컴퓨터 과학 고전 SICP의 JavaScript 판본. 함수·데이터·메타언어적 3대 추상화, 클로저 기반 데이터 구조(`pair`/`head`/`tail`), 평가의 환경 모형, 지연 평가 스트림, 메타순환 평가기(`evaluate`/`apply`) 및 레지스터 기계 컴파일러. `[E. Programming Languages/JavaScript-TypeScript]`
- [[2021-Designing React Hooks the Right Way]] — Fang Jin, Sagar Kale (Packt, 2021). 클로저와 전역 `states` 배열·호출 인덱스(`cursor`)를 이용한 `useState`/`useEffect` 밑바닥 구현을 통해 왜 Hooks 호출 규칙이 존재하는지 증명하고 내장·커스텀 훅 설계 원칙을 제시. `[H. Information Systems/React]`
- [[2021-Do it! 리액트 네이티브 앱 프로그래밍]] — 전예홍 (이지스퍼블리싱, 2021). TypeScript + 함수형 Hooks 기반의 React Native CLI 빌드 파이프라인(Metro/Gradle/CocoaPods), 커스텀 훅, Animated/PanResponder 제스처, React Navigation, Redux, Expo 및 JWT/카메라 풀스택 실습서. `[H. Information Systems/Mobile]`
- [[2021-Hands-on GitHub Actions]] — Chaminda Chandrasekara, Pushpa Herath (Apress, 2021). GitHub Actions 워크플로우, 매트릭스 빌드, Secrets/`GITHUB_TOKEN` 보안, `actions/cache` 의존성 캐싱, 통합 테스트용 Service Containers, 커스텀 액션 제작 및 모바일/컨테이너 CI/CD 자동화. `[Apple Books/DevOps & Cloud]`
- [[2021-JavaScript Cookbook 3e]] — Adam D. Scott, Matthew MacDonald, Shelley Powers (O'Reilly, 2021). 정규표현식/유니코드, 날짜/시간(`Intl`), 수치/배열 고차 연산, 클로저/부분 적용, 비동기 제너레이터(`for await...of`), Fetch/WebSocket, **Web Workers 멀티스레딩** 등 100+ 실전 문제 해결 레시피. `[E. Programming Languages/JavaScript-TypeScript]`
- [[2021-JavaScript from Beginner to Professional]] — Laurence Lars Svekis, Maaike van Putten, Rob Percival (Packt, 2021). ES6+ 기초 문법, 내장 배열/객체 메서드, DOM 이벤트 흐름, 비동기 동시성(Callbacks, Promises, `async`/`await`, Event Loop) 단계별 실습 입문서. `[E. Programming Languages/JavaScript-TypeScript]`
- [[2021-Mastering TypeScript Fourth Edition]] — Nathan Rozentals (Packt, 2021). TypeScript 타입 시스템 기초부터 고급 제네릭 추론(`keyof`, Mapped/Conditional Types, `infer`), 외부 JS 라이브러리용 `.d.ts` 선언 파일 작성법, `tsconfig.json` Strict 옵션 바이블. `[E. Programming Languages/JavaScript-TypeScript]`
- [[2021-React Hooks in Action]] — John Larsen (Manning, 2021). 핵심 내장 훅(`useState`, `useReducer`, `useEffect`, `useRef`, `useContext`, `useMemo`, `useCallback`)과 커스텀 훅, 그리고 React 18 동시성 훅(`Suspense`, `ErrorBoundary`, `useTransition`, `useDeferredValue`) 집중 마스터피스. `[H. Information Systems/React]`
- [[2021-컨테이너 인프라 환경 구축을 위한 쿠버네티스 도커]] — 조훈, 심근우, 문성주 (길벗, 2021). 모놀리식 vs MSA, Docker 멀티스테이지 빌드 및 `PID 1`/`runC` 저수준 원리, Kubernetes 아키텍처(Control Plane, Pod, Deployment, Service, Ingress, PV/PVC, HPA), Jenkins/GitOps CI·CD, Prometheus/Grafana 모니터링. `[Apple Books/DevOps & Cloud]`
- [[2019-React Native in Action]] — Nader Dabit (Manning, 2019). React Native의 초기 비동기 Bridge 원리, 코어 컴포넌트와 플랫폼별(iOS/Android) API 레퍼런스, StyleSheet/Flexbox 심화, Animated API, Redux 아키텍처 입문서. `[H. Information Systems/Mobile]`
- [[2015-인사이드 자바스크립트]] — 송형주, 고현준 (한빛미디어, 2015). 원시/참조 타입 메모리 구조, 일급 함수 객체, `this` 바인딩 4대 규칙, 프로토타입 체이닝, 실행 컨텍스트와 스코프 체인 및 클로저(Closure), 함수형 프로그래밍 원리를 도해로 설명한 고전 명저. `[E. Programming Languages/JavaScript-TypeScript]`

---

## Concepts (핵심 개념 & 아키텍처)

- [[AI Agent Architecture]] — `에이전트 = LLM + 컨텍스트 + 도구` 공식, 하네스 엔지니어링(Harness Engineering), 결정론적 워크플로우 vs 자율 에이전트, 통계적 평가 및 사후 학습(SFT/RL)/지속적 진화 폐루프.
- [[AI-Enhanced React Applications and Specification-Driven Development]] — Vercel AI SDK(`useChat`, `streamText`, Generative UI), LangChain.js 웹 RAG, [[Model Context Protocol (MCP)]] 통합 및 `CLAUDE.md` 기반 스펙 주도 AI 코딩 워크플로우.
- [[Beyond Vibe Coding and AI-Augmented Software Engineering]] — Addy Osmani의 '70%의 문제(The 70% Problem)'와 '다수결의 함정(Majority Problem)' 돌파, AI 생성 코드 보안 감사·테스트 안전망, 자율형 백그라운드 코딩 에이전트 및 엔지니어 성장 철학.
- [[Context Engineering]] — KV Cache(Prompt Cache) 친화적 접두사 불변 설계, 탄력적 스니펫(Elastic Snippets) 토큰 예산 조립, 컨텍스트 압축 및 동적 스킬 로딩.
- [[Agent Memory and Knowledge]] — 외부 도메인 지식(Knowledge: RAG/GraphRAG)과 에이전트 경험 기억(Memory: 단기/장기/시맨틱 메모리)의 구분 및 비모수/모수 학습.
- [[Multi-Agent Orchestration]] — 단일 에이전트에서 멀티에이전트로의 분할 기준, 중앙집중형(Supervisor) vs 탈중앙화 스웜(Swarm), 액터 프레임워크(Ray/Orleans/Akka)와 이벤트 버스.
- [[JavaScript Core Syntax and Modern ES6+ Fundamentals]] — 호이스팅/TDZ/스코프, 8대 코어 용어 사전(Stack/Heap, 얕은/깊은 복사, 일급 함수/콜백, 클로저, GC, 직렬화), 6대 Falsy와 React `&&` 함정, `?.`/`??`, 화살표 함수, 구조 분해·스프레드(`...`), 5대 배열 고차 함수, `Map`/`Set`, `#private` 클래스, ESM 모듈 기초–실전 문법.
- [[JavaScript Execution Context, Closures, and Prototype]] — 원시 vs 참조 타입과 React 불변성(Immutability)의 관계, 실행 컨텍스트와 렉시컬 스코프 체인, 클로저(Closure)와 React Hooks Stale Closure 원리, `this` 바인딩, 프로토타입 체이닝 및 ES2023–2025 불변 표준 API(`toSorted`, `structuredClone`).
- [[JavaScript Event Loop and Asynchronous Patterns]] — 단일 스레드 Call Stack과 Microtask/Macrotask Queue 이벤트 루프 아키텍처, `Promise.all`/`allSettled` 병렬화, `AbortController` 요청 취소, Throttling/Debouncing/Batching, Web Workers 및 `Promise.withResolvers`.
- [[TypeScript Core Syntax and Full-Stack Type Annotations]] — 8대 코어 용어 사전(Compile/Runtime, Type Erasure, 구조적 타이핑, 추론 vs 단언 `as`, Type Narrowing, 제네릭 `<T>`, DTO), 기본/튜플 타입, `interface` vs `type`, 함수/`Promise<T>` 타이핑, 8대 유틸리티 타입(`Omit`/`Pick`/`Partial`), React & Express/Prisma 풀스택 실전 타이핑.
- [[TypeScript Type System and Advanced Generics]] — 구조적 타이핑, `unknown` vs `never`, 판별 유니온(Discriminated Unions), 제네릭 제약(`keyof`), Mapped/Conditional Types(`infer`), TS 5 `satisfies`·`const` 제네릭·Branded Types·`NoInfer`·자동 타입 서술어 추론.
- [[Type-Safe Contract-First and Runtime Boundary Architecture]] — 타입 소거(Type Erasure)로 인한 `as` 캐스팅의 위험성, JSON 런타임 검증 게이트(Runtime Gate / Zod), 그리고 OpenAPI(Orval)·tRPC·GraphQL 기반 엔드투엔드 타입 동기화.
- [[Full-Stack Authentication, Token Refresh, and Observability]] — Argon2 패스워드 해싱, `jose` JWT Access/Refresh Token 분리 및 순환(Rotation), 클라이언트 401 동시 갱신 경쟁 상태(Single-Flight Refresh Mutex) 해결, OpenTelemetry · Prometheus · Grafana 분리형 관측성(Observability 2.0).
- [[NoSQL Document Modeling, Aggregation, and Vector Search]] — MongoDB 8.0 BSON 도큐먼트 모델링(Embedding vs Referencing), 5대 스키마 디자인 패턴, 집계 파이프라인, ESR 복합 인덱스 규칙, 멀티 도큐먼트 ACID 트랜잭션 및 `$vectorSearch` RAG 통합.
- [[Container Infrastructure, Kubernetes, and CI-CD Pipelines]] — Docker 멀티스테이지 빌드와 리눅스 컨테이너(`PID 1`/`runC`) 원리, Kubernetes 클러스터(Control Plane, Pod, Deployment, Service, Ingress, HPA), GitHub Actions(`actions/cache`, Service Containers) 및 GitOps 무중단 배포.
- [[Next.js Rendering Strategies (SSR, SSG, ISR, and RSC)]] — CSR·SSR·SSG·ISR·RSC(React Server Components) 5대 렌더링 스펙트럼 비교, 하이드레이션 불일치 방지, 내장 `<Image>`/`<Link>` 최적화 및 Vercel/Docker 풀스택 배포.
- [[Virtual DOM and Reconciliation from Scratch]] — 순수 JS로 구현하는 가상 DOM(`h`, `mountDOM`, `destroyDOM`), 속성/배열 시퀀스 Diff 알고리즘(`objectsDiff`, `arraysDiffSequence`, `key` Prop), `patchDOM` 재조정 및 훅 상태 슬롯 연결 원리.
- [[React Fiber and Reconciliation]] — Virtual DOM diffing, 구형 Stack Reconciler 대비 Fiber 연결 리스트(`child`/`sibling`/`return`)의 중단·재개 원리, Render/Commit 2단계 파이프라인(`react-dom` vs `react-native` Fabric 분리), Render Lanes 동시성 스케줄링.
- [[React Hooks and Component Design Patterns]] — 리렌더링의 3대 조건, 7대 핵심 Hooks와 의존성 배열 규칙, 리렌더링을 원천 차단하는 상태 코로케이션(State Colocation) & `children` 합성, 그리고 Provider / Composite(Compound) 컴포넌트 디자인 패턴.
- [[Web React vs React Native Shared Core and Differences]] — 웹 React와 모바일 React Native가 100% 공유하는 코어 계층(Fiber, Hooks, TypeScript, TanStack Query/Zustand) vs 플랫폼별 호스트 차이(DOM vs Native View, CSS vs Yoga Flexbox, React Router vs React Navigation) 및 모노레포 코드 공유 전략.
- [[React Native Architecture (Bridge to New Architecture)]] — Metro/Gradle/CocoaPods 빌드 파이프라인, 3대 런타임 스레드(JS, Yoga Shadow, Native UI), 비동기 JSON Bridge의 한계와 JSI·Fabric·TurboModules·Codegen·Hermes 기반 New Architecture.
- [[Mobile Layout and Styling with Flexbox]] — CSS-in-JS(`StyleSheet`), 웹 CSS와 다른 모바일 Flexbox 기본값(`flexDirection: 'column'`), Yoga 레이아웃 엔진, 플랫폼 분기(`Platform`) 및 다크모드/UI Kit 패턴.
- [[React Native Gesture and Animation System]] — 내장 `Animated` API와 `PanResponder` 제스처 상태 머신, 구형 Bridge의 16ms 프레임 드롭 한계, 그리고 UI 스레드(Worklet)에서 직접 실행되는 `React Native Reanimated` 비교.
- [[Offline-First and Mobile State Management]] — 모바일 상태의 4계층(로컬 Hooks, Context API, Zustand/RTK, TanStack Query/Apollo GraphQL) 및 `NetInfo` + `AsyncStorage` 기반 오프라인 감지·로컬 캐싱·재연결 동기화 아키텍처.
- [[Browser Rendering Pipeline and Frame Lifecycle]] — HTML·CSS·JS가 픽셀이 되는 단계(파싱 → 스타일 → 레이아웃 → 페인트 → 합성), 렌더링 차단 리소스, 60fps 프레임 예산과 jank, 속성별로 다시 도는 단계, 레이아웃 스래싱, 이벤트 루프 속 렌더링과 `requestAnimationFrame`, 메인·컴포지터 스레드.
- [[Web Performance Metrics and Core Web Vitals]] — LCP·INP·CLS의 정의와 기준(75번째 백분위), TBT·TTI·긴 작업(50 ms)과 양보, 실험실 vs 현장 측정, React 코드에서 지표가 나빠지는 흔한 원인과 대응, React Native와의 관계.
- [[React Component Lifecycle and Effects]] — 트리거→렌더→커밋→페인트, `useEffect`/`useLayoutEffect` 실행 시점(공식 문서 + React 19.3.1 실행 결과), 의존성·정리 함수, Strict Mode 두 번 실행, Effect가 필요 없는 경우, 클래스 생명주기 대응.
- [[React Native Rendering and App Lifecycle]] — RN 렌더 파이프라인(Render→Commit→Mount), UI·JS 스레드, New Architecture의 동기 `useLayoutEffect`, `AppState`, 화면을 언마운트하지 않는 내비게이션과 `useFocusEffect`.
- [[Frontend Error Handling and Error Boundaries]] — Error Boundary가 잡는 오류와 못 잡는 오류(react.dev + React 19.3.1 실행: `useEffect` 동기 오류는 잡고 비동기·이벤트 처리기 오류는 못 잡음), 앱·레이아웃·컴포넌트 세 수준 경계, `onCaughtError`/`onUncaughtError` 기록, 입력 검증·API 오류 처리.
- [[Frontend Security and Session Management]] — 브라우저에 내려간 것은 모두 보인다는 전제, 비즈니스 로직 검증, OWASP 세션 타임아웃(유휴·절대·갱신), 프론트엔드 환경 변수와 `localStorage` 토큰 저장의 위험, JWT는 기본적으로 암호화가 아님, 로그에 토큰 남기지 않기.
- [[Frontend Testing Strategy (Unit, Mocking, and E2E)]] — 무엇을 테스트할지 고르는 기준, Vitest·React Testing Library 단위 테스트와 훅 모킹 함정(`mockReturnValue` vs `Once`), MSW 시작 대기, Cypress·Playwright·Nightwatch 비교와 공식 문서 기준 예제 보정, 테스트 피라미드.
- [[Deployment Strategies, Environments, and Rollbacks]] — 프론트·백엔드 분리 배포 순서, API·UI 버전 관리, 블루-그린과 카나리(책 정의 vs Kubernetes 문서 정의), build·test·deploy 파이프라인과 Git 훅·브랜치 보호, feature·dev·staging·prod 환경, 롤백(`git revert`는 새 커밋을 만듦)과 핫픽스.

---

## Entities (기술 · 프레임워크 · 프로토콜)

- [[Model Context Protocol (MCP)]] — 에이전트가 외부 지식·메모리·상태·도구를 모델 프롬프트에 구조화해 주입하는 표준 프로토콜.
- [[LangGraph]] — 상태 머신 및 방향 그래프 기반의 에이전트 오케스트레이션 프레임워크 (AutoGen, CrewAI, OpenAI Agents SDK와의 비교 포함).
- [[JavaScript (ECMAScript)]] — 멀티패러다임 동적 언어 코어, 호스트별 JS 엔진 비교(V8 vs JavaScriptCore vs Hermes AOT 바이트코드), 모듈 시스템(CommonJS vs ESM 트리 셰이킹).
- [[TypeScript]] — 정적 타입 슈퍼셋, `tsc` 타입 검사와 번들러(Metro/Vite) 트랜스파일 역할 분담, `.d.ts` 선언 파일, `tsconfig.json` 필수 Strict 옵션 체계.
- [[React]] — 상태의 순수 함수(`UI = f(state)`)로 UI를 선언하는 메타의 코어 라이브러리 (호스트 렌더러 분리 구조 및 Solid/Svelte/Vue와의 반응성 모델 비교).
- [[React Native]] — 단일 JS/TS 코드베이스로 iOS(`UIView`)와 Android(`android.view`) 순정 네이티브 위젯을 렌더링하는 크로스플랫폼 모바일 프레임워크 (코어 컴포넌트 매핑 및 Native/WebView와의 비교).
- [[TanStack Query and Zustand]] — 웹 React와 React Native에서 고전 Redux 보일러플레이트를 대체하는 원격 서버 상태 반응형 캐싱(`TanStack Query`) 및 초경량 셀렉터 기반 클라이언트 스토어(`Zustand`, `Redux Toolkit`) 표준 조합.
- [[Expo]] — 네이티브 빌드 설정 없이 빠른 모바일 앱 개발·실기기 테스트(Expo Go, Snack)·배포를 지원하는 유니버설 React Native 플랫폼 (Managed vs Bare Workflow 비교, 최신 CNG `npx expo prebuild` · Development Build 기준 보강).
- [[React Navigation]] — 모바일 화면 전환 표준 라이브러리 (`NavigationContainer`, Stack / Native Stack, Bottom Tab, Drawer 내비게이터 아키텍처 및 라우트 파라미터 제어).
- [[Web Browser Architecture (Chromium Blink)]] — 멀티 프로세스와 렌더러 프로세스, 메인·컴포지터·래스터·워커 스레드의 역할, Blink 작업 스케줄러와 `scheduler.postTask`/`yield`, RenderingNG, React Native 렌더링과의 비교.

---

## Syntheses (주제 종합 & 비교 분석)

- [[RAG vs GraphRAG vs LLM Wiki]] — 전통적 벡터 RAG, GraphRAG, 그리고 현재 구축된 마크다운 기반 LLM Wiki 아키텍처의 지식 컴파일 시점·복합 추론·가독성 비교.
- [[React Native Learning Roadmap and Evolution (2019-2022)]] — MarginNote 4 PDF 21권 + Apple Books EPUB 9권(총 30권)을 유기적으로 결합한 풀스택 · 모바일 · DevOps · AI 엔지니어링 마스터 학습 로드맵.
- [[JS 첫걸음 00 - 로드맵]] — 프로그래밍을 처음 배우는 사람을 위한 JavaScript 기초 레슨 시리즈(12개 레슨) 안내. 레슨 구성(비유 → 직접 해 보기 → 정리 → 자주 하는 실수 → 퀴즈), 레슨 순서와 근거 장, 마친 뒤 읽을 위키 페이지.
- [[JS 첫걸음 01 - 프로그램과 콘솔]] — 프로그램·코드·JavaScript 엔진·ECMAScript의 뜻, 브라우저 콘솔 열기와 `console.log()`, 들여쓰기·세미콜론·주석, 초보자가 자주 만나는 `SyntaxError`/`ReferenceError`.
- [[JS 첫걸음 02 - 변수]] — 이름표 붙은 상자 비유로 배우는 변수, `let`/`const` 선언과 대입(`=`), `undefined`, 이름 짓기 규칙과 카멜 케이스, `var`를 쓰지 않는 이유, TDZ 에러와 Chrome 콘솔의 `let` 재선언 예외.
- [[JS 첫걸음 03 - 자료형]] — 기본 자료형 7가지, 문자열(따옴표·이스케이프·템플릿 리터럴), 숫자와 안전한 정수 범위·`NaN`, 불리언, `undefined` vs `null`, `typeof`와 `typeof null` 버그, 암묵적 형 변환(`2 + "2"`)과 `String()`/`Number()`/`Boolean()`, falsy 값.
- [[JS 첫걸음 04 - 연산자]] — 산술 연산자와 나머지 `%`(짝수·홀수, 분→시간), 우선순위와 괄호, `+=`·`++` 줄여 쓰기, `===` vs `==`, 크기 비교, `&&`/`||`/`!` 진리표와 실제 반환값, 초보자가 자주 하는 비교 실수.
- [[JS 첫걸음 05 - 조건문]] — 갈림길 비유로 배우는 `if`/`else`/`else if`(위에서부터 처음 참인 곳 하나만), 조건의 truthy/falsy 판단, 삼항 연산자, `switch`·`break`·`default`·폴스루와 `===` 비교, 상황별 선택 기준, `if (x = y)`·`if (...);` 실수.
- [[JS 첫걸음 06 - 반복문]] — 줄넘기 비유와 반복의 세 재료(시작·조건·변화), `while`/`do...while`/`for`의 실행 시점 차이, `break`·`continue`, 구구단과 별 계단으로 배우는 중첩 반복, 무한 루프가 생기는 세 가지 경우.
- [[JS 첫걸음 07 - 함수]] — 자판기 비유로 배우는 함수 정의·호출, 매개변수와 인수·기본값, `console.log`와 `return`의 차이, 함수 표현식과 화살표 함수의 자동 `return` 규칙, 콜백과 `setTimeout`, 재귀 맛보기, 호이스팅·`return` 줄바꿈 실수.
- [[JS 첫걸음 08 - 배열]] — 번호 붙은 사물함 비유로 배우는 배열과 0부터 시작하는 인덱스, `length`·`at(-1)`, `push`/`pop`/`shift`/`unshift`/`splice`, `includes`·`indexOf`·`find`, `for...of`·`forEach`, `map`·`filter`, 숫자 `sort()`의 글자순 함정, `...` 펼치기와 복사.
- [[JS 첫걸음 09 - 객체]] — 이름표 달린 서랍장 비유로 배우는 객체와 배열의 쓰임 구분, 점·대괄호 표기법, 속성 추가·삭제, 객체가 담긴 배열과 `filter`/`map`/`find`, `for...in`과 `Object.keys`/`values`/`entries`, 메서드, `{ ...obj }` 복사와 `===` 비교, `?.`.
- [[JS 첫걸음 10 - 스코프와 클로저]] — 집·동네 비유로 배우는 전역·지역·블록 스코프, 스코프 체인과 렉시컬 스코프, 같은 이름 가리기, 선언 없는 대입과 엄격 모드, 소풍 가방 비유의 클로저(카운터·저금통·`makeAdder`), 반복문 `var` 클로저 함정과 `let` 해결, IIFE 맛보기.
- [[JS 첫걸음 11 - 비동기 기초]] — 식당 진동벨 비유로 배우는 동기 vs 비동기, `setTimeout`의 실행 순서, Promise 세 상태와 `.then`/`.catch`, `async`/`await`(그 함수만 기다림), `fetch` 모양, `try...catch...finally`, 이벤트 루프 간단 모형.
- [[JS 첫걸음 12 - 모듈과 다음 단계]] — 레고 정리함 비유로 배우는 모듈, Node.js로 파일 실행, `export`/`import`, 이름 붙은 내보내기 vs 기본 내보내기, 모듈 스코프와 엄격 모드, 시리즈 회고와 이 위키에서 이어 읽을 페이지(TypeScript → React → React Native·Expo).
- [[렌더링 첫걸음 00 - 로드맵]] — 브라우저 렌더링 → React 생명주기 → React Native 생명주기를 잇는 레슨 시리즈(00–09, 각 레슨에 "더 깊이" 심화 절).
- [[렌더링 첫걸음 01 - 브라우저는 화면을 어떻게 그릴까]] — 공장 비유로 배우는 렌더링 파이프라인 6단계, DOM, 레이아웃·페인트·합성, 렌더링 차단, 다시 그리기.
- [[렌더링 첫걸음 02 - 프레임과 이벤트 루프]] — 플립북 비유의 프레임과 16.7 ms 예산, 이벤트 루프 속 렌더링, 긴 작업과 양보, INP, `requestAnimationFrame`.
- [[렌더링 첫걸음 03 - React 첫 만남]] — 컴포넌트·JSX·props·state, 지역 변수와 state의 차이, 스냅샷과 갱신 함수, `key`.
- [[렌더링 첫걸음 04 - 렌더와 커밋]] — 트리거 → 렌더 → 커밋 → 페인트, 바뀐 DOM만 수정, state는 위치에 붙음, `key`로 초기화, 렌더 순수성.
- [[렌더링 첫걸음 05 - 생명 주기와 useEffect]] — 마운트·업데이트·언마운트, Effect의 설정·의존성·정리, 실행 순서 실험, 정리 누락 버그.
- [[렌더링 첫걸음 06 - Effect 심화]] — `useLayoutEffect`, 상호작용이 원인인 Effect, Strict Mode 두 번 실행, Effect가 필요 없는 경우, 경쟁 상태와 `ignore`.
- [[렌더링 첫걸음 07 - React Native는 화면을 어떻게 그릴까]] — Render → Commit(Yoga) → Mount, Shadow Tree 구조 공유, UI·JS 스레드와 두 프레임 레이트, 동기 `useLayoutEffect`, View Flattening.
- [[렌더링 첫걸음 08 - 앱과 화면의 생명 주기]] — 컴포넌트·화면·앱 세 겹 생명 주기, `AppState`, 언마운트되지 않는 화면, `useFocusEffect`·`useIsFocused`, Page Visibility API.
- [[렌더링 첫걸음 09 - 버튼 하나를 누르면]] — 클릭 한 번의 웹·RN 타임라인(실행 순서 실험), INP 세 구간, 웹·RN 성능 체크리스트, 시리즈 정리.
- [[React and React Native Internal Implementation Deep Dive]] — Fiber Reconciler 데이터 구조(Double Buffering, workLoop), Hooks Dispatcher/연결 리스트, `react-reconciler` 호스트 분리, 구형 Bridge 병목 및 New Architecture(JSI, Fabric, Yoga C++ Shadow Tree, Hermes AOT) 심층 분석.

---

## Obsidian Dataview 동적 대시보드 (선택)

> Obsidian에서 **Dataview** 플러그인을 활성화하면 아래 쿼리가 YAML 프론트매터를 기반으로 자동 렌더링됩니다.

```dataview
TABLE type AS "유형", category AS "분류", updated AS "최근 갱신"
FROM ""
WHERE type != null
SORT type ASC, file.name ASC
```
