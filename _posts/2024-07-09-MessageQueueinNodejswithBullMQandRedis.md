---
title: "BullMQ와 Redis를 사용하는 Nodejs 메시지 큐 설정 방법"
description: ""
coverImage: "/assets/img/2024-07-09-MessageQueueinNodejswithBullMQandRedis_0.png"
date: 2024-07-09 17:16
ogImage:
  url: /assets/img/2024-07-09-MessageQueueinNodejswithBullMQandRedis_0.png
tag: Tech
originalTitle: "Message Queue in Node.js with BullMQ and Redis"
link: "https://medium.com/@techsuneel99/message-queue-in-node-js-with-bullmq-and-redis-7fe5b8a21475"
---

메시지 큐는 응용 프로그램의 다른 구성 요소간 통신하는 메커니즘으로, 비동기 방식으로 작용합니다. 이를 통해 구성 요소들이 서로 직접 상호 작용하지 않고도 독립적으로 메시지를 생성하고 소비할 수 있습니다. 이러한 느슨한 결합은 응용 프로그램에 유연성, 확장성 및 신뢰성 이점을 제공합니다.

![이미지](/assets/img/2024-07-09-MessageQueueinNodejswithBullMQandRedis_0.png)

Node.js에서는 BullMQ와 같은 라이브러리를 사용하여 메시지 큐를 구현할 수 있습니다. 메시지 브로커로 Redis를 결합하면 Node.js 앱을 위한 강력하고 견고한 대기열 시스템을 구축할 수 있습니다.

![이미지](/assets/img/2024-07-09-MessageQueueinNodejswithBullMQandRedis_1.png)

<div class="content-ad"></div>

이 포괄적인 가이드에서는 다음 내용을 배울 수 있습니다:

- 메시지 큐와 그 이점에 대한 소개
- BullMQ 소개
- Redis를 사용한 BullMQ 설정
- BullMQ의 핵심 개념
- 채용과 작업 소비
- 딜레이, 라이프사이클, 백오프 등 작업 옵션
- 오류 처리
- 작업 이벤트
- 큐 이벤트
- 반복 작업
- 동시성 및 속도 제한
- 큐 일시 중지
- 작업 진행률 추적
- UI 대시보드를 사용한 큐 모니터링
- 큐 테스트
- BullMQ 큐 사용을 위한 모범 사례

# 메시지 큐란 무엇인가요?

메시지 큐는 생성 구성 요소가 큐에 작업 또는 메시지를 추가하고, 별도의 소비 구성 요소가 작업을 큐에서 제거하고 처리함으로써 동작합니다. 이를 통해 작업의 생성과 사용이 분리되어 개별 관심으로 구현됩니다.

<div class="content-ad"></div>

메시지 큐를 사용하는 주요 이점은 다음과 같습니다:

- 비동기성 - 생산자는 작업이 처리되길 기다릴 필요 없이 큐에 작업을 추가할 수 있습니다. 작업은 소비자에 의해 비동기적으로 처리됩니다.
- 느슨한 결합 - 생산자와 소비자는 직접 상호 작용하거나 알 필요가 없습니다. 이는 스케일링 및 독립적으로 수정하는 것을 더 쉽게 만듭니다.
- 부하 분산 - 큐는 생산자의 고강도 작업과 느린 소비자 사이에 버퍼 역할을 하여 부하를 조절합니다.
- 신뢰성 - 실패한 작업은 나중에 자동으로 재시도될 수 있습니다. 생산자나 소비자가 충돌하더라도 작업이 손실되지 않습니다.
- 확장성 - 생산자와 소비자는 부하가 증가함에 따라 별도로 확장할 수 있습니다. 이는 단일 애플리케이션을 확장하는 것보다 쉽습니다.

메시지 큐의 일반적인 사용 사례는 백그라운드 작업 처리, 작업량의 변동을 완화, 실패한 작업을 다시 시도, 이메일 및 알림을 비동기적으로 전달하는 등이 있습니다.

