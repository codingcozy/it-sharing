---
title: "문제 해결을 위한 강력한 도구 해시 테이블 쉽게 이해하기"
description: ""
coverImage: "/assets/img/2024-07-09-HashTablesAPowerfulToolforSolvingProblems_0.png"
date: 2024-07-09 13:12
ogImage:
  url: /assets/img/2024-07-09-HashTablesAPowerfulToolforSolvingProblems_0.png
tag: Tech
originalTitle: "Hash Tables: A Powerful Tool for Solving Problems"
link: "https://medium.com/@arsathcomeng/hash-tables-a-powerful-tool-for-solving-problems-32ffd36a6311"
---

![Hash Tables](/assets/img/2024-07-09-HashTablesAPowerfulToolforSolvingProblems_0.png)

해시 테이블은 프로그래머의 도구 상자에서 가장 다재다능하고 강력한 데이터 구조 중 하나입니다. 삽입, 삭제 및 조회 작업에 대해 일정한 시간 평균 복잡도를 제공하여 다양한 문제를 효율적으로 해결하는 데 귀중한 역할을 합니다. 이 글에서는 해시 테이블의 개념을 탐구하고 일부 인기 있는 LeetCode 문제를 해결하는 데 해시 테이블의 응용을 보여드리겠습니다.

# 해시 테이블이란?

해시 테이블(해시 맵으로도 알려짐)은 키를 값에 매핑할 수 있는 구조인 연상 배열 추상 데이터 유형을 구현하는 데이터 구조입니다. 해시 함수를 사용하여 키를 값에 매핑할 수 있는 구조인 연상 배열 추상 데이터 유형을 구현하는 데이터 구조입니다. 해시 함수를 사용하여 키를 값으로 매핑할 수 있는 데이터 구조입니다. 해당 값은 찾을 수 있는 배열 또는 슬롯으로의 인덱스를 계산하는 데 해시 함수를 이용합니다.

<div class="content-ad"></div>

해시 테이블의 가장 큰 장점은 그 속도입니다. 각 조회 작업의 평균 시간 복잡도는 테이블에 저장된 요소 수와 관계 없습니다. 많은 해시 테이블 설계는 임의의 삽입 및 삭제를 허용하며, 각 작업당 평균 비용은 일정합니다.

JavaScript에서는 객체나 Map 클래스를 사용하여 해시 테이블을 구현할 수 있습니다. 고유한 값 집합만 필요한 문제에는 Set 클래스도 매우 유용합니다.

# 해시 테이블을 사용해 해결된 LeetCode 문제

해시 테이블을 효율적으로 활용하여 해결할 수 있는 몇 가지 LeetCode 문제를 살펴보겠습니다.

<div class="content-ad"></div>

# 1. 두 수의 합 (LeetCode 문제 1)

문제: 정수 배열 nums와 정수 target이 주어졌을 때, 합이 target이 되는 두 숫자의 인덱스를 반환합니다.

해결책:

```js
var twoSum = function (nums, target) {
  let map = new Map();

  for (let i = 0; i < nums.length; i++) {
    let complement = target - nums[i];

    if (map.has(complement)) {
      return [map.get(complement), i];
    }

    map.set(nums[i], i);
  }

  return []; // 해결책을 찾을 수 없음
};
```

<div class="content-ad"></div>

해설: 배열을 순회하면서 각 숫자와 해당 인덱스를 저장하기 위해 해시 맵을 사용합니다. 각 숫자에 대해 보수(complement)를 계산하고(목표값 - 현재 숫자), 맵에 해당 보수가 있는지 확인합니다. 있으면 쌍을 찾은 것이므로 해당 인덱스들을 반환합니다. 없다면 현재 숫자와 해당 인덱스를 맵에 추가합니다.

이 솔루션은 시간 복잡도 O(n)와 공간 복잡도 O(n)을 가지며, 무차별 대입법 O(n²) 접근 방식보다 상당히 향상된 성능을 제공합니다.

## 2. 중복 여부 확인 (LeetCode 문제 217)

문제: 정수 배열 nums가 주어지면, 배열에서 어떤 값이 한 번 이상 나타나면 true를 반환하고, 모든 요소가 고유하면 false를 반환합니다.

<div class="content-ad"></div>

해단 테이블 태그를 마크다운 형식으로 변경해 드리겠습니다.

해결책:

```js
var containsDuplicate = function (nums) {
  let set = new Set();

  for (let num of nums) {
    if (set.has(num)) {
      return true;
    }
    set.add(num);
  }

  return false;
};
```

설명: 이 코드는 Set을 활용하여 중복된 숫자를 확인합니다. 주어진 배열을 한 번 순회하면서 각 숫자가 Set에 이미 있는지 확인합니다. Set에 있는 경우 중복된 숫자이므로 true를 반환합니다. 중복된 숫자를 찾지 못한 경우 false를 반환합니다.

이 해결책의 시간 복잡도는 O(n)이며 공간 복잡도는 O(n)이므로 이 문제에 최적화되어 있습니다.

<div class="content-ad"></div>

# 3. 문자열에서 첫 번째 고유 문자 찾기 (LeetCode 문제 387)

문제: 문자열 s가 주어졌을 때, 첫 번째로 반복하지 않는 문자를 찾아 해당 인덱스를 반환합니다. 해당하는 문자가 없는 경우 -1을 반환합니다.

해결책:

```js
var firstUniqChar = function (s) {
  let charCount = new Map();

  // 각 문자의 발생 횟수 세기
  for (let char of s) {
    charCount.set(char, (charCount.get(char) || 0) + 1);
  }

  // 발생 횟수가 1인 첫 번째 문자 찾기
  for (let i = 0; i < s.length; i++) {
    if (charCount.get(s[i]) === 1) {
      return i;
    }
  }

  return -1; // 고유 문자를 찾을 수 없는 경우
};
```

<div class="content-ad"></div>

**해설:**
문자열에서 각 문자의 발생 횟수를 세는 데 맵을 사용합니다. 문자열을 두 번 반복하면서 각 문자의 발생 횟수를 세는 첫 번째 반복과 발생 횟수가 1인 첫 번째 문자를 찾는 두 번째 반복을 진행합니다. 이러한 문자를 찾으면 해당 인덱스를 반환합니다. 고유한 문자를 찾지 못하면 -1을 반환합니다.

이 솔루션은 시간 복잡도가 O(n)이며, 공간 복잡도가 O(k)입니다. 여기서 k는 문자 세트의 크기를 의미하며(일반적으로 소문자 영어 알파벳의 경우 26), k입니다.

# 결론

해시 테이블은 프로그래밍 문제를 효율적으로 해결하는 강력한 도구입니다. 키를 기반으로 항목을 빠르게 조회, 삽입 또는 삭제해야 할 때 특히 유용합니다. 여기서 살펴본 문제들은 해시 테이블을 사용하여 최적으로 해결할 수 있는 LeetCode의 다양한 도전 과제 중 일부에 불과합니다.

<div class="content-ad"></div>

새로운 문제에 접근할 때는 항상 해시 테이블이 작업에 적합한 도구인지를 고려해보세요. 해시 테이블을 사용하면 더 간단하고 효율적인 해결책을 얻을 수 있으며, 특히 카운팅, 고유 요소 찾기 또는 쌍 일치와 관련된 문제에 유용합니다.

즐거운 코딩하고, 해시 테이블이 항상 완벽하게 균형을 이루기를 바랍니다!
