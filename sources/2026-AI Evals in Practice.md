---
type: source
source_type: book
title: "AI Evals in Practice"
authors:
  - "Caio Incau"
publisher: "Packt Publishing"
year: 2026
category: "G. Artificial Intelligence/MLOps"
pdf_path: "/Users/yoshin/Documents/MarginNote4/G. Artificial Intelligence/MLOps/AI Evals in Practice-Caio Incau-Packt Publishing.pdf"
status: partial
tags:
  - llm-evaluation
  - evals
  - llm-as-judge
  - golden-dataset
  - agent-evaluation
  - trajectory
  - eval-driven-development
sources: ["[[2026-AI Evals in Practice]]"]
created: 2026-10-23
updated: 2026-10-23
---

# AI Evals in Practice (2026)

> **서지 정보**: Caio Incau | Packt Publishing (2026) | 216 Pages. 부제 "Testing, reliability, and quality for LLM systems in production" (p.2)
> **출간 연도 근거**: 파일명에 연도가 없다. 감사의 말 끝에 "Caio Incau June 2026" (p.6), PDF 메타데이터 생성일 2026-09-04.
> **분류**: `G. Artificial Intelligence / MLOps`
> **원천 PDF**: [AI Evals in Practice-Caio Incau-Packt Publishing.pdf](file:///Users/yoshin/Documents/MarginNote4/G.%20Artificial%20Intelligence/MLOps/AI%20Evals%20in%20Practice-Caio%20Incau-Packt%20Publishing.pdf)
> **쪽 표기**: `p.XX`는 PDF 쪽 번호. 본문은 PDF 쪽 = 인쇄 쪽 + 3.
> **텍스트 추출**: PDF의 텍스트 층이 글꼴 인코딩 문제로 깨져 있어(본문 거의 전 쪽), macOS Vision 프레임워크 OCR로 읽었다. 인용은 OCR 결과에서 옮겼으므로 철자 하나 수준의 인식 오류가 있을 수 있다.
> **투입 범위 (`status: partial`)**: 서문(p.4–6), Ch 1(p.6–15), Ch 5(p.60–74), Ch 8(p.103–117), Ch 13(p.169–180)을 정독. 나머지 장은 목차와 각 장 끝 요약으로만.

---

## 핵심 요약 (Core Thesis)

출발점은 관측과 평가의 구분이다: "Observability tells you that the system is running. Evals tell you that the system is working correctly." (p.6) 저자는 LangChain 설문을 인용해 "89% of teams with production agents have observability in place, but only 52% have evaluations"라고 쓴다 (p.4, 원 설문은 확인하지 않음).

평가(eval)의 최소 구성은 셋이다: "1. A dataset", "2. A scorer", "3. A runner" (p.8). 책 전체가 이 세 요소를 키워 `evalkit`이라는 순수 Python 평가 도구를 만든다 (p.5). 예제는 Anthropic SDK와 `claude-sonnet-4-6`·`claude-haiku-4-5`를 쓴다 (p.5).

위키의 개념 정리는 [[LLM Evaluation (Evals, LLM-as-Judge, and Agent Trajectories)]].

---

## 주요 챕터 및 구조

- **Ch 1. Why Evals Are the Missing Discipline (p.6–15)** — 정독: 관측 ≠ 평가 (p.6–7), 평가하지 않을 때의 비용(조용한 품질 저하, 프롬프트 변경 불안, 디버깅 불가, 반복 속도 저하) (p.7–8), 평가의 스펙트럼(정확 일치 → 패턴 → 구조 검증 → 의미 유사도 → LLM-as-Judge → 사람) (p.9–10), 개발·CI·배포 후·주기적 실행 (p.10).
- **Ch 2–4 (p.15–60)** — 목차: 데이터셋·채점기·실행기 추상화, 골든 데이터셋, 결정적 채점기(Levenshtein, JSON Schema, 임베딩 유사도, 제약 조건, 코드 구문 검사).
- **Ch 5. LLM-as-Judge (p.60–74)** — 정독: 판정 호출의 네 입력(작업 입력, 출력, 루브릭, 참조 답) (p.61), 가중 루브릭 (p.62–65), 루브릭 설계 원칙 (p.66), 사람 점수와의 보정 (p.67–69), 판정 모델 업그레이드 때의 드리프트 (p.69–70), 네 가지 편향과 완화 (p.70–73).
- **Ch 6–7 (p.74–103)** — 목차: 프롬프트 회귀 테스트와 CI 연동, RAG 평가(맥락 관련성, 충실성, 완결성, 재현율).
- **Ch 8. Evaluating Agents (p.103–117)** — 정독: 최종 답이 아니라 궤적(trajectory)을 평가 (p.103), 도구 호출 정확도(순서 무관 / 엄격 순서 + LCS 부분 점수) (p.108), 궤적 효율, 다중 턴 일관성, 작업 완료 (p.108–112), 도구를 부르지 말아야 하는 사례 (p.114), 사용자 시뮬레이터 (p.115), 오래된 평가 사례 정리 (p.115–116).
- **Ch 9–12 (p.117–169)** — 목차: 사람 주석, 운영 환경 온라인 평가와 가드레일, 평가 파이프라인의 비용·지연, 도구 비교(DeepEval, promptfoo, Langfuse, Braintrust).
- **Ch 13. Eval-Driven Development Workflow (p.169–180)** — 정독: EDD 6단계 (p.170), 성숙도 Level 0–4 (p.177), 평가 없는 코드베이스에 도입하는 순서 (p.177–178).
- **Ch 14–15, 부록 (p.180–216)** — 목차: 불안정한 에이전트를 99% 신뢰성으로 올린 사례, 최종 프로젝트, 지표 참조.

---

## 공식 문서와 대조한 점

| 책 | 확인 결과 |
|---|---|
| 판정 모델은 "Always use `temperature=0.0` for the judge model." 이유는 "as deterministic as possible" (p.65) | 미확인. 현재 Anthropic API 문서의 *Create a Message* 페이지에서 `temperature` 항목을 찾지 못했다 (확인 방법: platform.claude.com의 Messages API 레퍼런스와 모델별 파라미터 지원 문서를 열어 볼 것). |
| 예제 모델 `claude-sonnet-4-6`, `claude-haiku-4-5` (p.5) | *Models overview*에서 둘 다 "Legacy models (still available)" 목록에 있다. 현재 최신은 Sonnet 5.5·Haiku 5.5 |

이 책은 수치를 대부분 저자의 경험(예: "At a large fintech where I worked", p.105, p.177)으로 제시한다. 위키에서는 책의 주장으로만 기록한다.

---

## 추출된 핵심 개념 & 엔티티

- **Concepts (신규)**: [[LLM Evaluation (Evals, LLM-as-Judge, and Agent Trajectories)]]
- **Concepts (보강)**: [[AI Agent Architecture]] (평가 절에서 연결)

## 관련 도서

- [[2026-AI 에이전트를 깊이 이해하기]] — 에이전트 평가와 사후 학습 폐루프
- [[2026-The AI Cost Playbook]] — 같은 저자. 평가 파이프라인의 비용과 모델 라우팅의 품질 검증
- [[Frontend Testing Strategy (Unit, Mocking, and E2E)]] — 일반 소프트웨어 테스트와 비교
