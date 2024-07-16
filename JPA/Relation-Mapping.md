# JPA Relation Mapping (연관관계 매핑)

JPA에서 객체와 관계형 데이터베이스 테이블이 어떻게 매핑되는지 이해하는 것이 가장 중요하다고 볼 수 있다.

<br/>

## 연관관계의 주요 포인트

- 어떤 방향으로 매핑할 것인가? : 단방향, 양방향
- 연관 관계의 주인은 누구인가? : 관리 주체, 주인이 아닌 entity는 조회만 가능함. 자식에서 mappedBy로 주인을 명시.
- 관계 모델은 어떻게 정할 것인가? : **N:1**, **1:N**, **1:1**

<br/>

❗ 실무에서 1:N 단방향, 양방향은 쓰지 않음.

<br/>

## 다대일 N:1

### - N:1 단방향

```java
@Entity
public class Board {
    @Id @GeneratedValue
    @Column(name = "BOARD_ID")
    private Long id;

    private String title;

    @ManyToOne // N:1 단방향에서는 'N'쪽인 Board에서 @ManyToOne 추가
    @JoinColumn(name = "USER_ID")
    private User user;
}

@Entity
public class User {
    @Id @GeneratedValue
    private Long id;

    private String name;

    // 단방향이므로 User는 아무것도 참조하지 않음
```

### - N:1 양방향

```java
@Entity
public class Board {
    @Id @GeneratedValue
    @Column(name = "BOARD_ID")
    private Long id;

    private String title;

    @ManyToOne // 단방향과 마찬가지로 'N'쪽인 Board에서 @ManyToOne 추가
    @JoinColumn(name = "USER_ID")
    private User user;
}

@Entity
public class User {
    @Id @GeneratedValue
    private Long id;
    private String name;

    // N:1의 '1'쪽에 @OneToMany 추가, mappedBy로 연관관계의 주인 지정
    // 대상 객체 참조하는 변수명을 선언하면 됨 (Board의 user 변수)
    @OneToMany(mappedBy = "user")
    List<Board> boards = new ArrayList<>();
```

<br/>

## 일대일 1:1
한 개의 게시판(Board)에 한 개의 이미지(Image)만 올릴 수 있다고 가정.

### 1:1 단방향
주인이 되는 Board가 FK로 Image의 PK를 가짐
```java
@Entity
public class Board {
    @Id @GeneratedValue
    @Column(name = "BOARD_ID")
    private Long id;

    private String title;

    @OneToOne // N:1과 마찬가지로 관리주체의 대상이 되는 객체에서 @OneToOne 선언
    @JoinColumn(name = "IMAGE_ID")
    private Image image;
}

@Entity
public class Image {
    @Id @GeneratedValue
    private Long id;

    private String name;
```

### 1:1 양방향

```java
@Entity
public class Board {
    @Id @GeneratedValue
    @Column(name = "BOARD_ID")
    private Long id;

    private String title;

    @OneToOne
    @JoinColumn(name = "IMAGE_ID")
    private Image image;
}

@Entity
public class Image {
    @Id @GeneratedValue
    private Long id;

    private String name;

    @OneToOne(mappedBy = "image")
    private Board board;
```
