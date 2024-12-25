# Eager, Lazy Fetch (즉시 로딩과 지연 로딩)

JPA에서 `EAGER`와 `LAZY` fetch는 엔티티의 연관 관계를 로드할 때 데이터를 가져오는 방식(fetch strategy)을 설정하는 옵션이다. <br/>
이 전략은 연관된 엔티티 데이터를 불러오는 시점을 결정한다.

<br/>

## Lazy (지연 로딩)

- 연관된 엔티티 데이터를 실제로 사용할 때 조회한다.
-  연관된 엔티티는 프록시 객체로 로드되고, 필요할 때 데이터베이스에서 데이터를 가져온다.
- 연관된 엔티티를 사용하는 시점마다 추가 쿼리가 실행될 수 있어 `N+1 문제`가 발생할 수 있음.

```java
@Entity
public class Parent {
    @OneToMany(mappedBy = "parent", fetch = FetchType.LAZY)
    private List<Child> children;
}

// children 컬렉션은 프록시 객체로 로드되며, 실제 접근 시점에 데이터베이스에서 조회됨
```

<br/>

## Eager (즉시 로딩)

- 연관된 엔티티 데이터를 즉시 로드되며, 엔티티가 조회될 때 연관된 데이터도 함께 조회된다.
- 연관 데이터를 즉시 사용할 수 있고, 쿼리 호출 횟수를 줄여 단순한 연관 관계에서 성능이 유리할 수 있다.
- 연관된 데이터가 많을 경우 불필요한 데이터 로드로 인해 성능 저하가 발생될 수 있다.
- 여러 계층의 연관 관계가 있으면 복잡한 조인 쿼리가 생성되기 때문에 쿼리 최적화가 어렵다.

```java
@Entity
public class Parent {
    @OneToMany(mappedBy = "parent", fetch = FetchType.EAGER)
    private List<Child> children;
}

// children 컬렉션이 즉시 로드되어, Parent 엔티티 조회 시 연관된 데이터도 함께 로드됨
```

<br/>

---

<br/>

## 기본값
> `@OneToOne`, `@ManyToOne` : EAGER <br/>
> `@OneToMany`, `@ManyToMany` : LAZY
