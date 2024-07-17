---
title: "성능을 저하시키는 코드 작성 방법 6가지"
description: ""
coverImage: "/assets/img/2024-07-09-WhatDoYouDoinCodeThatCanSlowdowntheApplication_0.png"
date: 2024-07-09 08:41
ogImage:
  url: /assets/img/2024-07-09-WhatDoYouDoinCodeThatCanSlowdowntheApplication_0.png
tag: Tech
originalTitle: "What Do You Do in Code That Can Slow down the Application?"
link: "https://medium.com/stackademic/what-do-you-do-in-code-that-can-slow-down-the-application-1f5eaedf3efa"
---

## 프로그래밍

![코드 성능 저하의 원인](/assets/img/2024-07-09-WhatDoYouDoinCodeThatCanSlowdowntheApplication_0.png)

애플리케이션의 성능을 늦추는 요소는 수백 가지로 나타날 수 있어요. 저는 11살 때부터 아마추어 프로그래머로 활동해왔어요. 제 개인적인 경험을 바탕으로 코딩 중으로 가장 최악의 실수를 소개할게요:

- 루프 크기를 잘못 설정하는 것. 알고리즘에서 루프는 중요해요. 예전에는 카운터를 설정하고 결과를 얻을 때까지 한 단계씩 증가시키는 방식이었습니다. 오늘날에는 while, for과 같은 것들을 사용하죠.

<div class="content-ad"></div>

루프는 같은 항목을 여러 번 반복하기 때문에 콘텐츠의 작은 결함이나 루프 작동 방식에 큰 문제가 발생할 수 있습니다.

루프에서 발생하는 기본 오류:

- 잘못 조정된 루프는 이미 반복에서 제외되어야 할 항목들을 불필요하게 반복합니다. 조심하세요; 작은 항목을 불필요하게 반복하는 것은 기하급수적인 영향을 미칠 수 있습니다. 대신 데이터를 재정렬하거나 목록 중 하나를 정렬하거나 불필요한 반복을 피할 수 있는 트릭을 사용해보세요;
- 초기 조건을 테스트하는 비교 항목만 있는 루프는 모든 처리 과정을 느리게 만듭니다. 단 한 번만 사용될 항목을 테스트하려면 루프 내에서 If, then을 사용해야 합니다....(이어서)
