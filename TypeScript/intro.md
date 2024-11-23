type과 interface는 TypeScript에서 타입을 정의하는데 사용되며, 비슷한 기능을 제공하지만 몇 가지 차이점이 있다.

# 1. type과 interface의 공통점

둘 다 객체 타입을 정의할 때 사용하며, 대부분의 경우 서로 교환 가능하다.

₩₩₩
// 객체 타입 정의
type PersonType = {
  name: string;
  age: number;
};

interface PersonInterface {
  name: string;
  age: number;
}
₩₩₩

2. type과 interface의 차이점

A. 확장(Extend) 및 결합

	•	interface: 상속(extends)을 통해 확장 가능.
	•	type: 교차 타입(&)을 사용해 결합 가능.

// interface 상속
interface Animal {
  name: string;
}
interface Dog extends Animal {
  breed: string;
}

// type 교차
type Animal = {
  name: string;
};
type Dog = Animal & {
  breed: string;
};

B. 유니온 타입 지원

	•	type만 지원: 유니온 타입과 튜플을 정의할 때는 type만 사용 가능.

type Status = "success" | "error" | "loading";

type Coordinate = [number, number];

C. 선언 병합

	•	interface만 지원: 같은 이름의 인터페이스를 여러 번 선언하면 자동으로 병합됨.

interface User {
  name: string;
}
interface User {
  age: number;
}
// 병합: { name: string; age: number; }

type은 동일 이름으로 선언 시 오류가 발생한다.

3. 사용 가이드

interface를 사용하는 경우

	•	객체의 구조를 정의할 때 기본적으로 interface를 사용.
	•	확장 가능성이 중요할 때 사용.
	•	라이브러리와의 호환성(예: React의 props)을 고려할 때.

interface Person {
  name: string;
  age: number;
}

type을 사용하는 경우

	•	유니온 타입이나 기본 타입 조합이 필요할 때.
	•	복잡한 타입 조합이 필요한 경우.
	•	튜플, 함수 시그니처 정의 시.

type ID = string | number;

type Position = [number, number];

type Callback = (input: string) => void;

4. 실무적인 권장 사항

	1.	객체 타입 정의는 interface를 기본으로 사용:
	•	선언 병합과 확장성을 활용 가능.
	•	팀 협업에서 더 직관적으로 읽히는 경우가 많음.
	2.	type은 유니온, 튜플, 또는 함수 정의:
	•	객체 외의 복잡한 타입 조합에 적합.
	3.	복잡한 타입 정의가 필요한 경우 type으로 시작하되, 필요 시 interface로 전환.

5. 예제 코드

// 인터페이스로 객체 정의
interface Car {
  make: string;
  model: string;
}

interface ElectricCar extends Car {
  batteryCapacity: number;
}

// 타입으로 유니온 타입 정의
type FuelType = "gasoline" | "diesel" | "electric";

// 타입으로 함수 정의
type Drive = (distance: number) => void;

// 실전 예제
const myCar: ElectricCar = {
  make: "Tesla",
  model: "Model 3",
  batteryCapacity: 75,
};

const fuel: FuelType = "electric";

const drive: Drive = (distance) => {
  console.log(`Driving ${distance} km`);
};



