---
title: "Nodejs에서 node-cron 크론 잡 스케줄링하는 방법"
description: ""
coverImage: "/assets/img/2024-08-03-SchedulingCronJobsinNodeJS_0.png"
date: 2024-08-03 20:52
ogImage: 
  url: /assets/img/2024-08-03-SchedulingCronJobsinNodeJS_0.png
tag: Tech
originalTitle: "Scheduling Cron Jobs in NodeJS"
link: "https://medium.com/@bytePudding/scheduling-cron-jobs-in-node-js-d3a6fcbd65d9"
isUpdated: true
updatedAt: 1723816569912
---



<img src="/assets/img/2024-08-03-SchedulingCronJobsinNodeJS_0.png" />

회사에서 크론 작업을 예약하는 작업을 받았어요. 오늘은 지식을 더 확고히 하기 위해 Node.js에서 처음부터 작성하기로 결정했어요.

## 목차

- node-cron 설치
- 예제 코드
  - 특정 날짜
  - 특정 시간 범위
  - 스케줄링 취소

<div class="content-ad"></div>

# node-cron 설치

```js
npm install --save node-cron 
```

# 예시 코드

node-cron을 실행하려면 먼저 예약된 함수를 작성해야 합니다. 파일을 생성하는 함수를 작성했습니다

<div class="content-ad"></div>

```js
const fs = require('fs/promises');
const cron = require('node-cron');

async function createFile(count){
    try{
        await fs.copyFile("command.txt",`copied-promise${count}.txt`)
    }catch(error){
        console.log(error);
    }
}

// 매 분마다 새 파일 생성
let count = 0;
cron.schedule("* * * * *",function(){
  count++;
  createFile(count);
});
```

노드 크론 표현식의 의미는 다음과 같습니다:

```js
분    시   일   월  요일
 *      *    *    *    *
```

일반적으로 초는 포함하지 않을 수 있습니다.

<div class="content-ad"></div>

위의 예시에서 * * * * * 는 매 분마다 실행한다는 의미입니다. 왜냐하면 * 는 모든 값을 나타냅니다.

![image](/assets/img/2024-08-03-SchedulingCronJobsinNodeJS_1.png)

## 특정 날짜

```js
cron.schedule("15 11 3 8 6", function(){
      console.log("안녕하세요");
});
```

<div class="content-ad"></div>

위의 cron은 다음과 같이 번역될 수 있습니다. "8월 3일 토요일 오전 11시 15분에 실행". 요일 부분에서 0과 7은 일요일을 나타내며, 1부터 6까지는 월요일부터 토요일을 나타냅니다.

## 특정 시간 범위

```js
cron.schedule("*/30 * * * *",function(){
    console.log("안녕")
});
```

이 코드는 30분마다 실행됩니다. '*/숫자' 형식은 '매 ~ 숫자'를 의미합니다. 따라서 '*/30'은 '매 30분'을 의미합니다.

<div class="content-ad"></div>

## 예약 취소

간단히 stop() 메서드를 호출할 수 있습니다. 이 메서드는 cron.schedule()에서 반환됩니다.

```js
const task = cron.schedule("* * * * *", function () {
  console.log("안녕하세요, 세계");
});

setTimeout(function () {
  task.stop();
  console.log("작업이 중지되었습니다.");
}, 2 * 60 * 1000); // 2분 후 중지됨
```