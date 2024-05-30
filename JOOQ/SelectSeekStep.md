# SelectSeekStep

- `SelectSeekStep` 는 jOOQ에서 사용되는 인터페이스 중 하나로, 특정 정렬 순서로 정렬된 데이터를 쿼리하는 데 사용되며, 주로 `ORDER BY` 절과 관련된 작업을 수행할 때 사용
  
- `SelectSeekStep` 인터페이스는 여러 단계의 인터페이스로 나뉘며, 정렬된 데이터 페이징하거나 순서를 지정할 수 있는 방법 제공 <br/> 
예를 들어, `SelectSeekStep1`, `SelectSeekStep2` 등은 각각의 단계에서 다양한 정렬 기준을 정의할 수 있는 메서드를 포함

- `SelectSeekStep` 인터페이스는 주로 orderBy 메서드를 호출한 후 반환되며, 추가로 seek 메서드를 통해 페이징을 위한 커서 기반의 조회를 수행할 수 있음 (Cursor Paging)

<br/>

### 사용 예제

아래는 jOOQ를 사용하여 `SelectSeekStep`을 이용해 데이터를 정렬하고 페이징하는 예제

#### Java 코드 예제

```java
public class MyRepository {

    /* --- DI 생략 --- */

    // 예제 1: SelectSeekStep1 사용 (단일 정렬 기준)
    public List<Record> fetchSortedData(int limit, int offset) {
        // orderBy를 사용하여 SelectSeekStep1 반환
        SelectSeekStep1<Record, Integer> step = dslContext.select()
                                                    .from("my_table")
                                                    .orderBy(field("age").asc());
        // limit와 offset을 사용하여 페이징
        return step.limit(limit)
                   .offset(offset)
                   .fetch();
    }

    // 예제 2: SelectSeekStep2 사용 (복합 정렬 기준)
    public List<Record> fetchSortedDataWithTwoCriteria(int limit, int offset) {
        // orderBy를 사용하여 SelectSeekStep2 반환
        SelectSeekStep2<Record, Integer, String> step = dslContext.select()
                                                            .from("my_table")
                                                            .orderBy(field("age").asc(), field("name").desc());
        // limit와 offset을 사용하여 페이징
        return step.limit(limit)
                   .offset(offset)
                   .fetch();
    }

    // 예제 3: SelectSeekStep2 및 seek 사용 (커서 기반 페이징)
    public List<Record> fetchSortedDataWithSeek(int age, String name, int limit) {
        // orderBy를 사용하여 SelectSeekStep2 반환
        SelectSeekStep2<Record, Integer, String> step = dslContext.select()
                                                                  .from("my_table")
                                                                  .orderBy(field("age").asc(), field("name").desc());
        // seek와 limit을 사용하여 커서 기반 페이징
        return step.seek(age, name)
                   .limit(limit)
                   .fetch();
    }
}

```

<br/>

## `SelectSeekStep`와 관련된 메서드

jOOQ에서는 `SelectSeekStep` 인터페이스를 통해 여러 단계의 메서드를 제공하여 정렬된 데이터를 쿼리할 수 있다.

- `orderBy(Field<?>... fields)` : 결과를 특정 필드에 따라 정렬
- `limit(int numberOfRows)` : 반환할 행의 수를 제한
- `offset(int numberOfRows)` : 반환할 행의 시작 위치를 지정
- `seek(...)` : 특정 값에 대한 페이징을 제공. 예를 들어 특정 값 이후의 레코드를 반환 (ex. TimeStamp, createAt, regDate.. 같은 순차적인 데이터)

<br/>

### 예제 확장: `seek` 메서드 사용

```java

public class MyRepository {

    public List<Record> fetchSortedDataWithSeek(int age, String name, int limit) {
        // 특정 값 이후의 레코드를 정렬 기준에 따라 가져오는 예제
        return dslContext.select()
                         .from("my_table")
                         .orderBy(DSL.field("age").asc(), DSL.field("name").desc())
                         .seek(age, name)
                         .limit(limit)
                         .fetch();
    }
}
```

위의 예제에서는 `seek` 메서드를 사용하여 특정 값 이후의 데이터를 페이징하는 방법을 보여준다. `seek` 메서드는 주로 커서 기반의 페이징을 구현할 때 유용하다.

<br/>

## 요약

- `SelectSeekStep` 인터페이스는 jOOQ에서 `ORDER BY` 절과 관련된 작업을 수행하는 데 사용
- 다양한 정렬 기준을 정의하고 페이징 기능을 제공하는 메서드를 포함
- 정렬된 데이터를 페이징하거나 특정 값 이후의 데이터를 가져올 때 유용하게 사용할 수 있음
