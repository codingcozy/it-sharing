---
title: "LocalStorage  당시에는 좋은 아이디어 같았던 이유들"
description: ""
coverImage: "/assets/img/2024-07-07-LocalStorageItSeemedLikeaGoodIdeaattheTime_0.png"
date: 2024-07-07 18:59
ogImage:
  url: /assets/img/2024-07-07-LocalStorageItSeemedLikeaGoodIdeaattheTime_0.png
tag: Tech
originalTitle: "LocalStorage — It Seemed Like a Good Idea at the Time…"
link: "https://medium.com/@PurpleGreenLemon/localstorage-it-seemed-like-a-good-idea-at-the-time-ee9d1173c6a2"
---

![이미지](/assets/img/2024-07-07-LocalStorageItSeemedLikeaGoodIdeaattheTime_0.png)

LocalStorage의 약속은 데이터를 빠르게 저장하고 쉽게 액세스할 수 있는 브라우저 내의 공간이라는 점입니다. 이는 개발자에게 주머니에 예기치 못하게 $20을 발견하는 것과 같은 기쁨을 주는 것입니다. 그 뒤에는 그것을 즐겁고 재미있는 것(또는 적어도 즉시 해결할 수 있는 문제를 해결하는 것)에 쓰고 싶은 욕망이 따릅니다.

어느 정도 동안, 그것은 놀라울 정도로 느껴집니다. 사용자 설정을 저장해야 하는가요? LocalStorage를 사용하세요. 속도를 높이기 위한 빠른 캐시가 필요한가요? LocalStorage를 사용하세요. 처음에는 이렇게 많은 문제를 해결하는 솔루션이 되어줍니다.

그러나 어느 순간, 그 초기 설렘은 사라지기 시작합니다. 편의성이 오랜 기간을 생각할 때, 아마도... 아주 아주 아마도... 당신이 희망했던 장기적인 대답이 아니었을 수도 있다는 의심이 들기 시작합니다.

<div class="content-ad"></div>

# "쉬움"이 후회하기 쉬운 것으로 바뀌는 때

데이터 보안이 주요 걱정거리가 아니었던 때를 기억하나요? 로컬스토리지도 기억나지 않네요. 개발자 도구로 es 쿠키 저장 공간에 들여다 볼 수 있는 사람들이 있어요. 민감한 데이터? 그건 수면을 청할 수 없는 밤을 위한 조제품이에요.

그리고 "소멸 연극". 시크릿 모드, 사용자가 캐시를 지우거나 브라우저가 업데이트되면 — 휙. 신중하게 보관된 설정이 사라져 버릴 거에요. 만일 그것이 단순히 선호사항이었다면 짜증날 뿐이에요. 만일 그것이 중요했다면...
