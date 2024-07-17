---
title: "NextAuth 자격 증명  Nextjs 14 앱 라우터와 Zod 리졸버를 사용한 이메일 및 비밀번호로 간단한 회원가입 및 로그인 방법"
description: ""
coverImage: "/assets/img/2024-07-07-NextAuthCredentialseasysignuploginwithemailpasswordNextjs14ApprouterandZodresolver_0.png"
date: 2024-07-07 20:59
ogImage:
  url: /assets/img/2024-07-07-NextAuthCredentialseasysignuploginwithemailpasswordNextjs14ApprouterandZodresolver_0.png
tag: Tech
originalTitle: "NextAuth Credentials — easy signup , login with email , password (Next.js 14 App router and Zod resolver)"
link: "https://medium.com/@pether.maciejewski/nextauth-credentials-easy-signup-login-with-email-password-next-js-7e8f043b2084"
---

NEXTAUTH & ZOD 소개

NextAuth.js는 Next.js 애플리케이션에 맞게 디자인된 강력한 오픈 소스 인증 솔루션으로, Next.js 및 서버리스 환경과 매끄럽게 통합됩니다. NextAuth.js는 이메일 및 비밀번호 없는 로그인을 포함한 다양한 인증 옵션을 지원하여 개발자들에게 다재다능한 선택지를 제공합니다. Clerk와 같은 솔루션과 비교했을 때 구성에 조금 더 많은 노력을 요구하지만, 데이터 관리에 대한 맞춤형 제어 및 유연성을 뛰어난 성능으로 제공하며 추가 비용 없이 맞춤 설정이 가능합니다.

백엔드 시스템(예: Active Directory, LDAP)과 호환되는 종합적인 인증 기능 외에도 NextAuth.js는 JSON Web Tokens 및 데이터베이스 세션과 호환됩니다.

Zod와의 통합을 통해 양식 처리 기능을 향상시키는 주목할만한 기능은 TypeScript 우선 스키마 선언 및 유효성 검증 라이브러리인 Zod와의 통합입니다. Zod를 react-hook-form과 함께 사용하여 개발자들은 양식 데이터에 대한 강력한 유형 검사와 유효성 검사를 손쉽게 강제화할 수 있습니다. 이 조합은 개발 프로세스를 간소화하고 Zod의 스키마 유효성 검사를 통해 오류를 조기에 파악하고 유효한 데이터만 처리되도록 보장함으로써 데이터 무결성과 사용자 경험을 혁신적으로 향상시킵니다.

<div class="content-ad"></div>

넥스트 오드 .js 설정을 현대적인 풀 스택 넥스트.js 앱과 함께 구성하는 방법에 대해 안내해 드릴게요 (앱 라우터 사용, TailwindCSS 및 Shadcn으로 구동됨)

저장소는 여기에서 확인할 수 있습니다.

![이미지](/assets/img/2024-07-07-NextAuthCredentialseasysignuploginwithemailpasswordNextjs14ApprouterandZodresolver_0.png)

소개

<div class="content-ad"></div>

이미 Next.js 프로젝트를 초기화하고 GitHub에 푸시했다고 가정하면, 다음 단계를 통해 Vercel에서 프로젝트를 위해 직접 PostgreSQL 데이터베이스를 설정하는 방법을 안내해 드리겠습니다.

1- 스토리지를 클릭한 다음 데이터베이스 생성 버튼을 클릭합니다.

![img](/assets/img/2024-07-07-NextAuthCredentialseasysignuploginwithemailpasswordNextjs14ApprouterandZodresolver_1.png)

2- 그런 다음 Postgres Serverless SQL을 선택할 수 있습니다.

<div class="content-ad"></div>

<img src="/assets/img/2024-07-07-NextAuthCredentialseasysignuploginwithemailpasswordNextjs14ApprouterandZodresolver_2.png" />

3 — 데이터베이스에 이름을 지정하고 '만들기'를 클릭하세요

