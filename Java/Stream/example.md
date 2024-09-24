## 2장: Stream API의 구체적인 사용 예시와 중간 및 최종 연산


### 1. Stream API의 기본 구조

Java의 **Stream API**는 데이터를 일관된 방식으로 처리하기 위한 **데이터 처리 파이프라인**을 구축하는 데 중점을 둔다. 이 파이프라인은 **데이터 소스**에서 시작하여, **중간 연산**을 통해 데이터를 변환하고, **최종 연산**으로 결과를 반환하는 구조를 가진다.

Stream API의 기본 처리 흐름은 다음과 같다:

1. **데이터 소스** : 리스트, 배열, 파일, 컬렉션 등의 데이터를 Stream으로 변환한다.
   ```java
   List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
   Stream<String> nameStream = names.stream();

2. **중간 연산(Intermediate Operations)** : 스트림의 데이터를 변환하거나 필터링하는 단계로, 여러 번 체이닝이 가능하다. 중요한 특징은 이 연산들은 Lazy하다는 것. 즉, ***최종 연산이 호출되기 전까지*** 실제로 실행되지 않는다.
ex) filter(), map(), sorted()

3. **최종 연산(Terminal Operations)**: 최종적으로 스트림을 처리하고 결과를 반환하는 연산으로, 한 번만 호출할 수 있다. 이 연산이 호출되면 스트림이 닫히고, 더 이상 중간 연산을 추가할 수 없다.
ex) collect(), forEach(), reduce()

### 2. 중간 연산(Intermediate Operations)
중간 연산은 스트림을 변환하거나 필터링하며, 결과적으로 새로운 스트림을 반환한다. 중요한 중간 연산은 다음과 같다:

1. filter(Predicate<T> predicate)
filter()는 조건에 맞는 요소만을 필터링해 새로운 스트림을 생성한다. 조건은 Predicate 함수형 인터페이스로 전달되며, 해당 조건을 만족하는 요소들만 통과된다.
    ```java
    List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
    List<Integer> evenNumbers = numbers.stream()
                                    .filter(n -> n % 2 == 0)
                                    .collect(Collectors.toList());
    System.out.println(evenNumbers); // [2, 4]
    ```

2) map(Function<T, R> mapper)
map()은 각 요소를 매핑하여 다른 형태로 변환하는 역할을 한다. 이때 변환은 Function 인터페이스로 정의된 변환 함수를 사용한다.

    ```java
    List<String> names = Arrays.asList("alice", "bob", "charlie");
    List<String> upperCaseNames = names.stream()
                                    .map(String::toUpperCase)
                                    .collect(Collectors.toList());
    System.out.println(upperCaseNames); // [ALICE, BOB, CHARLIE]
    ```

3) sorted()
sorted()는 스트림의 요소들을 정렬한다. 정렬 기준을 제공하지 않으면 기본적으로 자연 순서(Comparable 구현)에 따라 정렬된다. 또는 Comparator를 전달하여 사용자 정의 정렬 기준을 사용할 수도 있다.

    ```java
    List<String> names = Arrays.asList("Charlie", "Bob", "Alice");
    List<String> sortedNames = names.stream()
                                    .sorted()
                                    .collect(Collectors.toList());
    System.out.println(sortedNames); // [Alice, Bob, Charlie]
    ```

4) distinct()
distinct()는 중복을 제거하여 고유한 요소만을 포함하는 스트림을 반환한다.
    ```java
    List<Integer> numbers = Arrays.asList(1, 2, 2, 3, 4, 4, 5);
    List<Integer> distinctNumbers = numbers.stream()
                                        .distinct()
                                        .collect(Collectors.toList());
    System.out.println(distinctNumbers); // [1, 2, 3, 4, 5]
    ```

5) limit(long maxSize)
limit()은 스트림에서 지정된 수만큼의 요소를 반환하는 데 사용된다.
    ```java
    List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
    List<Integer> limitedNumbers = numbers.stream()
                                        .limit(3)
                                        .collect(Collectors.toList());
    System.out.println(limitedNumbers); // [1, 2, 3]
    ```
### 3. 최종 연산(Terminal Operations)
최종 연산은 스트림의 마지막 단계로, 스트림을 처리한 후 결과를 반환한다. 스트림을 최종적으로 닫고, 더 이상 중간 연산을 적용할 수 없게 된다.

1) collect(Collector<T, A, R> collector)
collect()는 최종 연산 중 가장 자주 사용되며, 스트림의 결과를 리스트, 맵, 집합 등 다양한 형태로 수집할 수 있다. 이때 Collector를 통해 어떻게 수집할지 정의할 수 있다.
    ```java
    List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
    List<String> collectedNames = names.stream()
                                    .collect(Collectors.toList());
    System.out.println(collectedNames); // [Alice, Bob, Charlie]
    ```

2) forEach(Consumer<T> action)
forEach()는 각 요소에 대해 지정된 동작을 수행한다. 이 연산은 스트림을 소모하며, 반환값이 없다.
    ```java
    List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
    names.stream()
        .forEach(name -> System.out.println(name));
    // Alice
    // Bob
    // Charlie
    ```

3) reduce(BinaryOperator<T> accumulator)
reduce()는 스트림의 요소를 하나의 값으로 집계한다. 예를 들어, 모든 요소를 더하거나 곱하는 방식으로 사용할 수 있다.
    ```java
    List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
    int sum = numbers.stream()
                    .reduce(0, Integer::sum);
    System.out.println(sum); // 15
    ```

4) count()
count()는 스트림의 요소 개수를 반환한다.
    ```java
    List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
    long count = names.stream()
                    .count();
    System.out.println(count); // 3
    ```

5) anyMatch(), allMatch(), noneMatch()
이들 연산은 스트림의 요소가 특정 조건을 만족하는지 여부를 확인하는 데 사용된다. anyMatch()는 조건을 만족하는 요소가 하나라도 있는지, allMatch()는 모두 만족하는지, noneMatch()는 하나도 만족하지 않는지를 확인한다.
    ```java
    List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
    boolean anyEven = numbers.stream()
                            .anyMatch(n -> n % 2 == 0);
    ```System.out.println(anyEven); // true

### 4. 결론
Java Stream API는 선언적 프로그래밍을 가능하게 하고, 데이터를 변환, 필터링, 집계하는 다양한 연산을 통해 복잡한 데이터 처리 작업을 간결하고 효율적으로 수행할 수 있도록 한다. 중간 연산을 통해 데이터 파이프라인을 구성하고, 최종 연산을 통해 결과를 수집하거나 출력하는 방식은 데이터 처리의 새로운 패러다임을 제시한다. 현업에서의 Stream API 활용은 데이터 처리의 간소화, 가독성 향상, 그리고 병렬 처리를 통한 성능 최적화에 큰 기여를 한다.
