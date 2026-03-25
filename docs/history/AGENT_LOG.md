## [2026-03-25 13:52] 운영 문서 초기 정합성 보정

### Type
DESIGN

### Summary
- 루트 운영 문서의 프로젝트 목표/핵심 스택 placeholder를 실제 과제 준비 기준으로 교체했다.
- PLAN 절차 문구를 현재 `docs/plan` 디렉토리 구조에 맞게 수정했다.
- 백엔드 운영 문서의 기술 기준을 README와 현재 테스트 전략에 맞게 구체화했다.

### Details
- 작업 사유: 초기 설정 문서의 placeholder와 잘못된 PLAN 템플릿 참조를 제거해 문서 간 정합성을 확보하기 위해 수정했다.
- 영향받은 테스트: 없음 (문서 변경만 수행)
- 수정한 파일: `AGENTS.md`, `CLAUDE.md`, `backend/AGENTS.md`, `backend/CLAUDE.md`, `docs/history/AGENT_LOG.md`
- 다음 단계: `frontend/AGENTS.md` 부재 여부와 실제 프로젝트 초기화 시 Java/Gradle 버전을 build 파일 기준으로 최종 확정한다.
