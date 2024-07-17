---
title: "TypeScript의 keyof 연산자로 안전하고 유연한 코드 작성하는 방법 완벽 분석"
description: ""
coverImage: "/assets/img/2024-07-12-DeepDiveintoTypeScriptsKeyofOperatorforSaferandMoreFlexibleCode_0.png"
date: 2024-07-12 18:33
ogImage:
  url: /assets/img/2024-07-12-DeepDiveintoTypeScriptsKeyofOperatorforSaferandMoreFlexibleCode_0.png
tag: Tech
originalTitle: "Deep Dive into TypeScript’s Keyof Operator for Safer and More Flexible Code"
link: "https://medium.com/javascript-in-plain-english/deep-dive-into-typescripts-keyof-operator-for-safer-and-more-flexible-code-f5bf678d1a0b"
---

![Image](/assets/img/2024-07-12-DeepDiveintoTypeScriptsKeyofOperatorforSaferandMoreFlexibleCode_0.png)

TypeScript에서 keyof 연산자는 코드의 안전성과 유연성을 향상시킬 수 있는 강력한 도구입니다. 이 글에서는 keyof 연산자의 다양한 응용 시나리오를 살펴보며 보다 잘 이해하고 활용할 수 있도록 도와드릴 것입니다. 상세한 코드 예제를 통해 keyof 연산자를 어떻게 정의하고 강력한 기능을 활용할 수 있는지 실제 개발에서 어떻게 활용할 수 있는지를 보여드리겠습니다.

# 1. 예제 코드로 KeyOf 연산자 정의하기

keyof 연산자는 주어진 유형의 모든 키를 가져와 이러한 키들의 유니언 유형을 반환하는 데 사용할 수 있습니다. 간단한 예제를 통해 기본 사용법을 이해해 봅시다.

<div class="content-ad"></div>

```js
interface Person {
  name: string;
  age: number;
  address: string;
}

type PersonKeys = keyof Person; // "name" | "age" | "address"
const key: PersonKeys = "name"; // Valid
const anotherKey: PersonKeys = "gender"; // Error: Type '"gender"' is not assignable to type 'PersonKeys'
```

이 예제에서 Person의 keyof는 "name" | "age" | "address"와 같은 유니온 타입을 반환하며, 이는 Person 인터페이스에 있는 모든 속성 이름을 나타냅니다. Person 인터페이스에 없는 속성에 접근하려는 시도는 TypeScript에 의해 잡힐 것입니다.

# 2. 제네릭에서 KeyOf 연산자 사용하기

제네릭에서 keyof 연산자를 사용하면 유연성과 안전성을 향상시킬 수 있습니다. 이를 통해 더 일반적인 함수와 타입을 정의할 수 있습니다.

<div class="content-ad"></div>

```js
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}

const person: Person = { name: "Alice", age: 30, address: "123 Main St" };
const name = getProperty(person, "name"); // "Alice"
const age = getProperty(person, "age"); // 30
const invalidProperty = getProperty(person, "gender"); // Error: Type '"gender"' is not assignable to type '"name" | "age" | "address"'
```

여기 예제에서는 getProperty 함수가 객체와 속성 키를 취하여 해당 속성의 값을 반환합니다. keyof T를 사용하면 전달된 키가 객체의 유효한 속성인지 확인하여 형 안전성을 높입니다.

# 3. KeyOf와 Mapped Types의 결합

keyof와 매핑된 타입을 결합하면 더 유연하고 강력한 타입 정의를 만들 수 있습니다. 예를 들어, 타입의 모든 속성을 선택적으로 만드는 것이 가능합니다.

<div class="content-ad"></div>

```js
type Partial<T> = {
  [P in keyof T]?: T[P];
};

type PartialPerson = Partial<Person>;
const partialPerson: PartialPerson = { name: "Alice" }; // Valid
```

이 예시에서는 제네릭 Partial 타입을 정의합니다. Partial 타입은 타입 매개변수 T를 받아서 T의 모든 속성을 옵셔널하게 만들어줍니다. keyof T를 이용하여 올바른 매핑을 보장합니다.

# 4. 명시적 키와 KeyOf 연산자

keyof 연산자는 명시적 키와 함께 사용하여 특정 속성들에 안전하게 접근하고 조작할 수 있도록 도와줍니다.

<div class="content-ad"></div>

```js
인터페이스 Car {
  브랜드: 문자열;
  모델: 문자열;
  연도: 숫자;
}

함수 updateProperty<T, K extends keyof T>(obj: T, key: K, value: T[K]): void {
  obj[key] = value;
}

const 자동차: Car = { 브랜드: "Toyota", 모델: "Corolla", 연도: 2020 };
updateProperty(자동차, "모델", "Camry"); // 유효함
updateProperty(자동차, "가격", 30000); // 오류: '"price"' 타입은 '"brand" | "model" | "year"'에 할당할 수 없음
```

