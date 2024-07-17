---
title: "CSS Rotate 속성 완벽 가이드"
description: ""
coverImage: "/it-shar.ing/assets/no-image.jpg"
date: 2024-07-09 08:13
ogImage:
  url: /it-shar.ing/assets/no-image.jpg
tag: Tech
originalTitle: "CSS Rotate Property Explained"
link: "https://dev.to/code_passion/css-rotate-property-explained-1j48"
---

CSS Rotate 속성 이해하기:
회전 속성은 CSS 변환 모듈의 일부로, 개발자가 웹페이지의 요소에 다양한 변환을 적용할 수 있게 해줍니다. 회전 기능을 사용하면 지정된 각도로 항목을 회전하여 방향을 변경할 수 있지만 문서 흐름에서의 위치는 변경되지 않습니다. 이 특성은 시계방향 및 반시계방향 회전을 모두 허용함으로써 엄청난 다양성을 제공합니다.

구문과 사용법:

CSS 회전 속성의 구문은 매우 간단합니다. 개발자는 회전 함수의 괄호 안에 원하는 회전 각도를 정의합니다.

```js
.rotate {
    transform: rotate(45deg);
}
```

<div class="content-ad"></div>

이 예시에서 .rotate 클래스는 대상 요소를 시계방향으로 45도 회전시킵니다. 각도는 도(deg), 래디안(rad) 및 그라디언트(grad)와 같은 다양한 단위로 작성할 수 있으며, 개발자들이 필요에 맞는 가장 적절한 단위를 선택할 수 있도록 합니다. (CSS 회전 속성의 더 많은 예제 보기)

실제 예시-

CSS Transform 속성을 사용하여 애니메이션 효과를 준 뒤집힌 카드
결과-

![애니메이션 효과를 준 뒤집힌 카드](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fq2oj8ms99k03gebip0c8.gif)

<div class="content-ad"></div>

Markdown:

```css
.card-container {
  perspective: 1000px;
}
.card {
  width: 200px;
  height: 200px;
  position: relative;
  transform-style: preserve-3d;
  transition: transform 0.6s;
}
.card.flip {
  transform: rotateY(180deg);
}
.card .front,
.card .back {
  width: 100%;
  height: 100%;
  position: absolute;
  backface-visibility: hidden;
}
.card .back {
  transform: rotateY(180deg) translateZ(1px);
}
```

<div class="content-ad"></div>

이 코드는 간단한 CSS 기반의 카드 뒤집기 애니메이션을 생성합니다. 한 단계씩 알아보겠습니다:

- .card-container: 카드를 포함하는 컨테이너 요소에 할당된 클래스입니다. perspective 속성은 카드가 렌더링되는 3D 공간의 깊이를 지정합니다. 더 큰 값은 더 눈에 띄는 3D 효과를 만듭니다.
- .card: 각 개별 카드에 해당하는 클래스입니다. 너비와 높이는 200픽셀로 고정되어 있으며 위치가 상대적으로 설정되어 있습니다. transform-style: preserve-3d 기능은 카드의 하위 요소가 변환될 때 3D 위치를 유지하도록 보장합니다.
- .card.flip: 카드를 뒤집어야 할 때 추가되는 클래스입니다. rotateY(180deg)를 사용하여 카드를 Y축을 중심으로 180도 회전시키는 뒤집힌 효과를 제공합니다. transform 속성은 전환의 지속 시간(0.6초)과 이징을 제공합니다.
- .card .front, .card .back: 이러한 클래스는 각각 카드의 앞면과 뒷면을 나타냅니다. 카드 요소 내에서 완벽하게 위치하며 전체 너비와 높이를 차지합니다. backface-visibility: hidden 기능은 카드가 앞을 향할 때 뒷면이 보이지 않도록 합니다.
- .card .back: 이 세미나는 카드의 뒷면에만 집중합니다. 초기에는 Y축을 따라 180도 회전하고 Z축을 따라 한 픽셀만큼 이동합니다. 이 이동은 뒷면이 정확히 앞면 뒤에 있어 발생할 수 있는 깜빡임이나 Z-플라이팅 문제를 피하기 위해 필요합니다.

자바스크립트:

```js
function togglebuton() {
  document.getElementById("card").classList.toggle("flip");
}
```

<div class="content-ad"></div>

document.getElementById을 사용하여 ID 카드가 있는 HTML 요소를 선택합니다. document 객체는 웹페이지를 나타내며 getElementById는 ID 속성으로 요소를 검색하는 메서드입니다.

.classList.toggle('flip'); :- 이 부분은 선택한 요소의 classList 속성을 사용하며, 요소의 클래스를 조작하는 메서드를 제공합니다. toggle 메서드는 'flip'인자와 함께 호출됩니다. 이 메서드는 요소에 'flip' 클래스를 추가하거나 이미 존재한다면 제거합니다.

결론
CSS 회전 기능은 웹 항목에 회전 효과를 적용하기 위한 다재다능하고 효과적인 도구입니다. 이 속성을 효과적으로 사용하는 방법을 이해하면 간단한 회전부터 복잡한 애니메이션까지 생성할 때 온라인 애플리케이션의 시각적 매력과 상호작용을 크게 향상시킬 수 있습니다. 다양한 각도, 전환 및 조합을 실험하여 디자인에서 회전 기능의 모든 가능성을 발휘해보세요.
