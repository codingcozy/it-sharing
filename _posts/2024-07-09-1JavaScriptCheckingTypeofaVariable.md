---
title: "자바스크립트에서 변수 타입 확인하는 방법"
description: ""
coverImage: "/assets/img/2024-07-09-1JavaScriptCheckingTypeofaVariable_0.png"
date: 2024-07-09 13:17
ogImage:
  url: /assets/img/2024-07-09-1JavaScriptCheckingTypeofaVariable_0.png
tag: Tech
originalTitle: "1. JavaScript: Checking Type of a Variable"
link: "https://medium.com/@opensrc0/typechecking-in-javascript-4f9ba96184c6"
---

## JavaScript에서 변수의 타입을 확인하는 가장 좋은 방법

![이미지](/assets/img/2024-07-09-1JavaScriptCheckingTypeofaVariable_0.png)

## JavaScript에서 타입을 확인하는 방법

키워드 typeof로 타입을 확인하세요.

<div class="content-ad"></div>

```js
1. typeof 2                                      // "number"
2. typeof "Quikr-Treebo-Reliance-Jio"            // "string"
3. typeof undefined                              // "undefined"
4. typeof true                                   // "boolean"
```

```js
5. typeof null                                   // "object"
6. typeof {}                                     // "object"
7. typeof []                                     // "object"
```

앗, 원하는 결과를 얻을 수 없을 거에요. null, '', []의 typeof를 확인하면 예상치 못한 결과가 나올 거에요. 이제 자바스크립트에서 다른 방법으로 타입을 확인하는 방법을 살펴보도록 할게요.

자바스크립트에서 모든 것은 객체이다는 것을 알고 계셨죠. 아래와 같이 Object와 그 프로토타입을 사용하려고 해요.

<div class="content-ad"></div>

```js
1. Object.prototype.toString.call([1,2,3,4]);          // '[object Array]'
2. Object.prototype.toString.call({ });                // '[object Object]'
3. Object.prototype.toString.call(new Date())          // '[object Date]'
4. Object.prototype.toString.call(2);                  // '[object Number]'
5. Object.prototype.toString.call("Quikr-Treebo-Jio"); // '[object String]'
6. Object.prototype.toString.call(undefined);          // '[object Undefined]'
7. Object.prototype.toString.call(true);               // '[object Boolean]'
```

그렇죠. 아주 간단하죠. 이것이 자바스크립트에서 타입을 확인하는 올바른 방법입니다.