# BullMQ 소개

<div class="content-ad"></div>

BullMQ는 Redis를 기반으로 한 Node.js용 빠르고 견고하며 의견이 담긴 메시지 큐 라이브러리입니다. BullMQ는 백그라운드 작업을 처리하고 주기적으로 반복되는 작업을 예약하며 실패한 작업을 다시 시도하는 작업 대기열을 구현합니다.

BullMQ의 주요 기능:

- 고성능 및 확장성을 위해 Redis 기반으로 개발
- Redis에 지속적인 작업 저장
- 대기 중, 활성, 지연, 완료, 실패 등과 같은 상태가 있는 견고한 작업 라이프사이클 처리
- 지연, 실패한 작업 다시 시도, 비율 제한 기능
- 여러 워커 간에 분산된 대기열
- 작업, 대기열 및 반복 작업을 위한 고급 API
- 모니터링을 위한 통합된 대기열 대시보드
- Redis 이외의 대기열 백엔드로 대체 가능

Bull, Bee-Queue, Agenda 등의 다른 Node 대기열과 비교했을 때, BullMQ는 더 낮은 지연 시간과 더 높은 처리량으로 큰 성능 차이를 보입니다. 따라서 대부분의 사용 사례에 있어서 BullMQ는 Node.js 앱에서 견고한 작업 대기열을 구현하는 훌륭한 선택입니다.

<div class="content-ad"></div>

# BullMQ를 Redis와 설정하기

BullMQ는 Redis (버전 4+)가 설치되어 실행 중이어야 합니다. 먼저 BullMQ와 Redis npm 패키지를 설치합니다:

```js
npm install bullmq ioredis
```

그런 다음, BullMQ 큐 클라이언트 인스턴스를 생성하고 Redis 인스턴스에 연결합니다:

<div class="content-ad"></div>

```js
const { Queue, Worker } = require("bullmq");
const redisConnection = require("./redis-connection");

const videoEncodingQueue = new Queue("video encoding", {
  connection: redisConnection,
});
```

redisConnection 객체에는 Redis 연결 구성이 포함되어 있습니다:

```js
// redis-connection.js
const Redis = require("ioredis");

const redisConfig = {
  port: 6379,
  host: "127.0.0.1",
};

const redisConnection = new Redis(redisConfig);

module.exports = redisConnection;
```

이렇게 하면 큐가 Redis에 연결되어 작업 데이터를 로드하고 저장할 수 있습니다.

<div class="content-ad"></div>

# BullMQ 개념

BullMQ에서 몇 가지 주요 개념과 용어를 살펴보겠습니다:

- Queue — 작업이 추가되고 처리되는 대기열 인스턴스입니다. 이는 앱이 상호 작용하는 주요 인터페이스입니다.
- Job — 생성자가 대기열에 추가하는 작업의 단위입니다. 작업 데이터와 메타데이터로 구성됩니다.
- Worker — 대기열에서 작업을 소비하고 실행하는 프로세스입니다.
- Producer — 대기열에 작업을 추가하는 앱의 부분입니다.
- Consumer — 대기열에서 작업을 처리하는 워커입니다.
- Job 라이프사이클 — 작업이 대기, 활성, 완료, 실패, 지연 등과 같은 여러 상태를 거치는 것입니다.
- Job 데이터 — 작업을 생성할 때 전달된 사용자 정의 데이터로, 워커가 처리하는 데 사용됩니다.
- Job 옵션 — 지연, 라이프사이클 이벤트, 재시도 등과 같은 메타데이터로 작업 실행을 수정합니다.
- Redis — BullMQ가 작업 데이터를 유지하는 데 사용하는 메시지 브로커입니다.

이 배경 정보를 바탕으로 이제 BullMQ 작업을 생성하고 소비하는 방법을 살펴봅시다.

<div class="content-ad"></div>

# 작업 생성

새로운 작업을 생성하려면 큐 인스턴스에서 add() 메서드를 호출합니다:

