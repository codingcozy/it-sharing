---
title: "거품 정렬Bubble Sort 쉽게 이해하기 알고리즘과 구현 방법"
description: ""
coverImage: "/assets/img/2024-07-09-BubbleSort_0.png"
date: 2024-07-09 13:00
ogImage:
  url: /assets/img/2024-07-09-BubbleSort_0.png
tag: Tech
originalTitle: "Bubble Sort"
link: "https://medium.com/@nonsooranye/bubble-sort-d83987cb7cc6"
---

버블 정렬이란 무엇입니까?

버블 정렬은 배열이 특정 조건에 따라 재배열될 때까지 요소를 반복적으로 이동시키는 간단한 정렬 알고리즘입니다.

간단히 말하면, 버블 정렬은 배열의 요소를 다른 요소들과 비교하여 지정된 조건에 따라 반복적으로 재배열하는 알고리즘입니다.

제 능력을 최대한 발휘하여 여기서 버블 정렬을 설명하는 예제를 2개 사용하겠습니다.

<div class="content-ad"></div>

가장 먼저 할 예시는 버블 소트를 사용하여 순차적으로 정렬된 숫자 배열입니다.

![Bubble Sort](/assets/img/2024-07-09-BubbleSort_0.png)

여기서 array.slice()는 함수에 전달된 배열의 새로운 복사본을 만들어 함수를 순수하게 유지하고 전달된 배열을 변경하지 않도록 하는 역할을 합니다. 이렇게 할 수 있습니다.

첫 번째 루프는 나중에 비교할 요소를 선택하는 데 사용됩니다. 배열은 이후의 모든 요소와 비교됩니다. 배열의 조건은 "i ` arr.length-1"인데, 이는 마지막 요소보다 뒤에 비교할 요소가 없기 때문입니다.

<div class="content-ad"></div>

두 번째 반복문은 배열 재배열의 조건이 적용되는 곳입니다. 여기서 배열의 조건은 "j ` arr.length-1-i"입니다. 가장 큰 숫자는 배열의 끝에 정렬되며 정렬은 첫 번째 반복문 i의 인덱스와 일치합니다.

여기서 재배열 또는 뒤집기의 조건은 선택된 요소가 뒤집힐 요소보다 커야만 뒤집힐 수 있다는 것입니다.

두 번째 예시는 문자열 배열을 문자열의 길이를 기준으로 오름차순으로 정렬하는 것입니다.

![Bubble Sort Image](/assets/img/2024-07-09-BubbleSort_1.png)

<div class="content-ad"></div>

첫 번째 예제의 동일한 논리가 여기에도 적용됩니다.
