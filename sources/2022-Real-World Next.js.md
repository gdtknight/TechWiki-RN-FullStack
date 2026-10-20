---
type: source
title: "Real-World Next.js"
authors: ["Michele Riva"]
publisher: "Packt Publishing"
year: 2022
category: "Apple Books/Frontend & Full-Stack"
epub_path: "/Users/yoshin/Library/Mobile Documents/iCloud~com~apple~iBooks/Documents/Real-World Nextjs.epub"
status: ingested
tags: [nextjs, react, ssr, ssg, isr, seo, authentication, docker, graphql, full-stack]
sources: ["[[2022-Real-World Next.js]]"]
created: 2026-10-19
updated: 2026-10-19
---

# Real-World Next.js

## 1. 핵심 테제 (Core Thesis)
[[React]]는 뛰어난 UI 라이브러리이지만 프로덕션 규모의 웹 애플리케이션을 구축하려면 라우팅, 서버 사이드 렌더링(SSR), 정적 사이트 생성(SSG), 검색 엔진 최적화(SEO), 코드 분할, 이미지 최적화, API 서버 통합을 직접 구성해야 한다. **Michele Riva**는 **Next.js**가 어떻게 페이지 단위의 하이브리드 렌더링 전략(SSR · CSR · SSG · ISR)을 제공하여 성능(Core Web Vitals)과 SEO, 개발자 경험(DX)을 동시에 달성하는지 설명하고, 상태 관리·테스팅·커스텀 서버·Docker 배포·인증·이커머스 구축까지 프로덕션 전 과정을 다룬다.

---

## 2. 챕터별 핵심 요약 (Chapter-by-Chapter Summary)

### Part 1: Introduction to Next.js (Next.js 기초와 렌더링 아키텍처)
- **Ch 1. A Brief Introduction to Next.js**:
  - Gatsby, Razzle, Nuxt.js 등과 비교한 Next.js의 강점, 기본 디렉토리 구조, [[TypeScript]] 기본 통합, 그리고 커스텀 Babel 및 Webpack 설정 확장법.
- **Ch 2. Exploring Different Rendering Strategies**:
  - 웹 렌더링의 4대 전략을 페이지 성격에 맞게 조합하는 하이브리드 아키텍처 해부:
    1. **Server-Side Rendering (SSR, `getServerSideProps`)**: 매 요청마다 서버에서 HTML을 생성하여 최신 데이터와 SEO를 보장.
    2. **Client-Side Rendering (CSR, `useEffect` / `dynamic(..., { ssr: false })`)**: 개인화된 대시보드나 브라우저 전용 API(`window`) 의존 컴포넌트 처리.
    3. **Static Site Generation (SSG, `getStaticProps` / `getStaticPaths`)**: 빌드 타임에 HTML을 사전 렌더링하여 CDN 응답 속도를 극대화.
    4. **Incremental Static Regeneration (ISR, `revalidate`)**: 전체 사이트를 재빌드하지 않고도 백그라운드에서 지정된 주기로 정적 페이지를 갱신.
- **Ch 3. Next.js Basics and Built-In Components**:
  - 파일 시스템 기반 라우팅과 동적 라우트 파라미터, 프리페칭(Prefetching)을 지원하는 `<Link>`, 자동 WebP 변환·지연 로딩·CLS(레이아웃 이동) 방지를 수행하는 내장 `<Image>` 컴포넌트, `<Head>` 메타데이터 관리, 그리고 `_app.js`와 `_document.js` 커스터마이징.

### Part 2: Hands-On Next.js (실전 코드베이스 설계, 상태, 스타일링, 테스트, 배포)
- **Ch 4. Organizing the Code Base and Fetching Data in Next.js**:
  - 원자적 디자인(Atomic Design) 및 기능별 디렉토리 구조 설계, 서버 측 REST/GraphQL 데이터 페칭(CORS 우회 및 API 키 은닉)과 클라이언트 측 페칭 패턴.
- **Ch 5. Managing Local and Global States in Next.js**:
  - SSR/SSG 환경과 충돌하지 않도록 Context API와 Redux를 Next.js 하이드레이션 생명주기에 맞게 통합하는 패턴.
- **Ch 6–7. CSS, Built-In Styling Methods & UI Frameworks**:
  - Styled JSX, CSS Modules, SASS 통합(Ch 6) 및 Chakra UI, TailwindCSS, Headless UI를 SSR 깜빡임(FOUC) 없이 설정하는 방법(Ch 7).
- **Ch 8. Using a Custom Server**:
  - 기본 내장 서버 대신 기존 **Express.js** 또는 **Fastify** 백엔드 서버 위에 Next.js를 미들웨어로 마운트하여 커스텀 라우팅·웹소켓 등을 결합하는 아키텍처와 그 트레이드오프(Serverless 배포 제약).
- **Ch 9–10. Testing Next.js & Working with SEO and Managing Performance**:
  - Jest + React Testing Library 단위/통합 테스트와 Cypress E2E 테스트 파이프라인(Ch 9).
  - 페이지 유형(블로그, 전자상거래 상품 페이지, 비공개 대시보드)별 최적 렌더링 전략 의사결정 매트릭스와 Lighthouse Core Web Vitals 최적화(Ch 10).
- **Ch 11. Different Deployment Platforms**:
  - Vercel 제로 설정 배포, 정적 CDN 배포(`next export`), 그리고 **Node.js 서버 및 Docker 컨테이너(`Dockerfile` 멀티스테이지 빌드)** 를 통한 클라우드/온프레미스 배포.

### Part 3: Next.js by Example (실전 프로젝트: 인증 및 이커머스)
- **Ch 12. Managing Authentication and User Sessions**:
  - 세션 및 JWT의 구조, `HttpOnly` 쿠키를 활용한 커스텀 풀스택 인증 구현과 Auth0 기반 OAuth 통합.
- **Ch 13–14. Building an E-Commerce Website with Next.js, GraphCMS, and Stripe**:
  - GraphCMS(Headless CMS) + GraphQL로 상품 카탈로그를 SSG/ISR로 렌더링하고, 장바구니 상태 관리 및 Stripe 결제 게이트웨이를 연동한 실전 이커머스 구축.

---

## 3. 연결된 위키 페이지 (Extracted Nodes)

### Concepts
- [[Next.js Rendering Strategies (SSR, SSG, ISR, and RSC)]] — CSR·SSR·SSG·ISR 하이브리드 렌더링 선택 기준, 내장 최적화 컴포넌트(`<Image>`, `<Link>`), Pages Router에서 최신 App Router(RSC)로의 진화 및 Docker 배포 (Ch 1–4, Ch 10–11)
- [[React Fiber and Reconciliation]] — 서버 사이드 렌더링(SSR)과 클라이언트 하이드레이션(Hydration)의 연결 원리 (Ch 2)
- [[Full-Stack Authentication, Token Refresh, and Observability]] — Next.js 환경에서의 JWT/`HttpOnly` 쿠키 세션 관리 및 Auth0 통합 (Ch 12)
- [[Container Infrastructure, Kubernetes, and CI-CD Pipelines]] — Next.js 프로덕션 앱의 Docker 컨테이너화 (Ch 11)

### Entities
- [[React]] / [[TypeScript]] — Next.js의 기반이 되는 UI 코어 및 정적 타입 시스템 (Ch 1–14)