<img src="/assets/img/2024-07-07-NextAuthCredentialseasysignuploginwithemailpasswordNextjs14ApprouterandZodresolver_3.png" />

4 — 그 후에 새로 생성한 데이터베이스를 프로젝트에 연결하세요

<div class="content-ad"></div>

이미지 태그를 Markdown 형식으로 바꿔주세요.

![이미지](/assets/img/2024-07-07-NextAuthCredentialseasysignuploginwithemailpasswordNextjs14ApprouterandZodresolver_4.png)

Vercel CLI를 설치했는지 확인하세요:

```js
npm i -g vercel
```

5 - 그런 다음 프로젝트를 환경에 있는 PostgreSQL 데이터베이스와 연결하세요 (3개의 질문이 나올 것입니다):

<div class="content-ad"></div>

```js
vercel link
```

6 — 이제 Vercel에서 PostgreSQL을 설정할 때 생성된 모든 환경 변수를 복제하려고 합니다.

```js
vercel env pull .env.development.local
```

이제 .env.development.local에 모든 필요한 자격 증명이 포함된 것을 확인할 수 있을 것입니다.

<div class="content-ad"></div>

"VERCEL="1"에 대한 의견을 남겨보는 것을 제안드립니다. 로컬 호스트에서 작업 중일 때 https를 강제할 수 있기 때문입니다.

![screenshot](/assets/img/2024-07-07-NextAuthCredentialseasysignuploginwithemailpasswordNextjs14ApprouterandZodresolver_5.png)

필요한 모듈 설정하기

저는 Shadcn을 사용하여 구성요소 빌드를 가속화하고 있습니다."

<div class="content-ad"></div>

```bash
npm i bcrypt next-auth
npm i --save-dev @types/bcrypt
npm i @vercel/postgres
npx shadcn-ui@latest init
npx shadcn-ui@latest add form
```

**앱 디렉터리와 로직 설정**

레지스터 및 로그인 페이지를 준비해 봅시다.

```bash
>app
 >login
   form.tsx
   page.tsx
 >register
   form.tsx
   page.tsx
```

<div class="content-ad"></div>

그리고 NextAuth를 위한 API 폴더 구조

```js
> app
  > api
    > auth
      > [...nextauth]
        route.ts
```

NEXT AUTH 문서를 자세히 살펴보기

Credentials 제공자를 사용하면 사용자 이름 및 비밀번호, 도메인 또는 이중 인증 또는 하드웨어 장치(예: Yubikey U2F/FIDO)와 같은 임의의 자격 증명으로 로그인을 처리할 수 있습니다.

<div class="content-ad"></div>

1 - api/auth/[...nextauth]에 대한 route.ts를 문서에 따라 준비해 봅시다. 일단 권한 부여 로직을 제거했습니다.

```js
// app>api>auth>[...nextauth]>route.ts

import NextAuth from "next-auth/next";
import CredentialsProvider from "next-auth/providers/credentials";
import { sql } from "@vercel/postgres";
import { compare } from "bcrypt";

const handler = NextAuth({
  session: {
    strategy: "jwt",
  },

  pages: {
    signIn: "/login",
  },

  providers: [
    CredentialsProvider({
      // The name to display on the sign in form (e.g. 'Sign in with...')
      name: "Credentials",
      // The credentials are used to generate a suitable form on the sign-in page.
      // You can specify whatever fields you are expecting to be submitted.
      // e.g. domain, username, password, 2FA token, etc.
      // You can pass any HTML attribute to the <input> tag through the object.
      credentials: {
        email: {},
        password: {},
      },
      async authorize(credentials, req) {
        return null;
      },
    }),
  ],
});

export { handler as GET, handler as POST };
```

2 - 또한, Zod로 검증된 폼값을 올바르게 전달하고 있는지 확인하기 위해 api/auth/register/route.ts를 준비해 봅시다.

