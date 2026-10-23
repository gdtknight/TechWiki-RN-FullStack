---
type: concept
title: "Frontend Security and Session Management"
category: "I. Security/Application"
sources:
  - "[[2025-Full Stack JavaScript Strategies]]"
tags:
  - frontend-security
  - session-management
  - session-timeout
  - input-validation
  - business-logic
  - env-vars
  - local-storage
  - owasp
created: 2026-10-23
updated: 2026-10-23
---

# Frontend Security and Session Management

## 정의 (Definition)

프론트엔드 보안의 출발점은 **브라우저에 내려간 것은 모두 사용자가 볼 수 있다**는 사실이다. [[2025-Full Stack JavaScript Strategies]]: "Think about how you store and transmit data on the frontend because everything that loads in the browser is accessible by users if they just open the developer tools." (p.253)

그래서 프론트엔드의 검증은 사용자 경험을 위한 것이고, 보안을 보장하는 곳은 서버다. 책: "Frontend validation and sanitization are great for UX and can slow down some basic attacks. Just always remember that they can easily be bypassed." (p.258) OWASP도 같다: "Validate on the server even when the browser checks the same fields." (*Input Validation Cheat Sheet*)

인증 토큰의 발급·갱신 구조는 [[Full-Stack Authentication, Token Refresh, and Observability]]에서 다룬다. 이 페이지는 **브라우저 쪽에서 무엇을 조심할지**를 모은다.

---

## 1. 비즈니스 로직 검증

책은 많은 프론트엔드 취약점을 "insecure design"으로 분류하고 (p.254), 정상 흐름(golden path)에서 벗어난 사용법을 찾아보라고 한다 (p.254):

- 같은 상품을 몇 분 안에 여러 번 주문하면? 배송이 시작된 주문을 바로 취소하면?
- 상담 예약 수에 제한이 없으면, 봇이 모든 예약을 차지할 수 있다.
- 폼에 "남은 시도 횟수"나 "무엇이 틀렸는지"를 자세히 알려 주면 공격 자동화에 쓰인다. 대안은 "an unannounced limit on retries or an extra manual step, such as a CAPTCHA" (p.254).

운영 환경에서 확인할 것 (p.353–354): 다른 역할의 토큰으로 접근하면 안 되는 백엔드 자원에 접근되는지, 토큰이 설정대로 만료되는지, JWT를 고쳐서 권한을 바꿀 수 있는지. 책: "Anyone can decode your JWTs, see what the values are, and change them. Make sure they can't use the altered token to manually call endpoints through a tool like Postman." (p.354)

---

## 2. 세션 타임아웃

책의 세 가지(p.255)는 OWASP *Session Management Cheat Sheet*의 세 항목과 같다.

| 종류 | OWASP 정의 | 책의 강조점 |
|---|---|---|
| **유휴(Idle)** | "closing and invalidating the session upon the defined idle period since the last HTTP request received by the web application for a given session ID" | 세션 시간을 브라우저에 저장하지 말 것 (p.255) |
| **절대(Absolute)** | "All sessions should implement an absolute timeout, regardless of session activity. This timeout defines the maximum amount of time a session can be active" | 탈취된 세션도 영원히 쓸 수는 없음 (p.255) |
| **갱신(Renewal)** | "the session ID is automatically renewed, in the middle of the user session, and independently of the session activity" | 사용자는 로그인 상태를 유지하고, 공격자는 같은 ID를 계속 쓸 수 없음 (p.255) |

OWASP는 유휴 타임아웃의 한계도 적는다: "if the attacker is able to hijack a given session, the idle timeout does not limit the attacker's actions". 그리고 시간 관리는 서버에서 해야 한다: "Session timeout management and expiration must be enforced server-side."

책은 그 밖에 직접 로그아웃 버튼, 여러 기기 동시 로그인을 허용할지에 대한 결정도 든다 (p.255–256).

---

## 3. 브라우저에 두는 값

### 3.1 프론트엔드 환경 변수는 비밀이 아니다

책: "On the frontend, env vars are simulated and should never contain sensitive info. When you build the code, your env vars will be present in the compiled output in plain text." (p.220)

Vite 공식 문서도 같다: "`VITE_*` variables should _not_ contain sensitive information such as API keys. The values of these variables are bundled into your source code at build time." (*Env Variables and Modes*) 공개해도 되는 값(API 주소, 공개 키)만 둔다.

### 3.2 `localStorage`에 토큰 두기

