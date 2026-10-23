---
type: concept
title: "LLM Evaluation (Evals, LLM-as-Judge, and Agent Trajectories)"
category: "G. Artificial Intelligence/MLOps"
sources:
  - "[[2026-AI Evals in Practice]]"
tags:
  - llm-evaluation
  - evals
  - llm-as-judge
  - calibration
  - agent-evaluation
  - eval-driven-development
created: 2026-10-23
updated: 2026-10-23
---

# LLM Evaluation (Evals, LLM-as-Judge, and Agent Trajectories)

## 정의 (Definition)

**평가(eval)** 는 LLM 시스템의 출력이 맞는지를 체계적으로 재는 테스트다. [[2026-AI Evals in Practice]]의 정의: "An eval is a structured test that measures the quality of an LLM system's output." (p.8)

관측(observability)과는 다른 질문에 답한다: "Observability tells you that the system is running. Evals tell you that the system is working correctly." (p.6) 지연 시간·오류율·토큰 수가 모두 정상이어도 답이 틀릴 수 있다. 책의 표현으로 "The system was observably healthy and functionally broken." (p.6)

일반 소프트웨어 테스트와 다른 점은 출력이 비결정적이고, 틀린 답이 그럴듯해 보인다는 것이다: "A hallucinated fact in a fluent, well-structured paragraph is far harder to catch than a 500 error or a garbled response." (p.8) 웹 앱의 테스트 계층은 [[Frontend Testing Strategy (Unit, Mocking, and E2E)]].

---

## 1. 세 구성 요소

| 요소 | 책의 정의 (p.8) |
|---|---|
| **데이터셋** | "a collection of inputs, and optionally expected outputs or reference answers" |
| **채점기(scorer)** | "a function that takes the system's output (and optionally the expected output) and produces a score" |
| **실행기(runner)** | "infrastructure that executes the system on the dataset, applies the scorers, and aggregates results" |

"Everything else - LLM-as-judge, golden datasets, regression testing, CI integration - is an elaboration of these three components." (p.8)

---

## 2. 채점 방식의 스펙트럼

싼 것부터 비싼 것 순 (p.9–10):

| 방식 | 장점 | 한계 |
|---|---|---|
| 정확 일치 | 빠르고 결정적 | 답이 하나인 작업에만 |
| 패턴(정규식·부분 문자열) | 형식·필수 문구 확인 | 의미는 모름 |
| 구조 검증(JSON Schema, 코드 파싱) | 결정적, 빠름 | 내용의 옳음은 모름 |
| 의미 유사도(임베딩) | 바꿔 말한 답도 잡음 | 임베딩 모델 필요 |
| LLM-as-Judge | 어조·완결성·도움됨 같은 미묘한 품질 | 비싸고, 판정 자체가 비결정적 |
| 사람 | 기준점(gold standard) | 느리고 확장 안 됨. 주로 자동 채점기 보정용 |

책의 권고: "Start with deterministic scorers. They are fast, free, and catch more issues than you expect." (p.10) 결정적 검사 없이 LLM 판정부터 시작하는 것을 흔한 실수로 든다 (p.13).

---

## 3. LLM-as-Judge

### 3.1 판정 호출의 구성 (p.61)

입력 넷(작업 입력, 시스템 출력, 루브릭, 선택적 참조 답)과 출력 하나(점수와 이유). 이유를 반드시 받으라고 한다: "The reasoning is often more valuable than the number." (p.73)

### 3.2 루브릭 설계 (p.66)

1. 점수 단계마다 의미를 구체적으로 ("1=bad, 5=good"은 쓸모없음)
2. 작업에 맞는 기준 (고객 지원과 코드 리뷰는 기준이 다름)
3. 기준은 3–5개 ("More than seven and the judge starts losing focus.")
4. 가중치를 명시 (정확성이 문체보다 중요하면 그렇게 적음)

가중치의 효과 (p.65): 문체는 만점이지만 사실이 틀린 답(정확성 1, 완결성 2, 명료성 5, 간결성 5)은 기본 가중치 0.4/0.3/0.2/0.1로 2.5점이 되어 통과 기준 3.0에 못 미친다.

### 3.3 보정(calibration)

판정 점수가 사람 판단과 맞는지 재는 과정이다 (p.67–69). 50–100개 사례에 사람과 판정 모델이 같은 루브릭으로 점수를 매기고 비교한다. 책의 목표치: 1점 이내 일치율 80% 이상, 상관 0.7 이상, 편향 ±0.3 이내, 평균 절대 오차 0.8 미만 (p.69). 판정이 사람보다 후하면 루브릭이 느슨한 것이다 (p.69).

**판정 모델이 바뀌면 다시 보정한다**: 루브릭은 그대로인데 판정 모델 버전이 바뀌면 점수가 달라질 수 있다 (p.69). 판정 모델을 올릴 때마다 보정 세트로 다시 재고, 루브릭에 버전과 보정 결과를 함께 기록한다 (p.70).

