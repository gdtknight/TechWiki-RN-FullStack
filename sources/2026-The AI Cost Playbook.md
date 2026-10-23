---
type: source
source_type: book
title: "The AI Cost Playbook"
authors:
  - "Caio Incau"
publisher: "Packt Publishing"
year: 2026
category: "G. Artificial Intelligence/MLOps"
pdf_path: "/Users/yoshin/Documents/MarginNote4/G. Artificial Intelligence/MLOps/2026-The AI Cost Playbook-Caio Incau-Packt Publishing.pdf"
status: partial
tags:
  - llm-cost
  - token-economics
  - prompt-caching
  - model-routing
  - batch-processing
  - agent-cost
  - finops
sources: ["[[2026-The AI Cost Playbook]]"]
created: 2026-10-23
updated: 2026-10-23
---

# The AI Cost Playbook (2026)

> **서지 정보**: Caio Incau | Packt Publishing (2026, 서문 "June 2026", p.5) | 206 Pages
> **분류**: `G. Artificial Intelligence / MLOps`
> **원천 PDF**: [2026-The AI Cost Playbook-Caio Incau-Packt Publishing.pdf](file:///Users/yoshin/Documents/MarginNote4/G.%20Artificial%20Intelligence/MLOps/2026-The%20AI%20Cost%20Playbook-Caio%20Incau-Packt%20Publishing.pdf)
> **쪽 표기**: `p.XX`는 PDF 쪽 번호. 본문은 PDF 쪽 = 인쇄 쪽 + 4.
> **텍스트 추출**: [[2026-AI Evals in Practice]]와 같은 이유로 PDF 텍스트 층이 깨져 있어 macOS Vision OCR로 읽었다.
> **투입 범위 (`status: partial`)**: 서문(p.4–5), Ch 2(p.15–26), Ch 6(p.68–82), Ch 8(p.98–102), Ch 10(p.126–137)을 정독. 나머지는 목차로만.

---

## 핵심 요약 (Core Thesis)

LLM 비용은 고정 요금이 아니라 토큰 단위 사용량 과금이며, 이를 모르면 청구서가 폭주한다는 경험에서 출발한다 (p.4). 저자는 구체적인 가격을 사실로 못 박지 않겠다고 밝힌다: "By the time you read this, the specific per-token prices I could cite will be wrong. Instead of hardcoding prices, tokenwatch uses a pricing.yaml configuration file that you update as prices change." (p.5) 책 전체가 `tokenwatch`라는 비용 관측·최적화 도구를 만든다 (p.4–5).

비용을 줄이는 수단은 대략 이 순서로 쌓인다 (판단: 각 장의 연결 설명에서): 측정(계측·원가 배분) → 프롬프트 압축 → 프롬프트 캐싱 → 배치 → 모델 라우팅 → RAG·에이전트 최적화 → 예산·쿼터 거버넌스.

위키의 개념 정리는 [[LLM Cost Engineering (Token Economics, Caching, and Routing)]].

---

## 주요 챕터 및 구조

- **Ch 1 (p.6–15)** — 목차: AI 청구서 쇼크.
- **Ch 2. Token Economics 101 (p.15–26)** — 정독: 토큰화와 형식별 토큰 비용 차이(산문 vs JSON vs XML) (p.15–16), 컨텍스트 창은 "a capability ceiling, not a cost target" (p.18), 입력·출력·캐시·thinking 토큰의 비용 차이 (p.20–21), 사전 비용 추정 (p.22–24), 대화 기록이 늘어나는 비용: "The total cost of the conversation is not the sum of individual messages - it's the sum of all prefixes of the conversation." (p.24)
- **Ch 3–5 (p.26–68)** — 목차: 토큰 계측과 원가 배분, 요청·사용자·기능별 단위 경제, 비용을 위한 프롬프트 엔지니어링.
- **Ch 6. Prompt Caching (p.68–82)** — 정독: Anthropic 캐시의 쓰기·읽기·TTL·최소 길이 (p.68–69), "The single most important principle for prompt caching is: stable content first, variable content last." (p.73), 캐시가 조용히 깨지는 원인 (p.72–73), 손익분기 계산 (p.75–78), 흔한 실수 (p.80–81).
- **Ch 7 (p.82–98)** — 목차: 배치 처리와 비동기 작업.
- **Ch 8. Model Routing and Cascades (p.98–113)** — 앞부분 정독: 요청의 60–80%는 더 싼 모델로 충분하다는 주장 (p.98), 분류 방식 셋(규칙, 기능별, LLM 분류기) 중 기능별 분류부터 시작 (p.101).
- **Ch 9 (p.113–126)** — 목차: RAG 비용 최적화.
- **Ch 10. Agents: The Cost Multiplier (p.126–140)** — 정독: 비용 배수의 세 원천(루프 반복, 매 호출마다 다시 보내는 도구 정의, 불필요한 도구 호출·추론 루프) (p.126–127), 요청별 예산과 반복 상한 (p.128–131), 도구 호출 줄이기와 단계별 도구 선택 (p.133–135), 메모리 압축의 위험 (p.136–137).
- **Ch 11–14, 부록 (p.140–206)** — 목차: 자체 호스팅 손익분기, 거버넌스(예산·쿼터·책임), 협상과 예측, 최종 프로젝트, 체크리스트.

---

## Anthropic 공식 문서와 대조 (2026-10-23)

대조한 문서: [*Pricing*](https://platform.claude.com/docs/en/about-claude/pricing), [*Prompt caching*](https://platform.claude.com/docs/en/build-with-claude/prompt-caching), [*Models overview*](https://platform.claude.com/docs/en/about-claude/models/overview), [*Model IDs and versions*](https://platform.claude.com/docs/en/about-claude/models/model-ids-and-versions).

| 책 (PDF 쪽) | 공식 문서 | 판정 |
|---|---|---|
| 캐시 쓰기는 "typically 25% more than normal input rate", 읽기는 약 90% 할인 (p.68) | 5분 캐시 쓰기 1.25배, 읽기 0.1배. 단 "0.025x on Claude Fable 5.1 and Claude Mythos 5.1; 0.05x on Claude Opus 5.5 and Claude Sonnet 5.5" | **4.x 세대 기준으로 맞음**. 최신 모델은 할인이 더 큼 |
| TTL은 "currently 5 minutes for Anthropic, refreshed on each cache hit" (p.69) | "By default, the cache has a 5-minute lifetime. The cache is refreshed for no additional cost each time the cached content is used." 1시간 TTL(쓰기 2배)도 있음 | **맞음**. 책은 1시간 옵션을 다루지 않음 |
| Anthropic 캐싱은 "explicit and developer-controlled", `cache_control` 표시 필요 (p.68) | 문서 설명: "using automatic caching or explicit breakpoints" | **부분적으로 바뀜**. 자동 캐싱도 있음 |
| 최소 캐시 길이: Sonnet급 1,024토큰 (p.69, p.80) | Sonnet 4.6은 1,024, Opus 4.6·Haiku 4.5는 4,096, Opus 4.7은 2,048, 5.x 세대는 512 | **Sonnet 4.6은 맞음**. 모델마다 다르므로 모델별로 확인해야 함 |
| 배치 할인 50% (p.98) | "The Batch API allows asynchronous processing of large volumes of requests with a 50 percent discount on both input and output tokens." | **맞음** |
| `pricing.yaml`의 `claude-haiku-4-5-20250514`: 입력 $0.80 / 출력 $4.0 / 캐시 $0.08 (p.11) | Haiku 4.5: $1 / $5 / $0.10. 스냅샷 ID는 `claude-haiku-4-5-20251001` | **가격과 ID 모두 다름**. $0.80 / $4는 Haiku 3.5의 가격. 라우팅 예제(p.99)도 이 가격을 씀 |
| `pricing.yaml`의 Sonnet 4 $3 / $15, Opus 4 $15 / $75 (p.10–11) | 표의 Sonnet 4·Opus 4 가격과 같음. 단 둘 다 지금은 retired 표시 | **가격은 맞음**, 모델은 은퇴 |
| "Claude Sonnet supports a 200K-token context window." (p.18) | 현재 모델 표는 Fable 5.1·Opus 5.5·Sonnet 5.5·Haiku 5.5 모두 1M | **세대에 따라 다름** |
| 필요 없으면 extended thinking을 끄라 (p.21) | *Models overview*: extended thinking은 "deprecated on Claude Opus 4.6 and Claude Sonnet 4.6 and not accepted on later models". 현재는 effort로 조절하는 adaptive thinking | **방식이 바뀜**. 원리(생각 토큰은 출력 가격)는 같음 (판단) |

저자 스스로 가격은 바뀌니 설정 파일로 관리하라고 했으므로(p.5), 위 차이는 책의 결함이라기보다 **설정값을 공식 가격 페이지와 맞춰 써야 한다는 사례**로 기록한다 (판단).

---

## 추출된 핵심 개념 & 엔티티

- **Concepts (신규)**: [[LLM Cost Engineering (Token Economics, Caching, and Routing)]]
- **Concepts (보강)**: [[Context Engineering]] (캐시 친화적 프롬프트 구조)

## 관련 도서

- [[2026-하네스 엔지니어링 with 클로드 코드]] — 부록 D 토큰 경제(입력이 비용 대부분, 캐시 히트율이 첫 번째 레버)
- [[2026-AI Evals in Practice]] — 같은 저자. 라우팅의 품질 검증과 평가 비용
