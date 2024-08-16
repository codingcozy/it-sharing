---
title: "기술 스택을 자주 변경하지 말아야 하는 이유 5가지"
description: ""
coverImage: "/assets/img/2024-07-27-WhyyourTechStackshouldntchange_0.png"
date: 2024-07-27 13:53
ogImage: 
  url: /assets/img/2024-07-27-WhyyourTechStackshouldntchange_0.png
tag: Tech
originalTitle: "Why your Tech Stack shouldnt change"
link: "https://dev.to/joshlawson100/why-your-tech-stack-shouldnt-change-51m1"
isUpdated: true
updatedAt: 1723816892821
---



기술은 끊임없이 변화하고 있어요. 정말 매달 새로운 JavaScript 프레임워크가 등장하는 것 같아요. 그러나 JavaScript 프레임워크는 뒷담화로 남기고, SaaS를 빠르게 출시하기 위한 핵심은 더 빠르게 코드를 작성하는 것이에요.

코딩을 처음 배울 때, 선택할 수 있는 언어가 너무 많아서 당황했어요. 다른 수천 명처럼, 최고의 프로그래밍 언어를 찾기 위해 YouTube의 미궁에 빠져들었죠. 몇 년 후, 제가 웹 개발 기술 스택을 정교하게 다듬어 한 주 내에 출시할 수 있는 정도로 효율적으로 만들었어요.

이것이 Cli.ck를 개발하는 데 사용 중인 기술 스택이에요.

## 프론트엔드

<div class="content-ad"></div>


![이미지](/assets/img/2024-07-27-WhyyourTechStackshouldntchange_0.png)

내 기술 스택은 전부 자바스크립트 지향입니다.

저는 Next.js, Tailwind, 그리고 DaisyUI를 사용합니다.

DaisyUI는 개발 속도를 높이기 위해 사용하는 멋진 Tailwind 컴포넌트 라이브러리입니다. 여기에는 태양 아래의 모든 컴포넌트가 포함되어 있으며, 자체 아름다운 스타일링과 20가지 이상의 색상 테마를 제공합니다.


<div class="content-ad"></div>

저도 shadcn/ui가 좋다는 소문은 듣긴 했지만 아직 시도해보지는 않았어요.

## 백엔드

![이미지](/assets/img/2024-07-27-WhyyourTechStackshouldntchange_1.png)

저는 Vercel의 통합 PostgreSQL 데이터베이스를 사용하고 있어요. 취미용 플랜으로 무료로 제공되는 것이 있어서 꼭 한 번 확인해보시는 걸 추천드립니다.

<div class="content-ad"></div>

저는 원시 SQL을 작성하기 싫어서 Prisma를 데이터베이스 ORM으로 사용해요.

## 호스팅 + 부가 기능

![이미지](/assets/img/2024-07-27-WhyyourTechStackshouldntchange_2.png)

사용자 인증 및 사용자 관리에는 Auth.js를 사용하고, 결제에는 Lemon Squeezy를 사용해요.

<div class="content-ad"></div>

모든 이메일은 Resend를 통해 처리됩니다.

기술 스택은 중요하지 않아요. 중요한 건 어떻게 사용하는지죠. NASA는 계산기로 달에 사람을 보냈어요 (그렇게까지는 아니지만, 같은 계산 능력을 가진 컴퓨터로 보냈죠). 그래서 당신의 기술 스택에 어떤 언어/도구가 있는지는 중요하지 않아요.

- 조시

참고: 현재 SaaS인 Cli.ck를 개발 중이에요. 작은 밴드들이 클릭 트랙을 더 쉽게 사용할 수 있도록 목표로 합니다. 이에 관심이 있다면, 또는 제 이야기를 따르고 싶다면, 여기에서 대기 목록에 등록하세요. 개발 프로세스의 뒷 이야기에 대해 여러분께 계속 업데이트할 거예요.