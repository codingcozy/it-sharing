---
title: "ASTRO JS  P2  SSG와 SSR의 차이점 및 활용 방법"
description: ""
coverImage: "/it-shar.ing/assets/no-image.jpg"
date: 2024-07-07 01:50
ogImage:
  url: /it-shar.ing/assets/no-image.jpg
tag: Tech
originalTitle: "ASTRO JS | P2 | SSG and SSR"
link: "https://dev.to/shubhamtiwari909/astro-js-p2-ssg-and-ssr-2l2l"
---

안녕하세요! 웹 개발자 여러분, 오늘은 astro js 시리즈의 두 번째 파트를 이어가겠습니다. 이번에는 SSG, SSR, 그리고 Hybrid mode와 같은 주제들을 더 다뤄볼 것입니다. 우리가 어떻게 페이지를 렌더링하고 어느 모드에서 진행할지에 관한 내용이 주를 이룰 것입니다.

- SSG란 무엇인가요?
- SSR은 무엇을 의미하나요?
- Hybrid Mode란 무엇인가요?
- SSG, SSR 및 Hybrid(SSG + SSR) 모드 활성화
- 단일 페이지에서의 SSG 및 SSR
- 데이터 가져오기

## SSG란 무엇인가요?

- 정의: SSG는 페이지의 HTML을 빌드할 때 생성하여 페이지를 정적 HTML 파일로 미리 렌더링하는 것을 의미합니다.
- Use Case: 블로그, 문서 사이트 및 마케팅 페이지와 같이 자주 변경되지 않는 콘텐츠에 적합합니다.

<div class="content-ad"></div>

### 장점:

- 빠른 성능: 콘텐츠가 미리 렌더링되어 CDN에서 빠르게 제공될 수 있습니다.
- 서버 부하 감소: 콘텐츠를 실시간으로 생성할 필요가 없습니다.
- SEO 향상: 검색 엔진이 정적 콘텐츠를 쉽게 색인화할 수 있습니다.

## SSR이란?

- 정의: SSR은 요청이 있을 때 마다 페이지의 HTML을 실시간으로 생성합니다. 이는 서버에서 콘텐츠를 클라이언트 브라우저로 전송하기 전에 수행됩니다.
- 사용 사례: 사용자 대시보드, 전자 상거래 사이트, 그리고 개인화된 콘텐츠와 같이 빈도가 높이 변경되는 동적 콘텐츠에 적합합니다.

<div class="content-ad"></div>

### 혜택:

- 신선한 콘텐츠: 요청당 생성되기 때문에 항상 최신 콘텐츠를 제공합니다.
- SEO 친화적: 콘텐츠가 서버에서 렌더링되므로 검색 엔진이 색인화할 수 있습니다.
- 클라이언트 측 작업 감소: 클라이언트 측 렌더링보다 클라이언트 측에서 필요한 JavaScript가 적습니다.

## 하이브리드 모드란?

- 정의: 하이브리드 렌더링은 한 프로젝트 내에서 SSG와 SSR을 결합합니다. 사이트의 어떤 페이지 또는 부분을 정적으로 생성하고 어떤 부분을 서버 측에서 렌더링할지 선택할 수 있습니다.
- 사용 사례: 정적 및 동적 콘텐츠가 모두 있는 웹사이트에 유용합니다. 예를 들어, 정적 게시물을 가진 블로그지만 동적 사용자 프로필 페이지를 가지고 있는 경우입니다.

<div class="content-ad"></div>

### 혜택:

- 유연성: 각 사이트 부분을 필요에 따라 최적화할 수 있습니다.
- 성능: 정적 페이지는 빠르게 제공되고 동적 페이지는 최신 콘텐츠를 제공합니다.
- SEO 및 사용자 경험: SSG 및 SSR의 이점을 균형있게 유지하여 좋은 SEO와 필요한 경우 동적 사용자 경험을 제공합니다.

## SSG, SSR 및 하이브리드(SSG + SSR) 모드 활성화

- astro.config.mjs 파일을 업데이트하여 SSG(기본값), SSR 및 하이브리드를 활성화합니다. SSR 및 하이브리드의 경우 사용할 SSR 어댑터를 지정해야 합니다.
- SSR 어댑터의 주된 목적은 Astro와 대상 서버 환경 간의 간극을 줄여 SSR 가능한 Astro 애플리케이션을 Vercel, Netlify, Node.js 서버 등과 같은 플랫폼에 배포할 수 있게 하는 것입니다.

<div class="content-ad"></div>

```js
import { defineConfig } from "astro/config";
import vercel from "@astrojs/vercel/serverless";
export default defineConfig({
  output: "server", // server - enable SSR, hybrid - enable both SSR and SSG, static - SSG(default)
  adapter: vercel(),
});
```

## 한 페이지에서 SSG 및 SSR

- 모드가 static 또는 hybrid인 경우, false 값을 가진 1개의 prerender 변수를 추가하면 페이지가 사전 렌더링 모드에서 제외되고 서버 측에서 html을 생성합니다.

```js
---
export const prerender = false;
---

<html>
  <body>
    <h1>동적 페이지</h1>
    <p>이 페이지는 정적으로 생성되지 않습니다.</p>
  </body>
</html>
```

<div class="content-ad"></div>