```js
// 'mailer' 큐에 'email' 유형의 작업 추가
await mailerQueue.add("email", {
  to: "user@example.com",
  subject: "환영 이메일",
});
```

첫 번째 인수는 작업 이름 또는 유형이며, 두 번째는 작업 데이터 개체입니다. 이렇게 하면 작업이 Redis 큐에 추가됩니다.

<div class="content-ad"></div>

작업 데이터에는 작업을 처리하는 데 필요한 사용자 정의 데이터가 포함될 수 있습니다:

```js
await videoQueue.add("transcode", {
  inputFile: "video.mp4",
  outputFormat: "mkv",
});

await imageQueue.add("resize", {
  imageUrl: "example.jpg",
  width: 800,
  height: 600,
});
```

작업은 생성된 후에는 대기 상태로 시작하여 워커가 작업을 가져가기 전까지 대기합니다.

대기 이벤트를 청취하여 새로운 작업을 기록하거나 모니터링할 수 있습니다:

<div class="content-ad"></div>

```js
videoQueue.on("waiting", (jobId) => {
  console.log(`작업 ${jobId} 추가됨`);
});
```

# 작업 ID 및 메타데이터

각 추가된 작업은 고유한 ID를 받습니다. 이와 다른 메타데이터는 다음과 같이 가져올 수 있습니다:

```js
const job = await videoQueue.add(...)

job.id // '8a122454-9ccb-4777-9e4b-efdae12c18b3'

job.name // 'transcode'
job.data // { input: '...', output: '...' }

job.opts // {} (추가 시 전달된 옵션들)
```

<div class="content-ad"></div>

잡 ID는 작업을 고유하게 식별하며 나중에 해당 작업을 검색하는 데 사용할 수 있습니다.

# 작업 지연

우리는 즉시가 아닌 나중에 작업을 실행할 수 있도록 일정에 작업을 예약할 수 있습니다:

```js
await queue.add("email", jobData, { delay: 10000 }); // 10초 지연

await queue.add("Analytics", jobData, { delay: 1000 * 60 * 60 * 24 }); // 1일 지연
```

<div class="content-ad"></div>

딜레이는 밀리초 단위로 지정됩니다. 딜레이가 만료되면 디레이된 작업은 디레이 상태로 전환되어 대기 상태로 이동합니다.

# 작업 소비하기

큐에서 작업을 소비하기 위해 Worker 프로세스를 만듭니다:

```js
const worker = new Worker(
  "mailer",
  async (job) => {
    // 여기서 작업 처리
  },
  { connection: redisConnection }
);

worker.start();
```

<div class="content-ad"></div>

이 코드는 계속해서 메일러 큐 작업을 소비하고 각 작업에 대해 처리기를 호출할 것입니다.

처리기는 작업 개체를 세부 정보와 함께 받게 됩니다:

```js
worker.process(async (job) => {
  console.log(job.data);

  await sendEmail(job.data.to, job.data.subject);

  console.log(`Completed job ${job.id}`);
});
```

작업이 처리되면 완료된 상태로 전환됩니다. process() 메서드는 처리가 완료될 때 해결되어야 하는 Promise를 반환합니다.

<div class="content-ad"></div>

에러가 발생하면 작업을 다시 시도할 수 있도록 Promise를 거부해야 합니다:

```js
worker.process(async (job) => {
  try {
    // 작업 처리
  } catch (err) {
    // 에러 발생 시 Promise 거부
    throw err;
  }
});
```

기본적으로 작업은 3번 다시 시도됩니다. 이후에 이를 변경할 수 있습니다.

여러 큐에서 데이터를 소비하려면 각 큐에 대해 작업자를 만들 수 있습니다.

<div class="content-ad"></div>

# 대기열 이벤트

대기열은 모니터링을 위해 청취할 수 있는 이벤트를 발생시킵니다:

```js
videoQueue.on("completed", (job) => {
  console.log(`작업 ${job.id}이 완료되었습니다!`);
});

videoQueue.on("failed", (job, err) => {
  console.log(`작업 ${job.id}이 ${err}로 실패했습니다`);
});
```

일반적인 대기열 이벤트:

<div class="content-ad"></div>

- 대기 중: 대기열에 작업이 추가됨
- 활성: 작업이 작업자에 의해 시작됨
- 완료됨: 작업이 성공적으로 완료됨
- 실패함: 오류로 작업이 실패함
- 일시 중지됨: 대기열이 일시 중지됨
- 다시 시작됨: 대기열이 다시 시작됨
- 정리됨: 한계로 인해 작업이 제거됨

## 작업 수명주기

BullMQ에서의 작업은 다양한 상태를 거치게 됩니다:

- 대기 중: 대기열에 추가되어 처리될 대기 중인 상태
- 지연됨: 나중 시간에 예약된 작업
- 활성: 작업자에 의해 처리 중인 상태
- 완료됨: 성공적으로 처리된 상태
- 실패함: 오류로 인해 처리 실패한 상태
- 지연된 재시도: 지연 후 다시 시도를 기다리는 실패한 작업
- 일시 중지됨: 대기열/작업이 일시 정지되어 활성 상태가 아닌 상태

<div class="content-ad"></div>

job.status를 통해 현재 작업 상태에 액세스할 수 있어요.

```js
job.status; // 'completed'
```

작업 라이프사이클은 실패, 재시도, 지연 등을 포함한 강력한 처리를 가능하게 합니다.

# 작업 옵션

<div class="content-ad"></div>

작업을 추가할 때는 작업 옵션을 전달하여 라이프사이클 동작을 제어할 수 있어요:

```js
await queue.add(jobName, data, options);
```

일반적인 옵션은 다음과 같아요:

- delay: 작업 처리 전 x ms만큼 지연
- attempts: 실패한 작업을 재시도하는 횟수
- backoff: 작업 실패 시 백오프 전략
- lifo: FIFO 대신 LIFO 순서 사용
- priority: 숫자 우선순위 값. 높을수록 빨리 처리
- repeat: cron 일정에 따라 작업을 반복

<div class="content-ad"></div>

여기서 몇 가지를 더 자세히 살펴봅시다:

## 작업 시도

기본적으로, 실패한 작업은 실패 상태로 가기 전에 3번 재시도됩니다:

```js
await queue.add("email", jobData); // 3번 재시도됩니다
```

<div class="content-ad"></div>

우리는 시도 횟수를 변경할 수 있어요:

```js
await queue.add("email", jobData, { attempts: 5 }); // 5번 재시도
```

모든 시도를 소진한 후에는 작업이 실패하고 다시 시도되지 않아요.

# 백오프

<div class="content-ad"></div>

실패 시 다시 시도할 때 백오프 전략을 사용하여 재시도를 지연할 수 있습니다. 각 재시도마다 지연 시간이 증가합니다.

일반적으로 사용되는 전략은 다음과 같습니다:

- fixed: 재시도 사이의 고정된 지연 시간 (기본값)
- exponential: 지수적으로 증가하는 백오프

```js
await queue.add("email", jobData, {
  backoff: {
    type: "exponential",
    delay: 1000, // 초기 1초 백오프
  },
});
```

<div class="content-ad"></div>

이 작업은 1초, 2초, 4초 등의 지연 후 다시 시도합니다.

# 반복 작업

반복 옵션을 사용하여 cron 일정에 따라 자동으로 반복되는 작업을 생성할 수 있습니다.

```js
await queue.add(
  "analytics",
  {},
  {
    repeat: { cron: "0 0 * * *" }, // 매일 자정에 실행
  }
);
```

<div class="content-ad"></div>

이렇게 하면 매일 실행되는 반복 크론 작업이 생성됩니다.

다른 반복 옵션은 다음과 같습니다:

- every: 고정된 간격으로 반복
- cron: 크론탭 표현식 일정
- tz: 시간대