위 예제에서 updateProperty 함수는 Car 객체의 유효한 속성만 업데이트할 수 있도록 보장하여 잘못된 속성 할당을 방지합니다.

# 5. 인덱스 시그니처 및 KeyOf 연산자

인덱스 시그니처와 keyof 연산자를 결합하면 동적 속성을 다룰 때 더 유연한 객체 유형을 만들 수 있습니다.

<div class="content-ad"></div>

```js
인터페이스 Dictionary<T> {
  [키: string]: T;
}

유형 DictionaryKeys = Dictionary<number>의 키타입; // string 또는 number
const numDict: Dictionary<number> = { a: 1, b: 2, 3: 3 };
const value1 = numDict["a"]; // 1
const value2 = numDict[3]; // 3
```

이 예제에서 Dictionary 인터페이스는 인덱스 시그니처를 사용하며, keyof Dictionary`number`는 string 또는 number를 반환하여 문자열 또는 숫자를 키로 사용하여 객체 속성에 액세스할 수 있습니다.

# 6. 유틸리티 타입과 Keyof 사용하기

TypeScript는 keyof 연산자와 결합할 수 있는 여러 내장 유틸리티 타입을 제공하여 더 복잡하고 강력한 타입을 생성할 수 있습니다.

<div class="content-ad"></div>

```js
type Pick<T, K extends keyof T> = {
  [P in K]: T[P];
};

type PersonNameAndAge = Pick<Person, "name" | "age">;
const personDetails: PersonNameAndAge = { name: "Alice", age: 30 }; // Valid
```

이 예시에서 Pick 타입은 keyof 연산자와 매핑된 타입을 사용하여 Person 타입의 name과 age 속성만 포함하는 새로운 타입인 PersonNameAndAge을 생성합니다.

# 7. 조건부 타입과 KeyOf 연산자 함께 사용하기

keyof 연산자를 조건부 타입과 결합하면 더 복잡한 유형의 확인과 변환을 만들 수 있습니다.

<div class="content-ad"></div>

```js
type Exclude<T, U> = T extends U ? never : T;

type NonStringKeys<T> = {
  [K in keyof T]: T[K] extends string ? never : K
}[keyof T];

type NonStringPersonKeys = NonStringKeys<Person>; // "age" | "address"

const personKey: NonStringPersonKeys = "age"; // Valid
const anotherPersonKey: NonStringPersonKeys = "name"; // Error: Type '"name"' is not assignable to type '"age" | "address"'
```

이 예제에서 NonStringKeys 타입은 조건부 타입과 keyof 연산자를 사용하여 Person 타입에서 문자열 값을 가진 모든 속성을 제외한 새로운 타입 NonStringPersonKeys가 생성됩니다.

# 8. 동적 객체 속성과 KeyOf 연산자 사용

일부 경우에는 객체 속성에 동적으로 액세스하거나 조작해야 하는 경우가 있습니다. keyof 연산자와 동적 객체 속성을 결합하면 이러한 작업이 타입 안전하게 수행됩니다.

<div class="content-ad"></div>

```js
인터페이스(Config)와 그 인터페이스에서 ConfigKey 타입을 생성하는 keyof를 사용한 예제입니다. ConfigManager 클래스는 제네릭 메서드인 get과 set을 사용하여 구성 객체의 유형 안전한 액세스 및 수정을 보장합니다. 이렇게 함으로써 유형 오류에 대해 걱정하지 않고 동적으로 구성 속성을 액세스하고 업데이트할 수 있습니다.

위 실제 응용 프로그램 시나리오와 코드 예제 8개를 통해 우리는 TypeScript에서 keyof 연산자를 깊이 있는 방식으로 탐구했습니다. keyof 연산자가 코드 안전성과 유연성을 크게 향상시킬 수 있다는 것을 보았습니다. 이를 통해 유형 안전성을 보장하고 복잡한 유형 처리를 단순화할 수 있었습니다. 이 글이 keyof 연산자를 더 잘 이해하고 적용하도록 도와줄 것을 기대합니다. TypeScript 코드를 효율적이고 신뢰할 수 있게 만드는데 도움이 되었으면 좋겠습니다.

# 간단히 말하면 🚀
```

<div class="content-ad"></div>

In Plain English 커뮤니티의 일원이 되어주셔서 감사합니다! 떠나시기 전에:

- 작가를 박수로 찬미하고 팔로우해주세요 👏
- 팔로우하기: X | LinkedIn | YouTube | Discord | Newsletter
- 다른 플랫폼 방문하기: CoFeed | Differ
- PlainEnglish.io에서 더 많은 콘텐츠 확인하기
