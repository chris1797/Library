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

<br/>

REPEATABLE READ
---
MySQL InnoDB 스토리지 엔진에서 기본으로 사용되는 격리 수준. 위의 `NON-REPEATABLE READ` 이 발생하지 않는다. 

위에서 언급했듯이 InnoDB는 롤백 가능성에 대비해 <ins>변경되기 전 레코드를 언두(Undo) 공간에 백업해두고 실제 레코드를 변경</ins>한다. 이러한 변경 방식을 `MVCC(Multi Version Concurrency Control)` 라고 하는데, `REPEATABLE READ` 에서는 이 MVCC 를 위해서 언두 영역에 백업된 이전 데이터를 이용해 동일 트랜잭션에 동일한 결과를 보여줄 수 있게 된다.

사실 `READ COMMITTED` 도 MVCC 를 이용해 commit 되기 전의 데이터를 보여주는데, `REPEATABLE READ` 와의 차이는 <ins>언두 영역 백업된 레코드의 여러 버전 중에 어느 버전 이전까지 찾아 들어갈거냐의 차이다.</ins>

트랜잭션들에는 고유의, 순차적인 트랜잭션 번호가 있다. `REPEATABLE READ` 에서는 내 트랜잭션이 10번이라면, 10번보다 작은 트랜잭션 번호에서 변경한 것만 보게 된다.

<br/>

SERIALIZABLE
---
가장 단순하면서 엄격한 격리 수준, 그만큼 동시 처리도 떨어진다. 

InnoDB 에서는 기본적으로 순수한 조회(select) 작업에는 잠금이 설정되지 않는다. 하지만 `SERIALIZABLE` 레벨은 조회 작업에서도 `읽기 잠금`을 획득해야만 하고, 동시에 다른 트랜잭션은 해당 레코드를 변경하지 못한다.

단순히 한 트랜잭션이 읽거나 쓰고 있는 레코드는 다른 트랜잭션에서 절대 접근할 수가 없다는 말이다.
