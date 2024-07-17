---
title: "초보자를 위한 JavaScript 디버깅 테크닉과 기본 개념"
description: ""
coverImage: "/assets/img/2024-07-07-JavaScriptDebuggingTechniquesandBasicConceptsforBeginners_0.png"
date: 2024-07-07 18:58
ogImage:
  url: /assets/img/2024-07-07-JavaScriptDebuggingTechniquesandBasicConceptsforBeginners_0.png
tag: Tech
originalTitle: "JavaScript Debugging Techniques and Basic Concepts for Beginners"
link: "https://medium.com/@AlexanderObregon/javascript-debugging-techniques-and-basic-concepts-for-beginners-1ba493c644a0"
---

![image](/assets/img/2024-07-07-JavaScriptDebuggingTechniquesandBasicConceptsforBeginners_0.png)

# 소개

디버깅은 모든 JavaScript 개발자에게 중요한 기술입니다. 버그를 신속하고 효율적으로 식별하고 수정할 수 있다면 많은 시간과 불평을 절약할 수 있습니다. 이 기사는 브라우저의 개발자 도구, 중단점 및 콘솔 메서드를 활용한 JavaScript의 다양한 디버깅 개념과 기술에 대한 기본 개요를 제공합니다. 이러한 기술은 디버깅 프로세스를 간소화하고 코드를 개선하는 데 도움이 될 수 있습니다.

# 브라우저 개발자 도구 사용하기

<div class="content-ad"></div>

현대 웹 브라우저인 Google Chrome, Mozilla Firefox, Microsoft Edge 및 Safari와 같은 브라우저 개발자 도구는 내장 도구입니다. 이러한 도구는 웹 애플리케이션을 디버그하고 최적화하는 데 도움이 되는 여러 기능을 제공합니다.

## 개발자 도구 열기

대부분의 브라우저에서 개발자 도구를 열려면 F12 또는 Ctrl+Shift+I(윈도우/리눅스) 또는 Cmd+Opt+I(Mac)를 누를 수 있습니다. 또는 웹 페이지를 마우스 오른쪽 버튼으로 클릭하고 "검사" 또는 "요소 검사"를 선택할 수도 있습니다. 이렇게 하면 브라우저 창의 하단이나 측면에 다양한 도구와 옵션이 표시되는 패널이 열립니다.

## 개발자 도구의 중요 기능

<div class="content-ad"></div>

요소 패널: 이 패널은 웹페이지의 HTML 및 CSS를 검사하고 편집할 수 있게 해줍니다. 변경 사항이 페이지에 어떻게 영향을 미치는지 실시간으로 확인할 수 있습니다. 레이아웃 문제를 진단하거나 스타일을 실험하는 데 특히 유용합니다. 다음 작업을 수행할 수 있습니다:

- DOM 트리 검사
- HTML 요소 수정
- CSS 스타일 변경
- 실시간 업데이트 확인

콘솔 패널: 콘솔은 JavaScript 디버깅에 강력한 도구입니다. 메시지를 기록하고 JavaScript 코드를 실행하며 오류 및 경고를 확인할 수 있습니다. 콘솔은 즉각적인 피드백을 제공하여 신속한 문제 해결에 도움이 됩니다. 다음을 수행할 수 있습니다:

- console.log()를 사용하여 메시지 기록
- console.dir()로 객체 검사
- console.error()로 오류 표시
- console.clear()로 콘솔 지우기

<div class="content-ad"></div>

**Sources Panel:**  
이 패널을 사용하면 JavaScript 코드를 보거나 디버깅할 수 있습니다. 중단점을 설정하고 코드를 단계별로 실행하며 변수를 검사할 수 있습니다. 페이지에서 실행 중인 모든 스크립트를 자세히 볼 수 있습니다. 다음을 할 수 있습니다:

- JavaScript 파일을 탐색합니다.
- 실행을 일시 중단할 중단점을 설정합니다.
- 코드를 한 줄씩 실행합니다.
- 변수 및 값 검사합니다.

**Network Panel:**  
이 패널을 사용하면 웹페이지가 생성하는 모든 네트워크 요청을 볼 수 있습니다. 리소스 로딩 및 AJAX 요청과 관련된 문제를 디버깅하는 데 도움이 됩니다. 각 요청의 시간, 상태 및 데이터를 볼 수 있습니다. 다음을 할 수 있습니다:

- 모든 HTTP 요청을 모니터링합니다.
- 각 요청의 상태를 확인합니다.
- 요청 및 응답 헤더를 보여줍니다.
- 요청 시간을 확인합니다.

<div class="content-ad"></div>

