---
title: "Nextjs 14 서버 컴포넌트를 효과적으로 사용 하는 방법"
description: ""
coverImage: "/assets/img/2024-07-07-HowtoUseNextjs14sServerComponentsEffectively_0.png"
date: 2024-07-07 19:06
ogImage:
  url: /assets/img/2024-07-07-HowtoUseNextjs14sServerComponentsEffectively_0.png
tag: Tech
originalTitle: "How to Use Next.js 14’s Server Components Effectively"
link: "https://medium.com/@sassenthusiast/my-epiphany-with-next-js-14s-server-components-08f69a2c1414"
---

![2024-07-07-HowtoUseNextjs14sServerComponentsEffectively_0](/assets/img/2024-07-07-HowtoUseNextjs14sServerComponentsEffectively_0.png)

친구야, 우리가 기존의 관행에 도전하고 깊이 있는 사색으로 이끄는 지혜의 조각을 발견하는 것은 흔하지 않다고 해요. 이런 경험을 한 적이 있습니까? Reddit에서 의아한 댓글이 Next.js 14 사용 방법에 대한 접근 방식을 심층적으로 재평가하도록 자극했던 일이 바로 그것이었습니다. 단순하지만 깊은 내용을 담은 그 댓글은 "Next.js 14에서는 useState를 그렇게 많이 사용할 필요가 없다"는 것을 제안했습니다. 처음에는 상태 관리에 대한 간단한 관찰로만 보였지만, 이는 제가 Next.js의 최신 기능의 심연에 대한 탐험을 이끌었습니다. 저는 이 프레임워크의 서버 측 렌더링(SSR) 및 그 이상의 진화 가능성을 완전히 무시해왔음을 깨달았습니다. 이 깨달음은 상태를 덜 자주 사용하는 것뿐만이 아니라, 데이터 가져오기, 렌더링, 그리고 전반적인 응용 프로그램 아키텍처를 최적화하는 혁신적인 변화를 시사하는 것이었습니다. 이 글을 통해 이 발견의 층을 해체하고, 새로운 기능이 개발 경험을 재정의하는 방식과 이를 수용함으로써 웹 개발 여정에서 중요한 발전을 이룰 수 있는 이유를 명확하게 밝히려 합니다.

그 댓글 덕분에, Next.js와 그의 새로운 기능, 그리고 내 사용 방법에 대해 좀 더 사색하고 연구하게 되었습니다. 그 댓글이 옳았습니다. Next.js 응용 프로그램의 서버 측 렌더링(SSR)에서는 데이터 가져오기 및 렌더링 방식이 클라이언트 측 렌더링(CSR) 또는 정적 사이트 생성(SSG)과 비교하여 관리되기 때문에, 상태를 그렇게 자주 사용할 필요가 없었습니다.

구현에 들어가기 전에, 기본 개념을 이해해야 했습니다:

<div class="content-ad"></div>

## 클라이언트 측 렌더링 (CSR)

CSR에서는 대부분의 렌더링이 브라우저에서 발생합니다. 사용자가 애플리케이션에 액세스하면 먼저 최소한의 HTML 페이지와 React 애플리케이션을 실행하는 데 필요한 JavaScript를 다운로드합니다. 그런 다음 페이지가 브라우저에서 렌더링되며 초기 로드 후 서버나 API에서 필요한 데이터를 가져옵니다. 이 접근 방식은 클라이언트 측에서 상태를 관리하여 렌더링 프로세스를 제어하고 비동기로 가져온 데이터를 처리하는 데 많이 의존합니다.

## 서버 측 렌더링 (SSR)

반면에 SSR은 요청 시 서버에서 각 페이지를 사전 렌더링합니다. 사용자가 페이지를 요청하면 서버가 필요한 모든 데이터를 가져와 이 데이터로 페이지의 HTML을 렌더링한 다음 완전히 렌더링된 페이지를 클라이언트에 보냅니다. 데이터 가져오기와 초기 렌더링이 서버에서 수행되므로 데이터 가져오기와 렌더링을 처리하는 클라이언트 측 상태 관리에 대한 즉각적인 필요성이 줄어듭니다. 페이지가 클라이언트에 표시할 준비가 된 상태로 도착합니다.