반복 작업은 보고서, 백업, 이메일 등과 같은 작업을 예약하는 간단한 방법입니다.

<div class="content-ad"></div>

# 고급 작업 옵션

더 많은 고급 작업 옵션과 기능이 있습니다:

# Lifo 순서

FIFO (먼저 들어온 것이 먼저 처리됨)은 대기열의 기본 순서 지정 방식입니다. 작업은 추가된 순서대로 처리됩니다.

<div class="content-ad"></div>

대신 LIFO (last-in-first-out) 순서를 사용할 수 있습니다:

```js
await queue.add("print", data, { lifo: true });
```

LIFO는 가장 최근에 추가된 작업을 먼저 꺼냅니다. 우선 순위 큐에 유용합니다.

# 작업 우선순위

<div class="content-ad"></div>

우선순위를 사용하여 높은 우선순위 작업을 먼저 처리할 수 있어요:

```js
// 높은 우선순위 작업
await queue.add("email", data, { priority: 10 });

// 낮은 우선순위
await queue.add("analytics", data, { priority: 1 });
```

높은 우선순위 값이 먼저 처리돼요. 기본값은 0이에요.

# 큐 일시 중지하기

<div class="content-ad"></div>

큐를 일시적으로 처리 중인 새 작업을 중지하기 위해 일시 정지시킬 수 있습니다:

```js
queue.pause();
```

기존 작업은 완료되지만 새로운 작업은 일시 중지된 상태로 대기합니다.

중지된 상태에서 다시 시작하려면 queue.resume()을 사용하세요.

<div class="content-ad"></div>

유지 보수, 배포 등에 유용합니다.

# 작업 이벤트

각 작업은 수명주기 중에 이벤트를 발생시킵니다. 해당 이벤트를 듣을 수 있습니다:

```js
job.on("completed", () => {
  // 작업 완료
});

job.on("failed", (err) => {
  // 오류로 작업 실패
});
```

<div class="content-ad"></div>

일반적인 작업 이벤트:

- 대기 중
- 활성화
- 중단됨 (작업은 시작됐지만 완료되지 않음)
- 진행 중 (진행이 업데이트됨)
- 완료됨
- 실패함
- 일시 중지됨
- 다시 시작됨
- 제거됨

이는 상태 추적, 메트릭 기록 등을 가능하게 합니다.

# 에러 처리

<div class="content-ad"></div>

작업 핸들러에서 발생한 오류는 Promise를 reject해야 합니다:

```js
worker.process(async (job) => {
  try {
    // 작업 로직
  } catch (err) {
    // 에러 발생 시 Promise를 reject
    throw err;
  }
});
```

이렇게 하면:

- 작업을 실패로 표시합니다
- 시도 옵션에 따라 다시 시도합니다
- 정의된 경우 backoff 전략을 적용합니다
- 실패한 이벤트를 방출합니다

<div class="content-ad"></div>

모든 시도가 실패하면 작업은 실패 상태로 전환됩니다.

또한 실패한 이벤트를 수신하여 작업 및 대기열에서 실패를 기록하거나 알릴 수도 있습니다.

# 동시성 및 속도 제한

기본 설정에 따라 BullMQ 워커는 병행성을 사용하여 여러 작업을 동시에 처리할 것입니다.

<div class="content-ad"></div>

동시성은 시스템 자원을 모두 사용하지 않으면서 성능을 극대화하기 위해 제한되어 있습니다.

기본값은 CPU 코어 수, 즉 병렬 작업자의 수입니다.

우리는 본인만의 동시성 제한을 구성할 수 있습니다:

```js
const worker = new Worker(queueName, jobHandler, {
  connection,
  concurrency: 5,
});
```

<div class="content-ad"></div>

한 번에 처리되는 작업은 최대 5개까지입니다.

# 속도 제한

추가적인 쓰로틀링을 위해, 초당 처리되는 작업의 수를 제한할 수 있습니다:

```js
const worker = new Worker(queueName, jobHandler, {
  connection,
  limiter: {
    max: 10,
    duration: 1000, // 초당 10개까지
  },
});
```

<div class="content-ad"></div>

매 초 최대 10개의 작업을 처리하며 부하를 더 부드럽게 처리합니다.

# 작업 진행

오랜 시간 동안 실행되는 작업에 대해 0–100%까지 진행상황을 보고할 수 있습니다:

```js
worker.process(async (job) => {
  job.progress(0);

  // 비디오 처리

  job.progress(50);

  // 더 처리

  job.progress(100);
});
```

<div class="content-ad"></div>

작업의 진행률 값을 업데이트합니다.

또한 작업의 진행 이벤트를 수신하여 진행률을 추적할 수도 있습니다.

진행률 보고는 진행 바를 만들거나 작업 소요 시간을 추적하는 데 사용됩니다.

# 대기열 모니터링

<div class="content-ad"></div>

BullMQ에는 /admin/queues에서 대기열을 모니터링하는 통합 웹 UI 대시 보드가 함께 제공됩니다.

```js
const { UI } = require("bullmq");

router.use("/admin/queues", UI);
```

다음을 볼 수 있습니다:

- 대기열 개요 및 통계
- 작업 상태 및 세부 정보
- 실패 및 다시 시도
- 예약된 작업
- 대기열 일시 중지/다시 시작
- 실패한 작업 다시 시도

<div class="content-ad"></div>

테이블 태그를 Markdown 형식으로 변경해주세요.

Very useful for debugging and managing queues in production.

# Testing Queues

We can write tests for producers and consumers separately:

Producer test

<div class="content-ad"></div>

- 테스트 큐에 가짜 작업을 추가하세요.
- 예상 데이터로 작업이 추가되었는지 확인하세요.
- 작업 라이프사이클 이벤트 확인하기

컨슈머 테스트

- 테스트 큐에 가짜 작업을 추가하세요.
- 테스트 큐에서 워커 시작하기
- 예상대로 작업이 처리되었는지 확인하세요.
- 작업 실패와 재시도 테스트하기

테스트 중에 별도의 Redis 인스턴스를 실행하여 프로덕션에서 격리하세요.

<div class="content-ad"></div>

테스트를 통해 배포 전에 문제를 잡을 수 있어요.

# 최상의 실천 방법

BullMQ 큐를 사용할 때 따라야 할 일부 최상의 실천 방법:

- 충돌을 피하기 위해 별도의 Redis 데이터베이스를 BullMQ에 사용해요
- 충돌을 피하기 위해 대기열 이름에 접두사를 붙여요
- 작업 핸들러가 동등한식(동질성)을 갖도록 만들어, 작업이 안전하게 다시 시도할 수 있도록 해요
- 작업이 다시 시도할 수 있도록 오류를 처리하고 프로미스를 제대로 거절해요
- 실패한 작업을 모니터링하고 경고를 받아요
- 성능과 부하를 균형 있게 조절하기 위해 동시성과 속도 제한을 튜닝해요
- 본격적인 사용 전에 대기열을 스트레스 테스트해요
- 다른 종류의 작업을 위해 여러 대기열/워커를 사용해요
- 상태가 없고 확장 가능한 워커를 빌드하기 위해 Twelve Factor App 원칙을 따라요

<div class="content-ad"></div>

이것은 견고하고 탄력적인 BullMQ 기반 메시지 대기열을 구축하는 데 도움이 됩니다.

# 저와 연락하기:

Linkedin: https://www.linkedin.com/in/suneel-kumar-52164625a/

# 결론

<div class="content-ad"></div>

BullMQ은 Redis와 결합할 때 Node.js 앱을 위한 고성능 작업 큐 시스템을 제공합니다.

주요 기능은 다음과 같습니다:

- 지연, 활성, 완료, 실패, 반복 등과 같은 강력한 작업 라이프사이클
- 딜레이, 재시도, 백오프, LIFO 순서, 우선순위와 같은 강력한 옵션
