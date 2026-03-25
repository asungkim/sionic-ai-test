## Backend Template

```markdown
# Feature: <기능 이름>

## 1. Problem Definition
- 해결해야 할 문제를 기술한다.

## 2. Requirements
### Functional
- 기능 요구사항 목록

### Non-functional
- 성능, 보안, 확장성 등

## 3. API Design (Draft)
| Method | URL | Request | Response | 설명 |
|--------|-----|---------|----------|------|
| POST | /api/v1/xxx | `{ ... }` | `{ ... }` | 설명 |

## 4. Domain Model (Draft)
- Entity, Relation, Validation 규칙

## 5. TDD Plan
- 작성할 테스트의 순서를 정의한다.
1. Repository 테스트: ...
2. Service 테스트: ...
3. Controller 테스트: ...
```
