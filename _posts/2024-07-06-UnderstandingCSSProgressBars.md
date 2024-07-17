---
title: "CSS 진행률 바 이해하기 기본부터 고급까지"
description: ""
coverImage: "/it-shar.ing/assets/no-image.jpg"
date: 2024-07-06 09:50
ogImage:
  url: /it-shar.ing/assets/no-image.jpg
tag: Tech
originalTitle: "Understanding CSS Progress Bars"
link: "https://dev.to/code_passion/understanding-css-progress-bars-795"
---

웹 개발 세계에서 시각적으로 매력적이고 사용자 친화적인 인터페이스를 갖는 것이 중요합니다. 진행 표시 막대는 이러한 목표 달성에 중요한 역할을 합니다. 진행 표시 막대는 사용자에게 준비 상태와 피드백을 제공하는데만 그치지 않고 전체 사용자 경험을 향상시킵니다. 진행 표시 막대를 구현하는 다양한 방법이 있지만 CSS는 유연하고 다양한 접근 방식을 제공합니다. 이 글에서는 CSS 진행 표시 막대인 구조, 기능, 스타일 옵션 및 권장 구현 방법에 대해 알아보겠습니다.

원형 CSS 진행 표시 막대의 구조
진행 표시 막대는 과제 또는 과정의 완료 상태를 그래픽으로 표현한 것입니다. CSS를 사용하면 개발자들이 복잡한 JavaScript 도구나 프레임워크에 의존하지 않고도 간단한 마크업 및 스타일링 기술로 진행 표시 막대를 만들 수 있습니다. CSS 변수인 너비, 배경색 및 테두리 반경과 같은 것들을 활용하여 개발자들은 진행 표시 막대의 모양을 디자인 선호도 및 브랜딩 요구 사항에 맞게 조정할 수 있습니다.

원형 진행 표시 막대 동작 방식
원형 진행 표시 막대는 사용자 인터페이스에서 작업 또는 과정의 상태를 표시하는 데 사용되는 시각적인 지표입니다. 원형 진행 표시 막대는 좌에서 우로가 아닌 시계 방향으로 원 주위에 채워집니다. CSS 진행 표시 막대의 더 많은 예시를 보려면 아래를 참고하세요.

멋진 CSS 진행 표시 막대 예시

<div class="content-ad"></div>

1. CSS 진행 표시줄 스타일링을 사용하여 회전하는 원 테두리 효과를 만들었습니다.
   결과 -

![회전하는 원](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fzllzwfrstmualt26so2j.gif)

이제 단계별로 살펴보겠습니다:

```js
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>회전하는 원 테두리</title>
<style>
  body
  {
    margin: 0 auto;
    width: 500px;
  }
  .circle {
    margin-top: 200px;
    width: 400px;
    height: 400px;
    border: 7px solid transparent;
    border-radius: 50%;
    border-top-color: #4caf50;
    animation: rotate 2s linear infinite;
  }

  @keyframes rotate {
    from {
      transform: rotate(0deg);
    }
    to {
      transform: rotate(360deg);
    }
  }
</style>
</head>
<body>
<div class="circle"></div>
</body>
</html>
```

<div class="content-ad"></div>

테두리는 원의 테두리를 정의합니다. 초기에는 5픽셀의 고정 불투명도로 설정됩니다. 이는 테두리가 처음에는 보이지 않지만 너비가 5픽셀인 것을 의미합니다.

테두리 반지름은 50%로 설정되어 테두리를 원 형태로 만듭니다.

테두리의 상단 색상은 원의 상단 가장자리 색상으로 설정됩니다. 이 경우에는 그린빛의 #4caf50으로 설정되어 있습니다.

애니메이션 속성은 애니메이션을 회전하기 위해 사용됩니다.

<div class="content-ad"></div>

@keyframes rotate은 회전 애니메이션을 정의합니다:

- from은 애니메이션의 시작 상태를 지정하며, 원이 0도로 회전된 상태입니다 (즉, 회전 없음).
- to는 애니메이션의 종료 상태를 지정하며, 원이 360도로 회전된 상태입니다 (즉, 한 바퀴 회전).

설명:

- 웹 브라우저에서 이 HTML 파일을 로드하면, 회전 테두리가 있는 원 형태 요소(div)가 있는 웹페이지가 표시됩니다.
- CSS에서 정의된 rotate 애니메이션으로 인해 원의 테두리가 중심을 중심으로 계속 회전합니다.
- 애니메이션의 지속 시간은 2초로 설정되어 있어, 그 시간 안에 원이 한 바퀴 회전합니다.
- 애니메이션 타이밍 함수는 linear로 설정되어 있어, 애니메이션 중에 회전 속도가 일정합니다.
- infinite 키워드를 사용하여 애니메이션이 무한히 반복되도록 설정되어 있습니다.

<div class="content-ad"></div>

결론
CSS 프로그레스 바는 개발자가 시각적으로 매력적이고 작동하는 사용자 경험을 설계하는 데 도움이 되는 유용한 도구입니다. 개발자는 프로그레스 바의 모양과 행동을 자신의 디자인 취향에 맞게 사용자 정의할 수 있어 전체 사용자 경험을 향상시킬 수 있습니다. 구현 시 최상의 사례를 따르면 개발자는 프로그레스 바가 웹 앱에 접근할 수 있고 성능이 우수하며 원활하게 통합되도록 보장할 수 있습니다. 이 책을 통해 CSS 프로그레스 바를 잘 이해하고 웹 개발 프로젝트를 새로운 수준으로 이끌 수 있을 것입니다.
