---
title: "자바스크립트 폴리필Polyfill이란 무엇일까"
description: ""
coverImage: "/assets/img/2024-07-09-WhatisaPolyfillinJavaScript_0.png"
date: 2024-07-09 16:37
ogImage:
  url: /assets/img/2024-07-09-WhatisaPolyfillinJavaScript_0.png
tag: Tech
originalTitle: "What is a Polyfill in JavaScript?"
link: "https://medium.com/@Bharat2044/what-is-a-polyfill-in-javascript-3dcbcb57b526"
---

![Polyfill](/assets/img/2024-07-09-WhatisaPolyfillinJavaScript_0.png)

자바스크립트 컨텍스트에서 폴리필(polyfill)은 웹 브라우저가 네이티브로 지원하지 않는 기능을 제공하는 코드 조각(보통 자바스크립트로 작성됨)을 말합니다.

"폴리필"이란 용어는 "poly"란 다양한과 "fill"이란 메우기를 의미하여, 폴리필은 새로운 표준 기능을 오래된 브라우저나 지원하지 않는 환경으로 가져오기 위해 사용됩니다.

새로운 자바스크립트 기능이나 웹 표준이 변경될 때, 모든 브라우저가 즉시 해당 변경 사항을 구현하지는 않습니다. 이는 개발자가 최신 언어 기능이나 API를 사용하고자 하지만 여전히 오래된 브라우저를 지원해야하는 상황을 만들 수 있습니다. 이러한 경우, 개발자들은 폴리필을 포함하여 필요한 기능이 더 넓은 범위의 브라우저에서 사용 가능하도록 할 수 있습니다.

<div class="content-ad"></div>

폴리필은 일반적으로 새로운 API를 흉내내어 오래된 브라우저에 대비 기능을 제공합니다. 폴리필은 또한 각 브라우저에서 다르게 구현된 기능을 모든 브라우저에서 동일하게 작동하도록 할 수 있습니다.

폴리필에는 완벽성의 여러 수준이 있습니다:

- 완벽한 폴리필은 부작용없이 기능을 완벽하게 구현하는 폴리필입니다. 예를 들어, JSON 라이브러리는 전역 JSON 객체를 지원하지 않는 이전 브라우저에 JSON.stringify와 JSON.parse를 완벽하게 구현하는 폴리필의 한 예입니다.
- 일반 폴리필은 거의 완전한 기능을 구현하는 폴리필입니다. 일부 작은 기능이 누락되었거나, 일부 엣지 케이스가 제대로 작동하지 않을 수 있으며, 약간의 거슬리는 부작용이 있을 수 있습니다. 대부분의 폴리필이 이 범주에 속합니다.
- 부분적 폴리필은 특정 기능을 구현하지만 많은 기능이 누락되거나 작동하지 않을 수 있습니다. 예를 들어 ES5 쉼은 ECMAScript 3 엔진을 위해 ECMAScript 5 기능을 많이 구현하지만, 여러 중요한 기능이 누락되어 있습니다.
- 후폴 리필은 새로운 기능을 전혀 구현하지 않지만, 오래된 브라우저에 대한 우아한 페폴 리필 동작을 확인합니다. 예를 들어 웹 워커 폴리필이 있습니다. HTML5에서 웹 워커를 사용하면 JavaScript 코드를 여러 스레드에서 실행할 수 있지만, 이를 오래된 브라우저에서는 할 수 있는 방법이 없습니다. 이 폴리필은 모든 멀티 스레드 코드를 단일 스레드에서 실행하여 오래된 브라우저에서 코드가 실행되도록 하지만, 코드 실행 중에 브라우저가 멈출 것입니다.

보통 폴리필은 특징 감지와 특징 구현이라는 두 가지 기본 구성 요소를 갖고 있습니다.

<div class="content-ad"></div>

# 1. 기능 감지

먼저, 주어진 기능이 이미 브라우저에 구현되어 있는지 테스트해야 합니다. 그렇다면, 이미 존재하는 것을 다시 구현할 필요는 없으며 여기서 중지해야 합니다. 그렇지 않고 브라우저가 실제로 그 기능을 누락하고 있다면, 다음 단계로 진행할 수 있습니다.

이 단계는 기존 동작을 무시하고 대체하는 폴리필에서 제외됩니다. 대신에 고유한 구현을 사용하는데, 이 접근 방식은 모든 브라우저가 예상한 대로 동작하도록 보장하지만, 기능의 네이티브 구현을 대체함으로써 잠재적으로 불필요한 오버헤드를 발생시킬 수 있는 단점이 있습니다.

# 2. 기능 구현

<div class="content-ad"></div>

이 부분은 실제로 누락된 기능을 구현하는 폴리필의 핵심입니다.

# 첫 번째 폴리필 예제: 프로토타입 메서드 추가

ECMAScript 5에서는 배열을 조작하는 함수형 프로그래밍 접근을 강조하면서 많은 새로운 Array 프로토타입 메서드가 추가되었습니다. 여기에는 함수를 수용하고 해당 함수가 true를 반환하는 경우에만 원래 배열의 값을 포함하는 배열을 반환하는 filter 메서드가 있습니다. 예를 들어 배열에서 짝수 값만 포함하도록 배열을 필터링할 수 있습니다.

자바스크립트:

<div class="content-ad"></div>

```js
let isEven = function (n) {
  return n % 2 === 0;
};
```

```js
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10].filter(isEven);
// returns [2, 4, 6, 8, 10]
```

