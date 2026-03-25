# 최우선 원칙

- 문서와 코드는 반드시 일치해야 한다. 불일치 상태를 절대 남기지 않는다.
- 어떤 임무든 작업 순서는 문서 -> 코드다.
- 백엔드 작업은 `backend/AGENTS.md`, 프런트엔드 작업은 `frontend/AGENTS.md` 규칙을 추가로 참조한다.
- 하위 패키지/디렉토리에 `AGENTS.md`가 있으면 해당 규칙을 반드시 따른다.

# 프로젝트 개요

- **목표:** Spring Boot 3.5.12와 PostgreSQL 15.8 기반 사전 과제 대비용 서비스 설계/구현 연습
- **핵심 스택:** Spring Boot 3.5.12, PostgreSQL 15.8

# 문서 체인 & 작업 절차

- 문서 변경 순서: `docs/design/entity/vX.Y.md`(최신) -> `docs/design/erd/vX.Y.md`(최신) -> `docs/requirement/vX.Y.md`(최신) -> `docs/spec/vX.Y.md`(최신)
- TODO 갱신: 스펙 확정 후 `docs/todo/vX.Y.md`(최신)를 새 버전으로 생성한다. 구조 변경이 없고 상태만 바뀌면 기존 파일에서 수정한다.
- TODO 구조/상태: Phase -> Epic -> Task, 상태 이모지(✅ 완료, 🔄 진행, ⚪ 대기, ⛔ 차단)
- 기능 추가/수정 표준 플로우:
  1) TODO에 작업 항목을 먼저 갱신한다.
  2) 작업 범위를 의논한 뒤 PLAN 작성 범위를 확정한다.
  3) PLAN 작성 전에 문서 체인을 `design -> requirement -> spec` 순으로 선반영한다.
  4) PLAN을 작성/승인받은 후 TDD/구현을 진행한다.
  5) 구현 완료 후 `code -> spec -> requirement -> design` 역정합화로 문서/코드를 최종 일치시킨다.
- Implementation Gate: PLAN/TODO가 최신 Requirement/Spec 버전을 참조하지 않으면 구현 금지
- "go" 규칙: `docs/todo/vX.Y.md` 순서 준수. 🔄가 있으면 그 작업, 없으면 첫 ⚪ 작업, ⛔는 건너뜀
- PLAN 절차: `docs/plan/backend/backend_template.md` 또는 `docs/plan/frontend/frontend_template.md` + 영역별 `backend/AGENTS.md` 또는 `frontend/AGENTS.md` 지침을 기준으로 백엔드는 `docs/plan/backend/<feature>_plan.md`, 프론트엔드는 `docs/plan/frontend/<feature>_plan.md` 작성 -> 한국어 요약 -> 승인 후 TDD/구현
- 개발 종료 역정합화: 기능 개발이 끝나면 `code -> spec -> requirement -> design` 역순으로 재검증/통합하여 문서-코드 불일치를 해소한 뒤 종료한다.

# 기록 규칙

- 모든 의미 있는 변경은 `docs/history/AGENT_LOG.md`에 Append-only로 기록한다.
- STRUCTURAL과 BEHAVIORAL 변경은 같은 로그에 섞지 않는다.

```md
## [YYYY-MM-DD HH:mm] <작업 요약>

### Type
STRUCTURAL | BEHAVIORAL | DESIGN | TODO_UPDATE | BUGFIX | RELEASE

### Summary
- 수행한 작업 요약

### Details
- 작업 사유
- 영향받은 테스트
- 수정한 파일
- 다음 단계
```

# 브랜치 & PR

- 기본 브랜치 `main` 직접 push 금지. PR로만 merge.
- 브랜치: `feature/`, `fix/`, `refactor/`, `docs/`, `chore/` 규칙 준수

# MCP 활용

- Git 관련 작업은 GitHub MCP 우선 사용한다.
- 라이브러리 문서는 Context7 MCP 또는 1차 문서로 확인하고 참조 기록을 남긴다.
- 작업 시작 시 브랜치/이슈/TODO 문맥을 확인한다.