<div class="content-ad"></div>

## 상태 관리에 미치는 영향

- 초기 로드: SSR을 사용하면 초기 페이지 로드 시 클라이언트 측에서 데이터를 가져오지 않아도 데이터를 표시할 수 있어 처음 상태 설정과 클라이언트 측 데이터 가져오기 논리가 줄어듭니다.
- 상호 작용: 초기로드 이후 페이지를 상호작용적으로 만들거나 사용자 작업에 기반해 내용을 업데이트하려면 (예: 폼 제출 또는 데이터 필터링) 클라이언트 측에서 상태를 관리해야 할 수 있습니다. 그러나 초기 상태가 서버로부터 더 완전한 것이므로 상태 관리의 양과 복잡성을 줄일 수 있습니다.
- 클라이언트 측 복잡성 감소: 서버에서 데이터 가져오기와 렌더링을 처리함으로써 클라이언트 측 로직을 간소화할 수 있습니다. 복잡한 상태 관리 솔루션에 덜 의존하거나 전체 데이터 처리의 대부분이 서버 측에서 수행되므로 더 간단한 상태 관리 패턴을 사용할 수 있습니다.

SSR을 사용할 때 상태를 덜 사용한다는 주장은 초기 데이터 가져오기 및 렌더링을 위한 복잡한 클라이언트 측 상태 관리에 더 적은 필요성을 의미합니다. 그러나 초기 페이지 로드 이후 사용자 작업에 따른 상호작용 및 업데이트를 관리하는 데 상태는 여전히 중요합니다.

# Next.js V13 이해하기

<div class="content-ad"></div>

Next.js V13에서는 getStaticProps와 getServerSideProps라는 두 가지 데이터 가져오기 방법이 있습니다. 이 두 메서드는 각각 Static Site Generation (SSG) 및 Server-Side Rendering (SSR)과 같은 다른 렌더링 전략에 사용됩니다. 이 두 메서드의 차이를 이해하는 것이 V13를 활용해 최적화된 웹 애플리케이션을 구축하는 핵심입니다. 아래에서 각각에 대한 설명을 제시합니다:

## getStaticProps

- 사용 사례: getStaticProps는 Static Site Generation (SSG)에 사용됩니다. 빌드 시간에 데이터를 가져오며, 애플리케이션을 내보내거나 사이트를 빌드할 때 데이터가 가져옵니다.
- 동작: 페이지는 빌드 시간에 사전 렌더링됩니다. getStaticProps가 가져온 데이터로 각 페이지의 HTML이 생성됩니다. 이로 인해 성능상 이점을 위해 CDN에 캐싱할 수 있는 정적 페이지가 생성됩니다.
- 사용 시기: 빌드 후 자주 변경되지 않는 데이터로 사전 렌더링할 수 있는 페이지에 적합합니다. 예를 들어 블로그 게시물, 문서, 제품 목록 등이 해당됩니다.
- 제한 사항: 데이터는 빌드 시간에 가져오므로, 빌드 이후 데이터 변경 사항은 다음 빌드 시까지 반영되지 않습니다. 실시간 데이터에는 적합하지 않습니다.

## getServerSideProps

<div class="content-ad"></div>

