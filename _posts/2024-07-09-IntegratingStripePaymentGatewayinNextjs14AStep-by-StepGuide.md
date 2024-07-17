---
title: "Nextjs 14에서 Stripe 결제 게이트웨이 통합 단계별 가이드"
description: ""
coverImage: "/assets/img/2024-07-09-IntegratingStripePaymentGatewayinNextjs14AStep-by-StepGuide_0.png"
date: 2024-07-09 13:45
ogImage:
  url: /assets/img/2024-07-09-IntegratingStripePaymentGatewayinNextjs14AStep-by-StepGuide_0.png
tag: Tech
originalTitle: "Integrating Stripe Payment Gateway in Next.js 14: A Step-by-Step Guide"
link: "https://medium.com/@rakeshdhariwal61/integrating-stripe-payment-gateway-in-next-js-14-a-step-by-step-guide-1bd17d164c2c"
---

요즘 디지털 시대에는 웹 애플리케이션의 결제 처리 설정이 중요합니다. 전자 상거래 스토어나 구독 기반 서비스를 운영하더라도, 원활한 결제 경험 제공은 성공에 꼭 필요합니다. 이 글에서는 Next.js 애플리케이션에 Stripe 결제 게이트웨이를 통합하는 방법에 대해 알아보겠습니다.

# 문제 상황:

구독 기반 사용을 제공하는 소프트웨어 서비스(SaaS) 제품 소유자로서, 구독 구매를 위한 안전하고 원활한 결제 시스템을 통합하는 도전에 직면하게 됩니다. 사용자가 Stripe 결제 게이트웨이를 사용하여 다양한 요금제에 쉽게 가입할 수 있도록 하는 것이 목표입니다. 결제 성공 시 사용자의 구독 요금제 정보가 데이터베이스에 정확히 업데이트되어야 하며, 이를 통해 SaaS 제품의 기능에 중단 없이 액세스할 수 있습니다. 데이터베이스의 무결성을 유지하면서 이 프로세스를 효율적으로 구현하여 사용자 경험을 원활하게 하는 방법은 무엇일까요?

# 솔루션: Stripe 결제 통합을 위한 웹훅 구현

<div class="content-ad"></div>

구독 결제를 효율적으로 처리하고 결제 결과에 따라 데이터베이스를 업데이트하기 위해 웹훅 방식을 도입할 것입니다. 웹훅은 Stripe로부터 결제 이벤트에 대한 실시간 알림을 신뢰할 수 있는 방법을 제공합니다. 성공적인 결제 또는 실패와 같은 결제 이벤트에 대한 실시간 알림을 받을 수 있습니다. 이 솔루션을 구현하는 방법은 다음과 같습니다:

![이미지](/assets/img/2024-07-09-IntegratingStripePaymentGatewayinNextjs14AStep-by-StepGuide_0.png)

# 가격 ID 생성

- 제품을 위한 가격 ID를 생성하세요. [여기를 클릭하세요](링크)

<div class="content-ad"></div>

가용성 정보 - 2020-07-09

1. [Stripe 결제 게이트웨이 Next.js에 통합하는 방법: 스텝 바이 스텝 가이드](/assets/img/2024-07-09-IntegratingStripePaymentGatewayinNextjs14AStep-by-StepGuide_1.png)

2. 제품 추가 버튼을 클릭하고 모든 세부 정보를 입력해 주세요.

![이미지](/assets/img/2024-07-09-IntegratingStripePaymentGatewayinNextjs14AStep-by-StepGuide_2.png)

3. 모든 세부 정보 입력 후, price_1OtBCOSJjlksdcPyxHoCyHDF와 같은 가격 ID를 받게 됩니다.

<div class="content-ad"></div>

# NextJS에서 Stripe 구현하기 (프론트엔드 파트)

- stripe-js 설치하기.

```js
npm install @stripe/stripe-js
```

2. 환경 요구사항:

<div class="content-ad"></div>

