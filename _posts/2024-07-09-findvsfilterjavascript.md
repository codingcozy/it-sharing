---
title: "JavaScript에서 find와 filter 비교 어떤 것을 언제 사용해야 할까요"
description: ""
coverImage: "/it-shar.ing/assets/no-image.jpg"
date: 2024-07-09 13:02
ogImage:
  url: /it-shar.ing/assets/no-image.jpg
tag: Tech
originalTitle: "find vs filter javascript"
link: "https://medium.com/@vipin.kumar171983/find-vs-filter-javascript-2db6fa93d441"
---

자바스크립트에서 find 및 filter는 둘 다 배열 메소드로, 콜백 함수에 의해 정의된 조건에 따라 요소를 검색하는 데 사용됩니다. 그러나 각각 다른 목적을 가지고 서로 다른 결과를 반환합니다. 자세한 비교는 다음과 같습니다:

# find 메소드

- 목적: 제공된 테스트 함수를 만족하는 배열에서 첫 번째 요소를 반환합니다.
- 반환값: 테스트를 통과하는 첫 번째 요소의 값 또는 테스트를 통과하는 요소가 없는 경우 undefined를 반환합니다.
- 구문:

```js
array.find(callback(element, index, array), thisArg);
```

<div class="content-ad"></div>

- 예시:

```js
let numbers = [1, 2, 3, 4, 5];
let found = numbers.find((element) => element > 3);
console.log(found); // 4
```

# filter 메소드

- 목적: 제공된 함수에 의해 구현된 테스트를 통과하는 모든 요소로 새로운 배열을 생성합니다.
- 반환 값: 테스트를 통과하는 요소로 이루어진 새로운 배열 또는 테스트를 통과하는 요소가 없는 경우 빈 배열을 반환합니다.
- 구문:

<div class="content-ad"></div>

```js
array.filter(callback(element, index, array), thisArg);
```

- 예시:

```js
let numbers = [1, 2, 3, 4, 5];
let filtered = numbers.filter((element) => element > 3);
console.log(filtered); // [4, 5]
```

# 주요 차이점

<div class="content-ad"></div>

- 반환 값:

- find는 테스트를 통과하는 첫 번째 요소를 하나 반환하거나 undefined를 반환합니다.
- filter는 테스트를 통과하는 모든 요소로 이루어진 배열을 반환합니다.

- 사용 사례:

- 상황에 맞는 요소를 찾아야 할 때는 find를 사용하세요.
- 조건을 충족하는 모든 요소를 찾아야 할 때는 filter를 사용하세요.

<div class="content-ad"></div>

- 성능:

- find는 첫 번째 일치하는 요소를 찾으면 배열의 반복을 중지하므로 하나의 결과만 필요할 때 더 효율적일 수 있습니다.
- filter는 모든 일치하는 요소를 포함하는 새 배열을 만들기 위해 전체 배열을 계속 반복합니다.

- 연쇄:

- find는 하나의 요소를 반환하므로 이를 배열로 감싸지 않고는 계속해서 배열 메소드를 연결하기에 적합하지 않을 수 있습니다.
- filter는 배열을 반환하므로 다른 배열 메소드와 연쇄하는 데 적합합니다.

<div class="content-ad"></div>

# 구체적인 고려 사항

- 가독성: 첫 번째 일치 항목만 필요한 경우 명확성을 위해 'find'를 선택하고, 여러 결과가 필요한 경우 'filter'를 선택하세요.
- 오류 처리: 만약 요소를 찾지 못하면 'find'가 정의되지 않은 값을 반환할 수 있으므로, 코드에서 추가적인 확인이 필요할 수 있습니다.

# 명확화를 위한 예시

## 'find' 사용하기

<div class="content-ad"></div>

```js
let users = [
  { id: 1, name: "Alice" },
  { id: 2, name: "Bob" },
  { id: 3, name: "Charlie" },
];
let user = users.find((user) => user.id === 2);
console.log(user); // { id: 2, name: 'Bob' }
```

## filter 사용하기

```js
let users = [
  { id: 1, name: "Alice" },
  { id: 2, name: "Bob" },
  { id: 3, name: "Charlie" },
  { id: 4, name: "Alice" },
];
let alices = users.filter((user) => user.name === "Alice");
console.log(alices); // [{ id: 1, name: 'Alice' }, { id: 4, name: 'Alice' }]
```

요약하자면, 단일 요소를 찾을 때는 find를 사용하고 조건에 따라 여러 요소를 모을 때는 filter를 사용하세요.