### 3.4 알려진 편향 (p.71)

| 편향 | 내용 |
|---|---|
| 길이(verbosity) | 길수록 높게 줌 |
| 위치(position) | 두 답을 비교할 때 먼저 나온 쪽을 선호 |
| 자기 선호(self-preference) | 자기 모델 계열이 만든 답을 선호 |
| 권위(authority) | 출처 인용·전문 용어·단정적 어조에 높게 줌 |

완화책 (p.71–73): 길이 정규화, 여러 판정 모델의 중앙값(서로 다른 모델 계열이면 더 좋음), 판정 간 점수 차가 크면 사람에게 보냄. 판정 모델은 평가 대상 모델 이상의 능력이어야 한다 (p.73).

---

## 4. 에이전트 평가: 답이 아니라 궤적

에이전트는 함수가 아니다: "The path it takes matters as much as the destination." (p.103) 같은 정답에 도달해도 도구를 한 번 부른 에이전트와 엉뚱한 검색을 세 번 한 에이전트는 다르다 (p.103).

| 채점기 | 무엇을 보나 (책 Ch 8) |
|---|---|
| 도구 호출 정확도 | 필요한 도구를 맞는 인자로 불렀나. 순서 무관 모드와, 순서가 의미 있을 때 쓰는 엄격 모드(최장 공통 부분열로 부분 점수) (p.108) |
| 궤적 효율 | 최적 경로에 비해 반복·중복 호출이 얼마나 많은가 (p.108–109) |
| 다중 턴 일관성 | 모순, 반복, 맥락 손실, 목표 이탈 (p.110–111) |
| 작업 완료 | 결국 일을 끝냈나. 하위 목표별 부분 점수 (p.112, p.115) |

데이터셋에 꼭 넣을 것:
- **도구를 부르지 말아야 하는 사례**: "An agent's judgment about when NOT to act is as important as its judgment about which tool to use." (p.116)
- **오래된 사례 표시**: 도구가 바뀌면 기대 궤적이 무효가 된다. 사례의 도구가 현재 도구 목록에 있는지 확인하고, 없으면 "stale"로 빼 둔다 (p.115–116).

다중 턴 대화는 사용자 역할을 하는 다른 LLM(사용자 시뮬레이터)으로 만든다. 시뮬레이터가 너무 협조적이면 결과가 낙관적으로 나온다 (p.115).

도구 오류는 구분한다: 인자를 잘못 준 오류(에이전트 책임)와 외부 서비스 장애(에이전트 책임 아님) (p.116).

---

## 5. 평가 주도 개발 (EDD)

TDD처럼 기준을 먼저 정한다. "The eval suite is not a safety net - it is the steering wheel." (p.169) 6단계 (p.170):

1. 품질 기준 정의 → 2. 골든 데이터셋 → 3. 기준선 측정 → 4. 측정하며 반복 → 5. CI에서 배포 차단 기준 → 6. 운영 환경 표본 평가

성숙도 (p.177): Level 0 수동 확인 → 1 가끔 돌리는 스크립트 → 2 CI의 체계적 오프라인 평가 → 3 운영 환경 평가와 경보 → 4 평가가 개발 흐름의 중심.

평가가 없는 기존 코드베이스라면, 먼저 아무것도 바꾸지 말고 가장 중요한 기능 하나에 운영 로그로 만든 20개 사례로 기준선을 잰다 (p.178).

---

## 6. 관점 비교

| 주제 | [[2026-AI 에이전트를 깊이 이해하기]] ([[AI Agent Architecture]] 요약) | [[2026-AI Evals in Practice]] |
|---|---|---|
| 에이전트 평가 단위 | 작업 성공률(`pass@k`), 도구 호출 정확도, LLM-as-a-Judge의 통계적 유의성 | 궤적의 네 채점기(도구 정확도, 효율, 일관성, 완료) |
| 평가 이후 | 실패 궤적을 프롬프트·메모리·SFT·RL로 환류하는 폐루프 | 운영 실패를 새 회귀 테스트 사례로 추가 (p.175) |

[[Claude Code Harness Design (Agents, Skills, and Orchestration)]]의 생성-검증 패턴은 평가를 실행 중에 돌리는 형태로 볼 수 있다 (판단: 검증자가 명확한 기준으로 PASS/REDO를 판정한다는 점에서).

---

## 관련 개념 및 엔티티

- **상위/연관 개념**: [[AI Agent Architecture]], [[Claude Code Harness Design (Agents, Skills, and Orchestration)]], [[LLM Cost Engineering (Token Economics, Caching, and Routing)]], [[Frontend Testing Strategy (Unit, Mocking, and E2E)]]
- **출처 문헌**: [[2026-AI Evals in Practice]] (Ch 1, 5, 8, 13), [[2026-AI 에이전트를 깊이 이해하기]] (Ch 8)