성능 패널: 이 패널을 통해 웹 애플리케이션의 성능을 분석할 수 있습니다. 실행 시 성능을 기록하고 검토하여 병목 현상을 식별할 수 있습니다. 앱의 속도와 응답성을 최적화하는 데 유용합니다. 다음을 할 수 있습니다:

- 웹페이지의 성능을 기록
- 활동 타임라인 분석
- 실행 속도가 느린 스크립트 식별
- 렌더링 성능 최적화

애플리케이션 패널: 이 패널은 쿠키, 로컬 스토리지 및 세션 스토리지를 검사하고 관리하는 도구를 제공합니다. 또한 서비스 워커와 웹 앱 매니페스트를 검사하고 디버깅할 수 있습니다. 다음을 할 수 있습니다:

- 쿠키 보기 및 관리
- 로컬 및 세션 스토리지 검사
- 서비스 워커 디버깅
- 웹 앱 매니페스트 검토

<div class="content-ad"></div>

보안 패널: 이 패널을 통해 웹 애플리케이션의 보안을 검사할 수 있습니다. SSL/TLS 구성을 확인하고 보안 관련 문제를 확인할 수 있습니다. 이를 통해 애플리케이션이 보안을 위한 최선의 방법을 따르는지 확인할 수 있습니다. 다음을 할 수 있습니다:

- 연결의 보안성 확인
- SSL/TLS 인증서 확인
- 혼합 콘텐츠 문제 식별
- 보안 관련 헤더 검사

메모리 패널: 이 패널을 통해 메모리 사용량을 프로파일링하고 메모리 누수를 식별할 수 있습니다. 힙 스냅샷을 분석하고 메모리 할당을 추적하는 도구를 제공합니다. 애플리케이션의 메모리 풋프린트를 최적화하는 데 중요합니다. 다음을 할 수 있습니다:

- 힙 스냅샷 촬영
- 메모리 할당 분석
- 메모리 누수 식별
- 메모리 사용량 최적화

<div class="content-ad"></div>

## 예시: 콘솔 패널 사용하기

콘솔 패널은 자바스크립트를 디버깅하는 개발자들이 가장 먼저 찾는 곳입니다. console.log 메서드를 사용하여 메시지를 콘솔에 기록할 수 있습니다. 예를 들어:

```js
console.log("안녕하세요, 세계!");
```

이 코드는 "안녕하세요, 세계!"를 콘솔에 출력합니다. 또한 변수와 객체를 로깅하여 값들을 검사할 수도 있습니다.

<div class="content-ad"></div>

```js
let user = { name: "Alex", age: 28 };
console.log(user);
```

또한 콘솔은 계산을 수행하거나 코드 스니펫을 실시간으로 테스트하는 데 사용할 수 있습니다. 예를 들어:

```js
let a = 5;
let b = 3;
console.log(a + b); // 출력: 8
```

콘솔은 또한 console.warn()과 console.error()과 같은 다양한 수준의 메시지를 표시하기 위한 메서드를 제공합니다. 이는 코드에서 문제를 분류하고 우선순위를 지정하는 데 도움이 될 수 있습니다:

<div class="content-ad"></div>

```js
console.warn("이것은 경고 메시지입니다.");
console.error("이것은 오류 메시지입니다.");
```

# 중단점과 디버깅 구현

중단점은 JavaScript 디버깅의 강력한 기능으로, 코드 실행을 특정 지점에서 일시 중지할 수 있습니다. 이를 통해 애플리케이션의 상태를 검사하고 동작을 이해할 수 있습니다. 중단점을 효과적으로 구현하면 디버깅 프로세스를 크게 간소화하고 문제를 식별하고 해결하기 쉬워집니다.

## 중단점 설정

<div class="content-ad"></div>

브레이크포인트를 설정하려면 브라우저의 개발자 도구에서 '소스' 패널을 열고 디버그하려는 JavaScript 파일로 이동하세요. 브레이크포인트를 설정하려는 행 번호를 클릭하세요. 브레이크포인트가 설정되었음을 나타내는 파란색 표식이 나타납니다.

다음은 간단한 예제입니다:

```js
function calculateSum(a, b) {
  let sum = a + b;
  return sum;
}

let result = calculateSum(5, 3);
console.log(result);
```

이 예제에서 'let sum = a + b;' 행에 브레이크포인트를 설정할 수 있습니다. 코드를 실행하면 실행이 이 행에서 일시 중지되어 a와 b의 값을 검사할 수 있습니다.

<div class="content-ad"></div>

## 조건부 중단점

가끔은 특정 조건이 충족될 때에만 실행을 일시 중지하고 싶을 수 있습니다. 조걀부 중단점을 설정하려면 라인 번호를 우클릭한 후 "조건부 중단점 추가"를 선택하십시오. 나타나는 대화 상자에 조건을 입력하십시오.

예를 들어:

```js
function calculateSum(a, b) {
  let sum = a + b;
  return sum;
}

for (let i = 0; i < 10; i++) {
  let result = calculateSum(i, 2);
  console.log(result);
}
```

<div class="content-ad"></div>

이 경우에는 let sum = a + b;에 조건 a === 5를 설정하여 조건부 중단점을 설정할 수 있습니다. 실행은 a가 5와 동일할 때에만 일시 중지됩니다.

## 코드 단계별 실행

중단점이 도달하면 실행이 일시 중지되고 다음 컨트롤을 사용하여 코드를 한 줄씩 실행할 수 있습니다:

- 스크립트 실행 재개 (F8): 다음 중단점이나 스크립트 끝까지 실행을 계속합니다.
- 다음 라인으로 건너뛰기 (F10): 함수 내부로 들어가지 않고 다음 코드 라인을 실행합니다.
- 함수 안으로 들어가기(F11): 다음 함수 호출 내부로 진입합니다.
- 함수 밖으로 나가기 (Shift+F11): 현재 함수에서 벗어납니다.

<div class="content-ad"></div>

더 복잡한 예제를 보여드리겠습니다:

```js
function multiply(a, b) {
  return a * b;
}

function calculateArea(length, width) {
  let area = multiply(length, width);
  return area;
}

let area = calculateArea(5, 4);
console.log(area);
```

여기서 multiply 함수 내에 중단점을 설정하고 "Step Into"를 사용하여 calculateArea에서 multiply로 제어 흐름이 어떻게 이동하는지 확인할 수 있습니다.

## Watch Expressions

<div class="content-ad"></div>

워치 표현식을 사용하면 코드를 진행하는 동안 변수와 표현식의 값을 모니터링할 수 있어요. 워치 표현식을 추가하려면 소스 패널을 열고 "워치" 섹션을 클릭한 후 원하는 표현식을 입력하세요.

예를 들어:

```js
let counter = 0;

function increment() {
  counter++;
}

for (let i = 0; i < 5; i++) {
  increment();
  console.log(counter);
}
```

루프를 따라 진행하는 동안 counter의 값이 어떻게 변하는지 확인하려면 counter에 대한 워치 표현식을 추가할 수 있어요.

<div class="content-ad"></div>

## 호출 스택

소스 패널의 호출 스택 섹션은 현재 함수 호출 스택을 보여줍니다. 이는 현재 중단점으로 이어진 함수 호출 순서를 이해하는 데 도움이 됩니다. 이는 실행 흐름을 추적하고 오류가 발생한 곳을 식별하는 데 유용합니다.

중첩된 함수 호출이 포함된 예시가 있습니다:

```js
function first() {
  second();
}

function second() {
  third();
}

function third() {
  debugger;
  console.log("In third function");
}

first();
```

<div class="content-ad"></div>

이 코드를 실행하면 디버거 문이 중단점을 트리거하고 호출 스택이 첫 번째 - `두 번째 - `세 번째를 보여줍니다.

## 스코프 변수

소스 패널의 스코프 섹션에는 현재 스코프에 있는 변수와 해당 값이 표시됩니다. 이를 통해 중단점에서 응용 프로그램 상태를 검사할 수 있습니다.

다음 예시를 살펴보세요:

<div class="content-ad"></div>

```js
function greet(name) {
  let greeting = "Hello, " + name;
  return greeting;
}

let message = greet("Alice");
console.log(message);
```

`let greeting = "Hello, " + name;` 에 중단점을 설정하면 Scope 섹션에서 name과 greeting의 값을 볼 수 있습니다.

## 비동기 코드의 중단점

비동기 코드를 디버깅하는 것은 도전적일 수 있지만, 중단점을 사용하면 코드가 시간에 따라 어떻게 실행되는지 이해하는 데 도움이 될 수 있습니다. setTimeout을 사용한 예제를 살펴보겠습니다:

<div class="content-ad"></div>

```js
function delayedGreeting(name) {
  setTimeout(function () {
    console.log("Hello, " + name);
  }, 2000);
}

delayedGreeting("Bob");
```

타임아웃이 완료되면 실행을 일시 중지하려면 setTimeout 콜백 내부에 중지점을 설정할 수 있습니다.

Promise를 사용한 좀 더 복잡한 비동기 예제는 다음과 같습니다:

```js
function fetchData() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve("Data received");
    }, 1000);
  });
}

fetchData().then((data) => {
  console.log(data);
});
```

<div class="content-ad"></div>

setTimeout 콜백 내부와 .then 블록 내부에 중단점을 설정하여 프로미스가 해결되는 방식과 데이터가 처리되는 방식 등을 확인할 수 있어요.

