---
title: "React에서 useMemo와 memo의 진짜 차이점"
description: ""
coverImage: "/assets/img/2024-07-07-TheRealDifferenceBetweenuseMemoandmemoinReact_0.png"
date: 2024-07-07 21:10
ogImage:
  url: /assets/img/2024-07-07-TheRealDifferenceBetweenuseMemoandmemoinReact_0.png
tag: Tech
originalTitle: "The Real Difference Between useMemo and memo in React"
link: "https://medium.com/react-in-the-real-world/the-real-difference-between-usememo-and-memo-in-react-743c4cd3113a"
---

## useMemo 대 MEMO

![이미지](/assets/img/2024-07-07-TheRealDifferenceBetweenuseMemoandmemoinReact_0.png)

# 소개: React의 useMemo 훅 대 memo HOC

React는 사용자 인터페이스를 구축하기 위한 인기 있는 JavaScript 라이브러리로, 응용 프로그램의 성능을 최적화하기 위한 다양한 도구와 기술을 제공합니다. 그 중에서도 useMemo와 memo는 컴포넌트의 불필요한 다시 렌더링을 방지하는 데 널리 사용되는 두 가지 기술입니다. 비슷한 목표를 가지고 있지만, 실제로는 구현 및 사용 사례에서 상당히 다릅니다. 이 글에서는 React에서 useMemo와 memo 사이의 실제 차이를 규명하고, 이를 이모지 기반의 코드 예제와 함께 사용 사례를 탐구해 보겠습니다.

<div class="content-ad"></div>

# 🎯 useMemo: 캐싱 값에 대한 후크

useMemo는 렌더링 간에 동일한 값을 유지해야하거나 계산하는 데 많은 비용이 드는 값을 캐시 할 수있게 해주는 후크입니다. 이는 함수와 의존성 배열을 인수로 사용합니다. 함수는 의존성 중 하나가 변경 될 때만 실행되므로 값이 필요할 때만 다시 계산됩니다. 이모지를 사용한 예제가 여기 있습니다:
