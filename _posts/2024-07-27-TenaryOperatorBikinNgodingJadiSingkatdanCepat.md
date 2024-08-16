---
title: "자바스크립트에서 삼항 연산자를 효율적으로 사용하는 방법"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-07-27 13:52
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Tenary Operator Bikin Ngoding Jadi Singkat dan Cepat"
link: "https://dev.to/fikriqx/tenary-operator-bikin-ngoding-jadi-singkat-dan-cepat-48gp"
isUpdated: true
updatedAt: 1723813407609
---



테너리 연산자는 자바스크립트에서 if-else 문을 간단히 작성하는 방법이에요. 조건을 물음표 ?와 콜론 :을 사용해서 표현할 수 있어요.

## 기본 구문

```js
condition ? expressionIfTrue : expressionIfFalse;
```

작동 방식

<div class="content-ad"></div>

- 조건은 먼저 평가됩니다.
- 조건이 참이면 expressionIfTrue가 실행됩니다.
- 조건이 거짓이면 expressionIfFalse가 실행됩니다.

## 실제 예시

예를 들어, 누군가가 투표에 참여할 수 있는지 여부를 나이(age)에 따라 결정하려고 합니다.

```js
let age = 18;
let canVote = age >= 18 ? "투표 가능" : "투표 불가능";

console.log(canVote); // 출력: "투표 가능"
```

<div class="content-ad"></div>

태그를 Markdown 형식으로 변경하십시오.

noh di age `= 18 di cek kalau misal nilai age lebih besar dari atau sama dengan 18 (`=) maka Bolehh voting kalau enggak sesuai Tidak boleh voting

Contoh lain
Misal kita mau tampilin nilai A,B,C,D,E,F berdasarkan nilai skor.

```js
let score = 85;
let grade = score >= 90 ? "A" :
            score >= 80 ? "B" :
            score >= 70 ? "C" :
            score >= 60 ? "D" : "F";

console.log(grade); // Output: B
```

yep, bener banget ini bisa kita pakai sampe sepuasnya enggak cuman satu kali aja. intinya mah sama kayak if-else cuman lebih singkat aja.

<div class="content-ad"></div>

## 삼항 연산자를 사용하는 것의 장점

- 정말 간결해요: 코드를 절약하고 깔끔하며 짧아요.
- 읽기 쉬워요: 더 쉽게 읽을 수 있어요.

언제 삼항 연산자를 사용하는 게 좋을까요 🤔 ?
내 의견은 조건이 간단할 때 사용하는 거에요. 예를 들어, 예시에서처럼 조건이 단순한 경우에요. 조건문이 쉽고 복잡하지 않으면 좋지 않을까요. 하지만 복잡한 경우에는 여전히 if-else문을 사용하는 것을 추천해요.