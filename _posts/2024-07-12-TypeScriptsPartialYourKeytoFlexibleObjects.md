---
title: "TypeScript의 Partial 유연한 객체를 만드는 비법 "
description: ""
coverImage: "/assets/img/2024-07-12-TypeScriptsPartialYourKeytoFlexibleObjects_0.png"
date: 2024-07-12 19:00
ogImage:
  url: /assets/img/2024-07-12-TypeScriptsPartialYourKeytoFlexibleObjects_0.png
tag: Tech
originalTitle: "TypeScript’s Partial: Your Key to Flexible Objects 🗝️"
link: "https://medium.com/@xiuerold/typescripts-partial-your-key-to-flexible-objects-%EF%B8%8F-83ca2bc2d1c5"
---

![TypeScript Partial](/assets/img/2024-07-12-TypeScriptsPartialYourKeytoFlexibleObjects_0.png)

Type manipulation in TypeScript can take your coding skills to the next level. One powerful tool in TypeScript is the Partial utility type, which turns required object properties into optional ones like magic!

## When Can Partial Help You Out?

- Partial Updates: Easily update a few fields of an object without providing the entire object.
- Default Values: Override specific properties while keeping default values.
- Optional Configuration: Simplify working with configuration objects by making parameters optional.

<div class="content-ad"></div>

# Partial in Action: A Practical Example 💡

우리가 유저 인터페이스를 가지고 있다고 생각해 봅시다:

```js
interface User {
  id: number;
  name: string;
  age: number;
}
```

이제, 사용자의 나이만 업데이트해야 하는 상황을 상상해 보세요. Partial이 이를 어떻게 단순화하는지 살펴보죠:

<div class="content-ad"></div>

```js
// 원래 사용자 객체
const user: User = {
  id: 1,
  name: "Xiuer Old",
  age: 18,
};

// 사용자 정보를 업데이트하며 일부 속성만 수정
function updateUser(user: User, updatedProperties: Partial<User>): User {
  return { ...user, ...updatedProperties };
}
const updatedUser = updateUser(user, { age: 20 });
console.log(updatedUser); // { id: 1, name: 'Xiuer Old', age: 20 }
```

# 마법 공개: Partial 작동 방식

Partial은 TypeScript의 Mapped Types와 Index Types를 효과적으로 활용합니다:

- Mapped Types: 기존 타입에 변환을 적용하여 새로운 타입을 생성할 수 있습니다.
- Index Types: 객체 타입 키에 접근하는 데 유용합니다.

<div class="content-ad"></div>

여기에 부분이 그들을 활용하는 방법이 있습니다:

```js
type Partial<T> = {
  [P in keyof T]?: T[P];
};
```

본질적으로 부분은 원래 유형 T의 각 속성 P를 반복하면서 그것들을 선택적으로 만들면서(?) 그들의 원래 유형을 유지(T[P])합니다.

# 부분 게임을 업그레이드하세요: 고급 기법 🚀

<div class="content-ad"></div>

Partial은 이러한 기능을 통해 완전한 잠재력을 발휘합니다:

- 중첩된 객체: 중첩된 객체 내의 선택적 속성을 처리하기 위해 Partial을 재귀적으로 적용합니다.
- 연합 형식: 조건부 형식과 결합하여 연합 형식의 선택적 속성을 관리합니다.
- 매핑 및 조건부 형식: 복잡한 시나리오에 대한 강력한 형식 변환을 만듭니다.

# 복잡성 다루기: 실용적인 응용

- 깊게 중첩된 구조: 심층으로 중첩된 객체에서도 모든 속성을 선택적으로 처리합니다.
- 유연한 구성: 일부 속성은 필수이고 다른 속성은 선택적인 구성 객체를 처리합니다.

<div class="content-ad"></div>

# Partial의 힘: 결론

Partial 및 그 고급 사용 사례를 받아들이면 TypeScript 코드가 다음과 같이 더욱:

- 가독성이 높아집니다: 어떤 속성이 선택적인지 쉽게 이해할 수 있습니다.
- 유지보수가 용이해집니다: 객체를 수정하고 업데이트하는 작업이 더욱 쉬워집니다.
- 견고해집니다: 누락된 속성과 관련된 잠재적인 오류를 방지할 수 있습니다.

자, TypeScript의 Partial의 전체 잠재력을 펼쳐 보세요. 이건 진정한 변화를 가져다 줍니다! 🎉