```js
// app>api>auth>register>route.ts

import { NextResponse } from "next/server";

export async function POST(request: Request) {
  try {
    const { email, password } = await request.json();
    // 여기에 유효성 검증을 추가하는 것이 좋을 수 있습니다.

    console.log({ email, password });
  } catch (e) {
    console.log({ e });
  }

  return NextResponse.json({ message: "success" });
}
```

<div class="content-ad"></div>

3 — 레지스터 프론트 엔드를 준비해봅시다:

```js
import { getServerSession } from "next-auth";
import { redirect } from "next/navigation";

import FormPage from "./form";

export default async function RegisterPage() {
  const session = await getServerSession();

  if (session) {
    redirect("/");
  }

  return (
    <section className="bg-black h-screen flex items-center justify-center">
      <div className="w-[600px]">
        <FormPage />
      </div>
    </section>
  );
}
```

4 — 그리고 FormPage 자체 (react-hook-form 및 zod 검증을 사용합니다)

```js
"use client";

import { zodResolver } from "@hookform/resolvers/zod";
import { useForm } from "react-hook-form";
import * as z from "zod";

import { Button } from "@/components/ui/button";
import {
  Form,
  FormControl,
  FormDescription,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from "@/components/ui/form";
import { Input } from "@/components/ui/input";
import { toast } from "@/components/ui/use-toast";

const FormSchema = z.object({
  username: z.string().min(2, {
    message: "사용자 이름은 최소 2자여야 합니다.",
  }),
  password: z.string().min(6, {
    message: "비밀번호는 최소 6자여야 합니다.",
  }),
});

type FormData = z.infer<typeof FormSchema>;

export default function FormPage() {
  const form = useForm({
    resolver: zodResolver(FormSchema),
    defaultValues: {
      username: "",
      password: "",
    },
  });

  const onSubmit = async (data: FormData) => {
    console.log("양식 제출 중", data);

    const { username: email, password } = data;

    try {
      const response = await fetch("/api/auth/register", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({ email, password }),
      });
      if (!response.ok) {
        throw new Error("네트워크 응답이 올바르지 않습니다.");
      }
      // 여기서 응답 처리
      console.log("등록 성공", response);
      toast({ title: "등록 성공" });
    } catch (error: any) {
      console.error("등록 실패:", error);
      toast({ title: "등록 실패", description: error.message });
    }
  };

  return (
    <Form {...form} className="w-2/3 space-y-6">
      <form onSubmit={form.handleSubmit(onSubmit)}>
        <FormField
          control={form.control}
          name="username"
          render={({ field }) => (
            <FormItem>
              <FormLabel>사용자 이름</FormLabel>
              <FormControl>
                <Input placeholder="사용자 이름" {...field} />
              </FormControl>
              <FormDescription>
                이것은 공개 표시 이름입니다.
              </FormDescription>
            </FormItem>
          )}
        />
        <FormField
          control={form.control}
          name="password"
          render={({ field }) => (
            <FormItem>
              <FormLabel>비밀번호</FormLabel>
              <FormControl>
                <Input placeholder="비밀번호" {...field} type="password" />
              </FormControl>
            </FormItem>
          )}
        />
        <Button type="submit">제출</Button>
      </form>
    </Form>
  );
}
```

<div class="content-ad"></div>

FormPage의 상호 작용성으로 인해 클라이언트 구성요소여야 하므로 위에있는 "use client" 지시문을 주목했으면 합니다.

onSubmit 비동기 함수를 자세히 살펴보세요. 여기서는 폼 데이터 (이메일, 비밀번호)를 /api/auth/register에 전달합니다.

```js
const onSubmit = async (data: FormData) => {
    console.log("폼 제출 중", data);

    const { username: email, password } = data;

    try {
      const response = await fetch("/api/auth/register", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({ email, password }),
      });
      if (!response.ok) {
        throw new Error("네트워크 응답이 올바르지 않았습니다");
      }
      // 여기서 응답 처리
      console.log("등록 성공", response);
      toast({ title: "등록 성공" });
    } catch (error: any) {
      console.error("등록 실패:", error);
      toast({ title: "등록 실패", description: error.message });
    }
  };
```

