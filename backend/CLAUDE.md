# 백엔드 작업 원칙

## 1. 문서 우선

- 모든 백엔드 구현은 `docs/requirement`, `docs/spec` 최신 버전을 먼저 확인한 뒤 진행한다.
- 설계 변경이 있으면 `docs/design/entity/vX.Y.md`, `docs/design/erd/vX.Y.md`부터 먼저 갱신한다.
- PLAN/TODO가 최신 Requirement/Spec 참조하지 않으면 구현하지 않는다.

## 2. 현재 백엔드 기준

- 런타임: [Java 버전]
- 프레임워크: [Spring Boot 버전]
- 빌드: [Gradle 버전]
- DB: [DB 버전]
- 테스트 DB: [테스트 DB 버전]
- 테스트 프레임워크: [테스트 프레임워크 버전]
- 기본 검증 명령: `.gradlew test`

## 3. 패키지/구조 규칙

- 패키지 최상위는 `global`, `domain`만 사용한다.
- 기능별 패키지는 `domain.<feature>.web|application|model|repository` 구조를 따른다.
- 컨트롤러는 `domain.<feature>.web`에 둔다.
- 서비스/유스케이스는 `domain.<feature>.application`에 둔다.
- 영속 모델은 `domain.<feature>.model`, 저장소는 `domain.<feature>.repository`에 둔다.

## 4. 엔티티/응답/예외 규칙

- 모든 엔티티는 `global.entity.BaseEntity`를 상속한다.
- 공통 응답은 `global.response.RsData`, 응답 코드는 `global.response.RsCode`를 사용한다.
- 예외는 `BusinessException`, `GlobalExceptionHandler`를 통해 표준화한다.
- 공통 설정은 `global.config`, 환경 분리는 `application*.yml`, `.env.example`로 관리한다.

## 5. 테스트 전략

- 테스트 피라미드를 따른다: 단위 테스트 > 슬라이스 테스트 > 통합 테스트.
- 가능한 한 가장 좁은 테스트 범위를 먼저 선택한다.
- 테스트는 빠르고, 독립적이고, 순서 비의존적이어야 한다.
- 테스트는 구현 세부가 아니라 동작을 검증해야 한다.
- 테스트 커버리지는 실무 기준으로 80% 이상을 목표로 한다.

## 6. TDD 워크플로우

1. 먼저 실패하는 테스트를 작성한다.
2. 테스트를 통과시키는 최소 구현만 추가한다.
3. 테스트가 green인 상태에서 리팩터링한다.
4. 커버리지와 회귀 여부를 검증한다.

- 새 기능, 버그 수정, 리팩터링, API 추가는 기본적으로 이 순서를 따른다.
- 한 메서드를 검증하려면 테스트 케이스가 과도하게 많아질 경우, 구현을 더 작은 단위로 분해할지 먼저 검토한다.

## 7. 테스트 종류별 규칙

### 단위 테스트

- 대상: 서비스, 유틸, 순수 비즈니스 로직
- 도구: JUnit 5 + Mockito
- Spring Context 없이 실행하는 것을 기본으로 한다.
- 의존성은 `@Mock`, `@InjectMocks` 또는 명시적 생성자로 주입한다.
- 부분 목(partial mock)은 피하고 명시적 stubbing을 우선한다.

### 웹 레이어 테스트

- 대상: 컨트롤러, 요청/응답 매핑, HTTP 상태 코드, JSON 응답
- 도구: `@WebMvcTest` + `MockMvc`
- 서비스 계층은 `@MockBean`으로 격리한다.
- JSON 응답 검증은 `jsonPath`를 사용한다.

### 영속성 테스트

- 대상: JPA 매핑, 쿼리 메서드, repository 동작
- 도구: `@DataJpaTest`
- 기본은 테스트 DB(H2)로 검증하되, PostgreSQL 차이가 중요한 경우 실제 PostgreSQL 기반 검증도 추가를 검토한다.

### 통합 테스트

- 대상: 애플리케이션 흐름 전체, 필터/예외 처리/응답 래핑 포함 경로
- 도구: `@SpringBootTest` + `@AutoConfigureMockMvc` + `@ActiveProfiles("test")`
- HTTP 엔드포인트 전체 흐름과 예외 변환을 함께 검증한다.

## 8. JUnit 5 작성 규칙

- 테스트 클래스 이름은 `*Test` 접미사를 사용한다.
- 테스트는 Arrange-Act-Assert 패턴으로 작성한다.
- 테스트 이름은 동작 중심으로 구체적으로 작성한다.
- 필요 시 `@DisplayName`으로 의도를 명확히 드러낸다.
- 공통 준비/정리는 `@BeforeEach`, `@AfterEach`를 사용한다.
- 클래스 단위 준비가 정말 필요할 때만 `@BeforeAll`, `@AfterAll`을 사용한다.
- 변형 입력 검증에는 `@ParameterizedTest`와 `@ValueSource`, `@CsvSource`, `@MethodSource`를 우선 검토한다.
- 예외 검증은 `assertThrows` 또는 AssertJ의 예외 assertion을 사용한다.
- 관련 assertion을 묶어야 할 때는 `assertAll`을 사용한다.

## 9. Assertion/Mocking 규칙

- 기본 assertion은 JUnit 5 assertion 또는 AssertJ를 사용한다.
- 가독성이 중요한 값/컬렉션/예외 검증은 AssertJ를 우선한다.
- 예외 메시지와 실패 사유가 드러나도록 assertion을 작성한다.
- 외부 의존성은 mock/stub으로 격리한다.
- 테스트 더블은 interface 경계 또는 repository/service 경계에서 명확히 나눈다.

## 10. 테스트 데이터 규칙

- 테스트 데이터는 의도를 드러내는 최소 값만 사용한다.
- 같은 객체 조합이 반복되면 테스트 데이터 빌더 또는 fixture 메서드를 만든다.
- 테스트마다 독립된 데이터를 준비하고, 이전 테스트의 생성 결과에 의존하지 않는다.

## 11. 구현 시 백엔드 체크리스트

- 문서 체인을 먼저 갱신했는지 확인한다.
- TODO/PLAN이 최신 버전을 참조하는지 확인한다.
- 새 API는 응답 래핑과 예외 정책을 따른다.
- 테스트를 먼저 작성했는지 확인한다.
- 단위/웹/통합 중 가장 좁은 테스트부터 선택했는지 확인한다.
- 변경 후 `./gradlew test`와 필요 시 `./gradlew bootRun` 또는 Docker 경로를 검증한다.

## 12. 검증 명령

- 로컬 테스트: `./gradlew test`
- 테스트 프로파일 실행: `./gradlew bootRun --args='--spring.profiles.active=test'`
- 로컬 PostgreSQL 실행: `./gradlew bootRun --args='--spring.profiles.active=local'`
- Docker 구성 검증: `cd infra/docker && docker compose -f docker-compose-local.yml --env-file .env config`

## 13. 참고 기준

- `github/awesome-copilot`의 `java-junit` 스킬에서 JUnit 5 구조, parameterized test, AAA, Mockito 규칙을 참고해 현재 프로젝트에 맞게 정리했다.
- `affaan-m/everything-claude-code`의 `springboot-tdd` 스킬에서 TDD 순서, 테스트 레벨, MockMvc/Testcontainers/JaCoCo 기준을 참고해 현재 스택으로 정리했다.
- `github/awesome-copilot`의 `spring-boot-testing` 스킬에서 테스트 피라미드와 slice 선택 원칙을 참고했다.
