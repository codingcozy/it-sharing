---
title: "시간 복잡성 이해하기 친절한 입문 가이드"
description: ""
coverImage: "/assets/img/2024-07-07-UnderstandingTimeComplexityAFriendlyIntroduction_0.png"
date: 2024-07-07 20:56
ogImage:
  url: /assets/img/2024-07-07-UnderstandingTimeComplexityAFriendlyIntroduction_0.png
tag: Tech
originalTitle: "Understanding Time Complexity: A Friendly Introduction"
link: "https://medium.com/@jeanpierrecarvalho/understanding-time-complexity-a-friendly-introduction-fb433a9534bd"
---

![Understanding Time Complexity](/assets/img/2024-07-07-UnderstandingTimeComplexityAFriendlyIntroduction_0.png)

안녕하세요, 동료 코더 여러분! 오늘은 컴퓨터 과학의 기본 개념 중 하나인 시간 복잡도에 대해 이야기하려고 합니다. 프로그래밍 생활을 훨씬 쉽게 만들어주는 개념이죠. 걱정 마세요, 간단하고 쉽게 이해할 수 있도록 설명해 드릴게요. 함께 알아볼까요?

# 시간 복잡도란?

영농을 한다고 상상해봅시다. 어떤 일은 다른 것보다 더 오래 걸립니다. 소 한 마리를 먹이는 일은 빠르지만, 떼 한 마리를 먹이는 것은 시간이 좀 걸리죠! 마찬가지로 컴퓨터 프로그램(또는 알고리즘)마다 작업을 완료하는 데 걸리는 시간이 다릅니다. 시간 복잡도는 입력 크기가 커짐에 따라 알고리즘을 실행하는 데 걸리는 시간이 어떻게 증가하는지를 이해하는 데 도움이 됩니다.

<div class="content-ad"></div>

# 왜 중요할까요?

코드의 시간 복잡도를 알면 마치 슈퍼파워를 가진 것과 같습니다. 이는 대량의 데이터를 처리하는 효율적인 프로그램을 작성하는 데 도움이 되며 느려지지 않습니다. 또한 코딩 인터뷰에서 중요한 주제 중 하나입니다. 여러분의 지식으로 미래의 고용주들에게 인상을 주세요!

# 빅 오 표기법

시간 복잡도에 대해 이야기할 때, 우리는 빅 오 표기법이라 불리는 것을 사용합니다. 이는 알고리즘이 얼마나 오래 걸릴지에 대한 최악의 시나리오를 설명하는 방법입니다. 이 중 몇 가지 일반적인 것들은 다음과 같습니다:

<div class="content-ad"></div>

- O(1) — 상수 시간: 입력 크기와 상관없이 걸리는 시간이 변하지 않습니다. 나무에서 한 개의 사과를 따는 것과 같습니다. 빠른 동작 하나로 손에 넣을 수 있어요!
- O(n) — 선형 시간: 입력 크기와 직접적으로 증가하는 시간입니다. 농장의 모든 동물에 먹이를 주는 상황을 상상해보세요. 동물이 많을수록 일을 끝내는 데 더 오래 걸립니다.
- O(n²) — 이차 시간: 입력 크기의 제곱에 비례하여 시간이 증가합니다. 어떤 이유로 모든 동물을 서로 짝지어야 한다고 가정해보세요. 동물이 많을수록 고려해야 할 짝도 더 많아져요!
- O(log n) — 로그 시간: 입력 크기에 비해 시간이 천천히 증가합니다. 특정 작물을 찾기 위해 농장을 점점 더 작은 구역으로 나눠 가는 것과 같습니다. 각 분할은 빠르게 검색 범위를 좁혀줍니다.
- O(n log n) — 선형로그 시간: 조금 더 복잡하지만 합병 정렬과 같은 효율적인 정렬 알고리즘에서 흔하게 볼 수 있습니다. 크기가 큰 다양한 종자들을 정렬하는 것처럼, 그룹을 나누고 그 그룹을 정렬하는 것과 비슷합니다.

# 실생활 농장 예시

취미로 농부인 저는 이해를 돕기 위해 몇 가지 농장 관련 코딩 예시를 살펴보겠습니다 🌾🚜

# 예시 1: 첫 번째 동물 확인하기 (O(1))

<div class="content-ad"></div>

```js
function checkFirstAnimal(animals) {
  console.log(animals[0]);
}
```

이것은 O(1) 시간 복잡도입니다. 농장에 동물이 얼마나 많든지 첫 번째 동물을 확인하는 데 한 걸음만 걸립니다.

# 예제 2: 특정 동물 찾기 (O(n))

```js
function findAnimal(animals, target) {
  for (let i = 0; i < animals.length; i++) {
    if (animals[i] === target) {
      return true;
    }
  }
  return false;
}
```

<div class="content-ad"></div>

이것은 O(n²) 시간 복잡도를 가지고 있어요. 모든 동물을 한번씩 확인해야 할 수도 있는 최악의 경우에요.

# 예제 3: 동물들을 짝지어주기 (O(n²))

```js
function pairAnimals(animals) {
  let pairs = [];
  for (let i = 0; i < animals.length; i++) {
    for (let j = 0; j < animals.length; j++) {
      if (i !== j) {
        pairs.push([animals[i], animals[j]]);
      }
    }
  }
  return pairs;
}
```

이것은 O(n²) 시간 복잡도를 가지고 있어요. 각각의 동물에 대해 다른 모든 동물과 짝을 짓기 때문에 많은 단계가 필요해요!

<div class="content-ad"></div>

# 예제 4: 특정 작물 검색하기 (O(log n))

```js
function findCrop(crops, target) {
  let left = 0;
  let right = crops.length - 1;
  while (left <= right) {
    let middle = Math.floor((left + right) / 2);
    if (crops[middle] === target) {
      return true;
    } else if (crops[middle] < target) {
      left = middle + 1;
    } else {
      right = middle - 1;
    }
  }
  return false;
}
```

이것은 O(log n) 시간 복잡도입니다. 각 단계는 문제 크기를 절반으로 줄입니다.

# 예제 5: 씨앗 정렬하기 (O(n log n))

<div class="content-ad"></div>

```js
function mergeSort(seeds) {
  if (seeds.length <= 1) {
    return seeds;
  }
  const middle = Math.floor(seeds.length / 2);
  const left = mergeSort(seeds.slice(0, middle));
  const right = mergeSort(seeds.slice(middle));
  return merge(left, right);
}

function merge(left, right) {
  let result = [];
  let leftIndex = 0;
  let rightIndex = 0;
  while (leftIndex < left.length && rightIndex < right.length) {
    if (left[leftIndex] < right[rightIndex]) {
      result.push(left[leftIndex]);
      leftIndex++;
    } else {
      result.push(right[rightIndex]);
      rightIndex++;
    }
  }
  return result.concat(left.slice(leftIndex)).concat(right.slice(rightIndex));
}
```

이 코드의 시간 복잡도는 O(n log n)입니다. 이 코드는 아이템을 반복적으로 분할하고 병합하여 효율적으로 정렬하는 방법입니다.

# 결론

시간 복잡도를 이해하는 것은 더 나은, 더 빠른 코드를 작성하는 데 도움이 됩니다. 알고리즘의 입력 크기에 따른 알고리즘의 확장 방식에 주의를 기울여보세요. 이렇게 하면 보다 효율적인 프로그래머로 성장할 수 있을 거예요. 즐거운 코딩되세요!