따라서 이 단계에서 /register 페이지를 방문하면 다음이 표시됩니다:

<div class="content-ad"></div>

아래의 표를 Markdown 형식으로 변경해 주세요:

| Syntax    | Description |
| --------- | ----------- |
| Header    | Title       |
| Paragraph | Text        |

<div class="content-ad"></div>

현재 이 단계에서 데이터베이스에 사용자 테이블이 필요합니다.

![이미지](/assets/img/2024-07-07-NextAuthCredentialseasysignuploginwithemailpasswordNextjs14ApprouterandZodresolver_8.png)

이제 `api/auth/register/route.ts`에 비밀번호 해싱을 포함한 등록 로직을 통합합시다:

```js
import { NextResponse } from "next/server";
import { hash } from "bcrypt";
import { sql } from "@vercel/postgres";

export async function POST(request: Request) {
  try {
    const { email, password } = await request.json();
    // 여기에 일부 유효성 검사를 추가할 수 있습니다

    console.log({ email, password });

    const hashedPassword = await hash(password, 10);

    const response = await sql`INSERT INTO users (email, password) VALUES (${email}, ${hashedPassword})`;
  } catch (e) {
    console.log({ e });
  }

  return NextResponse.json({ message: "성공" });
}
```

<div class="content-ad"></div>

지금, 등록 과제를 해결한 후에는 Postgres 데이터베이스에 등록된 고객을 볼 수 있어야 합니다:

![image](/assets/img/2024-07-07-NextAuthCredentialseasysignuploginwithemailpasswordNextjs14ApprouterandZodresolver_9.png)

큰 성과 - 등록을 통해 우리의 Next.js 프론트엔드에서 Postgres DB로의 작업이 예상대로 작동 중 :)

이제 로그인 기능이 있습니다.

<div class="content-ad"></div>

로그인 페이지 코드입니다:

```js
// >app>login>page.tsx

import { getServerSession } from "next-auth";
import { redirect } from "next/navigation";
import LoginForm from "./form";

export default async function LoginPage() {
  const session = await getServerSession();
  console.log({ session });

  if (session) {
    redirect("/");
  }

  return (
    <section className="bg-black h-screen flex items-center justify-center">
      <div className="w-[600px]">
        <LoginForm />;
      </div>
    </section>
  );
}
```

그리고, LoginForm 컴포넌트 코드입니다:

```js
"use client";

import { signIn } from "next-auth/react";
import { useRouter } from "next/navigation";
import { zodResolver } from "@hookform/resolvers/zod";
import { useForm } from "react-hook-form";
import * as z from "zod";

import { Button } from "@/components/ui/button";
import {
  Form,
  FormControl,
  FormDescription,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from "@/components/ui/form";
import { Input } from "@/components/ui/input";
import { toast } from "@/components/ui/use-toast";

const FormSchema = z.object({
  email: z.string().email({
    message: "잘못된 이메일 주소입니다.",
  }),
  password: z.string().min(6, {
    message: "비밀번호는 최소 6자 이상이어야 합니다.",
  }),
});

type FormData = z.infer<typeof FormSchema>;

export default function LoginForm() {
  const router = useRouter();

  const form = useForm({
    resolver: zodResolver(FormSchema),
    defaultValues: {
      email: "",
      password: "",
    },
  });

  const onSubmit = async (data: FormData) => {
    console.log("폼 제출 중", data);

    const { email, password } = data;

    try {
      const response: any = await signIn("credentials", {
        email,
        password,
        redirect: false,
      });
      console.log({ response });
      if (!response?.error) {
        router.push("/");
        router.refresh();
      }

      if (!response.ok) {
        throw new Error("네트워크 응답이 문제가 있습니다");
      }
      // 여기서 응답 처리
      console.log("로그인 성공", response);
      toast({ title: "로그인 성공" });
    } catch (error: any) {
      console.error("로그인 실패:", error);
      toast({ title: "로그인 실패", description: error.message });
    }
  };

  return (
    <Form {...form} className="w-2/3 space-y-6">
      <form
        onSubmit={form.handleSubmit(onSubmit)}
        className="text-white p-4 md:p-16 border-[1.5px] rounded-lg border-gray-300 flex flex-col items-center justify-center gap-y-6"
      >
        <FormField
          control={form.control}
          name="email"
          render={({ field }) => (
            <FormItem>
              <FormLabel>이메일 입력</FormLabel>
              <FormControl>
                <Input
                  className="text-black"
                  placeholder="이메일 주소를 입력하세요"
                  {...field}
                  type="text"
                />
              </FormControl>
            </FormItem>
          )}
        />
        <FormField
          control={form.control}
          name="password"
          render={({ field }) => (
            <FormItem>
              <FormLabel>비밀번호 입력</FormLabel>
              <FormControl>
                <Input
                  className="text-black"
                  placeholder="비밀번호"
                  {...field}
                  type="password"
                />
              </FormControl>
            </FormItem>
          )}
        />
        <Button
          type="submit"
          className="hover:scale-110 hover:bg-cyan-700"
          disabled={form.formState.isSubmitting}
        >
          {form.formState.isSubmitting ? "열고 있습니다...." : "열어봅시다!"}
        </Button>
      </form>
    </Form>
  );
}
```

