---
title: "예제와 함께 알아보는 20가지 필수 TypeScript Array 메서드"
description: ""
coverImage: "/assets/img/2024-07-09-20ArraymethodsinTypescriptyouneedtoknowwithexamples_0.png"
date: 2024-07-09 16:40
ogImage:
  url: /assets/img/2024-07-09-20ArraymethodsinTypescriptyouneedtoknowwithexamples_0.png
tag: Tech
originalTitle: "20 Array methods in Typescript you need to know with examples"
link: "https://medium.com/canopas/typescript-array-methods-and-their-usages-daa8d498b4fd"
---

## 미리 만들어진 기능은 항상 시간을 절약해 주는 것으로 입증되어 왔습니다. TypeScript에서 사용 가능한 유틸리티 함수 목록과 데모를 찾아보세요.

![Utility Functions in TypeScript](/assets/img/2024-07-09-20ArraymethodsinTypescriptyouneedtoknowwithexamples_0.png)

우리 프로그래머들은 항상 배열을 다루는데 시간을 보내지만 대부분의 경우 TypeScript에서 제공되는 모든 도우미 함수를 알지 못합니다.

저는 알아야 할 20가지 메소드 목록을 재미있는 이모지와 함께 열거했습니다.

<div class="content-ad"></div>

# 1. indexOf()

모든 배열 요소는 색인을 가지고 있습니다. 이 메서드는 배열 내 요소의 색인을 반환합니다.

a. 특정 요소가 배열 내에 존재하지 않는 경우, indexOf()는 -1을 반환합니다.

따라서, 이 메서드는 요소가 배열에 존재하는지 여부를 확인하는 데 사용할 수 있습니다.

<div class="content-ad"></div>

문법:
array.indexOf(element)

![이미지](/assets/img/2024-07-09-20ArraymethodsinTypescriptyouneedtoknowwithexamples_1.png)

## 2. lastIndexOf()

이 메소드는 배열의 마지막 요소의 인덱스를 반환합니다.

<div class="content-ad"></div>

a. 배열이 비어있으면 indexOf() 함수는 -1을 반환합니다.
b. 배열에 둘 이상의 동일한 요소가 있는 경우, 중복 항목의 최대 인덱스를 반환합니다.

구문:
array.lastIndexOf(element)

![image](/assets/img/2024-07-09-20ArraymethodsinTypescriptyouneedtoknowwithexamples_2.png)

# 3. concat()

<div class="content-ad"></div>

이름에서 알 수 있듯이이 방법은 단순히 두 배열을 병합하여 결과물을 반환합니다.

구문:
array1.concat(array2)

![image](/assets/img/2024-07-09-20ArraymethodsinTypescriptyouneedtoknowwithexamples_3.png)

# 4. join()

<div class="content-ad"></div>

이 메소드는 이름처럼 배열의 모든 요소를 지정된 연산자와 함께 문자열로 결합합니다.

a. 연산자가 제공되지 않으면 쉼표(,)로 요소를 결합합니다.

구문:
array.join(operator)

![image](/assets/img/2024-07-09-20ArraymethodsinTypescriptyouneedtoknowwithexamples_4.png)

<div class="content-ad"></div>

# 5. push()

이 방법은 배열의 끝에 하나 이상의 요소를 추가합니다.

구문:

```javascript
array.push(element);
```

![image](/assets/img/2024-07-09-20ArraymethodsinTypescriptyouneedtoknowwithexamples_5.png)

<div class="content-ad"></div>

# 6. pop()

이 메서드는 배열의 마지막 요소를 뺍니다/제거합니다.

구문:
array.pop()

<img src="/assets/img/2024-07-09-20ArraymethodsinTypescriptyouneedtoknowwithexamples_6.png" />

<div class="content-ad"></div>

# 7. reverse()

이름에서 알 수 있듯이, 이 메소드는 배열의 순서를 뒤집습니다.

구문:
array.reverse()

![이미지](/assets/img/2024-07-09-20ArraymethodsinTypescriptyouneedtoknowwithexamples_7.png)

<div class="content-ad"></div>

# 8. shift()

이 메소드는 배열에서 첫 번째 요소를 제거하고 제거된 요소를 반환합니다.
pop() 메소드의 정확한 반대라고 할 수 있습니다. pop() 메소드는 마지막 요소를 제거하고 결과를 반환합니다.

구문:
array.shift()

<img src="/assets/img/2024-07-09-20ArraymethodsinTypescriptyouneedtoknowwithexamples_8.png" />

<div class="content-ad"></div>

# 9. unshift()

`unshift()` 메서드는 `shift()` 메서드와 정반대의 동작을 합니다. 배열의 시작 부분에 요소를 추가하고 새로운 배열을 반환합니다.

구문:
array.unshift(element)

![image](/assets/img/2024-07-09-20ArraymethodsinTypescriptyouneedtoknowwithexamples_9.png)

<div class="content-ad"></div>

# 10. slice()

이 메서드는 우리가 원하는 방식으로 배열을 잘라내고 잘라낸 배열을 반환합니다.
a. 명시된 범위의 마지막 인덱스는 제외됩니다.

구문:
array.slice(시작 인덱스, 끝 인덱스)

![이미지](/assets/img/2024-07-09-20ArraymethodsinTypescriptyouneedtoknowwithexamples_10.png)

<div class="content-ad"></div>

# 11. splice()

