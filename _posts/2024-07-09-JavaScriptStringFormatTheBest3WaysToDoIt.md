---
title: "자바스크립트 문자열 포맷 최상의 3가지 방법"
description: ""
coverImage: "/assets/img/2024-07-09-JavaScriptStringFormatTheBest3WaysToDoIt_0.png"
date: 2024-07-09 17:21
ogImage:
  url: /assets/img/2024-07-09-JavaScriptStringFormatTheBest3WaysToDoIt_0.png
tag: Tech
originalTitle: "JavaScript String Format: The Best 3 Ways To Do It"
link: "https://medium.com/@onlinemsr/javascript-string-format-the-best-3-ways-to-do-it-c6a12b4b94ed"
---

![이미지](/assets/img/2024-07-09-JavaScriptStringFormatTheBest3WaysToDoIt_0.png)

JavaScript에서 문자열은 원시 데이터 유형입니다. JavaScript 문자열 형식은 문자열을 동적으로 구성하고 실행 중에 값으로 대체해야 하는 자리 표시자가 있는 경우에 유용합니다. 때로는 숫자와 통화 값을 문자열로 형식화해야 할 수도 있습니다. 이때 JavaScript에서 문자열 형식화가 필요합니다.

이 게시물에서 다음을 살펴볼 것입니다:

- 변수 문자열 포맷의 다양한 방법
- 더하기 (+) 기호를 사용한 형식화
- 자리 표시자 사용
- 문자열 보간을 사용하여 문자열 형식화
- 숫자 형식화 문자열
- 통화 형식 문자열

<div class="content-ad"></div>

각 항목을 자세히 살펴봅시다.

자바스크립트 문자열 형식에 깊이 들어가기 전에, 안전한 JavaScript 및 Node 애플리케이션을 구축하고 싶다면 "JavaScript Security Cookbook (40개 이상의 레시피)"를 무료로 다운로드하세요!

![image](https://miro.medium.com/v2/resize:fit:1400/1*EQ9R2Mnd5zUfMdlpX4qWzg.gif)

# 세 가지 다른 방법으로 JavaScript 문자열 형식 설정

<div class="content-ad"></div>

문자열을 서식 지정하는 가장 일반적인 이유 중 하나는 JavaScript 변수를 문자열에 출력하는 것입니다. JavaScript 문자열을 서식 지정하는 방법은 많이 있습니다.

이 기사에서는 변수와 함께 문자열의 최상위 세 가지 서식 지정 방법을 살펴보겠습니다.

![Method 1: Format the JavaScript string using the plus (+) sign](/assets/img/2024-07-09-JavaScriptStringFormatTheBest3WaysToDoIt_1.png)

# 방법 1: 플러스 (+) 기호를 사용하여 JavaScript 문자열 서식 지정하기

<div class="content-ad"></div>

간단한 문자열을 포맷하려면 플러스 기호(+)를 사용하는 것이 전통적인 방법입니다. 이 방법을 사용하면 + 기호로 구분된 문자열 내에 JavaScript 변수를 삽입해야 합니다. 이 접근 방법은 JavaScript 문자열 연결 과정과 유사합니다.

```js
const customerName = "John";
const orderId = 10001;
console.log("Hello " + customerName + ", your order " + orderId + " has been shipped.");
// 출력: "Hello John, your order 10001 has been shipped."
```

괄호 없이 문자열 포맷팅에 산술 표현식을 삽입하면 예상치 못한 결과가 발생할 수 있습니다. 다음 예제는 예상치 못한 결과를 출력합니다. 이는 다른 숫자를 더하기 전에 숫자가 JavaScript 문자열로 변환되기 때문입니다.

```js
console.log("5+5의 합은 " + 5 + 5);
// 출력: "5+5의 합은 55."
```

<div class="content-ad"></div>

해당 문제를 해결하기 위해 괄호 안에 정수를 넣으면 간단히 해결할 수 있습니다. 이 경우 정수를 먼저 합산한 다음에 문자열에 추가합니다.

```js
console.log("5 + 5의 합은 " + (5 + 5));
// 출력: "Sum of 5 + 5 is 10"
```

# 방법 2: JavaScript 문자열 형식 지정자 ' '

C#과 같은 다른 프로그래밍 언어와 달리, JavaScript는 플레이스홀더 ' '를 사용한 내장 문자열 형식 지정 함수를 지원하지 않습니다.

<div class="content-ad"></div>

우리는 사용자 정의 JavaScript 문자열 형식 함수 또는 문자열 프로토콜 메서드를 작성해야합니다.

```js
const formatString = (template, ...args) => {
  return template.replace(/{([0-9]+)}/g, function (match, index) {
    return typeof args[index] === "undefined" ? match : args[index];
  });
};
console.log(formatString("Hello {0}, your order {1} has been shipped.", "John", 10001));
// 출력: "Hello John, your order 10001 has been shipped."
```

위의 예제에서 formatString() 메서드는 템플릿 문자열과 args를 매개변수로 받습니다. 정규 표현식을 사용하여 각 자리 표시자를 args의 해당 값으로 대체합니다.

아래 예제에서 JavaScript 문자열 프로토콜 메서드로 작성할 수 있습니다:

<div class="content-ad"></div>

```js
String.prototype.format = function () {
  var args = arguments;
  return this.replace(/{([0-9]+)}/g, function (match, index) {
    return typeof args[index] == "undefined" ? match : args[index];
  });
};

console.log("Hello {0}, your order {1} has been shipped.".format("John", 10001));
// Output: "Hello John, your order 10001 has been shipped."
```

이 방식을 사용하면 자리 표시자가 포함된 모든 문자열에 format() 메서드를 호출할 수 있습니다.

이 방법의 문제는 자리 표시자 개수가 증가할 때 발생합니다. 인덱스 위치와 전달할 값 결정이 어려워집니다.

# 메소드 3: 템플릿 리터럴을 사용하여 변수로 문자열 형식 지정하기

<div class="content-ad"></div>

가장 깔끔하고 직관적인 방법 중 하나는 템플릿 리터럴이나 문자열 보간을 사용하여 문자열을 형식화하는 것입니다. 이 접근 방식에서는 단일 또는 이중 인용부호 대신 백틱(`)을 사용해야 합니다.

JavaScript 변수는 $ 기호로 접두사가 붙은 괄호 안에 배치할 수 있습니다. 이 변수가 백틱으로 둘러싸인 JavaScript 문자열에 배치되면, 실행 중에 해당 변수는 값으로 확장됩니다.

```js
const customerName = "John";
const orderId = 10001;
console.log(`Hello ${customerName}, your order ${orderId} has been shipped.`);
// 출력: "Hello John, your order 10001 has been shipped."
```

템플릿 리터럴을 사용하여 변수로 문자열을 형식화할 때는 변수 색인 위치를 걱정할 필요가 없습니다.

<div class="content-ad"></div>

<img src="/assets/img/2024-07-09-JavaScriptStringFormatTheBest3WaysToDoIt_2.png" />

# 문자열 형식화 숫자

문자열을 형식화하는 또 다른 유용한 방법은 숫자를 쉼표로 형식화하는 것입니다. 내장 toLocaleString() 메서드를 사용하여 이 작업을 수행할 수 있습니다. toLocaleString() 문자열 메서드는 숫자를 사용자의 로캘 설정에 따라 형식화합니다.

```js
const number = 12345.55;
console.log(number.toLocaleString());
// 출력: "12,345.55"
```

<div class="content-ad"></div>

이 코드는 미국이나 인도와 같이 소수점으로 쉼표를 사용하는 지역에서 "12,345.55"를 출력합니다. 사용자가 익숙한 형식으로 숫자를 표시하고 싶을 때 이 방법이 유용합니다.

예를 들어 프랑스 로캘을 사용하고 싶다면 locales 매개변수로 "fr-FR"을 전달할 수 있습니다:

```js
const number = 12345.5;
const formattedNumber = number.toLocaleString("fr-FR");
console.log(formattedNumber);
// 출력: "12 345,5"
```

# 문자열 보간법으로 숫자 형식화

<div class="content-ad"></div>

자바스크립트에서는 템플릿 리터럴과 `$` 기호를 사용하여 숫자를 포맷팅하는 문자열 보간을 할 수 있어요.

다음은 자바스크립트에서 숫자를 포맷팅하는 문자열 보간을 사용하는 예시에요:

```js
const number = 10;
const formattedString = `The number is: ${number}`;
console.log(formattedString);
// 출력: The number is: 10
```

# 돈을 문자열로 포맷팅

<div class="content-ad"></div>

자바스크립트에서 돈을 문자열로 형식화하려면 toLocaleString() 메서드를 사용할 수 있어요. 이 메서드는 숫자를 언어에 맞게 표시된 돈의 문자열로 변환해줘요.

다음은 예제입니다:

```js
const money = 1234.5;
let options = { style: "currency", currency: "USD" };
const formattedMoney = money.toLocaleString("en-US", options);
console.log(formattedMoney);
// 출력: "$1,234.50"
```

이 예제에서 toLocaleString() 메서드는 'en-US' 로케일 및 스타일을 '화폐'로, 통화를 'USD'로 지정한 options 객체와 함께 money 변수에 호출됩니다. 이렇게 하면 money 변수가 미국 달러로 형식화된 문자열로 변환됩니다. 변환된 문자열은 브라우저 콘솔에 출력됩니다.

<div class="content-ad"></div>

귀하의 지역에 해당 통화 코드로 'USD'를 대체할 수 있습니다. toLocaleString() 메서드에는 소수점 이하 자릿수의 최소 및 최대값을 지정하는 등 다른 옵션을 사용할 수도 있습니다.

## 문자열 형식 테이블 구조

문자열 메서드 padStart()와 padEnd()를 사용하여 값 배열을 테이블 구조로 형식화할 수 있습니다. 현재 문자열을 주어진 길이에 도달하도록 지정된 문자열로 (반복될 수도 있는) 패딩합니다. 패딩은 현재 문자열의 시작 (padStart() 메서드를 사용하는 경우) 또는 끝 (padEnd() 메서드를 사용하는 경우)부터 적용됩니다.

책 객체의 배열을 가지고 있다고 가정하고 그것들을 테이블 구조로 출력하려면 다음과 같이 하면 됩니다.

<div class="content-ad"></div>

여기 예시가 있어요:

```js
const books = [
  { title: "Eloquent JavaScript", price: 29.99 },
  { title: "JavaScript: The Good Parts", price: 19.99 },
];

const titleColumnWidth = 30;
const priceColumnWidth = 10;
books.forEach((b) => {
  let title = b.title.padEnd(titleColumnWidth, " ");
  let price = b.price.toString().padStart(priceColumnWidth, " ");

  console.log(`| ${title} | ${price} |`);
  console.log("".padStart(titleColumnWidth + priceColumnWidth + 7, "-"));
});
```

위의 코드는 책 객체 배열을 다음 형식으로 출력할 겁니다:

```js
| Eloquent JavaScript            |      29.99 |
-----------------------------------------------
| JavaScript: The Good Parts     |      19.99 |
```

<div class="content-ad"></div>

<img src="/assets/img/2024-07-09-JavaScriptStringFormatTheBest3WaysToDoIt_3.png" />

# 결론

이 게시물에서는 다양한 JavaScript 문자열 형식화 방법을 살펴보았습니다.

더하기 기호를 사용하는 것은 문자열을 형식화하는 전통적인 방법입니다. 사용자 정의 함수나 문자열 프로토타입 메서드를 사용하여 문자열을 형식화할 수 있습니다. 이 방법은 자리 표시자의 수가 늘어날수록 복잡해집니다. 문자열 보간을 사용하여 문자열을 형식화하는 것은 간단하고 간결한 방법입니다.

<div class="content-ad"></div>

toLocaleString() 메소드를 사용하여 숫자와 통화를 로캘별 문자열로 변환할 수 있어요.

문자열을 포맷하는 데 어떤 방법을 사용할 건가요?