- @stripe/stripe-js 패키지가 설치되어 있는지 확인해주세요.
- NEXT_PUBLIC_STRIPE_PUBLIC_KEY를 포함한 Next.js 프로젝트환경 변수를 설정해주세요.

3. Subscribe 컴포넌트를 생성하세요.

```js
'use client';
import { loadStripe } from '@stripe/stripe-js';
import axios from 'axios';
type props = {
  priceId: string;
  price: string;
  description:string;
};
const SubscribeComponent= ({ priceId, price, description }: props) => {
  const handleSubmit = async () => {
    const stripe = await loadStripe(
      process.env.NEXT_PUBLIC_STRIPE_PUBLIC_KEY as string
    );
    if (!stripe) {
      return;
    }
    try {
      const response = await axios.post('/api/stripe/checkout', {
        priceId: priceId
      });
      const data = response.data;
      if (!data.ok) throw new Error('Something went wrong');
      await stripe.redirectToCheckout({
        sessionId: data.result.id
      });
    } catch (error) {
      console.log(error);
    }
  };
  return (
<div>
{description}을(를) 받으려면 아래 버튼을 클릭하세요.
<button onClick={handleSubmit}>
      {price} 내에서 업그레이드
    </button>
</div>
  );
};
export default SubscribeComponent;
```

# SubscribeComponent 함수 설명

<div class="content-ad"></div>

기능 목적:

- SubscribeComponent 함수는 React 컴포넌트로, 구독 버튼을 렌더링하고 Stripe 결제 게이트웨이를 사용하여 구독 구매 프로세스를 처리하는 역할을 합니다.

속성:

- priceId: 구독 요금제와 관련된 가격의 고유 식별자(ID)를 나타냅니다.
- price: 구독 요금을 나타냅니다.
- description: 구독 요금제의 설명 또는 이름을 제공합니다.

<div class="content-ad"></div>

기능 실행:

- 컴포넌트가 렌더링될 때 "가격으로 업그레이드"라고 표시된 버튼이 표시됩니다. 여기서 '가격'은 구독 요금입니다.

handleSubmit 함수:

- handleSubmit 함수는 구독 구매 프로세스를 시작하는 비동기 함수입니다.
- 먼저 @stripe/stripe-js 패키지의 loadStripe 메서드를 사용하여 Stripe 클라이언트 사이드 라이브러리를 로드합니다.
- 그런 다음 Stripe 객체가 성공적으로 로드되었는지 확인합니다. 그렇지 않으면 함수를 종료합니다.
- Stripe 객체가 성공적으로 로드되면 서버 측 엔드포인트 (/api/stripe/checkout)로 POST 요청을 보내어 체크아웃 세션을 만듭니다.
- 서버로부터의 응답을 기다리며, 체크아웃 세션을 위한 세션 ID를 기대합니다.
- 응답이 오류를 나타내면 (!data.ok), 오류 메시지를 기록합니다.
- 오류가 발생하지 않으면, Stripe가 제공하는 redirectToCheckout 메서드를 사용하여 사용자를 Stripe 체크아웃 페이지로 리다이렉션합니다.
- 서버로부터 얻은 세션 ID를 redirectToCheckout에 전달하여 체크아웃 프로세스를 시작합니다.

<div class="content-ad"></div>

사용 방법:

- 이 구성 요소를 사용하려면, Next.js 애플리케이션에 가져와서 필요한 props(priceId, price, description)를 전달하면 됩니다.
- 응용 프로그램에서 구독 버튼이 나타나길 원하는 위치에 SubscribeComponent를 렌더링하면 됩니다.

오류 처리:

- 구독 구매 과정 중 발생하는 오류는 디버깅을 위해 콘솔에 로그가 남깁니다.

<div class="content-ad"></div>

프론트엔드 쪽으로 모두 준비되었습니다.

## Stripe 통합을 위한 백엔드 엔드포인트

우리의 Next.js 애플리케이션에 Stripe 결제 처리를 원활하게 통합하기 위해 백엔드에 두 개의 엔드포인트를 만들어야 합니다: /api/stripe/checkout과 /api/webhooks/checkout.

## 의존성:

<div class="content-ad"></div>

- Stripe: 이 코드는 Stripe API와 상호 작용하기 위한 기능을 제공하는 Stripe 패키지를 활용합니다.

```js
npm install stripe
```

## 환경 변수:

- STRIPE_SECRET_KEY: 이 환경 변수에는 Stripe API의 인증 및 요청을 위해 사용되는 시크릿 키가 포함되어 있어야 합니다.
- NEXT_BASE_URL: 이 환경 변수는 Next.js 애플리케이션의 기본 URL을 나타내며, 체크아웃 세션의 성공 및 취소 URL을 구성하는 데 사용됩니다.
- STRIPE_SECRET_WEBHOOK_KEY: 환경 변수는 서버 측에서 Stripe 이벤트를 처리하는 데 사용되는 webhook 엔드포인트를 안전하게 하는 데 중요한 역할을 합니다. 이후에 웹훅을 만들 예정입니다.

<div class="content-ad"></div>

/api/stripe/checkout Endpoints

- 이 엔드포인트는 사용자가 구독하기로 결정했을 때 체크아웃 프로세스를 시작합니다.
- 요청을 받으면 Stripe API와 상호 작용하여 체크아웃 세션을 생성합니다.
- 선택한 구독 요금제의 priceId를 입력으로 받습니다.
- 체크아웃 세션이 성공적으로 생성되면 세션 ID를 프론트엔드에 반환합니다.
- 이 엔드포인트는 지불 흐름을 시작하고 사용자를 Stripe 체크아웃 페이지로 안내하는 역할을 합니다.
- metadata 객체에는 데이터베이스를 업데이트하기 위해 웹훅 엔드포인트에서 활용할 수 있는 다양한 키-값 쌍을 전달할 수 있습니다.

```js
import Stripe from 'stripe';
import { NextRequest, NextResponse } from 'next/server';
const stripe = new Stripe(process.env.STRIPE_SECRET_KEY as string);
export async function POST(request: NextRequest) {
  try {

    // 사용자가 유효한지 여부 같은 기본적인 확인을 여기서 구현할 수 있습니다.
    const data = await request.json();
    const priceId = data.priceId;
    const checkoutSession: Stripe.Checkout.Session =
      await stripe.checkout.sessions.create({
        payment_method_types: ['card'],
        line_items: [
          {
            price: priceId,
            quantity: 1
          }
        ],
        mode: 'payment',
        success_url: `${process.env.NEXT_BASE_URL}/billing`,
        cancel_url: `${process.env.NEXT_BASE_URL}/billing`,
        metadata: {
          userId: loggedUser.id,
          priceId
        }
      });
    return NextResponse.json({ result: checkoutSession, ok: true });
  } catch (error) {
    console.log(error);
    return new NextResponse('내부 서버 오류', { status: 500 });
  }
}
```

/api/webhooks/checkout 엔드포인트:

<div class="content-ad"></div>

- 이 엔드포인트는 Stripe로부터 결제 이벤트에 관한 웹훅 알림을 받기 위해 만들어졌습니다.
- Stripe는 결제 프로세스 중 중요한 이벤트가 발생할 때, 성공적인 결제, 결제 실패, 또는 구독 취소와 같이 이 엔드포인트로 알림을 보냅니다.
- Stripe로부터 웹훅 이벤트를 받으면, 이 엔드포인트는 이벤트 데이터를 처리하고 관련 정보를 데이터베이스에 업데이트합니다.
- 예를 들어, 결제가 성공적으로 처리된 경우, 이 엔드포인트는 사용자의 구독 상태를 업데이트하거나 관련 작업을 트리거합니다.
- 응용 프로그램이 최신 결제 상태를 유지하고 데이터 일관성을 유지할 수 있도록 보장합니다.