이 메소드는 여러 가지 목적으로 사용할 수 있습니다. 예를 들면,

1. 배열에 요소 추가
2. 배열 내의 특정 요소 교체
3. 배열에서 특정 요소 제거

구문:
array.splice(index, 제거할 요소 수, element1,..,elementN)

![이미지](/assets/img/2024-07-09-20ArraymethodsinTypescriptyouneedtoknowwithexamples_11.png)

<div class="content-ad"></div>

# 12. toString()

이 메서드는 배열을 쉼표로 구분된 문자열로 변환합니다.

구문:

```javascript
array.toString();
```

![이미지](/assets/img/2024-07-09-20ArraymethodsinTypescriptyouneedtoknowwithexamples_12.png)

<div class="content-ad"></div>

# 13. filter()

이 메서드는 여러 가지 상황에서 사용할 수 있어요. 예를 들어 배열에서 짝수를 찾거나, 두 배열에서 공통 항목을 찾거나, 고유한 배열을 얻는 등의 작업에 사용할 수 있어요.

기본적으로, 제공된 조건을 확인하고 필터링된 배열을 반환해요.

구문:

array.filter(callback)

<div class="content-ad"></div>

![이미지](/assets/img/2024-07-09-20ArraymethodsinTypescriptyouneedtoknowwithexamples_13.png)

# 14. map()

이 메소드는 이 배열의 모든 요소에 제공된 함수를 호출한 결과로 새로운 배열을 생성합니다.
예시에서는 Math.ceil을 사용하여 가장 큰 최소 숫자를 반환하는 map()을 호출했습니다.

구문:
array.map(callback)

<div class="content-ad"></div>

![image](/assets/img/2024-07-09-20ArraymethodsinTypescriptyouneedtoknowwithexamples_14.png)

# 15. every()

이 메서드는 제공된 함수에 의해 구현된 테스트를 통과하는 배열의 모든 요소를 테스트합니다.

예제에서는 짝수를 확인했습니다.

<div class="content-ad"></div>

문법:
array.every(callback)

![image](/assets/img/2024-07-09-20ArraymethodsinTypescriptyouneedtoknowwithexamples_15.png)

# 16. reduceRight()

이 메서드는 배열의 두 값에 대해 함수를 동시에 적용하여 배열을 단일 값으로 줄입니다 (오른쪽에서 왼쪽으로).

<div class="content-ad"></div>

예시에서는 배열이 이전 요소에 요소를 추가함으로써 축소됩니다(오른쪽에서 왼쪽으로).

구문:
array.reduceRight(callback)

![이미지](/assets/img/2024-07-09-20ArraymethodsinTypescriptyouneedtoknowwithexamples_16.png)

# 17. reduce()

<div class="content-ad"></div>

이 메서드는 reduceRight() 메서드와 정반대로 동작합니다.
배열의 두 값에 동시에 함수를 적용하여 배열을 하나의 값으로 줄입니다(왼쪽에서 오른쪽으로).

예시에서 배열은 이전 요소에서 요소를 뺄셈하여(왼쪽에서 오른쪽으로) 줄어듭니다.

구문:
array.reduce(callback)

![이미지](/assets/img/2024-07-09-20ArraymethodsinTypescriptyouneedtoknowwithexamples_17.png)

<div class="content-ad"></div>

# 18. some()

이 메소드는 일반적으로 테스트 목적으로 사용됩니다.
즉, 배열에서 적어도 한 항목이 주어진 조건을 충족하는지 여부를 파악하는 데 사용됩니다.

예를 들어, 이 예제에서는 배열에서 적어도 하나의 짝수가 있는지 확인했습니다.

구문:
array.some(callback)

<div class="content-ad"></div>

![이미지](/assets/img/2024-07-09-20ArraymethodsinTypescriptyouneedtoknowwithexamples_18.png)

# 19. sort()

이름에서 알 수 있듯이, 이 메서드는 배열 요소를 정렬 순서대로 정렬합니다.

예제에서는 배열을 오름차순으로 정렬했습니다. 내림차순으로 정렬할 수도 있고, a-b 대신 b-a의 조건으로도 정렬될 것입니다. 마치 js에서 하는 것과 같이요.

<div class="content-ad"></div>

syntax:
array.sort(callback)

![array.sort() method](/assets/img/2024-07-09-20ArraymethodsinTypescriptyouneedtoknowwithexamples_19.png)

# 20. fill()

This method changes all elements in an array to a static value, from a start index (default 0) to an end index (default array.length) and returns the modified array.

<div class="content-ad"></div>

a. 특정(다중) 위치에 새 요소를 추가할 수 있습니다.

구문:
array.fill(value, start_index, end_index)

![이미지](/assets/img/2024-07-09-20ArraymethodsinTypescriptyouneedtoknowwithexamples_20.png)

오늘도 무언가를 배웠기를 바랍니다. 전체 코드는 TypeScript 배열 메소드에서 확인할 수 있습니다.

<div class="content-ad"></div>

피드백과 제안을 환영합니다 🎉.

# 관련 인기 기사

지원해 주셔서 감사합니다!

읽는 내용이 마음에 드신다면, 아래에서 👏 👏👏 눌러주세요 — 작가로서 이는 세상을 의미합니다!
흥미로운 기사의 업데이트를 받으려면 Canopas를 팔로우해주세요!

<div class="content-ad"></div>

계속 사용해보세요! 👍
