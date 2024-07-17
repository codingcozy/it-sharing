---
title: "텔레그램 봇을 위한 무료 호스팅 생각보다 쉬운 방법"
description: ""
coverImage: "/assets/img/2024-07-09-FreeHostingforYourTelegramBotItsEasierThanYouThink_0.png"
date: 2024-07-09 16:53
ogImage:
  url: /assets/img/2024-07-09-FreeHostingforYourTelegramBotItsEasierThanYouThink_0.png
tag: Tech
originalTitle: "Free Hosting for Your Telegram Bot: It’s Easier Than You Think"
link: "https://medium.com/devops-dev/free-hosting-for-your-telegram-bot-its-easier-than-you-think-66a5e5c000bb"
---

`<img src="/assets/img/2024-07-09-FreeHostingforYourTelegramBotItsEasierThanYouThink_0.png" />`

무료로 완전히 사용자 정의된 텔레그램 봇을 만들고 실행하는 방법에 대해 이야기해 봅시다 24/7.

## 소개

많은 분들이 아실 것처럼, 텔레그램 봇 개발에는 주로 폴링(polling)과 웹훅(webhooks) 두 가지 접근 방식이 있습니다.

<div class="content-ad"></div>

그럼, 파이썬, 자바 또는 노드.js로 텔레그램 봇을 만들 때 폴링 방식으로 접근하는 경우, 코드는 항상 서버로부터 수신된 모든 메시지를 가져와야 하므로 24시간 7일 운영되어야 합니다. 이 방법으로는 서버를 24시간 7일 유지하기 위한 무료 옵션이 많이 없다는 것이 안타깝습니다.

그러나 웹훅 관점에서는 상황이 다를 수 있습니다. 텔레그램 봇에 대한 웹훅을 설정한다는 것은 텔레그램 서버가 새로운 메시지를 수신할 때 마다 사용자, 메시지 텍스트, 시간 및 기타 사용 가능한 정보를 포함한 페이로드와 함께 POST 요청을 엔드포인트로 보내도록 특정 함수를 설정하는 것을 의미합니다. 그러면 엔드포인트의 함수에서 이 정보를 사용하여 원하는 작업을 수행할 수 있습니다.

이것은 또한 미리 정의된 엔드포인트가 활성화되어 있고 POST 요청을 수신하기 위해 서버에서 실행되어야 한다는 것을 의미합니다. 그러나 이 서버는 24시간 7일 동안 실행될 필요는 없습니다. 대신에 서버리스 함수를 사용할 것입니다. 무료 서버리스 함수 옵션 중 하나는: Cloudflare Workers입니다.

오늘은 Cloudflare Workers에서 텔레그램 봇을 위한 사용자 정의 POST 엔드포인트를 무료로 생성하고 JavaScript로 사용자 정의하는 방법을 알아볼 것입니다.

<div class="content-ad"></div>

## 텔레그램 봇 등록/생성하기

텔레그램 봇을 만들 때 가장 먼저 할 일은 텔레그램 BotFather에서 봇을 등록하는 것입니다. 텔레그램에서 BotFather를 시작하고 새로운 봇을 만드세요:

<img src="/assets/img/2024-07-09-FreeHostingforYourTelegramBotItsEasierThanYouThink_1.png" />

이제 여기에서 봇 프로필 사진, 설명 및 기타 세부 정보를 사용자 정의할 수 있습니다.

<div class="content-ad"></div>

이 봇의 토큰을 개발 중에 계속해서 사용하려면 보관해두세요.

## 클라우드플레어 워커 생성

dash.cloudflare.com에 가서 로그인하거나 가입하세요.

왼쪽 메뉴에서 Workers & Pages로 이동하고 "Create application" 버튼을 눌러 "Create Worker" 버튼을 누르세요.

<div class="content-ad"></div>

![Worker Image](/assets/img/2024-07-09-FreeHostingforYourTelegramBotItsEasierThanYouThink_2.png)

Edit your worker’s name and press “Deploy”.

Congrats, you created your worker, now press the “Edit code” button to customize it.

![Customizing Worker](/assets/img/2024-07-09-FreeHostingforYourTelegramBotItsEasierThanYouThink_3.png)

<div class="content-ad"></div>