- 사용 사례: getServerSideProps는 서버 측 렌더링 (SSR)과 함께 사용됩니다. 각 요청마다 데이터를 가져옵니다.
- 동작: 페이지는 요청 시 서버에서 렌더링되며, 각 페이지 로드마다 새로운 데이터가 가져와집니다. 서버는 데이터와 함께 페이지를 렌더링하고, 렌더링된 페이지를 클라이언트로 보냅니다.
- 언제 사용해야 하는가: 실시간 데이터, 사용자별 데이터가 필요한 페이지 또는 최신 데이터가 중요한 콘텐츠가 자주 변경되는 페이지에 이상적입니다.
- 제한 사항: 페이지가 요청 시 렌더링되기 때문에 CDN에서 제공되는 정적 페이지보다 페이지로드 속도가 느릴 수 있습니다. 그러나 이는 동적이고 실시간 콘텐츠를 가능하게 합니다.

## 주요 차이점 요약

- 데이터 가져오기 시간: getStaticProps는 빌드 시간에 데이터를 가져오지만, getServerSideProps는 요청 시간에 데이터를 가져옵니다.
- 페이지 재생성: getStaticProps에서 페이지는 사전 생성되어 CDN에서 즉시 제공될 수 있습니다. 그러나 getServerSideProps는 페이지를 필요에 따라 생성하므로 약간의 지연이 발생할 수 있지만 최신 내용을 보장합니다.
- 사용 사례: 자주 변경되지 않는 정적 콘텐츠에는 getStaticProps, 정기적으로 변경되거나 사용자 세션에 의존하는 동적 콘텐츠에는 getServerSideProps를 사용합니다.

두 방법 모두 Next.js에서 개발자가 애플리케이션의 필요에 따라 가장 적합한 렌더링 전략을 선택할 수 있도록 하는 강력한 도구이며, 성능을 최적화하고 동적이고 사용자 중심적인 경험을 제공합니다.

<div class="content-ad"></div>

# Next.js V14 이해하기

getStaticProps와 getServerSideProps와 같은 기존의 데이터 가져오기 방법이 새로운 Server Components 접근 방식으로 재구성되었습니다. 이 변경 사항은 Next.js를 사용하여 웹 애플리케이션을 구축하는 보다 통합되고 유연한 방법으로 진화하려는 것을 나타냅니다. 정적 생성과 서버사이드 렌더링의 장점을 결합하는 것을 목표로 합니다.

## Server Components

Next.js의 Server Components는 서버에서만 렌더링되는 컴포넌트를 작성할 수 있게 합니다. 이 메커니즘은 컴포넌트 코드를 클라이언트로 보내지 않기 때문에 번들 크기가 줄어들고 로드 시간이 빨라질 수 있습니다. Server Components는 데이터를 컴포넌트 자체에서 직접 가져올 수 있으며, getStaticProps와 getServerSideProps로 인해 존재했던 데이터 가져오기와 UI 렌더링 간의 엄격한 분리를 제거합니다.

<div class="content-ad"></div>

## 서버 컴포넌트로 데이터 가져오기

- 직접 가져오기: 서버 컴포넌트 내에서 데이터 가져오기 라이브러리나 기본 fetch를 직접 사용할 수 있습니다. 이 데이터는 서버에서 렌더링 시에 가져와서 클라이언트 컴포넌트로 props로 전달됩니다.
- 스트리밍: 서버 컴포넌트는 스트리밍 기능을 지원하여 생성 중인 콘텐츠가 클라이언트로 스트리밍될 수 있어, 최종 사용자에게 더 나은 성능을 제공할 수 있습니다.

## 장점

- 유연성: 서버 컴포넌트는 서버 측 및 클라이언트 측 컴포넌트를 원활하게 혼합할 수 있어 더 유연한 아키텍처를 제공합니다. 데이터 요구 사항에 따라 어떤 컴포넌트를 서버 측으로 만들어야 할지와 어떤 것을 클라이언트 측으로 만들어야 하는지를 결정하여 애플리케이션을 최적화할 수 있습니다.
- 성능: 서버 컴포넌트는 브라우저로 전송되는 JavaScript 번들에 추가되지 않으므로 클라이언트가 다운로드, 구문 분석 및 실행해야 하는 코드 양을 크게 줄일 수 있습니다. 이는 더 빠른 로드 시간과 향상된 성능을 이끌어냅니다.
- 점진적 채택: 서버 컴포넌트는 점진적으로 채택할 수 있어 한꺼번에 모든 것을 리팩토링할 필요 없이 애플리케이션의 일부분에서 사용을 시작할 수 있습니다.

