---
title: "2024 React 프로젝트에서 Recharts 대신 다른 라이브러리를 사용해야 하는 이유"
description: ""
coverImage: "/assets/img/2024-07-09-TimetoStopusingRechartsinReact_0.png"
date: 2024-07-09 13:03
ogImage:
  url: /assets/img/2024-07-09-TimetoStopusingRechartsinReact_0.png
tag: Tech
originalTitle: "Time to Stop using Recharts in React"
link: "https://medium.com/gitconnected/time-to-stop-using-recharts-in-react-647fa3e2ddf5"
---

<img src="/assets/img/2024-07-09-TimetoStopusingRechartsinReact_0.png" />

어드민 대시보드용으로 아름다운 차트가 필요하다면, 좋은 소식이 있어요.

오늘은 shadcn-ui에서 Charts의 출시로 축복 받은 날이에요!

오늘 shadcn의 트윗을 보고는 너무 흥분되어 여러분과 나누고 싶었어요:

<div class="content-ad"></div>

아래와 같이...

- 65가지 차트 버전
- 다양한 색상 팔레트
- 제로 추상화...

Shadcn Charts은 React용 기본 Recharts 패키지를 대체할 것입니다.

그리고 더 중요한 것은, Shadcn이 Recharts의 래퍼(wrapper)가 아니라 Recharts 패키지 위에서 구축되었다는 것입니다.

<div class="content-ad"></div>

샤드씬은 구성을 사용하여 추상화 대신 구성 요소가 완전히 당신 것임을 보장합니다. 여기는 샤드씬 자신이 그것을 설명하는 방법입니다:
