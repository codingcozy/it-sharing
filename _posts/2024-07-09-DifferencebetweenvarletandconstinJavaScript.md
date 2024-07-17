---
title: "JavaScript에서 var, let, const의 차이점 제대로 이해하기"
description: ""
coverImage: "/assets/img/2024-07-09-DifferencebetweenvarletandconstinJavaScript_0.png"
date: 2024-07-09 17:36
ogImage:
  url: /assets/img/2024-07-09-DifferencebetweenvarletandconstinJavaScript_0.png
tag: Tech
originalTitle: "Difference between var, let and const in JavaScript"
link: "https://medium.com/@jackpritomsoren/difference-between-var-let-and-const-in-javascript-c6236899ca4d"
---

<img src="/assets/img/2024-07-09-DifferencebetweenvarletandconstinJavaScript_0.png" />

자바스크립트의 Var, Let, Const 차이점 이해하기

유연하고 인기 있는 프로그래밍 언어인 JavaScript를 사용하면 프로그래머들은 동적이고 인터랙티브한 온라인 앱을 만들 수 있습니다. 변수를 통해 데이터를 저장하고 변경하는 능력은 JavaScript의 기본 개념 중 하나입니다. Var, let, const는 JavaScript에서 변수를 선언하는 세 가지 주요 방법입니다. 명확하고 관리하기 쉬운 코드를 생성하기 위해 이러한 키워드가 서로 다른 방식으로 어떻게 다른지 인식하는 것이 중요합니다. 각각의 키워드는 특정한 목적을 가지고 있으며 독특한 범위를 가지고 있습니다. 이 게시물에서는 자바스크립트에서 var, let, const의 차이를 예제와 함께 살펴보겠습니다.

# Var:

<div class="content-ad"></div>

자바스크립트가 탄생한 이후로 var은 변수를 선언하는 가장 전통적인 방법이었습니다. var로 선언된 변수는 함수 스코프를 가지며, 이는 해당 변수가 지정된 함수 내에서 제한된 범위에 있음을 의미합니다. var 키워드를 사용하여 선언된 변수는 루프나 조건식과 같은 블록 내에서도 액세스할 수 있습니다.

다음은 var을 사용한 예시입니다:

```js
function exampleVar() {
  if (true) {
    var x = 10;
  }
  console.log(x); // 10이 출력됩니다
}

exampleVar();
```

이 예시에서 x는 if 블록 내에서 var을 사용하여 선언되었지만, 콘솔에 로깅할 때 블록 외부에서도 접근할 수 있습니다.

<div class="content-ad"></div>

# let:

let은 ECMAScript 6 (ES6)에 추가된 키워드로, 변수를 선언하는 더 일관되고 블록 스코프 변수를 선언하는 방법을 제공합니다. let 선언은 변수를 지정된 블록 내로 제한합니다. 이러한 이유로 let은 의도하지 않은 변수 호이스팅을 방지하고 스코프 관련 오류 가능성을 줄이는 데 특히 도움이 됩니다.

다음은 let을 사용하는 예시입니다:

```js
function exampleLet() {
  if (true) {
    let y = 20;
  }
  console.log(y); // ReferenceError: y is not defined 오류가 발생합니다
}

exampleLet();
```

<div class="content-ad"></div>

이 예시에서는 y가 if 블록 내부에서 let을 사용하여 선언되었으며, 블록 외부에서 접근할 수 없습니다. 블록 외부에서 y를 기록하려고 시도하면 ReferenceError가 발생합니다.

## const:

변경할 수 없는 변수인 상수(const)는 ES6의 새로운 키워드 const를 사용하여 선언됩니다. const는 정의된 블록에 제한되어 있으며, let처럼 블록 스코프를 가지고 있습니다. 따라서 const는 코드 블록이 실행되는 동안 변경되어서는 안 되는 변수를 선언하기에 좋은 선택입니다.

다음은 const를 사용하는 예시입니다:

<div class="content-ad"></div>

```js
function exampleConst() {
  const z = 30;
  z = 40; // TypeError가 발생합니다: 상수 변수에 할당할 수 없음
}

exampleConst();
```

이 예제에서 z는 const를 사용하여 선언되고 즉시 값 30을 할당받습니다. 40으로 재할당하려고 시도하면, const 변수는 재할당할 수 없기 때문에 TypeError가 발생합니다.

# 요약:

요약하면, var, let, const는 JavaScript에서 변수를 선언하는 세 가지 다른 방법으로, 각각이 자체 범위와 사용 사례를 가지고 있습니다:

<div class="content-ad"></div>

- var은 함수 범위를 갖고 있으며 JavaScript의 초기부터 존재했습니다.
- let은 블록 범위를 갖고 있으며 ES6에서 도입되었으며 더 예측 가능한 스코프 메커니즘을 제공합니다.
- const도 블록 범위를 갖고 있으며 초기화 후 재할당할 수 없는 상수를 선언하는 데 사용됩니다.

JavaScript 코드를 작성할 때 var 대신에 let과 const를 사용하여 예상치 못한 스코프 문제를 방지하고 더 예측 가능하고 유지보수 가능한 코드를 작성하세요. 변수를 재할당해야 할 경우 let을 사용하고 상수를 선언하려면 const를 사용하세요. 이 세 용어 간의 차이를 인식함으로써 보다 깨끗하고 신뢰할 수 있는 JavaScript 코드를 작성할 수 있습니다.

Github: https://github.com/jps27cse

Linkedin: https://www.linkedin.com/in/jps27cse