```js
/**
 * 클라우드플레어 워커에 오신 것을 환영합니다! 이것은 여러분의 첫번째 워커입니다.
 *
 * - 터미널에서 "npm run dev"를 실행하여 개발 서버를 시작하세요
 * - http://localhost:8787/ 에 브라우저 탭을 열어 워커가 작동하는 것을 확인하세요
 * - 워커를 게시하려면 "npm run deploy"를 실행하세요
 *
 * 더 알아보기: https://developers.cloudflare.com/workers/
 */
export default {
  async fetch(request, env, ctx) {
    return new Response("Hello World!");
  },
};
```

에디터에서 위와 같은 코드를 볼 수 있을 것입니다. 이것은 엔드포인트의 진입점입니다.

## 코딩

더 진행하기 전에, 텔레그램 POST 페이로드가 어떻게 보이는지 기억해봅시다:

<div class="content-ad"></div>

{
"update_id": 123456789,
"message": {
"message_id": 987654321,
"from": {
"id": 123456789, // 사용자 ID
"first_name": "John",
"last_name": "Doe",
"is_bot": false,
"username": "johndoe",
"language_code": "en"
},
"chat": {
"id": 123456789, // 채팅 ID
"first_name": "John",
"last_name": "Doe",
"username": "johndoe",
"type": "private"
},
"date": 1644661389,
"text": "안녕하세요, 어떻게 지내세요?"
}
}

그 이외에도 위에 표시된 것 외에도 많은 다른 세부 정보들이 있습니다. 그러므로 현재로써 페이로드로부터 사용자가 누구인지, 메시지가 무엇인지 및 언제 보냈는지 등을 알 수 있습니다.

이제 텔레그램 API URL이 어떻게 생겼는지 살펴봅시다. 이 URL은 페이로드를 처리하고 우리 봇이 응답을 반환하는 데 사용됩니다:

```js
https://api.telegram.org/bot${API_KEY}/sendMessage?chat_id=${chatId}&text=${text}
```

<div class="content-ad"></div>

위에서 언급한 API_KEY는 이전에 BotFather로부터 받은 토큰이며, chatId는 페이로드로부터 얻은 채팅 ID이며, text는 우리의 응답입니다.

사용자로부터 메시지를 받고 다음과 같이 응답하는 봇을 위한 코드 조각을 작성해봅시다.

```js
{사용자의 이름} 님이 {사용자의 메시지} 라고 말했습니다.
```

아래와 같이 작성하시면 됩니다:

<div class="content-ad"></div>

```js
export default {
  async fetch(request, env, ctx) {
    if (request.method === "POST") {
      const payload = await request.json();
      if ("message" in payload) {
        const chatId = payload.message.chat.id;
        const input = String(payload.message.text);
        const user_firstname = String(payload.message.from.first_name);
        const response = user_firstname + " said " + input;
        await this.sendMessage(env.API_KEY, chatId, response);
      }
    }
    return new Response("OK");
  },

  async sendMessage(apiKey, chatId, text) {
    const url = `https://api.telegram.org/bot${apiKey}/sendMessage?chat_id=${chatId}&text=${text}`;
    const data = await fetch(url).then((resp) => resp.json());
  },
};
```

이제 "저장 및 배포" 버튼을 눌러 코드를 실행하세요.

## 배포

위의 코드에서 저는 Telegram 봇의 토큰인 "env.API_KEY" 환경 변수를 사용했습니다. 이 변수를 워커 환경에 설정하려면 Cloudflare의 워커 대시보드로 돌아가서 설정 섹션에서 변수를 설정할 수 있는 하위 섹션이 있습니다:

<div class="content-ad"></div>

![Image](/assets/img/2024-07-09-FreeHostingforYourTelegramBotItsEasierThanYouThink_4.png)

변수 추가를 눌러 변수 이름을 우리가 사용한 코드에 있는 "API_KEY"로 설정하고 값은 BotFather로부터 받은 텔레그램 봇의 토큰입니다. 저장하고 배포하세요.

하지만 아직 다 끝나지 않았어요. 이제 남은 일은 우리의 웹훅 엔드포인트를 우리의 텔레그램 봇에 설정하는 것 뿐입니다. 이를 위해 브라우저를 열고 아래 URL로 설정한 후 'enter'를 누르세요:

```js
https://api.telegram.org/bot{bot-token}/setWebhook?url={worker-url}
```

<div class="content-ad"></div>

여기서 'bot-token'은 BotFather에서 받은 텔레그램 봇 토큰이고, 'worker-url'은 클라우드플레어 워커 URL입니다. 이 URL은 워커 대시보드에서 찾을 수 있어요. 제 경우에는 "tech-art-bot.apulatjonov.workers.dev"였어요.

여기서 중요한 점은 텔레그램 서버에 웹훅을 설정하는 'GET' 요청을 보내는 거예요. 요청을 보내면 브라우저에서 아래와 같은 결과를 확인할 수 있어요:

```js
{
    ok: true,
    result: true,
    description: "Webhook was set"
}
```

![이미지](/assets/img/2024-07-09-FreeHostingforYourTelegramBotItsEasierThanYouThink_5.png)

<div class="content-ad"></div>

여기 갑니다. 우리의 텔레그램 봇이 준비되었습니다!

## 추가 개발

우리는 무료로 텔레그램 봇을 성공적으로 배포했지만, 키보드 추가나 인라인 버튼 같은 추가 기능을 봇에 추가하고 싶다면 어떻게 해야 할까요?

아래에서도 그 방법을 살펴보겠습니다.

<div class="content-ad"></div>

## 답변 마크업: 키보드

텔레그램 API가 URL 형식으로 작동하는 방법을 알고 나면 꽤 간단합니다.

```js
https://api.telegram.org/bot${API_KEY}/sendMessage?
chat_id=${chatId}&text=${text}
```

이 형식은 상당히 기본적인 형식이므로 답변 마크업 키보드를 추가하려면 아래의 URL에 몇 가지 라인을 추가해야 합니다:

<div class="content-ad"></div>

```js
&reply_markup={"keyboard":[["Button 1", "Button 2"],["Button 3"],["Button 4"]],"one_time_keyboard":true,"resize_keyboard":true}
```

위 코드는 기존 텔레그램 API의 한 줄입니다. 완전한 형태로는 다음과 같을 것입니다:

```js
https://api.telegram.org/bot${apiKey}/sendMessage?chat_id=${chatId}&text=${text}&reply_markup={"keyboard":[["Button 1", "Button 2"],["Button 3"],["Button 4"]],"one_time_keyboard":true,"resize_keyboard":true}
```

그 결과는 다음과 같습니다:

<div class="content-ad"></div>

<img src="/assets/img/2024-07-09-FreeHostingforYourTelegramBotItsEasierThanYouThink_6.png" />

## 답장 마크업: 인라인 키보드

이것도 위의 키보드 버튼과 동일한 규칙을 따릅니다.

인라인 키보드 버튼이 있는 응답의 텔레그램 API URL을 확인해 봅시다:

<div class="content-ad"></div>

```json
{
  "inline_keyboard": [
    [
      { "text": "Button 1", "callback_data": "button_1" },
      { "text": "Button 2", "callback_data": "button_2" }
    ],
    [
      { "text": "Button 3", "url": "https://example.com" },
      { "text": "Button 4", "url": "https://example.com" }
    ]
  ]
}
```

여기서는 버튼 이름과 할당될 콜백 데이터 또는 URL을 지정할 수 있습니다. 남은 것은 replyMarkup 키보드 URL 문자열을 위의 인라인 키보드 문자열로 바꾸는 것입니다.

```js
https://api.telegram.org/bot${apiKey}/sendMessage?chat_id=${chatId}&text=${text}&reply_markup={"inline_keyboard": [[{"text": "Button 1", "callback_data": "button_1"}, {"text": "Button 2", "callback_data": "button_2"}], [{"text": "Button 3", "url": "https://example.com"}, {"text": "Button 4", "url": "https://example.com"}]]}
```

위는 최종 URL 모습이며 결과는 아래와 같습니다:

<div class="content-ad"></div>

<img src="/assets/img/2024-07-09-FreeHostingforYourTelegramBotItsEasierThanYouThink_7.png" />

이제 무료로 서버리스 JavaScript 텔레그램 봇을 만들고 사용자 정의하고 배포하는 것을 완료했습니다. 보시다시피 매우 간단합니다. 더 많은 내용을 보고 싶다면 이 튜토리얼을 좋아해주세요!
