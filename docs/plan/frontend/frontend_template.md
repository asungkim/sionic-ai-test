## Frontend Template

```markdown
# Feature: <기능 이름>

## 1. Problem Definition
- 사용 시나리오 / 페르소나 / 핵심 Pain Point를 설명한다.

## 2. User Flows & Use Cases
- 주요 플로우를 단계별로 나열하고, 각 단계에서 필요한 화면/상태/검증을 적는다.

## 3. Page & Layout Structure
- 페이지(또는 뷰)별 섹션/영역을 묘사한다 (텍스트 다이어그램/Bullet OK).
- Breakpoint/반응형 고려 사항을 명시한다.

## 4. Component Breakdown
- 컴포넌트 목록
  - <ComponentName>: 역할, props/state, 이벤트, 재사용 여부, 스타일 노트
- 신규/수정 컴포넌트는 "왜 필요한지 → 어떻게 동작하는지 → 어디에 붙는지" 순으로 설명한다.

## 5. State & Data Flow
- 전역/지역 상태 구분, 상태 소유 위치, 상태 전파 방식을 서술한다.
- API 계약 (요청/응답 필드, 유효성 규칙), 에러 처리, 로딩 전략을 명시한다.

## 6. Interaction & UX Details
- 사용자 입력/Validation/Empty/Error/Loading 시 UI 변화를 명시한다.
- 접근성/포커스 순서/키보드 조작, 애니메이션 등 UX 고려사항을 포함한다.

## 7. Test & Verification Plan
- 단위 테스트(컴포넌트/훅), 통합 테스트, 수동 QA 체크리스트를 정의한다.
```
