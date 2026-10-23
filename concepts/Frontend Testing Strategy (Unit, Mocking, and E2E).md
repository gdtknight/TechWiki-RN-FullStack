---
type: concept
title: "Frontend Testing Strategy (Unit, Mocking, and E2E)"
category: "D. Software Engineering/Testing"
sources:
  - "[[2025-Full Stack JavaScript Strategies]]"
tags:
  - testing
  - unit-test
  - e2e-test
  - vitest
  - react-testing-library
  - msw
  - cypress
  - playwright
  - nightwatch
  - testing-pyramid
created: 2026-10-23
updated: 2026-10-23
---

# Frontend Testing Strategy (Unit, Mocking, and E2E)

## 정의 (Definition)

프론트엔드 테스트는 코드를 바꿀 때 기존 기능이 깨지지 않았는지(회귀) 자동으로 확인하는 일이다. [[2025-Full Stack JavaScript Strategies]]는 목적을 두 가지로 든다: "The two goals of test writing are preventing unexpected broken code from ending up in front of users and documenting the app so that everyone knows how it's supposed to work." (p.277)

이 페이지는 세 층을 다룬다.

| 층 | 무엇을 확인하나 | 도구 (책) |
|---|---|---|
| **단위 테스트** | 컴포넌트 하나의 조건부 렌더링·경계 사례 | Vitest, Jest, React Testing Library (p.279) |
| **네트워크 모킹** | 실제 백엔드 없이 API 응답 흉내 | 훅 모킹(`vi.mock`), Mock Service Worker (p.279–284) |
| **E2E 테스트** | 사용자 흐름 전체 (브라우저에서) | Cypress, Playwright, Nightwatch (p.284–286, p.320–331) |

---

## 1. 무엇을 테스트할까

책의 체크리스트 (p.278):

- 조건부로 렌더링되는 것
- API를 요청하는 곳
- 오류가 날 수 있는 곳
- 데이터를 가공하는 곳
- "Pretty much anything that causes a change in the way a component is rendered"

**커버리지 100%를 목표로 하지 않는다.** 책: "Personally, I've never seen a frontend app written in any framework with 100% test coverage." 이유는 "timing inconsistencies, triggering events, and external dependencies" 때문이다 (p.278). 구현보다 테스트가 더 오래 걸리면 비용을 따져 보라고 한다 (p.278).

**변하지 않는 글자만 확인하는 테스트는 약하다**: "Testing for static text can be misleading because it doesn't change regardless of what state the component is in." (p.281)

---

## 2. 단위 테스트

### 2.1 Vitest + React Testing Library

Vite로 만든 프로젝트라서 Vitest를 쓴다. Vitest는 Jest와 호환되고 더 빠르다고 한다 (p.279). 설정은 `vite.config.ts`의 `test: { globals: true, environment: 'jsdom' }` (p.198). 책은 pre-push Git 훅에서 테스트를 돌린다 (p.194).

### 2.2 테스트하기 쉬운 코드로 바꾸기

책의 예 (p.282): 함수 안에서 `new Date()`를 직접 부르면 결과가 매번 달라 테스트가 불안정하다. 날짜를 인자로 받을 수 있게 바꾸면 같은 입력에 같은 결과가 나온다. "Refactoring code to be more testable is a good thing." (p.282)

### 2.3 훅 모킹 — 책 예제의 함정

책은 `useQuery`를 `vi.mock`으로 가짜로 만들고, `beforeEach`에서 `mockReturnValue`를 **두 번 연달아** 부른다. 첫 번째는 주문 데이터, 두 번째는 사용자 데이터다 (p.280). Vitest 문서에 따르면 `mockReturnValue`는 "Accepts a value that will be returned whenever the mock function is called."이므로, 나중에 부른 값이 앞의 값을 덮어쓴다. 그러면 컴포넌트 안의 두 `useQuery` 호출이 **모두 사용자 데이터**를 받는다. 호출 순서대로 다른 값을 주려면 `mockReturnValueOnce`를 쓴다: "If chained, each consecutive call will return the specified value." (Vitest *Mock Functions*) (판단: 두 문서 설명을 책 코드에 적용함. 책 코드를 직접 실행해 보지는 않음)

### 2.4 스냅샷 테스트

출력 HTML을 저장된 스냅샷과 비교한다. 책은 "usually easy to maintain since you don't have to write code for them"이라고 한다 (p.281–282).

---

## 3. 네트워크 모킹: Mock Service Worker

MSW는 브라우저의 서비스 워커로 요청을 가로채 가짜 응답을 준다 (p.283). 핸들러는 `http.get(url, () => HttpResponse.json(data))` 형태다 (p.283–284). 백엔드가 준비되기 전에 프론트엔드를 개발할 때 쓰는데, 책은 백엔드와 소통하지 않으면 중복 작업이 생긴다고 경고한다 (p.283).