filter 메서드에는 함수의 this 값에 바인딩하기 위한 선택적 두 번째 매개변수가 있습니다. 예를 들어, filter에 전달된 함수에 this로 객체를 바인딩할 수 있습니다.

JavaScript:

<div class="content-ad"></div>

```js
let fruits = {
  banana: "yellow",
  strawberry: "red",
  pumpkin: "orange",
  apple: "red",
};
let isRedFruit = function (name) {
  return this[name] === "red";
};
["pumpkin", "strawberry", "apple", "banana", "strawberry"].filter(isRedFruit, fruits);
// returns ["strawberry", "apple", "strawberry"]
```

이 기능은 ES5에서 추가된 것이기 때문에 인터넷 익스플로러 8 이하와 같은 이전 브라우저는 filter 메서드를 지원하지 않습니다. 다행히도 이 기능을 대체할 수 있는 폴리필을 쉽게 만들 수 있습니다.

먼저, filter 메서드가 이미 지원되는지 여부를 feature detection으로 확인해야 합니다. 이 경우에는 Array 프로토타입에 filter라는 함수가 있는지 확인하면 됩니다. 그렇지 않다면 이를 생성할 수 있습니다.

JavaScript:

<div class="content-ad"></div>

```js
if (typeof Array.prototype.filter !== "function") {
  Array.prototype.filter = function () {
    // 여기에 구현 코드를 작성하세요
  };
}
```

이제 우리는 구현을 작성할 수 있어요. 에지 케이스를 확인하는지 주목해주세요.

JavaScript:

```js
Array.prototype.filter = function (fn, thisp) {
  if (this === null) throw new TypeError();
  if (typeof fn !== "function") throw new TypeError();
  let result = [];
  for (let i = 0; i < this.length; i++) {
    if (i in this) {
      let val = this[i];
      if (fn.call(thisp, val, i, this)) {
        result.push(val);
      }
    }
  }
  return result;
};
```

<div class="content-ad"></div>

그것은 완벽한 폴리필이 아니라 일반적인 폴리필임을 알 수 있습니다. 이것은 "filter"를 각 배열에 대한 열거 가능한 속성으로 추가함으로써 for..in 루프에서 예상치 못한 동작을 유발합니다.

자바스크립트:

```js
let arr = [0, 1, 2];
for (let i in arr) {
  console.log(i);
}
```

```js
//LOG: 0
//LOG: 1
//LOG: 2
//LOG: filter
```

<div class="content-ad"></div>

# 두 번째 폴리필 예제: 'Blink' 태그 지원

이것은 실용적이기보다는 재미있는 학습 예제입니다. 이렇게 하는 것이 실제로는 해로울 수 있으니 주의하세요. 'Blink' 태그는 내용을 깜박거리게 만드는 비표준 요소입니다. 다행히 이 태그를 지원하는 현대 브라우저는 Mozilla Firefox와 Opera 뿐입니다.

안타깝게도 브라우저가 'Blink' 태그를 지원하는지 감지하는 간단한 방법은 알려져 있지 않습니다 (하지만 알게 되면 듣고 싶습니다). 즉, 브라우저의 내장 동작을 덮어쓸 폴리필을 작성해야 합니다.

기능 감지가 첫 번째 단계가 아니라 모든 'Blink' 태그를 다른 태그로 대체하는 것부터 시작될 것입니다. 해당 태그에는 브라우저에서 깜박거리지 않을 다른 태그를 사용하며, 기존 스타일과 충돌하지 않을 것입니다. 여기서 모든 'Blink' 태그를 'Blinky' 태그로 대체할 것입니다.

<div class="content-ad"></div>

자바스크립트:

```js
(function replaceBlinks() {
  let blinks = document.getElementsByTagName("blink");
  while (blinks.length) {
    let blink = blinks[0];
    let blinky = document.createElement("blinky");
    blinky.innerHTML = blink.innerHTML;
    blink.parentNode.insertBefore(blinky, blink);
    blink.parentNode.removeChild(blink);
  }
})();
```

그래서 그 문제는 해결되어, 실제 깜박임 동작을 구현할 수 있게 되었어요.

자바스크립트:

<div class="content-ad"></div>

```js
(function blink(visible) {
  let blinkies = document.getElementsByTagName("blinky"),
    visibility = visible ? "visible" : "hidden";
  for (let i = 0; i < blinkies.length; i++) {
    blinkies[i].style.visibility = visibility;
  }
  setTimeout(function () {
    blink(!visible);
  }, 500);
})(true);
```

그리고 와라! 우리는 모든 브라우저에서 blink 태그가 작동하는 폴리필을 만들었습니다. 이를 확인하려면 이 데모 페이지에서 확인할 수 있어요.

이것은 완벽한 폴리필이 아닌 일반적인 폴리필이기 때문에 몇 가지 이유가 있습니다:

- 깜박임에 대한 CSS 스타일은 아무런 효과가 없습니다. 대신 스타일을 blinky에 적용하면 효과가 날 수 있어요.
- 이미 blink를 지원하는 브라우저의 구현을 덮어쓰며, 결국
- 이 방법은 브라우저에서 정의한 깜박임 시간 간격을 유지하지 않습니다. 예를 들어, Firefox에서는 깜박임이 0.75초 동안 보이고 0.25초 동안 보이지 않습니다. Opera는 다른 시간 간격 정의를 가질 수 있어요. 사용자 에이전트가 이미 결정한 것과 관계없이, 우리의 구현은 0.5초마다 보이거나 보이지 않음을 번갈아 가면서 전환하는 거예요.

<div class="content-ad"></div>

MDN 참조
