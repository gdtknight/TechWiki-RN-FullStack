# CLAUDE.md — TechWiki Schema & Operations Guide

이 문서는 **LLM Wiki 패턴**의 3계층 중 **Schema(스키마)** 계층이다.
LLM 에이전트(Claude Code, Codex, Antigravity 등)는 이 볼트에서 작업할 때 반드시 이 문서의 규칙과 워크플로우를 따른다.

---

## 1. 아키텍처 (3-Layer Architecture)

1. **Raw Sources (원천 소스 — 불변/읽기 전용)**
   - 경로: `/Users/yoshin/Documents/MarginNote4` (`대분류/소분류/{연도}-{제목}-{저자}-{출판사}.pdf`, 약 670권)
   - 원천 PDF는 절대 수정하거나 이동하지 않는다 (파일 관리와 MN4 태그는 `mn4-librarian` 전용).
2. **The Wiki (지식 베이스 — LLM 전담 유지보수)**
   - 경로: `/Users/yoshin/Documents/TechWiki` (실제 위치: Obsidian iCloud `.../iCloud~md~obsidian/Documents/TechWiki`)
   - LLM이 모든 마크다운 문서를 생성·갱신·교차참조(`[[위키링크]]`)하며, 사용자는 Obsidian에서 탐색 및 그래프 뷰로 열람한다.
3. **The Schema (규칙 및 워크플로우)**
   - 경로: `CLAUDE.md` (및 `AGENTS.md` 심볼릭 링크)

---

## 2. 디렉토리 구조

```text
TechWiki/
├── CLAUDE.md          # 위키 스키마 및 에이전트 운영 지침 (AGENTS.md 심볼릭 링크)
├── index.md           # 전체 위키 콘텐츠 카탈로그 (Ingest/Query/Lint 시 항상 갱신)
├── log.md             # 시간순 작업 로그 (append-only, grep 파싱 가능 형식)
├── sources/           # 원천 도서/문서 요약 페이지 (1권당 1페이지 기본, 필요 시 챕터 분리)
├── concepts/          # 핵심 개념·이론·아키텍처·알고리즘·디자인 패턴 페이지
├── entities/          # 구체적 기술·언어·프레임워크·프로토콜·시스템·도구 페이지
├── syntheses/         # 여러 소스를 교차 분석한 비교표·심층 질문 답변·주제 종합 페이지
└── templates/         # 페이지 유형별 마크다운 & YAML 프론트매터 템플릿
```

---

## 3. PDF 원문 추출 방법 (`pymupdf` / `pymupdf4llm`)

별도의 커스텀 스크립트 없이 시스템 Python(`/Users/yoshin/.pyenv/shims/python3`)에 설치된 **`pymupdf`** 및 **`pymupdf4llm`**을 사용한다.

### 3.1 도서 검색
```bash
find -L /Users/yoshin/Documents/MarginNote4/ -maxdepth 3 -iname "*키워드*.pdf"
```

### 3.2 목차(TOC) 및 총 페이지 확인
```bash
/Users/yoshin/.pyenv/shims/python3 -c "
import sys, pymupdf
doc = pymupdf.open(sys.argv[1])
print('Total pages:', len(doc))
for lvl, title, page in doc.get_toc():
    if lvl <= 2:
        print(f'{"  "*(lvl-1)}- {title} (p.{page})')
" "/Users/yoshin/Documents/MarginNote4/대분류/소분류/파일명.pdf"
```

### 3.3 특정 페이지 범위 마크다운 변환 (`pymupdf4llm`)
전체 PDF를 한 번에 변환하지 말고, 목차(TOC)를 먼저 확인한 뒤 필요한 페이지 범위(0-indexed)만 지정해 추출한다.
```bash
/Users/yoshin/.pyenv/shims/python3 -c "
import sys, pymupdf4llm
# 예: 20~35페이지 추출 (0-indexed 리스트 전달)
md = pymupdf4llm.to_markdown(sys.argv[1], pages=list(range(20, 35)))
print(md)
" "/Users/yoshin/Documents/MarginNote4/대분류/소분류/파일명.pdf"
```

---

## 4. 문서 작성 규칙 (Obsidian & Dataview 호환)

### 4.1 YAML 프론트매터 (필수)
모든 위키 페이지는 상단에 YAML 프론트매터를 포함해야 하며, Obsidian Properties 및 **Dataview** 플러그인 쿼리와 호환되도록 아래 필드를 표준화한다.

