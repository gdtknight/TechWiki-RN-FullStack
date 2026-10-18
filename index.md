# TechWiki Index

이 파일은 **TechWiki**의 전체 문서를 안내하는 콘텐츠 중심 카탈로그(`index.md`)입니다.
LLM 에이전트는 **Ingest / Query / Lint** 작업 시 항상 이 인덱스를 먼저 읽고 최신 상태로 유지합니다.

- **원천 라이브러리**: `/Users/yoshin/Documents/MarginNote4` (약 670권 PDF)
- **운영 스키마**: [[CLAUDE|CLAUDE.md]]
- **작업 이력 로그**: [[log|log.md]]

---

## Sources (원천 도서 요약)

- [[2025-Building Applications with AI Agents]] — Michael Albada (O'Reilly, 2025). 프로덕션 AI 에이전트 시스템의 4대 구성요소(Model, Tools, Memory, Orchestration), Context Engineering, GraphRAG, 멀티에이전트 아키텍처, 운영/보안 총론. `[G. Artificial Intelligence/Agents]`

---

## Concepts (핵심 개념 & 아키텍처)

- [[AI Agent Architecture]] — 에이전트 4대 구성요소(Model, Tools, Memory, Orchestration), 결정론적 워크플로우 vs 자율 에이전트, 4대 설계 트레이드오프, 자율성 슬라이더(Autonomy Slider).
- [[Context Engineering]] — 프롬프트 엔지니어링을 넘어 매 스텝 필요한 상태·지식·도구 출력을 동적으로 조립하는 오케스트레이션 핵심 기법.
- [[Agent Memory and Knowledge]] — 외부 도메인 지식(Knowledge: RAG/GraphRAG)과 에이전트 경험 기억(Memory: 단기/장기/시맨틱 메모리)의 구분 및 비모수/모수 학습.
- [[Multi-Agent Orchestration]] — 단일 에이전트에서 멀티에이전트로의 분할 기준, 중앙집중형(Supervisor) vs 탈중앙화 스웜(Swarm), 액터 프레임워크(Ray/Orleans/Akka)와 이벤트 버스.

---

## Entities (기술 · 프레임워크 · 프로토콜)

- [[Model Context Protocol (MCP)]] — 에이전트가 외부 지식·메모리·상태·도구를 모델 프롬프트에 구조화해 주입하는 표준 프로토콜.
- [[LangGraph]] — 상태 머신 및 방향 그래프 기반의 에이전트 오케스트레이션 프레임워크 (AutoGen, CrewAI, OpenAI Agents SDK와의 비교 포함).

---

## Syntheses (주제 종합 & 비교 분석)

- [[RAG vs GraphRAG vs LLM Wiki]] — 전통적 벡터 RAG, GraphRAG, 그리고 현재 구축된 마크다운 기반 LLM Wiki 아키텍처의 지식 컴파일 시점·복합 추론·가독성 비교.

---

## Obsidian Dataview 동적 대시보드 (선택)

> Obsidian에서 **Dataview** 플러그인을 활성화하면 아래 쿼리가 YAML 프론트매터를 기반으로 자동 렌더링됩니다.

```dataview
TABLE type AS "유형", category AS "분류", updated AS "최근 갱신"
FROM ""
WHERE type != null
SORT type ASC, file.name ASC
```
