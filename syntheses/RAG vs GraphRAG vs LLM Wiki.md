---
type: synthesis
title: "RAG vs GraphRAG vs LLM Wiki"
category: "G. Artificial Intelligence/Agents"
sources:
  - "[[2025-Building Applications with AI Agents]]"
tags:
  - rag
  - graphrag
  - llm-wiki
  - knowledge-base
  - memory
created: 2026-10-18
updated: 2026-10-18
---

# RAG vs GraphRAG vs LLM Wiki

## 질문 / 문제 의식

에이전트가 모델 가중치 밖의 방대한 도메인 지식(수백 권의 기술 서적, 논문, 사내 문서)을 활용하려 할 때, **전통적인 벡터 RAG**, [[2025-Building Applications with AI Agents]] (Ch 6, p.145)에서 다루는 **GraphRAG**, 그리고 현재 이 볼트가 채택한 **LLM Wiki 패턴**은 각각 어떻게 다르며 어떻게 상호보완될 수 있는가?

---

## 3대 지식 아키텍처 비교 분석

| 차원 | 전통적 Vector RAG | GraphRAG (Ch 6, p.145) | LLM Wiki 패턴 (현재 볼트) |
|---|---|---|---|
| **저장 단위** | 고정 크기 텍스트 청크(Chunk) + 임베딩 벡터 | 엔티티(Node) + 관계(Edge) 지식 그래프 + 커뮤니티 요약 | LLM이 유지보수하는 **구조화된 마크다운 문서 + `[[위키링크]]` 그래프** |
| **지식 컴파일 시점** | **Query Time** (질의 시점에 조각들을 검색해 매번 처음부터 재합성) | **Ingest Time** (사전에 엔티티/관계 추출 및 그래프 클러스터링 수행) | **Ingest & Query Time** (사전 요약·교차참조·모순 기록 + 질의 결과의 위키 환류) |
| **복합 추론 (Multi-hop)** | 취약함 (여러 문서에 흩어진 단서를 한 번의 코사인 유사도로 찾기 어려움) | 강함 (그래프 경로 탐색과 계층적 커뮤니티 요약 활용) | **매우 강함** (이미 개념/엔티티별로 여러 도서의 관점이 종합·비교되어 있음) |
| **인간 가독성 (Human-in-the-loop)** | 낮음 (벡터 DB 내부는 블랙박스) | 중간 (그래프 시각화 가능하나 직접 읽고 편집하긴 번거로움) | **최상** (Obsidian에서 인간이 그대로 읽고 탐색하는 마크다운 코드베이스) |
| **인프라 복잡도** | 임베딩 파이프라인 + Vector DB 필요 | 엔티티 추출 파이프라인 + Graph DB + Vector DB 필요 | **제로 인프라** (순수 마크다운 파일 + `index.md` + Git) |

---

## 핵심 통찰 (Synthesis)

1. **LLM Wiki는 "인간과 LLM이 공유하는 마크다운 기반 GraphRAG + 비모수 경험 메모리"다**
   - [[2025-Building Applications with AI Agents]] (p.145)에서 GraphRAG가 해결하려는 핵심 문제(단순 청크 유사도 검색으로는 전체 코퍼스를 가로지르는 종합 질문에 답하지 못하는 한계)를, LLM Wiki는 **별도의 그래프 DB 없이 마크다운 페이지(`concepts/`, `entities/`, `syntheses/`)와 `[[위키링크]]`, 그리고 `index.md` 카탈로그**로 해결한다.
2. **Query의 복리 효과 (Compounding Knowledge)**
   - 일반 RAG나 GraphRAG에서는 사용자가 훌륭한 질문을 던져 얻어낸 종합 분석이 채팅 히스토리 속에서 사라진다.
   - 반면 LLM Wiki에서는 질의 결과 자체가 `syntheses/` 페이지로 위키에 다시 파일링(Filing Back)되므로, **질문을 할수록 지식 베이스의 밀도와 추론 수준이 복리로 높아진다** ([[Agent Memory and Knowledge]]의 비모수 학습 효과).

---

## 참고 출처

- [[2025-Building Applications with AI Agents]] (Ch 6 "Knowledge and Memory", p.137–156; Ch 7 "Nonparametric Learning", p.157)