책의 예제는 인증 토큰을 `localStorage`에서 읽어 Axios 기본 헤더에 넣는다 (p.225). 책은 이 값을 "a placeholder value"라 부르고 19장에서 다루겠다고 했지만 (p.226), 19장에는 토큰 저장 위치에 대한 절이 없다.

OWASP *HTML5 Security Cheat Sheet*:
- "A single Cross Site Scripting can be used to steal all the data in these objects, so again it's recommended not to store sensitive information in local storage."
- "Do not store session identifiers in local storage as the data is always accessible by JavaScript. Cookies can mitigate this risk using the `httpOnly` flag."

위키의 다른 도서들도 Refresh Token은 `HttpOnly` 쿠키에 두는 구성을 설명한다 ([[Full-Stack Authentication, Token Refresh, and Observability]] 2.2절).

### 3.3 JWT는 기본적으로 암호화가 아니다

책은 민감 정보를 평문으로 보내지 않는 방법으로 "JWTs or by simply having HTTPS"를 든다 (p.354). 그러나 RFC 7519 §1에 따르면 JWT의 클레임은 JWS(서명) 또는 JWE(암호화) 구조에 담기며, 서명만 한 JWT는 내용을 누구나 읽을 수 있다. 책도 같은 쪽에서 "Anyone can decode your JWTs"라고 쓴다. 전송 구간을 보호하는 것은 HTTPS다 (판단).

---

## 4. 노출 정보와 로그

- **응답에는 필요한 것만**: 화면에 안 보여도 Network 탭에서는 보인다. "Just because data doesn't load on the page, that doesn't stop people from checking network requests in the browser devtools." (p.255)
- **인프라 단서 숨기기**: "`X-Powered-By` headers and URLs that expose the hosting provider (like using default AWS API endpoints instead of using custom domains)" (p.259)
- **오류 메시지는 가공해서**: [[Frontend Error Handling and Error Boundaries]] 5.2절.
- **로그에 토큰을 남기지 않기**: 책은 디버깅을 위해 "the user's auth token"을 로그에 넣을 수 있다고 쓴다 (p.289). OWASP *Logging Cheat Sheet*는 반대로, 다음 항목들은 "should usually not be recorded directly in the logs, but instead should be removed, masked, sanitized, hashed, or encrypted"라고 한다. 이 목록에 "Session identification values"와 "Access tokens"가 들어 있다.

---

## 5. 패키지 관리

책: 오래된 패키지의 취약점은 공개 목록에 정리되어 있어 공격자가 쓰기 쉽다 (p.256). 한 달에 한 번 이상 패키지를 갱신하는 티켓을 두고, Dependabot·npm audit·Snyk·retire.js로 점검하라고 권한다 (p.256). CI 파이프라인에 보안 스캔 단계를 넣는 예는 [[Deployment Strategies, Environments, and Rollbacks]] 4절.

---

## 6. 스스로 공격해 보기

책은 PortSwigger Web Security Academy의 실습(2FA 우회, 비즈니스 로직 결함)을 권한다 (p.260). 허가 없는 실제 서비스 공격은 법적 문제가 될 수 있다고 경고한다 (p.260).

---

## 관련 개념 및 엔티티

- **상위/연관 개념**: [[Full-Stack Authentication, Token Refresh, and Observability]], [[Frontend Error Handling and Error Boundaries]], [[Deployment Strategies, Environments, and Rollbacks]], [[Type-Safe Contract-First and Runtime Boundary Architecture]]
- **구현 기술/엔티티**: [[React]], [[Web Browser Architecture (Chromium Blink)]]
- **출처 문헌**: [[2025-Full Stack JavaScript Strategies]] (Ch 19, p.253–260; p.220, p.225, p.289, p.353–354), OWASP [*Session Management Cheat Sheet*](https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html), [*HTML5 Security Cheat Sheet*](https://cheatsheetseries.owasp.org/cheatsheets/HTML5_Security_Cheat_Sheet.html), [*Input Validation Cheat Sheet*](https://cheatsheetseries.owasp.org/cheatsheets/Input_Validation_Cheat_Sheet.html), [*Logging Cheat Sheet*](https://cheatsheetseries.owasp.org/cheatsheets/Logging_Cheat_Sheet.html), Vite [*Env Variables and Modes*](https://vite.dev/guide/env-and-mode), IETF [*RFC 7519*](https://www.rfc-editor.org/rfc/rfc7519)
