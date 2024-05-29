# Result

- jOOQ의 `Result` 객체는 데이터베이스 쿼리의 결과를 나타내는 객체 <br/>
- `Result` 객체는 여러 레코드를 포함할 수 있고, 각 레코드는 `Record` 객체로 표현 <br/>
- `Result` 객체는 jOOQ의 `fetch()` 메서드를 사용하여 데이터베이스 쿼리를 실행한 후 반환 <br/>
- `Result` 객체로 집계 작업 수행 가능

<br/>

 `Result` 객체는 Java의 `List` 인터페이스를 구현하므로, Java의 일반적인 리스트 메서드를 사용할 수 있고, <br/>
jOOQ가 제공하는 다양한 유틸리티 메서드를 통해 결과를 쉽게 처리할 수 있다.

<br/>

`Result` 객체를 사용 기본 예제:

### 1. 데이터베이스 쿼리를 실행하고 `Result` 객체를 얻기

```java
public class MyRepository {

    public void fetchAndProcessResults() {
        // 쿼리 실행하고 Result 객체 얻기
        Result<Record> result = dslContext
            .select()
            .from("my_table")
            .fetch();

        // Result 객체를 처리
        for (Record record : result) {
            Integer id = record.getValue("id", Integer.class);
            String name = record.getValue("name", String.class);
            System.out.println("ID: " + id + ", Name: " + name);
        }
    }
}
```

### 2. `Result` 객체를 특정 타입의 리스트로 변환

```java
public class MyRepository {

    public List<MyBean> fetchAndConvertResults() {
        // 쿼리 실행하고 Result 객체 얻기
        Result<Record> result = dslContext
            .select()
            .from("my_table")
            .fetch();

        // Result 객체를 특정 타입의 리스트로 변환
        List<MyBean> list = result.into(MyBean.class);

        return list
    }
}
```

### 3. `Result` 객체를 사용하여 집계 작업 수행

```java
public class MyRepository {

    public void fetchAndAggregateResults() {
        // 쿼리 실행하고 Result 객체 얻기
        Result<Record> result = dslContext
            .select()
            .from("my_table")
            .fetch();

        // 집계 작업 수행
        int totalCount = result.size();
        int sumOfIds = result.sum("id", Integer.class);

        System.out.println("Total Count: " + totalCount);
        System.out.println("Sum of IDs: " + sumOfIds);
    }
}
```

### 요약

- `Result` 객체는 쿼리 결과를 나타내는 객체, 여러 레코드를 포함할 수 있음
- `Result` 객체는 `fetch()` 메서드를 통해 얻을 수 있음
- `Result` 객체는 Java의 `List` 인터페이스를 구현하므로, 리스트 메서드를 사용할 수 있음
- `Result` 객체를 특정 타입의 리스트로 변환하거나, 집계 작업을 수행할 수 있음
