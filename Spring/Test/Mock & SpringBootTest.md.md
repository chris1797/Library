# Mock Test vs @SpringBootTest

Mock 테스트와 `@SpringBootTest` 는 테스트 범위, 목적에 따라 사용하는 방식이 다르다. <br/>
Mock 테스트는 개별적인 단위 테스트를 위한 방법이고, `@SpringBootTest` 는 애플리케이션 전체를 로드하여 통합테스트를 하기 위한 수단이다.

<br/>

## 1. Mock 테스트
Mock 테스트는 개별 클래스나 메서드를 테스트하기 위해 외부 의존성을 `모킹(Mocking)` 하여 단위 테스트를 수행하는 방식.

특징
---
- 의존성 분리 : 실제 구현체 대신 `Mock`  객체를 사용하여 의존성을 분리한다.
- 빠른 실행 : `@SpringBootTest` 와는 달리 실제 애플리케이션을 전체 로드하지 않기 때문에 테스트가 빠르게 실행된다.
- Mockito 활용 : 주로 `Mockito` 와 같은 라이브러리를 사용하여 Mock 객체를 생성하고 테스트한다.

> @ExtendWith(MockitoExtension.class) : JUnit 5 <br/>
> @RunWith(MockitoJUnitRunner.class) : JUnit 4 <br/>

```java

@ExtendWith(MockitoExtension.class)
public class UserServiceTest {

  @Mock
  private UserRepository userRepository; // Mock 객체 생성

  @InjectMocks
  private UserService userService; // Mock 객체를 주입받은 테스트 대상

  @Test
  void test() {
    // Given: 테스트 대상 Mock 의 동작을 정의
    when(userRepository.findById(1L)).thenReturn(Optional.of(new User(1L, "John")));
    
    // When: 테스트 대상 호출
    User user = userService.findUserById(1L);
    
    // Then: 검증
    assertEquals("Chris", user.getName());
    verify(userRepository).findById(1L);
}

```


** MockitoExtension 없이도 Mock 객체를 수동으로 생성, 초기화가 가능하지만 이를 위한 코드량이 많아질 수 밖에 없다. <br/>
`Mockito` 는 `Mocking` 을 간단하고 일관성 있게 처리할 수 있기 때문에 유용하다.


<br/>

---

<br/>

## 2. @SpringbootTest

`@SpringBootTest` 는 애플리케이션 컨텍스트를 통합적으로 로드하여 애플리케이션 전체의 동작을 테스트한다. <br/>
이를 통해 Service Layer, Repository, Controller 간의 통합 테스트를 수행할 수 있다.

특징
---
- 전체 컨텍스트 로드 : 애플리케이션의 모든 Bean을 로드하여 통합 테스트 환경을 구성한다. 테스트 로드에 Mock 테스트보다 확실히 시간이 걸린다.
- 실제 구현 사용 : Mock 객체 대신 진짜 구현체를 사용하고, 테스트용 데이터베이스 등을 구성할 수도 있다.

```java
// 예시 코드 
@SpringBootTest
@ActiveProfiles("test") // 테스트 프로파일 활성화 (예: application-test.yml 사용)
public class UserServiceIntegrationTest {

  @Autowired
  private UserService userService; // 실제 서비스 bean
}
```

<br/>

---

<br/>

## 둘의 차이점 비교

| **특징**               | **Mock 테스트**                                  | **@SpringBootTest**                            |
|------------------------|-----------------------------------------------|-----------------------------------------------|
| **목적**               | 단위 테스트 (개별 클래스나 메서드 테스트)         | 통합 테스트 (전체 애플리케이션 동작 테스트)      |
| **애플리케이션 컨텍스트** | 로드하지 않음                                    | 전체 컨텍스트를 로드                           |
| **의존성**             | Mock 객체 사용                                   | 실제 구현체 사용                                |
| **속도**               | 빠름                                           | 느림                                           |
| **테스트 범위**         | 개별 클래스 또는 메서드                         | 애플리케이션 전반                              |
| **사용 도구**          | Mockito, @Mock, @InjectMocks                   | @SpringBootTest, @ActiveProfiles               |

<br/>

---

<br/>

## 언제 사용할까?
  
### Mock Test
- 특정 클래스의 로직만 테스트하고 싶을 때.
- 외부 의존성(DB, 네트워크 등)을 분리해서 테스트하고 싶을 때.
- 빠른 테스트

### @SpringBootTest
- 애플리케이션의 전체 동작을 테스트.
- 여러 계층간 (Service, Repository, Controller..) 상호 작용에 대한 검증이 필요할 때.
- 실제 `Bean` 을 사용한 테스트가 필요할 때.

<br/>

---

<br/>



번외) @SpringBootTest 와 Mocking Test 의 조합
---
상황에 따라 둘을 조합해서 사용도 가능하다. 예를 들어 통합 테스트에서 특정 `Bean` 을 `Mocking` 할 때.

```java
@SpringBootTest
public class UserIntegrationTest {

  @MockBean
  private UserRepository userRepository; // 특정 빈을 Mock으로 대체

  @Autowired
  private UserService userService;

  @Test
  void test() {
    // userService가 주입받는 userRepository는 모킹된 객체
  }
}
```