<div class="content-ad"></div>

## getStaticProps 및 getServerSideProps에서의 이주

정적 생성을 위해 getStaticProps, 또는 서버 측 렌더링을 위해 getServerSideProps을 사용하던 페이지 및 컴포넌트의 경우, 이제 데이터 가져오기 로직을 직접 Server Components 내에 통합할 수 있습니다. 이 접근 방식은 데이터 가져오기 프로세스를 단순화하여, 컴포넌트 라이프사이클과 통합하기가 더 직관적해지며 응용 프로그램 아키텍처를 효율적으로 설계할 수 있습니다.

Next.js 애플리케이션을 구축하는 강력한 새로운 모델을 제공하는 Server Components이지만, 언제, 어떻게 효과적으로 사용해야 하는지를 이해하는 것이 중요합니다. 그렇지 않으면 사용자 경험을 희생하지 않고 혜택을 누릴 수 있는 열쇠가 될 것입니다.

# 예제를 하나 들어줄 수 있나요?

<div class="content-ad"></div>

간단한 예제로 getServerSideProps를 사용하여 새로운 Server Components 모델로 마이그레이션하는 방법을 살펴보겠습니다.

## getServerSideProps를 사용한 원본 코드

API에서 요청 시 가져온 포스트 목록을 보여주는 페이지가 있다고 상상해보세요.

```js
import axios from 'axios';

export async function getServerSideProps() {
  const response = await axios.get('https://api.example.com/posts');
  const posts = response.data;

  return {
    props: {
      posts,
    },
  };
}

export default function Posts({ posts }) {
  return (
    - 리스트
      - 포스트.map((post) => (
        - key={post.id} : post.title
      ))
  );
}
```

<div class="content-ad"></div>

# 서버 컴포넌트로 코드 이전

서버 컴포넌트를 사용하면 컴포넌트 내에서 데이터를 직접 가져올 수 있습니다. 서버 컴포넌트 모델에서 서버 컴포넌트를 포함하는 파일은 클라이언트 측 컴포넌트와 구분하기 위해 .server.js 확장자를 가져야 합니다. 이전 예제를 이전하는 방법은 다음과 같습니다:

pages/posts.server.js

```js
import axios from "axios";

export default async function Posts() {
  const response = await axios.get("https://api.example.com/posts");
  const posts = response.data;

  return (
    <ul>
      {posts.map((post) => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  );
}
```

<div class="content-ad"></div>

## 주요 변경 사항

- 파일 확장자: 파일은 이제 posts.server.js로 이름이 변경되었습니다. 이는 서버 구성요소임을 나타냅니다.
- 직접 데이터 가져오기: 데이터 가져오기는 await를 사용하여 구성요소 내에서 직접 처리됩니다. 이는 서버 구성요소가 서버에서 실행되기 때문에 데이터 가져오기와 같은 서버 사이드 로직을 직접 통합할 수 있기 때문에 가능합니다.
- getServerSideProps 없음: getServerSideProps의 사용이 제거되었습니다. 대신, 구성요소 자체가 데이터를 가져와 해당 데이터에 기반하여 렌더링하는 책임을 처리합니다.

## 주의해야 할 점

- 클라이언트 구성요소: 페이지 내에서 상호 작용이 필요한 경우(예: 이벤트 핸들러), 해당 부분을 클라이언트 구성요소(.js 또는 .jsx 확장자를 갖는 파일)로 분리하여 Server Components에 가져옵니다. Server Components는 읽기 전용이며 상호 작용을 포함하지 않습니다.
- 환경 변수: Server Components에서 환경 변수를 직접 사용할 때 주의하십시오. 클라이언트에 민감한 정보가 노출되지 않도록 서버에서 실행되는 것을 확인하십시오.
- API Routes: 여전히 API routes를 사용하여 클라이언트 측 데이터 가져오기, 폼 제출 등을 할 수 있습니다. Server Components는 API routes를 대체하지 않지만 페이지 및 구성요소를 더 효율적으로 렌더링하고 서버 사이드에 직접 데이터 종속성을 가진 방식을 제공합니다.

