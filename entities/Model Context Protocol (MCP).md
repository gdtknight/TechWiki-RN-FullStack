---
type: entity
title: "Model Context Protocol (MCP)"
entity_type: "protocol"
category: "G. Artificial Intelligence/Agents"
sources:
  - "[[2025-Building Applications with AI Agents]]"
tags:
  - mcp
  - protocol
  - tool-use
  - context-engineering
created: 2026-10-18
updated: 2026-10-18
---

# Model Context Protocol (MCP)

## 개요

**Model Context Protocol (MCP)** 은 AI 에이전트가 외부 지식(Knowledge), 메모리(Memory), 시스템 상태(State), 그리고 실행 가능한 도구(Tools)를 언어 모델의 프롬프트에 구조화하여 실시간으로 연결·주입하기 위한 **표준화된 프로토콜 및 스키마**다 ([[2025-Building Applications with AI Agents]], Ch 2 p.48).

---

## 아키텍처 및 특징

- **컨텍스트와 도구의 표준 인터페이스**: 과거에는 각 에이전트 프레임워크마다 외부 API나 데이터 소스를 연동하는 커스텀 글루 코드(Glue code)를 중복 작성해야 했으나, MCP는 이를 클라이언트-서버 표준 스키마로 추상화한다.
- **동적 컨텍스트 주입 (Dynamic Context Injection)**: 단순 일회성 REST API 호출과 달리, 에이전트가 작업 맥락에 맞춰 풍부하고 구조화된 외부 상태와 리소스를 프롬프트 컨텍스트로 안전하게 가져올 수 있도록 돕는다 ([[Context Engineering]]).
- **모듈성 및 재사용성**: 한 번 작성된 MCP 서버(예: 데이터베이스, 문서 검색, Git, 브라우저 제어 등)는 다양한 에이전트 런타임과 호환된다.

---

## 관련 링크

- **연관 개념**: [[AI Agent Architecture]], [[Context Engineering]]
- **연관 엔티티**: [[LangGraph]]
- **출처 문헌**: [[2025-Building Applications with AI Agents]] (p.48)