<div class="content-ad"></div>

다음은 콘솔에 입력된 이메일과 비밀번호를 정확히 표시해야 합니다:

![이미지](/assets/img/2024-07-07-NextAuthCredentialseasysignuploginwithemailpasswordNextjs14ApprouterandZodresolver_10.png)

이제 권한 부여 로직을 구현할 차례입니다. 이는 데이터베이스에서 사용자를 가져와서 비밀번호 해시를 해제하고 제공된 자격 증명과 비교하는 과정을 포함합니다. 이 튜토리얼에서는 간단한 SQL 쿼리를 사용할 것이지만, 이러한 방식의 장단점을 인정합니다.

```js
import NextAuth from "next-auth/next";
import CredentialsProvider from "next-auth/providers/credentials";
import { sql } from "@vercel/postgres";
import { compare } from "bcrypt";

const handler = NextAuth({
  session: {
    strategy: "jwt",
  },

  pages: {
    signIn: "/login",
  },

  providers: [
    CredentialsProvider({
      // The name to display on the sign in form (e.g. 'Sign in with...')
      name: "Credentials",
      credentials: {
        email: {},
        password: {},
      },
      async authorize(credentials, req) {
        const response = await sql`
          SELECT * FROM users WHERE email=${credentials?.email}
        `;
        const user = response.rows[0];

        const passwordCorrect = await compare(credentials?.password || "", user.password);

        if (passwordCorrect) {
          return {
            id: user.id,
            email: user.email,
          };
        }

        console.log("credentials", credentials);
        return null;
      },
    }),
  ],
});

export { handler as GET, handler as POST };
```

<div class="content-ad"></div>

또한 .env 파일에 NEXTAUTH_URL 및 NEXTAUTH_SECRET을 포함해야합니다. 터미널에서 openssl rand -base64 32를 실행하여 안전한 NEXTAUTH_SECRET 값을 생성할 수 있습니다.

```js
NEXTAUTH_URL = "http://localhost:3000";
NEXTAUTH_SECRET = password;
```

제한된 페이지 구성

페이지에 대한 액세스 제어를 강화하려면 Next.js 프로젝트의 루트 디렉토리에 middleware.ts 파일을 생성하십시오.

<div class="content-ad"></div>

```js
export { default } from "next-auth/middleware";

export const config = {
  // 지정한 페이지에 대한 액세스 제한을 설정합니다
  matcher: ["/"],
};
```

이 설정을 통해 애플리케이션이 지정된 페이지로의 액세스를 제한하는 방식으로 구성되었으며, [...] nextauth.ts 구성에 정의된 JWT 전략 및 리다이렉션 정책과 원활하게 작동합니다.

좋은 코딩하세요 :)

Piotr