- **공통 필드**:
  - `type`: `source` | `concept` | `entity` | `synthesis`
  - `title`: 문서 제목
  - `category`: MarginNote4 대분류/소분류 (예: `"G. Artificial Intelligence/Agents"`)
  - `tags`: 배열 형식 태그 (예: `[ai-agents, orchestration, context-engineering]`)
  - `sources`: 이 페이지를 뒷받침하는 `sources/` 문서 위키링크 배열 (예: `["[[2025-Building Applications with AI Agents]]"]`)
  - `created`: `YYYY-MM-DD`
  - `updated`: `YYYY-MM-DD`
- **`source` 전용 추가 필드**:
  - `authors`: 저자 배열 (예: `["Michael Albada"]`)
  - `publisher`: 출판사 (예: `"O'Reilly Media"`)
  - `year`: 출판연도 (숫자, 예: `2025`)
  - `pdf_path`: 원천 PDF 절대 경로
  - `status`: `ingested` | `partial` | `reading`

### 4.2 위키링크 (`[[...]]`) 및 교차참조 규칙
- 파일명은 고유하고 명확한 명사구로 짓는다 (`concepts/Context Engineering.md`, `entities/LangGraph.md`).
- 본문에서 다른 개념·엔티티·도서를 언급할 때는 적극적으로 `[[문서명]]` 또는 `[[문서명|표시 텍스트]]` 위키링크를 건다.
- 새로운 정보가 기존 페이지의 내용과 충돌하거나 발전시킬 경우, 기존 페이지를 덮어쓰기만 하지 말고 **"관점 비교 / 출처별 차이"** 섹션에 출처(`[[도서명]] (p.XX)`)와 함께 누적 기록한다.
- 모든 주요 주장과 정의에는 가능한 한 원천 도서와 페이지 번호(`[[도서명]] p.XX`)를 인용한다.

---

## 5. 핵심 오퍼레이션 (Operations Workflow)

### 5.1 Ingest (도서/문서 투입)
사용자가 특정 PDF(또는 주제 폴더)의 Ingest를 요청하면 다음 순서를 따른다:
1. **목차 및 구조 파악**: `pymupdf`로 PDF의 TOC와 서문/핵심 챕터를 추출한다.
2. **Source 페이지 생성/갱신**: `sources/{연도}-{제목}.md`를 생성하고 핵심 테제, 챕터별 요약, 주요 개념/엔티티 목록을 정리한다.
3. **Concept & Entity 페이지 확장**: 도서에서 다루는 핵심 개념(`concepts/`)과 기술/도구(`entities/`)를 **최소 3~8개** 생성하거나 기존 페이지에 새로운 인사이트·인용을 통합한다.
4. **Index & Log 갱신**:
   - `index.md`의 해당 섹션에 새로 생성/갱신된 페이지와 1줄 요약을 반영한다.
   - `log.md` 맨 아래에 `## [YYYY-MM-DD] ingest | {도서 제목}` 항목을 추가하고 생성/수정된 파일 목록을 기록한다.

### 5.2 Query (질의 및 지식 환류)
사용자가 위키에 대해 질문하면:
1. **탐색**: `index.md`를 먼저 읽고 관련 `sources/`, `concepts/`, `entities/`, `syntheses/` 페이지를 열람한다. 필요 시 원천 PDF에서 추가 페이지를 발췌한다.
2. **답변 및 인용**: 위키 내 문서를 `[[위키링크]]`로 인용하며 종합 답변을 제시한다.
3. **Filing Back (위키 환류)**: 가치 있는 비교 분석, 아키텍처 정리, 여러 책에 걸친 종합 답변은 채팅창에서 휘발시키지 않고 `syntheses/`(또는 `concepts/`)에 새 페이지로 저장한 뒤 `index.md`와 `log.md`(`## [YYYY-MM-DD] query | ...`)에 기록한다.

### 5.3 Lint (위키 건강검진)
주기적으로 사용자가 Lint를 요청하면:
1. **고아 페이지/끊어진 링크 점검**: 인바운드 링크가 없는 페이지, 아직 문서로 만들어지지 않은 주요 미생성 `[[위키링크]]` 후보를 찾는다.
2. **모순/오래된 서술 점검**: 여러 도서 간 정의나 권장 패턴이 상충하는 부분을 찾아 정리한다.
3. **Index 동기화 검증**: 실제 파일 목록과 `index.md`가 100% 일치하는지 확인하고 보정한다.
4. **탐구 제안**: 현재 위키 상태에서 `~/Documents/MarginNote4` 내의 어떤 책을 다음으로 Ingest하면 지식 그래프가 가장 잘 연결될지 추천한다.
5. `log.md`에 `## [YYYY-MM-DD] lint | ...` 항목을 기록한다.