**시작을 기다려야 한다**: 책의 예제는 `shouldEnableMocking()`을 부른 뒤 결과를 기다리지 않고 바로 `render`한다 (p.283). MSW 공식 문서는 `enableMocking().then(() => render)`로 쓰고 이렇게 경고한다: "Make sure to await the `worker.start()` Promise! Service Worker registration is asynchronous and failing to await it may result in a race condition between the worker registration and the initial requests your application makes." (*Browser integration*) 이 경쟁 상태의 원리는 [[JavaScript Event Loop and Asynchronous Patterns]] 참고.

---

## 4. E2E 테스트: 세 도구 비교

책은 같은 시나리오 세 개(주문 표 로딩, 주문 제출 성공, 잘못된 이메일로 제출 실패)를 세 도구로 각각 쓴다 (p.320).

| 도구 | 책의 설명 |
|---|---|
| **Cypress** | "one of the most widely used tools for e2e testing" (p.320). `cy.intercept`로 요청을 가로챔 (p.321). Gherkin(Given–When–Then)으로 Product 팀과 시나리오를 함께 쓸 수 있음 (p.284–286) |
| **Playwright** | "It automatically runs tests in Chromium, Firefox, and WebKit" (p.322) |
| **Nightwatch** | Selenium WebDriver 기반이며 W3C WebDriver 명세를 따름 (p.328–329) |

고르는 기준 (p.331–332): 팀이 테스트를 쓰고 유지하는 시간, CI에서의 실행 시간, 문서와 커뮤니티. 세 도구 모두 `devDependencies`로 설치해 운영 번들에 들어가지 않게 한다 (p.331).

### 4.1 책 예제를 공식 문서와 맞춰 보기

| 책 코드 | 공식 문서 | 정리 |
|---|---|---|
| Playwright: `expect(orderRow).toBeVisible()`를 `await` 없이 씀 (p.324) | "Note that retrying assertions are async, so you must `await` them." (Playwright *Assertions*) | `await expect(...).toBeVisible()`로 써야 함 |
| Playwright: `apiContext.get('/v1/orders')`를 "API mock"이라고 부름 (p.323–324) | `APIRequestContext`는 "send requests to the server directly from Node.js without loading a page" 하는 용도 (*API testing*). 페이지의 요청을 가짜 응답으로 바꾸는 것은 `page.route` + `route.fulfill` (*Mock APIs*) | 책의 코드는 모킹이 아니라 **별도의 실제 요청** (판단) |
| Cypress: `cy.find('Order submitted successfully')` (p.322) | "`cy.find('.progress') // Errors, cannot be chained off 'cy'`" (Cypress *find*) | `.find()`는 DOM을 내놓는 명령 뒤에 이어야 하고, 인자는 선택자임 |

### 4.2 테스트 피라미드

책에 실린 Ethan Brown의 의견: "The conventional wisdom is that you have the fewest e2e tests because they're so expensive." 다만 원칙은 "invest in a technology in proportion to its cost (in either dollars or person-hours) and value"라고 정리한다 (p.332).

---

## 5. React Native에서는 (판단)

이 책은 웹 앱만 다룬다. 단위 테스트의 원칙(조건부 렌더링, 모킹, 결정적인 입력)은 [[React Native]]에도 그대로 적용된다. 다만 E2E 도구는 브라우저용(Cypress·Playwright)을 쓸 수 없어 도구가 다르다. 이 위키에는 아직 RN 테스트 도구를 다룬 원천 문서가 없다 (미확인: [[2026-React Native Official Documentation]]의 *Testing* 문서를 투입해 확인).

---

## 관련 개념 및 엔티티

- **상위/연관 개념**: [[Frontend Error Handling and Error Boundaries]], [[Deployment Strategies, Environments, and Rollbacks]], [[Container Infrastructure, Kubernetes, and CI-CD Pipelines]], [[JavaScript Event Loop and Asynchronous Patterns]]
- **구현 기술/엔티티**: [[React]], [[TanStack Query and Zustand]]
- **출처 문헌**: [[2025-Full Stack JavaScript Strategies]] (Ch 21 p.277–286, Ch 24 p.319–333), Vitest [*Mock Functions*](https://vitest.dev/api/mock), MSW [*Browser integration*](https://mswjs.io/docs/integrations/browser), Playwright [*Assertions*](https://playwright.dev/docs/test-assertions), [*Mock APIs*](https://playwright.dev/docs/mock), [*API testing*](https://playwright.dev/docs/api-testing), Cypress [*find*](https://docs.cypress.io/api/commands/find)
