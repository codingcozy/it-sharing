---
title: "Nextjs 14에서 Authjs로 GitHub OAuth 통합하는 방법"
description: ""
coverImage: "/assets/img/2024-07-13-IntegrateGitHubOAuthWithAuthjsinNextjs14_0.png"
date: 2024-07-13 18:45
ogImage:
  url: /assets/img/2024-07-13-IntegrateGitHubOAuthWithAuthjsinNextjs14_0.png
tag: Tech
originalTitle: "Integrate GitHub OAuth With Auth.js in Next.js 14"
link: "https://medium.com/javascript-in-plain-english/integrate-github-oauth-with-auth-js-in-next-js-14-c618049c539f"
---

Auth.js는 표준 웹 API를 기반으로한 런타임에 중립적인 라이브러리로, 다수의 최신 JavaScript 프레임워크와 깊게 통합되어 시작하기 쉽고 확장하기 쉽며 항상 개인 정보 보호와 안전한 인증 경험을 제공합니다!

![이미지](/assets/img/2024-07-13-IntegrateGitHubOAuthWithAuthjsinNextjs14_0.png)

이 글에서는 Auth.js(NextAuth.js v5)에서 제공하는 GitHub Provider를 사용하여 Next.js 14에서 GitHub OAuth 인증을 구현하는 방법을 설명하겠습니다.

## 1. Next.js 프로젝트 초기화

<div class="content-ad"></div>

```js
npx create-next-app@latest nextjs-authjs
```

<img src="/assets/img/2024-07-13-IntegrateGitHubOAuthWithAuthjsinNextjs14_1.png" />

## 2. Install next-auth module

After creating the nextjs-authjs project, you can install the next-auth@beta module using the following command:

<div class="content-ad"></div>

```js
pnpm add next-auth@beta
또는
npm install next-auth@beta
```

## 3. AUTH_SECRET 키 생성하기

다음 명령어를 사용하여 토큰 및 이메일 인증을 암호화하는 데 사용되는 AUTH_SECRET 키를 생성하세요.

```js
npx auth secret
```

<div class="content-ad"></div>

만약 Linux 또는 macOS를 사용 중이라면 openssl 명령어를 사용하여 키를 생성할 수 있어요.

```js
openssl rand -base64 33
```

키를 성공적으로 생성한 후, 프로젝트 루트 디렉토리에 있는 .env.local 파일에 키를 추가하세요:

```js
AUTH_SECRET = JWvs / uOcvzjiAcruuEKGbPaoWc / rP25B6xqs0G5QT8b6;
```

<div class="content-ad"></div>

## 4. GitHub에서 새 OAuth 앱 만들기

![이미지](/assets/img/2024-07-13-IntegrateGitHubOAuthWithAuthjsinNextjs14_2.png)

![이미지](/assets/img/2024-07-13-IntegrateGitHubOAuthWithAuthjsinNextjs14_3.png)

OAuth 애플리케이션을 성공적으로 등록한 후, 애플리케이션 세부 정보 페이지로 리디렉션됩니다. "새 클라이언트 비밀 생성" 버튼을 클릭하여 클라이언트 비밀을 생성해야 합니다:

<div class="content-ad"></div>

<img src="/assets/img/2024-07-13-IntegrateGitHubOAuthWithAuthjsinNextjs14_4.png" />

## 5. GitHub 클라이언트 ID 및 클라이언트 시크릿 구성

.env.local 파일에 GitHub 클라이언트 ID 및 클라이언트 시크릿을 추가하세요.

```js
AUTH_GITHUB_ID = { clientId };
AUTH_GITHUB_SECRET = { clientSecret };
```

<div class="content-ad"></div>

## 6. next-auth 모듈 구성하기

"Nextjs-authjs" 프로젝트의 루트 디렉토리에 auth.ts 파일을 생성하고 아래 내용을 입력하세요:

```js
import NextAuth from "next-auth";
import GitHub from "next-auth/providers/github";

export const { handlers, signIn, signOut, auth } = NextAuth({
  providers: [GitHub],
});
```