- 만약 모드가 서버이면, true 값을 가진 1 개의 prerender 변수를 추가하면 페이지가 SSR 모드를 선택하지 않고 SSG와 같이 빌드 시간에 HTML을 생성합니다.

```js
---
export const prerender = true;
import Layout from "../layouts/Layout.astro";
import Bears from "../components/Bears";
import CardComponent from "../components/Card";
---

<Layout title="Astro에 오신 것을 환영합니다.">
  <div class="py-10">
    <h1 class="text-center text-3xl mb-6">홈페이지</h1>
    <Bears client:load />
    <CardComponent client:load />
  </div>
</Layout>
```

## 데이터 가져오기

### SSG

<div class="content-ad"></div>

- SSG에서는 getStaticPaths라는 메서드를 사용하여 가능한 경로의 페이지를 빌드 시간에 생성하고, 이 외의 경로는 404 페이지를 표시합니다.

```js
---
export const prerender = true;
import Layout from "../../layouts/Layout.astro";
import { getCollection, type CollectionEntry } from "astro:content";

export const getStaticPaths = async () => {
  const blogs = await getCollection("blog");
  const paths = blogs.map((blog) => {
    return {
      params: {
        blogId: blog.slug,
      },
      props: {
        blog,
      },
    };
  });
  return paths;
};

type Props = {
  blog: CollectionEntry<"blog">;
};

const { blog } = Astro.props;
const { Content } = await blog.render();
---

<Layout title={blog?.data.title}>
  <div
    class="bg-slate-900 text-slate-100 grid place-items-center min-h-screen pt-16"
  >
    <h1 class="text-3xl mb-4">{blog?.data.title}</h1>
    <div class="px-10 prose lg:prose-xl text-slate-100">
      <Content />
    </div>
  </div>
</Layout>
```

- 이 예제에서는 getCollection 메서드를 사용하여 마크다운으로 빌드된 블로그를 가져오고, createCollection 메서드로 생성된 마크다운 블로그를 가져오는 방법을 보여줍니다.
- 그런 다음 블로그 배열을 매핑하여 생성될 빌드 시간 경로가 될 params 값을 반환하고 두 번째는 개별 블로그 데이터가 있는 props입니다. 마지막으로 경로 변수 자체를 반환합니다.
- CollectionEntry 타입은 블로그 데이터를 우리가 생성한 컬렉션 스키마와 바인딩 합니다. 이 경우 "blog"입니다.
- 마지막으로, Astro.props를 사용하여 블로그를 구조분해하고, 블로그에서 Content를 블로그.render() 메서드에서 가져오는데, 이는 astro 파일에서 마크다운 콘텐츠를 렌더링하는 데 도움이 되며, 이를 `Content`와 같이 컴포넌트로 사용합니다.

### SSR

<div class="content-ad"></div>

- SSR에서는 데이터 가져오기를 직접 수행하고 try catch 블록을 사용하여 예외와 오류를 처리할 수 있습니다.

```js
---
import Layout from "../../layouts/Layout.astro";
import { Image } from "astro:assets";
import type Blogs from "../../interfaces/blogs";
import fetchApi from "../../lib/strapi";

const { blogId } = Astro.params;

let article: Blogs;

try {
  article = await fetchApi<Blogs>({
    endpoint: `blogs`,
    wrappedByKey: "data",
    wrappedByList: true,
    query: {
      populate: "*",
      "filters[slug][$eq]": blogId,
    },
  });
} catch (error) {
  console.log("Error", error);

  return Astro.redirect("/404");
}
if (!article) {
  return Astro.redirect("/404");
}
---

<Layout
  title={article?.attributes.meta.title}
  description={article?.attributes.meta.description}
>
  <section class="min-h-screen bg-slate-900 text-slate-100">
    <div class="grid justify-center pt-20 px-10">
      <div class="prose prose--md prose-invert">
        <Image
          src={`${article?.attributes.image.data.attributes.url}`}
          alt={article?.attributes.image.data.attributes.alternativeText}
          inferSize
          loading="lazy"
          class="object-cover max-h-[400px]"
        />
        <h1 class="text-3xl mb-4 text-center">{article?.attributes.title}</h1>
        <p class="mb-4 text-base">{article?.attributes.body}</p>
      </div>
    </div>
  </section>
</Layout>
```

- 이것은 페이지 슬러그를 사용하여 개별 블로그를 찾기 위해 Astro.params를 사용하는 예시입니다.
- 그런 다음 사용자 정의 fetchApi 메서드를 사용하여 블로그 데이터를 가져와서 article 변수에 저장합니다.
- 만약 article이 없다면, 404 페이지로 리디렉션됩니다.
- 마지막으로 데이터를 UI에 매핑했습니다.

이것으로 이 게시물을 마칩니다. 제 3부에서는 Astro API 참조에 대해 다룰 예정입니다.

<div class="content-ad"></div>

아래 연락처로 연락하실 수 있어요 -

Instagram - [supremacism\_\_shubh](https://www.instagram.com/supremacism__shubh/)
LinkedIn - [shubham-tiwari-b7544b193](https://www.linkedin.com/in/shubham-tiwari-b7544b193/)
Email - [shubhmtiwri00@gmail.com](mailto:shubhmtiwri00@gmail.com)

아래 링크에서 기부를 도와주실 수 있어요. 감사합니다! 👇👇
[Buy Me a Coffee](https://www.buymeacoffee.com/waaduheck)

또한 아래 게시물도 확인해보세요.
