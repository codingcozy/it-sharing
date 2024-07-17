---
title: "웹사이트에 생명을 불어넣는 CSS 애니메이션 소개 기초부터 고급까지"
description: ""
coverImage: "/it-shar.ing/assets/no-image.jpg"
date: 2024-07-13 18:21
ogImage:
  url: /it-shar.ing/assets/no-image.jpg
tag: Tech
originalTitle: "Introduction to CSS Animation: Bringing Websites to Life"
link: "https://dev.to/code_passion/introduction-to-css-animation-bringing-websites-to-life-5bn3"
---

웹 개발 분야에서 웹 사이트의 미학은 사용자를 끌어들이고 그들의 경험을 향상시키는 데 중요합니다. CSS 애니메이션은 개발자에게 효과적인 도구입니다. CSS 애니메이션을 사용하면 개발자가 정적인 웹 요소에 생명을 불어넣어 시각적으로 매력적이고 다이내믹한 사용자 인터페이스를 만들어 방문자들을 매료시킬 수 있습니다.

CSS 애니메이션 이해하기
CSS 애니메이션을 사용하면 JavaScript나 다른 도구 없이도 HTML 요소를 애니메이트할 수 있습니다. 키프레임, 트랜지션 및 기타 CSS 속성을 사용하여 개발자가 요소의 모양과 동작을 시간에 걸쳐 제어할 수 있습니다.

주요 개념

1. 키프레임
   CSS의 키프레임은 애니메이션 순서 중 특정 시점에서 요소가 가져야 하는 스타일을 정의합니다. 요소가 애니메이트되는 동안 따라갈 경로에 중간점을 설정하는 것을 고려해 보세요. 이러한 중간점은 0%에서 100%까지의 백분율로 제공되며 애니메이션의 각 단계에서 요소가 어떻게 나타나야 하는지 정의합니다. (더 읽기)

<div class="content-ad"></div>

구문과 사용법
CSS에서 키프레임을 정의하려면 애니메이션 시퀀스의 이름이 지정된@keyframes 규칙을 사용하십시오. @keyframes 블록 내에서는 특정 지점에서 적용해야 할 백분율 값(또는 from 및 to와 같은 키워드) 및 CSS 속성과 값도 지정합니다.

다음은 기본 예제입니다.

```js
@keyframes slide-in {
  from {
    transform: translateX(-100%);
  }
  to {
    transform: translateX(0);
  }
}
```

이 예제에서 slide-in 애니메이션은 요소를 왼쪽에서 오른쪽으로 슬라이드하며 뷰포트 바깥쪽(-100%의 너비)에서 시작하여 원래 위치(0%)에서 끝냅니다.

<div class="content-ad"></div>

키프레임 적용하기
키프레임을 설정한 후 CSS의 애니메이션 속성을 사용하여 요소에 적용하세요. 이 속성을 사용하여 애니메이션의 이름, 지속 시간, 타이밍 함수, 지연 시간 및 기타 특성을 제공할 수 있습니다.

```js
.element {
  animation: slide-in 1s ease-out;
}
```

.slide-in 애니메이션이 .element 클래스에 할당되며, 1초 동안 지속되며 부드러운 가속 및 감속을 위해 이징 함수를 사용합니다.

예시를 살펴보겠습니다.
이 애니메이션은 텍스트를 수평으로 흔들게 만듭니다.

<div class="content-ad"></div>

아래는 웹 페이지에 CSS 키프레임 애니메이션을 포함시키고 매력적인 시각 효과를 생성하는 방법을 보여주는 HTML 예제입니다.

```js
<!DOCTYPE html>
<html>
<head>
    <meta charset='utf-8'>
    <meta http-equiv='X-UA-Compatible' content='IE=edge'>
    <title>페이지 제목</title>
    <meta name='viewport' content='width=device-width, initial-scale=1'>
  <style>
    .shaker {
  font-size: 24px;
  animation: shake 0.5s cubic-bezier(0.36, 0.07, 0.19, 0.97) infinite;
}
div
{
    width:500px;
    margin: 0 auto;
    margin-top:300px;
}

@keyframes shake {
  0%, 100% { transform: translateX(0); }
  25% { transform: translateX(-5px); }
  50% { transform: translateX(5px); }
  75% { transform: translateX(-3px); }
  100% { transform: translateX(3px); }
}


  </style>

</head>
<body>
    <div class="shaker">흔들리는 텍스트</div>
</body>
</html>
```

<img src="https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F2rh86ihigypqi4lxnvqb.gif" />

<div class="content-ad"></div>

CSS 애니메이션의 애니메이션 속성
CSS 애니메이션 속성을 사용하면 항목이 웹 페이지에서 이동, 변형 및 전환하는 방식을 정확하게 제어할 수 있습니다. 이러한 기능을 사용하면 JavaScript나 외부 라이브러리를 사용하지 않고도 매력적인 애니메이션을 만들 수 있습니다. (더 많은 CSS 애니메이션 예제를 보려면 여기를 클릭하세요)
구문:

```js
animation: 이름 지속시간 타이밍 함수 지연 반복 횟수 방향 채우기 모드 재생 상태;
```

다른 예제를 살펴보겠습니다

output:

<div class="content-ad"></div>

```js
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Cubic Bezier Bouncing Animation Example</title>

<style>

.ball {
    width: 100px;
    height: 100px;
    background-color: #df6412;
    border-radius: 50%;
    position: absolute;
    top: 50%;
    left: 50%;
    animation: bounce 1.5s cubic-bezier(0.68, -0.55, 0.265, 1.55) infinite;
  }

  @keyframes bounce {
    0%, 100% {
      transform: translateY(0);
    }
    50% {
      transform: translateY(-100px);
    }
  }

</style>
</head>
<body>
<div class="ball"></div>
</body>
</html>
```

여기 예시에서

- 애니메이션 속성은 키프레임 애니메이션을 사용하여 공 요소에 바운스 효과를 추가합니다.
- 애니메이션에서 사용된 cubic Bézier 타이밍 함수는 특정 제어점이 있는 cubic-bezier(0.68, -0.55, 0.265, 1.55) 함수로 정의됩니다.
- 바운스 키프레임 애니메이션은 바운스 효과를 만들어냅니다. 초기 상태(0%)에서 시작하여 반반 이후(50%) 공이 100px만큼 위로 이동합니다(translateY(-100px)), 그리고 다시 초기 상태(100%)로 돌아갑니다.
- cubic Bézier 곡선의 제어점을 조정함으로써 바운스 애니메이션의 타이밍과 동작을 사용자가 원하는대로 맞춤화할 수 있습니다.

<div class="content-ad"></div>

## 결론

CSS 애니메이션 기능은 개발자들에게 시각적으로 아름답고 상호 작용적인 웹 경험을 만들기 위한 다양한 도구를 제공합니다. 이러한 기능을 숙달하고 새로운 방식으로 통합함으로써, 개발자들은 자신의 디자인을 생동감 있게 만들고 사용자 참여도를 높일 수 있습니다. CSS 애니메이션은 실험과 연습을 통해 가장 잘 배울 수 있습니다. (CSS 애니메이션에 대해 더 읽기)
