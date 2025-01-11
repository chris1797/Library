# IntStream

IntStream은 Java 8의 스트림 API에서 제공하는 특화된 스트림으로, 기본 타입 int를 처리하는 데 최적화되어 있다. <br/> 
박싱(Boxing)을 피하고 성능을 높이기 위해 설계되었으며, 컬렉션, 배열, 범위 등에서 int 데이터를 효율적으로 처리할 수 있다.

## 주요 특징
	1.	기본 타입 int 처리:
	•	오토박싱/언박싱 없이 int 타입을 직접 처리한다.
	•	성능상 이점이 있음.
	2.	연속적인 데이터 처리:
	•	람다 표현식과 스트림 연산을 사용하여 데이터를 선언적이고 간결하게 처리.
	3.	종료 연산:
	•	스트림의 데이터 흐름은 종료 연산(예: sum, average, forEach)에서 실제로 수행된다.
	4.	Immutable(불변성):
	•	스트림을 사용해도 원본 데이터는 변경되지 않는다.



### 생성 메서드

- IntStream.of(int... values)	주어진 값을 스트림으로 변환.
- IntStream.range(int startInclusive, int endExclusive)	시작값에서 끝값(제외)까지 정수 스트림 생성.
- IntStream.rangeClosed(int startInclusive, int endInclusive)	시작값에서 끝값(포함)까지 정수 스트림 생성.
- Arrays.stream(int[] array)	배열을 IntStream으로 변환.
- IntStream.generate(IntSupplier)	무한 스트림 생성.
- IntStream.iterate(int seed, IntUnaryOperator)	초기값(seed)에서 시작하여 연속적으로 값을 생성.


### 중간 연산

- filter(IntPredicate)	조건에 맞는 요소만 포함.
- map(IntUnaryOperator)	요소를 변환.
- distinct()	중복 제거.
- sorted()	요소를 정렬.
- limit(long maxSize)	최대 n개의 요소만 반환.
- skip(long n)	처음 n개의 요소를 건너뜀.

### 종료 연산

- sum()	요소의 합을 반환.
- average()	요소의 평균을 반환.
- min() / max()	최소값 또는 최대값을 반환.
- count()	요소의 개수를 반환.
- toArray()	스트림의 모든 요소를 배열로 반환.
- forEach(IntConsumer)	각 요소에 대해 작업을 수행.
- reduce(int identity, IntBinaryOperator)	누적 합산이나 다른 집계 작업 수행.



### 요약
	•	IntStream은 int 타입 데이터를 처리하기 위한 스트림.
	•	박싱을 피하므로 성능이 우수.
	•	범위 처리, 필터링, 집계 등 다양한 작업을 간단히 수행 가능.
	•	중간 연산과 종료 연산을 조합하여 데이터를 처리한다.