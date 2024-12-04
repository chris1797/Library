# Transaction Level
트랜잭션 격리 수준이란, 여러 트랜잭션이 동시에 처리될 때 특정 트랜잭션이 다른 트랜잭션에서 변경하거나 조회하는 데이터를 어디까지 볼 수 있게 허용할지를 결정하는 것이다. 아래로 갈수록 고립 정도가 높아지고 동시 처리 성능은 떨어지는 것이 일반적이라 볼 수 있다.

<br/>

READ UNCOMMITTED
---
: READ UNCOMMITTED 격리 수준에서는 각 트랜잭션에서의 변경 내용이 `COMMIT` 이나 `ROLLBACK` 여부에 상관없이 다른 트랜잭션에서 보인다. <br/>
A에서 commit 하지도 않은 변경 내용이 B에서 가져다 쓸 수 있다. 만에 하나 A의 트랜잭션이 롤백된다고 해도 B는 변경했었던 데이터 계속 쓰고 있을 것이다.

이처럼 어떤 트랜잭션에서 처리한 작업이 완료되지 않았는데 다른 트랜잭션에서 볼 수 있는 현상을 더티 리드(Dirty read)이다. <br/>
이 `Dirty read` 를 허용하는게 READ UNCOMMITTED.

RDBMS 표준에서는 트랜잭션 격리 수준으로 인정하지도 않을 정도라고 하니 그냥 쓰지 않으면 된다.

<br/>

READ COMMITTED
---
Oracle db에서는 기본으로 사용되는 레벨. `READ UNCOMMITTED` 와는 달리 commit이 완료된 데이터만 다른 트랜잭션에서 조회할 수 있기 때문에 `Dirty read` 는 일어날 일이 없다.
> A 트랜잭션이 값을 변경했다면 원래 데이터는 언두 영역에 백업된다. B 가 조회했을 때는 이 언두 영역에 백업된 레코드를 조회하게 되기 때문에 `Dirty read` 를 피할 수 있게 된다.


하지만 READ COMMITTED 수준에서도 `NON-REPEATABLE READ` 라는 문제가 있다. <br/>
해당 문제가 별 이슈가 아닌 서비스들도 물론 있겠지만 분명 문제가 되는 서비스도 있을 수 있다.
> `NON-REPEATABLE READ` ?? <br/>
> : B 트랜잭션 시작 후 name 조회 ➔ A 트랜잭션이 name 값을 업데이트 후 commit ➔ B 가 같은 트랜잭션 안에서 name을 다시 조회할 경우 값이 달라짐.

