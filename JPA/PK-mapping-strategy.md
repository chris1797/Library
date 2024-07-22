# 기본 키 매핑 전략

JPA가 제공하는 DB 기본 키 생성 전략은 다음과 같다.

- 직접 할당 : 기본 키를 애플리케이션에서 직접 할당
- 자동 생성 : 대리 키 사용 방식
  - IDENTITY : 기본 키 생성을 DB에 위임
  - SEQUENCE : DB 시퀀스를 사용해서 기본 키 할당
  - TABLE : 키 생성 테이블 사용

<br/>

## 기본키 직접 할당 전략

: 기본 키 직접 할당 전략은 `em.persist()`로 엔티티를 저장하기 전에 애플리케이션에서 기본 키를 직접 할당하는 방법이다.

```java
@ID
@Column(name = "id")
private String id;

-------------------------------------

Board board = new Board();
board.setId("id1"); // 기본 키 직접 할당
em.persist(board);
```

### @Id 적용 가능 자바 타입

- 자바 기본형
- 자바 wrapper 형
- String
- java.util.Date
- java.sql.Date
- java.math.BigDecimal
- java.math.BigInteger

<br/>

## IDENTITY 전략

: 기본 키 생성을 DB에 위임하는 전략이다. 주로 MySQL, Postgre, SQL Server, DB2에서 사용한다.
DB에 엔티티를 저장해서 식별자 값을 획득한 후, 영속성 컨텍스트에 저장한다.
(IDENTITY 전략은 테이블에 데이터를 저장하고 나서야 식별자 값을 획득할 수 있다.)

```java
@Entity
public class Board {

  @Id
  @GenerateValue(strategy = GenerationType.IDENTITY)
  private Long id;
}
```

IDENTITY 전략은 ID가 `AUTO_INCREMENT` 로 지정된 테이블 처럼 DB에 값이 저장되고 나서야 기본 키 값을 구할 수 있을 때 사용한다.

개발자가 엔티티에 직접 식별자를 할당할 경우에는 그냥 `@Id` 어노테이션만 있으면 되지만 `AUTO_INCREMENT` 처럼 식별자를 DB에서 생성해주는 경우에는 `@GeneratedValue`를 사용하고 식별자 생성 전략을 선택해줘야 한다.

### ⚠️ 주의사항

엔티티가 영속 상태가 되려면 식별자가 반드시 필요하다. 앞서 말했듯이 IDENTITY 전략은 엔티티를 DB에 저장해야지 식별자를 구할 수 있기 때문에 `em.persist()`를 호출하는 즉시 insert 쿼리가 날아간다. 따라서 <u>IDENTITY 전략은 쓰기 지연이 동작하지 않는다.</u>

<br/>

## SEQUENCE 전략

: 유일한 값을 순서대로 생성하는 DB의 시퀀스를 사용해서 기본 키를 생성하는 전략.
시퀀스를 지원하는 오라클, Postgre, DB2, H2에서 사용 가능하다.

```java
@Entity
@SequenceGenerator (
  name = "BOARD_SEQ_GENERATOR",
  sequenceName = "BOARD_SEQ", // 매핑시킬 DB의 시퀀스 이름
  initialValue = 1, allocationSize = 1) // allocationSize의 기본값이 50인 것에 주의.
public class Board {

  @ID
  @GeneratedValue(strategy = GenerationType.SEQUENCE,
                  generator = "BOARD_SEQ_GENERATOR")
  private Long id;
}
```

`@SequenceGenerator` 를 이용해서 시퀀스 생성기를 등록 후 속성 이름으로 `BOARD_SEQ`를 지정한다.
JPA는 이 속성 이름으로 DB의 시퀀스와 매핑시킨다.

시퀀스를 사용하는 코드는 IDENTITY 전략과 비슷하지만 내부 동작 방식이 다른데, SEQUENCE 전략은 `em.persist()` 를 호출할 때 먼저 DB 시퀀스를 사용해서 식별자를 조회한다.

그리고 조회한 식별자를 엔티티에 할당한 후에 엔티티를 영속성 컨텍스트에 저장한다. 그러면 트랜잭션이 커밋 후 플러시가 일어나면 엔티티를 DB에 저장한다.

<u>먼저 엔티티를 저장한 후에 식별자를 조회하는 IDENTITY 전략과는 식별자 조회 시점이 상반되는 점이다.</u>
