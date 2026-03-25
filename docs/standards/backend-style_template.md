# 백엔드 스타일 가이드

이 문서는 백엔드 개발 표준을 정의한다.
<!-- 프로젝트 스택에 맞게 전체적으로 수정하세요. 아래는 Java + Spring Boot 기준 예시입니다. -->

## 1. 아키텍처 및 패키징

- 최상위 패키지
  - `global`: 설정, 보안, 에러 처리, 공통 유틸 등 횡단 관심사
  - `domain`: 기능 중심 코드
- `domain.<feature>` 하위 구조
  - `domain.<feature>.web`: 컨트롤러, 요청/응답 DTO
  - `domain.<feature>.application`: 서비스/유스케이스, 트랜잭션 조율
  - `domain.<feature>.model`: 엔티티, 값 객체, 애그리거트
  - `domain.<feature>.repository`: Spring Data 리포지토리
- 네이밍
  - 클래스: PascalCase / 메서드·필드: camelCase / 상수: UPPER_SNAKE_CASE
  - 접미사: `XxxController`, `XxxService`, `XxxRepository`

## 2. BaseEntity 및 식별자 정책

- 모든 엔티티는 공통 필드를 가진 `BaseEntity`를 상속한다.
  - `UUID id` (또는 Long — 프로젝트에 맞게 선택)
  - `LocalDateTime createdAt`
  - `LocalDateTime modifiedAt`
- 구현 예시 (JPA)

```java
@MappedSuperclass
@EntityListeners(AuditingEntityListener.class)
public abstract class BaseEntity {
  @Id
  @GeneratedValue
  @UuidGenerator
  private UUID id;

  @CreatedDate
  private LocalDateTime createdAt;

  @LastModifiedDate
  private LocalDateTime modifiedAt;
}
```

- 감사 기능 활성화

```java
@Configuration
@EnableJpaAuditing
public class JpaConfig {}
```

## 3. 엔티티 & ORM

- 엔티티에 과도한 도메인 로직을 넣지 않고, 필요 시 값 객체를 활용한다.
- 연관관계: 단방향 선호, 반드시 필요할 때만 양방향.
- equals/hashCode: 변경 가능한 필드를 사용하지 않는다.

## 4. DTO & 검증

- 요청/응답 DTO를 분리하고, 불변성을 위해 record 또는 data class를 사용한다.
- 입력 검증은 프레임워크 제공 어노테이션을 활용한다.
- 도메인 ↔ DTO 매핑은 application/web 레이어에서 수행한다.

## 5. 예외 및 API 에러

- 중앙집중 예외 처리 핸들러를 사용한다.
- 에러 응답은 `code/message/details` 구조를 유지한다.

## 6. 서비스 & 트랜잭션 규칙

- application 레이어에서 오케스트레이션 및 트랜잭션 경계를 관리한다.
- 컨트롤러와 리포지토리에는 비즈니스 로직을 넣지 않는다.
- 트랜잭션은 서비스 경계에서 선언하고, 읽기 전용 여부를 명시한다.

## 7. 테스트 규칙

- 네이밍: `shouldDoX_whenY()` 형태
- 단위 테스트: 도메인, 서비스 (의존성 목킹)
- 슬라이스 테스트: 컨트롤러 (MockMvc 등)
- 통합 테스트: 리포지토리 + 트랜잭션 경계

## 8. 포매팅 & 정적 분석

- 포매팅 도구: {{FORMATTER}} (예: Spotless + Google Java Format, Black, Prettier)
- 정적 분석: {{STATIC_ANALYSIS}} (예: Checkstyle, Error Prone, pylint)
- CI에서 포맷/체크를 자동 실행한다.

## 9. 기타

- 시간 처리: {{TIMEZONE_POLICY}} (예: UTC 저장, 한국 표준시 표시)
- 식별자 노출: {{ID_POLICY}} (예: API에서 UUID 문자열, DB에는 BINARY(16))