<div class="content-ad"></div>

본 예시는 기본적인 마이그레이션 전략을 보여줍니다. 애플리케이션의 복잡성에 따라, 오류 처리, 로딩 상태, 상호작용 요소를 위해 클라이언트 측 컴포넌트 통합과 같은 추가 요소를 고려해야 할 수도 있습니다.

## 상호작용에 대한 고려사항은?

상호작용(게시물 업데이트 및 삭제)을 포함한 예시를 확장하려면, 데이터 검색을 위한 서버 컴포넌트와 버튼과 같은 상호 작용 요소를 처리하기 위한 클라이언트 컴포넌트 조합이 필요합니다. 먼저, Next.js 13 버전의 getServerSideProps를 사용하여 이를 구현하는 방법을 개요로 살펴보고, 이후 버전 14에서 서버 컴포넌트를 사용하도록 마이그레이션하는 방법을 살펴봅시다.

pages/posts.js (V13)

<div class="content-ad"></div>

```js
// pages/posts.js

import axios from "axios";

export async function getServerSideProps() {
  const res = await axios.get("https://api.example.com/posts");
  return {
    props: {
      posts: res.data,
    },
  };
}

export default function Posts({ posts }) {
  async function updatePost(id) {
    // Implementation for updating a post
    // E.g., sending a PATCH request to an API
  }

  async function deletePost(id) {
    // Implementation for deleting a post
    // E.g., sending a DELETE request to an API
  }

  return (
    <ul>
      {posts.map((post) => (
        <li key={post.id}>
          {post.title}
          <button onClick={() => updatePost(post.id)}>Update</button>
          <button onClick={() => deletePost(post.id)}>Delete</button>
        </li>
      ))}
    </ul>
  );
}
```

## 상호 작용을 사용한 서버 컴포넌트로 마이그레이션

서버 컴포넌트를 사용하여 처리를 분리하면 버전 14에는 Server Components를 사용하게 됩니다. 게시물 목록을 가져오고 표시하는 데 서버 컴포넌트를 사용하고 업데이트 및 삭제 버튼과 같은 상호 작용 요소를위한 클라이언트 컴포넌트를 사용합니다.

게시물 가져오기 및 표시를 위한 서버 컴포넌트

<div class="content-ad"></div>

```js
components / PostsList.server.js;

import axios from "axios";
import PostActions from "./PostActions.client";

export default async function PostsList() {
  const res = await axios.get("https://api.example.com/posts");
  const posts = res.data;

  return (
    <ul>
      {posts.map((post) => (
        <li key={post.id}>
          {post.title}
          <PostActions postId={post.id} />
        </li>
      ))}
    </ul>
  );
}
```

## 클라이언트 컴포넌트 업데이트 및 삭제 작업

```js
// components/PostActions.client.js

export default function PostActions({ postId }) {
  async function updatePost() {
    // 게시물 업데이트를 위한 구현
    // 예: API로 PATCH 요청 보내기
  }

  async function deletePost() {
    // 게시물 삭제를 위한 구현
    // 예: API로 DELETE 요청 보내기
  }

  return (
    <>
      <button onClick={updatePost}>업데이트</button>
      <button onClick={deletePost}>삭제</button>
    </>
  );
}
```

## 주요 변경 사항 및 고려 사항

<div class="content-ad"></div>

- 서버 및 클라이언트 컴포넌트 분리: PostsList.server.js 서버 컴포넌트는 게시물 목록을 가져와 표시하는 역할을 합니다. PostActions.client.js 클라이언트 컴포넌트는 상호 작용 요소(업데이트 및 삭제 버튼)를 처리하며 클라이언트 측에서 실행되도록 정규 .client.js 파일이어야 합니다.