```js
import Stripe from 'stripe';
import { NextRequest } from 'next/server';
import { headers } from 'next/headers';
type METADATA = {
  userId: string;
  priceId: string;
};
const stripe = new Stripe(process.env.STRIPE_SECRET_KEY!);

export async function POST(request: NextRequest) {
  const body = await request.text();
  const endpointSecret = process.env.STRIPE_SECRET_WEBHOOK_KEY!;
  const sig = headers().get('stripe-signature') as string;
  let event: Stripe.Event;
  try {
    event = stripe.webhooks.constructEvent(body, sig, endpointSecret);
  } catch (err) {
    return new Response(`Webhook Error: ${err}`, {
      status: 400
    });
  }

  const eventType = event.type;
  if (
    eventType !== 'checkout.session.completed' &&
    eventType !== 'checkout.session.async_payment_succeeded'
  )
    return new Response('Server Error', {
      status: 500
    });
  const data = event.data.object;
  const metadata = data.metadata as METADATA;
  const userId = metadata.userId;
  const priceId = metadata.priceId;
  const created = data.created;
  const currency = data.currency;
  const customerDetails = data.customer_details;
  const amount = data.amount_total;
  const transactionDetails = {
    userId,
    priceId,
    created,
    currency,
    customerDetails,
    amount,
  };
  try {
    // database update here
    return new Response('Subscription added', {
      status: 200
    });
  } catch (error) {
    return new Response('Server error', {
      status: 500
    });
  }
}
```

# Stripe에서 웹훅 엔드포인트를 만드는 단계

<img src="/assets/img/2024-07-09-IntegratingStripePaymentGatewayinNextjs14AStep-by-StepGuide_3.png" />

<div class="content-ad"></div>

- Stripe 대시보드에 로그인하십시오.
- 웹훅 설정으로 이동하십시오:

로그인한 후 대시보드에서 “개발자” 섹션을 찾으세요. 이 섹션은 일반적으로 왼쪽 사이드바 메뉴에서 접근할 수 있습니다.

3. 웹훅에 액세스하십시오:

- “개발자” 섹션 내에서 “웹훅” 옵션을 찾아 클릭하십시오. 이 동작으로 웹훅 설정 페이지로 이동하게 됩니다.

<div class="content-ad"></div>

**4. 엔드포인트 추가:**

- 웹훅 설정 페이지에서 "엔드포인트 추가" 버튼을 찾아 클릭하여 새 웹훅 엔드포인트를 생성하는 프로세스를 시작합니다.

**5. 웹훅 엔드포인트 구성:**

- "엔드포인트 추가" 양식에서 다음 세부 정보를 제공하십시오:
  - 엔드포인트 URL: Stripe에서 웹훅 이벤트를 수신할 애플리케이션의 서버 측 엔드포인트 URL을 입력합니다.
  - 전송할 이벤트: Stripe가 이 엔드포인트로 전송할 이벤트 유형을 선택합니다. 미리 정의된 이벤트 유형 목록 중에서 선택하거나 "모든 이벤트"를 선택하여 모든 이벤트 유형의 알림을 받을 수 있습니다.
  - 버전: 웹훅 이벤트에 사용할 API 버전을 선택합니다.
  - 서명 비밀 (선택 사항): 선택적으로 웹훅 이벤트에 서명하여 추가 보안을 위한 서명 비밀을 생성할 수 있습니다. 이는 웹훅 엔드포인트의 무단 액세스를 방지하기 위해 권장되는 작업입니다.

<div class="content-ad"></div>

5. 저장 엔드포인트:

- 필요한 정보를 제공한 후 "엔드포인트 추가" 또는 "저장" 버튼을 클릭하여 웹훅 엔드포인트를 생성하세요.

6. 웹훅을 클릭한 후에 서명 비밀키를 공개하고, 해당 비밀키를 STRIPE_SECRET_WEBHOOK_KEY 환경 변수에 붙여넣으세요.

![이미지](/assets/img/2024-07-09-IntegratingStripePaymentGatewayinNextjs14AStep-by-StepGuide_4.png)

<div class="content-ad"></div>

# 로컬에서 테스트 중

로컬에서 웹훅을 테스트하려면 Stripe의 공식 문서를 참조할 수 있어요.

[공식 문서 보기](https://docs.stripe.com/webhooks)
