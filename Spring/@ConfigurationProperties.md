

@ConfigurationProperties
---
- Spring 환경 설정 값(application.yml 같은 것들)을 POJO 로 바인딩하기 위해 사용한다.
- 주로 애플리케이션 설정을 명시적이고 Type safe하게 관리하기 위해 사용된다.

<br/>

@EnableConfigurationProperties
---
- `@ConfigurationProperties` 로 정의된 클래스의 빈을 Spring Context에서 사용할 수 있도록 등록한다.
- 일반적으로, `@EnableConfigurationProperties` 는 애플리케이션 초기화 시점에 `@ConfigurationProperties` 클래스를 스프링 빈으로 등록해준다.