- 서버 컴포넌트에서 직접 데이터 가져오기: 이전 예제와 마찬가지로 데이터 가져오기 작업은 서버 컴포넌트 내에서 직접 수행됩니다.

- 클라이언트 컴포넌트에서 상호 작용 관리: 클라이언트 컴포넌트가 업데이트 및 삭제 등 모든 상호 작용을 처리합니다. 이는 서버 컴포넌트가 상호 작용적이지 않으며 서버에서 사전 렌더링할 수 있는 응용 프로그램 부분을 위해 사용되기 때문입니다.

이러한 설정을 통해 응용 프로그램이 서버 컴포넌트의 효율성과 성능 이점을 활용할 수 있으면서 필요한 경우 동적이고 상호 작용적인 사용자 경험을 제공할 수 있습니다.

## 클라이언트 컴포넌트에서 상태 업데이트

서버 컴포넌트와 클라이언트 컴포넌트를 함께 사용하는 Next.js 애플리케이션에서 항목이 삭제(또는 업데이트)되었을 때, 클라이언트 측 코드는 서버 측 코드에 데이터를 새로 고침하고 UI를 업데이트하도록 알려야 합니다. 데이터 가져오기 및 초기 렌더링을 처리하는 서버 컴포넌트는 클라이언트 측 상호 작용에 직접적으로 반응하여 다시 실행되지 않으므로 삭제와 같은 작업 후 유저에게 표시되는 데이터를 업데이트하는 전략을 활용해야 합니다.

<div class="content-ad"></div>

게시물을 삭제한 후, 클라이언트 측 데이터를 다시 불러와 게시물 목록을 새로고침할 수 있습니다. 이 방법은 클라이언트 측 상태 관리를 사용하는데, React hooks인 useState와 useEffect를 이용할 수 있습니다. Client Component에서 이 방법을 사용할 수 있습니다.

## 예시:

서버 컴포넌트에서 데이터를 초기에 가져오지만 클라이언트 측에서 데이터를 가져오는 기능도 있는 PostsList.client.js 컴포넌트가 있다고 가정해봅시다:

```js
import React, { useEffect, useState } from "react";
import axios from "axios";
import PostActions from "./PostActions.client";

// 클라이언트 사용
function PostsList() {
  const [posts, setPosts] = useState([]);

  useEffect(() => {
    async function fetchPosts() {
      const response = await axios.get("https://api.example.com/posts");
      setPosts(response.data);
    }

    fetchPosts();
  }, []);

  async function refreshPosts() {
    const response = await axios.get("https://api.example.com/posts");
    setPosts(response.data);
  }

  return (
    <ul>
      {posts.map((post) => (
        <li key={post.id}>
          {post.title}
          <PostActions postId={post.id} refreshPosts={refreshPosts} />
        </li>
      ))}
    </ul>
  );
}

export default PostsList;
```

<div class="content-ad"></div>

PostActions.client.js 파일에서는 게시물을 성공적으로 삭제한 후 refreshPosts를 호출해야 합니다:

```js
async function deletePost(postId) {
  // 게시물 삭제 요청
  await axios.delete(`https://api.example.com/posts/${postId}`);
  // 게시물 목록 새로고침
  props.refreshPosts();
}
```

# 결론

Next.js 버전 13에서 버전 14로 전환하면 데이터 가져오기, 렌더링 및 구성 요소 구조화 방식에 영향을 주는 중요한 아키텍처 변경과 개선 사항이 도입됩니다. 이러한 변화는 성능, 사용자 경험 및 개발자 효율성 향상을 목표로 합니다. getServerSideProps에서 Server Components 및 Client Components로 진화하는 과정 및 Next.js 버전 14에서 새로운 모델 채택의 장점에 대한 종합적인 요약을 제공합니다.

<div class="content-ad"></div>

## Next.js 버전 13 및 이전

- getServerSideProps 및 getStaticProps: 이러한 데이터 가져오기 메서드는 개발자가 Next.js에서 서버 사이드 렌더링 (SSR) 및 정적 사이트 생성 (SSG)을 구현하는 데 필수적이었습니다. getServerSideProps는 요청 시간에 데이터를 가져오므로 자주 변경되는 동적 콘텐츠에 적합하며, getStaticProps는 빌드 시간에 데이터를 가져와 정적 콘텐츠를 위한 것입니다.
- 렌더링 전략: 서버에서 렌더링되거나 빌드 시간에 정적으로 생성된 컴포넌트는 HTML로 클라이언트로 전송됩니다. 그런 다음 클라이언트는 상호 작용을 위해 이러한 컴포넌트를 재수현화하며, 클라이언트 측 JavaScript가 이후 네비게이션 및 동적 업데이트를 처리합니다.
- 도전과제: 이 모델은 효과적이지만 종종 클라이언트로 전송되는 JavaScript 번들의 크기가 커지고 성능에 영향을 미칠 수 있었습니다. 또한 데이터 가져오기와 컴포넌트 로직 사이에 명확한 분리가 유지되어 때로는 개발 프로세스를 복잡하게 만들었습니다.

## Next.js 버전 14: 서버 컴포넌트 및 클라이언트 컴포넌트 소개

- 서버 컴포넌트: 이러한 컴포넌트는 서버에서 전용으로 실행되며 컴포넌트 내에서 데이터를 직접 가져올 수 있습니다. 코드를 클라이언트로 보내지 않아 브라우저에서 다운로드 및 구문 분석할 필요가 있는 JavaScript 번들의 크기를 크게 줄입니다. 서버 컴포넌트는 .server.js 확장자를 사용합니다.
- 클라이언트 컴포넌트: 클라이언트 측 상호 작용을 처리하도록 설계된 클라이언트 컴포넌트는 브라우저에서 실행되며 .client.js 확장자를 사용합니다. 사용자 입력, 클릭 및 기타 상호 작용과 같은 동적 기능을 위한 중요한 역할을 합니다.
- 스트리밍 SSR: Next.js 14는 스트리밍 기능을 활용하여 SSR을 개선하며 페이지 일부가 준비되는 대로 클라이언트로 전송되도록 합니다. 이 접근 방식은 페이지를 점진적으로 렌더링하여 첫 번째 바이트까지 걸리는 시간 및 전반적인 사용자 경험을 개선합니다.
- 마이그레이션 이점: 기존 getServerSideProps 접근 방식에서 서버 컴포넌트 및 클라이언트 컴포넌트를 사용하는 것은 여러 가지 장점을 제공합니다:
  - JavaScript 번들 크기 축소: 서버 컴포넌트 코드를 클라이언트로 보내지 않음으로써 초기로드 시간이 줄어들고 성능이 향상됩니다.
  - 더 많은 유연성: 서버 및 클라이언트 컴포넌트를 혼합함으로써 개발자는 최적화된 데이터 가져오기 전략 및 렌더링 경로를 활성화하여 더 많은 제어와 유연성을 얻을 수 있습니다.
  - 향상된 사용자 경험: 스트리밍 SSR 및 선택적 수화 기능을 통해 사용자가 내용을 빠르게 볼 수 있도록 하여 복잡한 페이지에서도 사용자 경험을 향상시킵니다.
  - 단순화된 개발 과정: 서버 컴포넌트에서 데이터 직접 가져오기는 데이터 가져오기 프로세스를 단순화하여 코드베이스를 더 직관적이고 관리하기 쉽게 만들 수 있습니다.

<div class="content-ad"></div>

Next.js 14의 Server Components 및 Client Components를 채택하면 Next.js로 웹 애플리케이션을 구축하는 패러다임이 바뀝니다. 이 접근 방식은 보다 효율적이고 성능 최적화된 아키텍처를 제공할 뿐만 아니라 사용자 경험과 개발자 생산성을 강조하는 현대 웹 개발 관행과 일치합니다. 이러한 전환은 컴포넌트 구조 및 데이터 가져오기 방식을 재구성하는 것을 포함하지만 번들 크기 감소, 빠른 로드 시간, 향상된 확장성의 이점은 Next.js 애플리케이션의 효율적인 업그레이드를 만드는 것으로 만들어줍니다.

현재 getServerSideProps를 많이 사용하는 팀 및 프로젝트를 위해 Server Components와 Client Components로의 단계적 이관을 계획하고 잠재적인 영향을 평가하는 것은 이러한 진전을 활용하고 애플리케이션이 빠르고 현대적이며 유지보수가 용이하도록 보장하는 데 도움이 될 것입니다.

# 여러분의 기술 여정을 지원하세요:

여러분의 기술 프로젝트와 이해를 높이기 위해 설계된 다양한 지식을 탐색해 보세요. 애플리케이션을 안전하게 보호하고 서버리스 아키텍처를 숙달하는 등 여러분의 야망과 공명하는 기사들을 발견해 보세요.

<div class="content-ad"></div>

## 새로운 프로젝트 또는 컨설팅

새로운 프로젝트 협업 또는 맞춤형 컨설팅 서비스에 대해 직접 문의하여 여러분의 아이디어를 현실로 변화시켜 보세요. 프로젝트를 다음 수준으로 이끌 준비가 되셨나요?

- one@upskyrocket.com 으로 이메일 보내기
- 커스텀 솔루션을 위해 My Partner In Tech 방문하기

## 프론트 엔드

<div class="content-ad"></div>

- Next.js 14의 Server Components를 효과적으로 활용하는 방법
- React-Hook-Form, Yup, 및 MUI로 웹 UX 향상시키는 연락 양식
- Next.js와 AWS Amplify로 단 24시간 안에 기관 웹사이트 설정하기
- React 상태 관리를 위한 게임 체인저인 Zustand 전환하기
- MUI 5로 Next.js의 측면 네비게이션 UX 향상시키기: 실용적인 접근
- ChatGPT 도움으로 Next.js의 반복된 API 요청 해결하기
- 현대적인 배경으로 남다른 인상 주기
- Tailwind CSS 애니메이션을 MUI V5에 통합하여 화려한 로그인 경험 제공하기

## 라우트 보호

- React, Next.js, 및 AWS Amplify를 사용하여 보호된 라우트 생성하는 방법
- HOC를 사용하여 React Next.js의 관리자용 보호 라우트 만들기
- AWS Cognito 그룹을 사용한 고급 사용자 관리로 Next.js 앱 보호하기

## Serverless Series 마스터하기

<div class="content-ad"></div>

- Mastering Serverless (Part I): Document Client를 사용하여 DynamoDB 상호 작용 향상하기
- Mastering Serverless (Part II): 더 부드러운 경험을 위한 AWS DynamoDB 배치 쓰기 오류 극복
- Mastering Serverless (Part III): 의존성 주입을 통해 AWS Lambda 및 DynamoDB 상호 작용 향상하기
- Mastering Serverless IV: Jest로 DynamoDB 의존성 주입 테스트하기
- Mastering Serverless (Part V): AWS Lambda를 위한 고급 로깅 기술

## 고급 Serverless 기술

- Advanced Serverless Techinques I: 반복하지 말 것
- Advanced Serverless Techniques II: DAL로 데이터 접근 최적화
- Advanced Serverless Techniques III: 사용자 지정 DynamoDB 미들웨어를 통해 Lambda 함수 간결화
- Advanced Serverless Techniques IV: 서버리스 데이터 분석을 위한 AWS Athena
- Advanced Serverless Techniques V: DynamoDB Streams vs. SQS/SNS to Lambda
