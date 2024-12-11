# toMap() 과 groupingBy()

`Collectors.toMap()`와 `Collectors.groupingBy()`는 모두 Java Stream API에서 컬렉션 데이터를 Map으로 변환할 때 사용하는 Collector이다.
하지만, 언제 어떤 경우에 사용해야 하는지에 대한 개념이 부족하여 정리해본다. 둘은 목적과 사용 방식에서 차이가 있다.

<br/>

Collectors.toMap()
---
목적 : 데이터를 고유한 key-value 로 구성된 Map 으로 전환하고 싶을 때.

- 데이터를 key-value 로 매핑하여 Map 을 생성한다.
- 중복 키가 발생하면 예외를 던지지만 별도의 해결방법을 정해줄 수 있다.
- 생성할 Map 의 구현체를 지정해줄 수 있다.

```java
// 파라미터 구성
Collectors.toMap(
  key 를 생성하는 함수,
  value 를 생성하는 함수,
  중복키 처리 함수,
  생성할 Map<> 제공자
)

List<BenefitInfo> benefitList = ~~;

// benefitList를 내부 필드 중 gradeCode enum을 key, benefitInfo 자체를 value로 매핑
benefitList.stream()
        .collect(Collectors.toMap(
            info -> UserCode.UserGrade.valueOf(info.getGradeCode()), // Key
            info -> info, // Value
            (info1, info2) -> info1, // 중복된 key가 있을 경우 첫 번째 값으로
            () -> new EnumMap<>(UserCode.UserGrade.class)
        ));
```


<br/>

Collectors.groupingBy()
---
목적 : 데이터를 특정 기준으로 그룹화하고, 각 그룹의 값을 별도로 처리하거나 집계하는 용도로 사용함
- 데이터를 그룹화하여 Map으로 구성함
- 기본적으로 각 key에 해당하는 값들이 List 형태로 그룹화됨, 그룹화된 List를 별도 Collector 로 추가적인 처리

```java
Collectors.groupingBy(
  그룹화 기준이 되는 key 를 생성하는 함수,
  생성할 Map<> 제공자,
  value에 해당하는, 그룹화된 값에 추가 적으로 적용할 Collector(선택)
)

List<UserItem> userItems = ~~;

Map<String, Integer> itemCountMap = userItems.stream()
        .collect(Collectors.groupingBy(
                UserItemResponse::getItemType, // key (그룹화 기준)
                () -> new EnumMap<>(Product.ProductCode.class), // 결과 Map 구현체
                Collectors.summingInt(UserItemResponse::getCount))); // 그룹화된 값 집계
```
