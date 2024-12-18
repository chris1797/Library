# Reflection 이란?

> 자바 Reflection은 런타임에 클래스, 메서드, 필드 등의 정보를 조회하고 조작할 수 있게 해주는 기능. <br/>
> 이를 통해 컴파일 시점에 알 수 없는 클래스의 정보를 동적으로 다룰 수 있다.

<br/>

## Reflection 주요 기능
- 런타임에 클래스 정보 조회
- 객체 동적 생성
- 메서드 동적 호출
- 필드 접근 및 수정

<br/>

## 활용 예시
- Spring : 의존성 주입 및 어노테이션 처리
- JPA : 엔티티 매핑
- Jackson, GSON : JSON 직렬화/역직렬화
- 어노테이션 기반 기능 구현
```java

// Reflection을 사용하여 @NotEmpty 어노테이션이 붙은 필드 검증
@NotEmpty
private String name;

```
 <br/>
 
__** Reflection은 강력한 기능이지만, 성능 저하와 보안 위험이 있다.__