그 후, /app/api/auth/[...nextauth] 디렉토리에 새 route.ts 파일을 생성하여 App Router 라우트 핸들러를 구성하세요.

<div class="content-ad"></div>

```js
import { handlers } from "@/auth";

export const { GET, POST } = handlers;
```

## 7. 세션 정보 컴포넌트 생성

다음으로 세션 정보 컴포넌트를 생성합니다. 이 컴포넌트는 사용자가 로그인했는지 여부를 결정하고, 사용자가 로그인한 경우 사용자 정보를 표시합니다. 사용자가 로그인하지 않은 경우 로그인 버튼이 표시되어 사용자가 로그인할 수 있습니다.

루트 디렉토리에 새로운 components 디렉토리를 생성하고, session-info.tsx 파일을 추가한 다음 아래 내용을 입력하세요.

<div class="content-ad"></div>

```js
"use client";

import { signIn, signOut, useSession } from "next-auth/react";

export default function SessionInfo() {
  const { data: session, status } = useSession();

  if (status === "loading") {
    return <p>Loading...</p>;
  }

  if (status === "unauthenticated") {
    return (
      <div>
        <p>아직 로그인이 되어 있지 않습니다!</p>
        <button
          onClick={() => signIn()}
          className="bg-blue-500 hover:bg-blue-600 text-white font-bold py-2 px-4 rounded"
        >
          로그인
        </button>
      </div>
    );
  }

  return (
    <div>
      <p>환영합니다, {session?.user?.name}님!</p>
      <p>이메일: {session?.user?.email}</p>
      <button
        onClick={() => signOut()}
        className="bg-red-500 hover:bg-red-600 text-white font-bold py-2 px-4 rounded mt-2"
      >
        로그아웃
      </button>
    </div>
  );
}
```

## 8. SessionProvider 컴포넌트 추가하기

useSession 훅이 세션을 읽을 수 있도록 하려면 next-auth/react 모듈의 SessionProvider를 사용해야 합니다:

```js
// app/layout.tsx
import { SessionProvider } from "next-auth/react";

export default function RootLayout({
  children,
}: Readonly<{
  children: React.ReactNode,
}>) {
  return (
    <html lang="en">
      <body className={inter.className}>
        <SessionProvider>{children}</SessionProvider>
      </body>
    </html>
  );
}
```

<div class="content-ad"></div>

## 9. SessionInfo 컴포넌트 사용하기

앱/page.tsx 파일을 열고 이전에 생성한 SessionInfo 컴포넌트를 import하세요:

```jsx
import SessionInfo from "@/components/session-info";

export default function Home() {
  return (
    <div className="flex items-center justify-center bg-gray-100">
      <SessionInfo />
    </div>
  );
}
```

## 10. GitHub OAuth 테스트

<div class="content-ad"></div>

![Image 1](/assets/img/2024-07-13-IntegrateGitHubOAuthWithAuthjsinNextjs14_5.png)

![Image 2](/assets/img/2024-07-13-IntegrateGitHubOAuthWithAuthjsinNextjs14_6.png)

![Image 3](/assets/img/2024-07-13-IntegrateGitHubOAuthWithAuthjsinNextjs14_7.png)

![Image 4](/assets/img/2024-07-13-IntegrateGitHubOAuthWithAuthjsinNextjs14_8.png)

<div class="content-ad"></div>

위 단계를 통해 Github OAuth 통합을 완료했습니다. 통합 과정 중 문제가 발생하면 언제든지 메시지를 보내주세요.

TypeScript는 정말 멋지고 배울 가치가 있는데, TypeScript를 배우고 싶다면, 더 많은 TS 및 JS 관련 내용을 보려면 저를 Medium이나 Twitter에서 팔로우해주세요!

# 간단한 용어로 🚀

In Plain English 커뮤니티의 일원이 되어 주셔서 감사합니다! 떠나시기 전에:

<div class="content-ad"></div>

- 작가를 박수 👏 하고 팔로우해 주세요!
- 팔로우하기: X | LinkedIn | YouTube | Discord | 뉴스레터
- 다른 플랫폼 방문하기: CoFeed | Differ
- PlainEnglish.io에서 더 많은 콘텐츠 확인하기
