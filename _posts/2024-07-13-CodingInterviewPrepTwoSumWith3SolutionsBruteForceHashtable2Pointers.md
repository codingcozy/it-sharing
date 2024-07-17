---
title: "코딩 인터뷰 준비 두 수 합 문제 3가지 풀이법 브루트 포스, 해시테이블, 투 포인터"
description: ""
coverImage: "/assets/img/2024-07-13-CodingInterviewPrepTwoSumWith3SolutionsBruteForceHashtable2Pointers_0.png"
date: 2024-07-13 18:39
ogImage:
  url: /assets/img/2024-07-13-CodingInterviewPrepTwoSumWith3SolutionsBruteForceHashtable2Pointers_0.png
tag: Tech
originalTitle: "Coding Interview Prep: Two Sum With 3 Solutions (Brute Force, Hashtable, 2 Pointers)"
link: "https://medium.com/@allaboutcode/coding-interview-prep-two-sum-with-3-solutions-brute-force-hashtable-2-pointers-3db5e940d982"
---

# 문제

<img src="/assets/img/2024-07-13-CodingInterviewPrepTwoSumWith3SolutionsBruteForceHashtable2Pointers_0.png" />

# 해결책 1 — 무차별 대입

```js
/*
 무차별 대입 방법: 2개의 중첩된 반복문
 복잡도: O(n^2)
 공간: O(n)
 실행 시간: 60 ms
 */
var twoSum_bruteForce = function (nums, target) {
  for (let i = 0; i < nums.length; i++) {
    for (let j = i + 1; j < nums.length; j++) {
      if (nums[i] + nums[j] == target) {
        return new Array(i, j);
      }
    }
  }
  return [];
};
```

<div class="content-ad"></div>

# 해결책 2 — 해싱

```js
/*
해싱 메소드: 맵의 키는 배열 내의 숫자 값이고, 값은 색인입니다.
복잡도: 정렬된 맵 O(n), 정렬되지 않은 맵 O(n^2)
공간 복잡도: O(n)
런타임: 69밀리초
 */
var twoSum_hashing = function(nums, target){
    const map = new Map();
    for (let i=0; i<nums.length; i++){
        const num = nums[i];
        const complement = target - nums[i];
```

```js
        if (map.has(complement)){
            return new Array(map.get(complement), i);
        }
        map.set(nums[i], i);
    }
    return [];
}
```

해시맵은 키-값 쌍을 저장할 수 있는 데이터 구조입니다. 모든 JavaScript 객체는 키로 문자열이나 심볼을 수용하는 간단한 해시맵입니다. 따라서 탐색의 복잡도는 O(1)입니다.

<div class="content-ad"></div>

# 해결책 3–2 포인터 방법 (탐욕 알고리즘)

```js
/**
2 포인터 방법 (탐욕 알고리즘)
복잡도: O(n)
*/
```

```js
var twoSum = function (nums, target) {
  const sorted = nums.sort((a, b) => a - b);
  console.log(sorted);
  let left = 0;
  let right = nums.length - 1;

  while (left < right) {
    const sum = nums[left] + nums[right];
    if (sum == target) {
      return [left, right];
    } else if (sum < target) {
      left++;
    } else {
      right--;
    }
  }
  return [];
};
```

탐욕 알고리즘은 최적화 문제를 해결하는 간단하고 직관적인 방법입니다. 각 단계마다 현재 상태에서 지역 최적의 선택을 하는 방식으로 전역 최적해를 찾으려는 방법입니다. 탐욕 알고리즘의 주요 장점은 구현과 이해가 쉽다는 것입니다.
