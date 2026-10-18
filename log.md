# TechWiki Activity Log

시간순으로 기록되는 위키 작업 로그(append-only)입니다.
각 항목은 `## [YYYY-MM-DD] <operation> | <title>` 형식을 따르므로 `grep "^## \[" log.md | tail -5` 등으로 최근 이력을 빠르게 조회할 수 있습니다.

---

## [2026-10-18] init | TechWiki Vault & Schema Initialization
- Obsidian iCloud 경로(`/Users/yoshin/Library/Mobile Documents/iCloud~md~obsidian/Documents/TechWiki`)에 볼트 생성 및 `/Users/yoshin/Documents/TechWiki` 심볼릭 링크 연결.
- Git 저장소 초기화 및 디렉토리 구조(`sources/`, `concepts/`, `entities/`, `syntheses/`, `templates/`) 생성.
- 오픈소스 PDF→Markdown 라이브러리 `pymupdf4llm` (PyMuPDF 1.28.2) 설치 및 스키마(`CLAUDE.md`, `AGENTS.md`) 수립.

## [2026-10-18] ingest | Building Applications with AI Agents (Michael Albada, O'Reilly 2025)
- **원천 PDF**: `/Users/yoshin/Documents/MarginNote4/G. Artificial Intelligence/Agents/2025-Building Applications with AI Agents-Michael Albada-O'Reilly Media.pdf` (355p)
- **생성된 Source 페이지**:
  - [[2025-Building Applications with AI Agents]]
- **생성된 Concept 페이지 (4개)**:
  - [[AI Agent Architecture]]
  - [[Context Engineering]]
  - [[Agent Memory and Knowledge]]
  - [[Multi-Agent Orchestration]]
- **생성된 Entity 페이지 (2개)**:
  - [[Model Context Protocol (MCP)]]
  - [[LangGraph]]
- **생성된 Synthesis 페이지 (1개)**:
  - [[RAG vs GraphRAG vs LLM Wiki]]
- **인덱스 갱신**: `index.md`에 총 8개 위키 페이지 등록 완료.
