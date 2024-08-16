---
title: "Java로 배열을 빈도순으로 정렬하는 방법 1636 Frequency Sort Solution"
description: ""
coverImage: "/assets/img/2024-07-24-1636SortArraybyIncreasingFrequencyFrequencySortSolutionJava_0.png"
date: 2024-07-24 12:04
ogImage: 
  url: /assets/img/2024-07-24-1636SortArraybyIncreasingFrequencyFrequencySortSolutionJava_0.png
tag: Tech
originalTitle: "1636 Sort Array by Increasing Frequency  Frequency Sort Solution Java"
link: "https://medium.com/@jameskodes/1636-sort-array-by-increasing-frequency-frequency-sort-solution-java-9d248ce77a27"
isUpdated: true
updatedAt: 1723816784093
---



<img src="/assets/img/2024-07-24-1636SortArraybyIncreasingFrequencyFrequencySortSolutionJava_0.png" />

1636번 문제인 Java에서 배열을 빈도별로 정렬하는 해결책을 살펴봅시다.

이 문제는 쉬운 문제로 나와 있지만, 다양한 주제와 사용자 지정 비교자를 사용해야 합니다. 최소한 중간 정도의 어려움이 있어야 하지만, 실제로는 어려운 문제로 설정할 수도 있겠네요.

맨 아래에 붙여넣을 수 있는 코드가 있습니다.

<div class="content-ad"></div>

# 소개

이 문제에 대한 설명입니다:

여기가 시작점입니다:

```js
    public int[] frequencySort(int[] nums) {
        
    }
```

<div class="content-ad"></div>

예제 1:

```js
입력: nums = [1,1,2,2,2,3]
출력: [3,1,1,2,2,2]
설명: '3'은 빈도수가 1이고, '1'은 빈도수가 2이며, '2'는 빈도수가 3이다.
```

# 해결책: HashMap과 정렬

## 설명

<div class="content-ad"></div>

이 문제는 여러 부분으로 이루어져 있습니다. 먼저 각 숫자의 빈도를 세야 합니다. 그런 다음 숫자를 오름차순으로 정렬하지만, 숫자의 빈도가 같은 경우에는 빈도가 같은 숫자를 내림차순으로 정렬해야 합니다.

## 설정

이를 수행하기 위해 두 개의 변수가 필요합니다: 빈도를 세는 해시맵과 Integer로 새로운 배열을 만들어 정렬할 수 있도록 해주는 newNums 배열입니다.

```java
// 각 요소의 값 빈도를 세기 위한 맵 생성
Map<Integer, Integer> map = new HashMap<>(); 
// Integer Array로 배열 정렬
Integer[] newNums = new Integer[nums.length]; 
```

<div class="content-ad"></div>

## HashMap

이제 nums 배열을 for 루프로 순회합니다. 각 인덱스마다 HashMap에 있는지 확인하고, 있다면 값을 증가시킵니다. 그렇지 않으면, 새로운 키를 값이 1인 새로운 값으로 생성합니다. 또한 각 인덱스마다 newNums[i] = nums[i]를 설정하여 배열을 효과적으로 복사합니다.

```js
for (int i = 0; i < nums.length; i++) {
  if(map.containsKey(nums[i])) { // 키가 있으면 값 증가
    map.put(nums[i], map.get(nums[i]) + 1);
  } else { // 키가 없으면 값 1로 추가
    map.put(nums[i],  1);
  }
  newNums[i] = nums[i]; // 정렬을 위해 newNums의 값 설정
}
```

## 정렬

<div class="content-ad"></div>

이제 배열을 정렬하는 복잡한 부분은 빈도 값에 따라 다르게 정렬되어야 하며 값이 동일한지 여부에 따라 정렬되어야 한다는 것입니다. 이를 위해 Java에서 사용자 지정 비교자가 필요합니다. 우리는 Arrays.sort()를 사용할 것입니다.

```js
Arrays.sort(newNums, (a, b) -> { // 색인 값과 함께 newNums을 정렬하는 부분
  if (map.get(a) != map.get(b)) { // 빈도 값이 일치하지 않으면,
    return map.get(a) - map.get(b); // 어떻게 조정할지 반환
  } else { // 빈도 값이 일치하면, 큰 값부터 작게 정렬
    return b - a; // 감소하는 순서를 위해 b - a
  }
});
```

간단하게 유지하기 위해 사용자 지정 비교자를 만듭니다. 배열과 newNums를 추가하고 각 비교되는 요소에 변수 이름을 할당합니다. "a"와 "b"를 사용했습니다만 원하는 변수 이름을 사용할 수 있습니다. "a"와 "b"는 비교되는 색인 값들을 나타내며 어떻게 정렬할지 반환합니다. 여기 .sort()에 대해 반환할 수 있는 내용을 확인할 수 있습니다:

- 음수 — a가 b보다 낮아야 할 때
- 0 — 두 값이 동일하게 배치되어야 할 때
- 양수 — a가 b보다 높아야 할 때

<div class="content-ad"></div>

빈도 값이 다를 경우 증가하는 순서대로 반환하고, 빈도 값이 같을 경우 감소하는 인덱스 값으로 반환합니다.

마지막으로 newNums를 nums로 매핑하고 nums를 반환합니다.

```js
for (int i = 0; i < nums.length; i++) {
 nums[i] = newNums[i];
}

return nums;
```

전체 코드 솔루션은 다음과 같습니다:

<div class="content-ad"></div>

```java
public int[] frequencySort(int[] nums) {
    Map<Integer, Integer> map = new HashMap<>(); // 각 요소의 값 수를 세기 위한 맵 생성
    Integer[] newNums = new Integer[nums.length]; // 배열 정렬을 위한 정수 배열

    for (int i = 0; i < nums.length; i++) {
        if(map.containsKey(nums[i])) { // 키가 존재하면 값 증가
            map.put(nums[i], map.get(nums[i]) + 1);
        } else { // 키가 존재하지 않으면 1로 추가
            map.put(nums[i], 1);
        }
        newNums[i] = nums[i]; // 정렬을 위한 newNums 값 설정
    }

    /** 사용자 정의 비교자 a와 b를 사용한 배열 정렬
     * 주어진 두 요소를 어떻게 정렬해야하는지에 따라 반환
     * 음수 - a가 b보다 낮아야 하는 경우
     * 제로 - 두 값이 동등하게 위치하는 경우
     * 양수 - a가 b보다 높아야 하는 경우
    **/
    Arrays.sort(newNums, (a, b) -> { // 인덱스 값과 newNums 정렬
        if (map.get(a) != map.get(b)) { // 빈도 수가 일치하지 않는 경우,
            return map.get(a) - map.get(b); // 조정 방법 반환
        } else { // 빈도 수가 일치하는 경우, 큰 순서대로 정렬
            return b - a; // 감소하는 순서를 위해 b - a
        }
    });

    for (int i = 0; i < nums.length; i++) {
        nums[i] = newNums[i];
    }

    return nums;
}
```

- LeetCode에서 Frequency Sort
- 내 YouTube 솔루션 확인하기