## 이벤트 리스너에서 중단점 설정하기

이벤트 리스너는 웹 애플리케이션에서 사용자 상호작용을 처리하는 일반적인 방법입니다. 이벤트 기반 코드를 디버깅하는 것은 어려울 수 있지만, 이러한 상호작용 중에 응용 프로그램의 동작과 상태를 검사하기 위해 이벤트 리스너 콜백 내부에 중지점을 설정할 수 있어요.

```js
document.getElementById("myButton").addEventListener("click", function () {
  let message = "Button clicked!";
  console.log(message);
});
```

<div class="content-ad"></div>

이벤트 리스너 내에서 중단점을 설정하여 버튼을 클릭하면 실행이 일시 중지되어 메시지 변수 및 기타 관련 상태를 검사할 수 있습니다.

## AJAX 요청에서 중단점

AJAX 요청을 디버깅하는 것은 응용 프로그램이 서버와 통신하는 방식을 이해하는 데 중요합니다. AJAX 콜백 내에서 중단점을 설정하여 보내고 받는 데이터를 검사할 수 있습니다.

```js
function fetchData() {
  let xhr = new XMLHttpRequest();
  xhr.open("GET", "https://api.example.com/data", true);
  xhr.onload = function () {
    if (xhr.status === 200) {
      let data = JSON.parse(xhr.responseText);
      console.log(data);
    }
  };
  xhr.send();
}

fetchData();
```

<div class="content-ad"></div>

## onload 콜백 내부에 중단점을 설정하여 응답을 받았을 때 xhr 객체 및 data 변수를 검사해보세요.

## 루프 내의 중단점

루프는 복잡한 논리나 대규모 데이터 집합과 관련된 경우 버그의 일반적인 원인이 될 수 있습니다. 루프 내에 중단점을 설정하면 각 반복의 상태를 검사하고 루프의 진행 상황을 이해할 수 있습니다.

```js
let numbers = [1, 2, 3, 4, 5];
let sum = 0;

for (let i = 0; i < numbers.length; i++) {
  sum += numbers[i];
  console.log(sum);
}
```

<div class="content-ad"></div>

루프 내에 중단점을 설정하여 각 반복마다 i, numbers[i], 그리고 sum의 값을 검사해보세요.

## 재귀 함수에서의 중단점

재귀 함수는 자기 자신을 호출하는 특성 때문에 디버깅하기 어려울 수 있습니다. 재귀 함수에 중단점을 설정하여 재귀 호출을 추적하고 함수가 기본 케이스로 진행하는 방식을 이해하는 데 도움이 됩니다.

```js
function factorial(n) {
  if (n === 0) {
    return 1;
  } else {
    return n * factorial(n - 1);
  }
}

let result = factorial(5);
console.log(result);
```

<div class="content-ad"></div>

팩토리얼 함수 내부에 중단점을 설정하여 각 재귀 호출마다 n과 반환 값의 값을 검사해 보세요.

## 동적 코드에서의 중단점

JavaScript는 eval과 같은 함수를 사용하여 동적 코드 실행을 허용합니다. 동적으로 실행되는 코드의 디버깅은 복잡할 수 있지만, 중단점을 설정하여 코드가 어떻게 실행되는지 이해하고 애플리케이션에 미치는 영향을 파악할 수 있습니다.

```js
let code = 'console.log("동적 코드 실행");';
eval(code);
```

<div class="content-ad"></div>

eval 줄이나 동적으로 실행된 코드 내에서 중단점을 설정하여 그 동작을 검사해보세요.

# 결론

디버깅은 JavaScript 개발자에게 필수적인 기술이며, 다양한 디버깅 기술을 습득하면 작업 효율과 효과를 크게 향상시킬 수 있습니다. 브라우저 개발자 도구를 활용하고, 중단점을 설정하고 관리하며, 콘솔 메서드를 활용함으로써 코드 동작에 대한 깊은 통찰을 얻고, 문제를 신속히 식별하고 문제를 확신을 갖고 해결할 수 있습니다. 이러한 기술을 꾸준히 연습하면 디버깅 프로세스를 간소화하고 코드 품질을 향상시키며, 궁극적으로 보다 숙련된 JavaScript 개발자가 될 수 있습니다.

- 구글 크롬 개발자 도구
- JavaScript 콘솔 메서드
- 비동기 JavaScript 디버깅

<div class="content-ad"></div>

감사합니다! 만약 이 기사가 도움이 되었다면, 제 트위터/X에 하이라이트, 박수, 응답 또는 연락하기를 고려해주세요. 이런 작은 지원은 매우 감사히 받고 있으며, 이를 통해 이와 같은 콘텐츠를 계속해서 무료로 제공할 수 있습니다!
