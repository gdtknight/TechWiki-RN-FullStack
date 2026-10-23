---
type: concept
title: "LLM Cost Engineering (Token Economics, Caching, and Routing)"
category: "G. Artificial Intelligence/MLOps"
sources:
  - "[[2026-The AI Cost Playbook]]"
  - "[[2026-하네스 엔지니어링 with 클로드 코드]]"
tags:
  - llm-cost
  - token-economics
  - prompt-caching
  - model-routing
  - batch-processing
  - agent-cost
created: 2026-10-23
updated: 2026-10-23
---

# LLM Cost Engineering (Token Economics, Caching, and Routing)

## 정의 (Definition)

LLM API는 토큰 단위로 과금한다. 비용 엔지니어링은 **어떤 토큰을 몇 번, 어느 가격으로 보내는지**를 설계하는 일이다. [[2026-The AI Cost Playbook]]: "Every dollar on your LLM bill traces back to tokens." (p.15)

가격은 자주 바뀌므로 이 페이지는 **원리**를 정리하고, 숫자는 2026-10-23에 Anthropic 공식 [*Pricing*](https://platform.claude.com/docs/en/about-claude/pricing)·[*Prompt caching*](https://platform.claude.com/docs/en/build-with-claude/prompt-caching) 문서에서 확인한 값만 쓴다. 두 책의 가격표에는 공식 가격과 다른 값이 있다 ([[2026-The AI Cost Playbook]], [[2026-하네스 엔지니어링 with 클로드 코드]]의 대조표).

---

## 1. 토큰의 종류와 가격 비율

공식 *Pricing* 표 기준 (예: Claude Sonnet 4.6, 단위 USD / 백만 토큰):

| 토큰 | Sonnet 4.6 | 기본 입력 대비 |
|---|---|---|
| 기본 입력 | $3 | 1배 |
| 5분 캐시 쓰기 | $3.75 | 1.25배 |
| 1시간 캐시 쓰기 | $6 | 2배 |
| 캐시 읽기(히트) | $0.30 | 0.1배 (Opus 5.5·Sonnet 5.5는 0.05배, Fable 5.1·Mythos 5.1은 0.025배) |
| 출력 | $15 | 5배 |

배치 API: "a 50 percent discount on both input and output tokens" (*Pricing*). 최신 모델(Sonnet 5.5 $2 / $10, Opus 5.5 $4 / $20)도 출력이 입력의 5배다.

**토크나이저 주의**: "Claude 4.7 and later models ... use a newer tokenizer ... This tokenizer produces approximately 30% more tokens for the same text." (*Pricing*) 모델을 바꾸면 같은 프롬프트의 토큰 수도 달라진다.

---

## 2. 비용이 실제로 쌓이는 곳: 반복되는 입력

### 2.1 대화와 에이전트 루프는 앞부분을 매번 다시 보낸다

[[2026-The AI Cost Playbook]]: "The total cost of the conversation is not the sum of individual messages - it's the sum of all prefixes of the conversation." (p.24) 에이전트도 같다. 매 반복이 시스템 프롬프트·도구 정의·이전 도구 결과를 모두 다시 보낸다 (p.126–127). 책의 예에서 8회 반복 에이전트는 단발 호출보다 22배 비싸다 (p.127).

[[2026-하네스 엔지니어링 with 클로드 코드]] 부록 D도 같은 결론이다. 에이전트 작업에서 입력과 출력의 비율을 "약 100:1"로 보고, 최적화할 곳은 입력 쪽이라고 한다 (p.296).

### 2.2 컨텍스트 창은 상한이지 목표가 아니다

"context windows are a capability ceiling, not a cost target." (p.18) 책의 사례: 계약서 전체(4–8만 토큰)를 매 질문마다 넣던 기능을 RAG로 바꿔 요청당 입력을 6만에서 3천 토큰으로 줄였다 (p.18).

---

## 3. 프롬프트 캐싱

### 3.1 동작 (공식 문서)

- 캐시는 **접두사** 단위다. 앞부분이 같아야 재사용된다 ([[Context Engineering]] 1절과 같은 원리).
- 수명: "By default, the cache has a 5-minute lifetime. The cache is refreshed for no additional cost each time the cached content is used." 1시간 수명도 있다(쓰기 2배).
- 최소 길이는 모델마다 다르다: 5.x 세대 512, Sonnet 4.6 1,024, Opus 4.7 2,048, Opus 4.6·Haiku 4.5 4,096토큰. "Shorter prompts cannot be cached, even if marked with `cache_control`. Any requests to cache fewer than this number of tokens are processed without caching, and no error is returned."
- 캐시 적용 여부는 응답의 `cache_creation_input_tokens`와 `cache_read_input_tokens`로 확인한다. 둘 다 0이면 캐시되지 않은 것이다.

### 3.2 설계 원칙

| 원칙 | 출처 |
|---|---|
| 안정된 내용을 앞에, 바뀌는 내용을 뒤에: "The single most important principle for prompt caching is: stable content first, variable content last." | [[2026-The AI Cost Playbook]] p.73 |
| 안정적 접두사, 추가 전용(중간 수정 금지), 바이트 동일 접두사 | [[2026-하네스 엔지니어링 with 클로드 코드]] p.298 |

**조용히 깨지는 캐시** (Cost Playbook p.72–73): 시스템 프롬프트에 현재 시각을 넣거나, 기능 플래그로 프롬프트 변형이 둘이 되거나, 리팩터링 중 도구 정의 위치가 바뀌면 캐시 히트율이 떨어진다. 기능은 그대로 동작하므로 비용으로만 드러난다. 그래서 히트율을 감시하고 경보를 건다.

**손익분기** (공식 문서): 캐시 읽기가 기본 가격의 10%인 모델에서는 "caching pays off after one cache read for the 5-minute duration (1.25x write), or after two cache reads for the 1-hour duration (2x write)." 요청이 드문 엔드포인트는 5분 안에 다음 요청이 오지 않아 쓰기 비용만 낼 수 있다 (Cost Playbook p.80).

---

## 4. 모델 라우팅

모든 요청에 가장 비싼 모델이 필요하지는 않다. 책의 주장: "In a typical production workload, 60-80% of requests can be handled by a model that costs 5-10x less than the top-tier option, with no measurable quality difference." (Cost Playbook p.98) 위키에서는 저자의 경험 수치로만 기록한다.

라우터의 세 부분 (p.99): 복잡도 분류기, 라우팅 정책, 품질 검증기. 분류 방식은 기능 이름으로 정하는 방식부터 시작하고, 기능 안에서 복잡도 차이가 크면 규칙 기반, 그래도 부족하면 LLM 분류기를 쓴다 (p.101). 원칙: "The routing system needs to be conservative: when in doubt, send to the better model." (p.99)

라우팅이 품질을 해치지 않는지는 평가로 확인해야 한다 ([[LLM Evaluation (Evals, LLM-as-Judge, and Agent Trajectories)]]). [[Claude Code Harness Design (Agents, Skills, and Orchestration)]]에서 에이전트마다 `model`을 추론 깊이에 맞춰 고르는 것도 같은 발상이다.

---

## 5. 에이전트 비용 통제

Cost Playbook Ch 10:

- **요청별 예산과 반복 상한**: 상한 없이 배포한 에이전트가 거래 내역 도구를 12번 호출해 평균 요청의 약 1,000배 비용을 쓴 사례 (p.128).
- **도구 정의 줄이기**: 도구가 15개면 매 반복마다 도구 스키마만 1,500–3,000토큰 (p.127). 단계에 맞는 도구만 보낸다 (p.135).
- **중복 호출 줄이기**: 같은 인자로 같은 도구를 다시 부르지 말라고 프롬프트에 적는 것만으로도 효과가 있다고 한다 (p.133).
- **메모리 압축의 위험**: 오래된 도구 결과를 요약하면 앞 단계 정보를 잃어 같은 호출을 반복할 수 있다 (p.137).

하네스 책의 생성-검증 루프 재시도 상한(2–3회)도 같은 문제(토큰이 조용히 소모됨)를 막는 장치다 ([[Claude Code Harness Design (Agents, Skills, and Orchestration)]] 6절).

---

## 6. thinking 토큰

Cost Playbook은 필요 없으면 extended thinking을 끄라고 한다 (p.21). 현재 모델에서는 방식이 바뀌었다. *Models overview*: extended thinking은 "deprecated on Claude Opus 4.6 and Claude Sonnet 4.6 and not accepted on later models"이고, 현재는 adaptive thinking을 effort로 조절한다. 생각에 쓰는 토큰이 비용에 들어간다는 원리는 같다 (판단).

---

## 관련 개념 및 엔티티

- **상위/연관 개념**: [[Context Engineering]], [[AI Agent Architecture]], [[Claude Code Harness Design (Agents, Skills, and Orchestration)]], [[LLM Evaluation (Evals, LLM-as-Judge, and Agent Trajectories)]]
- **출처 문헌**: [[2026-The AI Cost Playbook]] (Ch 2, 6, 8, 10), [[2026-하네스 엔지니어링 with 클로드 코드]] (부록 D), Anthropic [*Pricing*](https://platform.claude.com/docs/en/about-claude/pricing), [*Prompt caching*](https://platform.claude.com/docs/en/build-with-claude/prompt-caching), [*Models overview*](https://platform.claude.com/docs/en/about-claude/models/overview)
