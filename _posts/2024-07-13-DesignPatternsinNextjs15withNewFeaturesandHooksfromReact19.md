---
title: "Nextjs 15에서 React 19의 새로운 기능과 훅을 사용한 디자인 패턴"
description: ""
coverImage: "/assets/img/2024-07-13-DesignPatternsinNextjs15withNewFeaturesandHooksfromReact19_0.png"
date: 2024-07-13 18:24
ogImage:
  url: /assets/img/2024-07-13-DesignPatternsinNextjs15withNewFeaturesandHooksfromReact19_0.png
tag: Tech
originalTitle: "Design Patterns in Next.js 15 with New Features and Hooks from React 19"
link: "https://medium.com/@guhaprasaanth/design-patterns-in-next-js-15-with-new-features-and-hooks-from-react-19-be19f0bebbd0"
---

![이미지](/assets/img/2024-07-13-DesignPatternsinNextjs15withNewFeaturesandHooksfromReact19_0.png)

Next.js 15과 React 19의 출시로 개발자들은 개발 경험을 크게 향상시키는 강력한 도구와 기능 조합을 만날 수 있습니다. 이러한 새로운 기능은 고급 디자인 패턴의 채택을 가능케하며, 응용 프로그램을 더 효율적이고 유지보수성이 뛰어나며 성능이 우수하도록 만들어줍니다. 이 기사에서는 Next.js 15와 React 19의 새로운 기능과 훅을 활용하는 디자인 패턴을 탐색합니다.

# Next.js 15의 새로운 기능

Next.js 15에서는 다음과 같은 여러 가지 개선 사항이 소개되었습니다:

<div class="content-ad"></div>

- 서버 동작
- 향상된 미들웨어
- 향상된 정적 사이트 생성 (SSG) 및 점진적 정적 재생성 (ISR)
- 서버 사이드 서스펜스
- 성능 최적화 개선 사항

# 새로운 React 19 기능과 훅

React 19은 Next.js 15를 보완하는 여러 새로운 훅과 기능을 소개합니다:

- useOptimistic 훅
- useFormStatus 훅
- useTransition 훅
- useDeferredValue 훅
- 병행 렌더링 향상 기능

<div class="content-ad"></div>

# Next.js 15과 React 19를 활용한 디자인 패턴

## 1. useOptimistic을 활용한 낙관적 UI 업데이트

낙관적 UI 업데이트는 서버가 변경을 확인하기 전에 UI에 변경 사항을 반영하여 사용자에게 즉각적인 피드백을 제공합니다. 이 패턴은 응용 프로그램이 더 반응적으로 느껴지도록하여 사용자 경험을 향상시킵니다.

예시:

<div class="content-ad"></div>

```js
import { useOptimistic } from "react";

const LikeButton = () => {
  const [optimisticLikes, setOptimisticLikes] = useOptimistic((likes) => likes + 1, 0);

  const handleLike = async () => {
    setOptimisticLikes();
    await fetch("/api/like", { method: "POST" });
  };

  return <button onClick={handleLike}>Likes: {optimisticLikes}</button>;
};

export default LikeButton;
```

## 2. useFormStatus로 양식 제출 관리

양식 제출 상태를 관리하는 것, 즉 로딩, 성공 및 오류 상태를 포함하는 것은 useFormStatus 훅을 사용하여 간단하게 처리할 수 있습니다. 이 패턴을 사용하면 양식 처리를 개선하고 사용자에게 명확한 피드백을 제공할 수 있습니다.

예시:

<div class="content-ad"></div>

```js
import { useFormStatus } from "react";

const ContactForm = () => {
  const { status, setLoading, setSuccess, setError } = useFormStatus();

  const handleSubmit = async (event) => {
    event.preventDefault();
    setLoading();

    try {
      await fetch("/api/contact", { method: "POST", body: new FormData(event.target) });
      setSuccess();
    } catch {
      setError();
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <input type="text" name="name" required />
      <input type="email" name="email" required />
      <button type="submit" disabled={status === "loading"}>
        Submit
      </button>
      {status === "loading" && <p>Loading...</p>}
      {status === "success" && <p>Form submitted successfully!</p>}
      {status === "error" && <p>There was an error submitting the form.</p>}
    </form>
  );
};

export default ContactForm;
```

## 3. useTransition을 사용한 지연 상태 업데이트

useTransition 훅은 지연 상태 업데이트를 허용하여, 즉각적이지 않은 업데이트를 차단하지 않고 UI를 더 반응적으로 만듭니다.

예시:

<div class="content-ad"></div>

```js
import { useState, useTransition } from "react";

const SlowComponent = ({ data }) => {
  return <div>{data}</div>;
};

const App = () => {
  const [isPending, startTransition] = useTransition();
  const [data, setData] = useState("Initial Data");

  const handleClick = () => {
    startTransition(() => {
      setData("Updated Data");
    });
  };

  return (
    <div>
      <button onClick={handleClick}>Update Data</button>
      {isPending ? "Loading..." : <SlowComponent data={data} />}
    </div>
  );
};

export default App;
```

## 4. useDeferredValue를 사용한 지연 값 처리

useDeferredValue 훅은 비긴급한 값의 업데이트를 지연시키는데 유용하며, 다른 업데이트를 우선할 수 있도록하고 주 스레드 차단을 피하는 데 도움이 됩니다.

예시:

<div class="content-ad"></div>

```js
import { useState, useDeferredValue } from "react";

const App = () => {
  const [input, setInput] = useState("");
  const deferredInput = useDeferredValue(input);

  const handleChange = (event) => {
    setInput(event.target.value);
  };

  return (
    <div>
      <input type="text" value={input} onChange={handleChange} />
      <p>Deferred Value: {deferredInput}</p>
    </div>
  );
};

export default App;
```

## 5. 서버 액션

Next.js 15에서는 서버 액션을 소개하여 클라이언트에서 호출할 수 있는 서버 측 로직을 정의할 수 있습니다. 이 패턴은 데이터 가져오기와 서버 측 작업을 간소화합니다.

예시:

<div class="content-ad"></div>

```js
// pages/api/hello.js
export default function handler(req, res) {
  res.status(200).json({ name: 'John Doe' });
}

// pages/index.js
import { useState } from 'react';

const HomePage = () => {
  const [data, setData] = useState(null);

  const fetchData = async () => {
    const res = await fetch('/api/hello');
    const json = await res.json();
    setData(json);
  };

  return (
    <div>
      <h1>Hello, {data?.name || 'Guest'}!</h1>
      <button onClick={fetchData}>Fetch Data</button>
    </div>
  );
};

export default HomePage;
```

## 6. Middleware

Next.js 15 introduces middleware for server-side logic, enabling developers to run code before a request is completed. This is useful for tasks like authentication, logging, and redirects.

Example:

<div class="content-ad"></div>

```js
// middleware.js
import { NextResponse } from "next/server";

export function middleware(req) {
  const token = req.cookies["token"];

  if (!token) {
    return NextResponse.redirect("/login");
  }

  return NextResponse.next();
}
```

## 7. Server-Side Suspense

React 19은 Suspense를 통합하여 서버 측 렌더링(SSR)을 개선하여 렌더링 및 데이터 가져오기를 원활하게 수행할 수 있습니다. 이 패턴은 SSR을 간단하게 만들어주고 비핵심 업데이트를 지연시켜 성능을 향상시킵니다.

예시:

<div class="content-ad"></div>

```js
import React from "react";
import ReactDOMServer from "react-dom/server";

const fetchData = () => {
  return fetch("/api/data").then((res) => res.json());
};

const DataComponent = React.lazy(() =>
  fetchData().then((data) => {
    return () => <div>{data}</div>;
  })
);

const App = () => {
  return (
    <div>
      <h1>서버 측 렌더링 및 서스펜스로 기다림</h1>
      <React.Suspense fallback={<div>Loading...</div>}>
        <DataComponent />
      </React.Suspense>
    </div>
  );
};

// 서버 측 렌더링
const html = ReactDOMServer.renderToString(<App />);
console.log(html);
```

# 새로운 리액트 기능과 후크를 활용한 Atomic 상태 관리 컴바이너

Atomic 상태 관리와 최신 리액트 기능 및 후크를 결합하면 더 모듈화되고 효율적이며 유지보수가 용이한 애플리케이션을 만들 수 있습니다.

## Jotai 및 useFormStatus 예시:

<div class="content-ad"></div>

```js
import { atom, useAtom } from 'jotai';
import { useFormStatus } from 'react';

// 원자 정의
const formDataAtom = atom({ name: '', email: '' });

const ContactForm = () => {
  const [formData, setFormData] = useAtom(formDataAtom);
  const { status, setLoading, setSuccess, setError } = useFormStatus();

  const handleChange = (e) => {
    setFormData({ ...formData, [e.target.name]: e.target.value });
  };

  const handleSubmit = async (e) => {
    e.preventDefault();
    setLoading();

    try {
      await fetch('/api/contact', {
        method: 'POST',
        body: JSON.stringify(formData),
        headers: { 'Content-Type': 'application/json' },
      });
      setSuccess();
    } catch {
      setError();
    }
  };

  return (
    ![사진](https://image.unsplash.com/photo-1633137959486-bd1cf22657cd?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&ixid=Mnw0Njc4OXwwfDF8c2VhcmNofDF8fGNvbnRhY3R8ZW58MHx8fHx8fHx8fHx8fHx8fHx8fHx8fHx8fHx8fHx8fHx8fHx8fHx8fHx8fHx8fB8GI8uYtTM9idoRjMUPUtvLA&ixlib=rb-1.2.1&q=80&auto=format&fit=crop&w=500&h=500)
      <input type="text" name="name" value={formData.name} onChange={handleChange} required />
      <input type="email" name="email" value={formData.email} onChange={handleChange} required />
      <button type="submit" disabled={status === 'loading'}>Submit</button>
      {status === 'loading' && <p>Loading...</p>}
      {status === 'success' && <p>Form submitted successfully!</p>}
      {status === 'error' && <p>There was an error submitting the form.</p>}
    </form>
  );
};

export default ContactForm;
```

## Zustand과 useFormStatus를 사용한 예시:

```js
// lib/store.js
import create from "zustand";

const useStore = create((set) => ({
  formData: { name: "", email: "" },
  setFormData: (data) => set({ formData: data }),
}));

export default useStore;
```

```js
// components/ContactForm.js
import { useState } from "react";
import { useFormStatus } from "react";
import useStore from "../lib/store";

const ContactForm = () => {
  const { formData, setFormData } = useStore();
  const { status, setLoading, setSuccess, setError } = useFormStatus();
  const [inputValues, setInputValues] = useState(formData);

  const handleChange = (e) => {
    const { name, value } = e.target;
    setInputValues((prevValues) => ({
      ...prevValues,
      [name]: value,
    }));
  };

  const handleSubmit = async (e) => {
    e.preventDefault();
    setLoading();

    try {
      await fetch("/api/contact", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(inputValues),
      });
      setFormData(inputValues);
      setSuccess();
    } catch {
      setError();
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <input type="text" name="name" value={inputValues.name} onChange={handleChange} required />
      <input type="email" name="email" value={inputValues.email} onChange={handleChange} required />
      <button type="submit" disabled={status === "loading"}>
        Submit
      </button>
      {status === "loading" && <p>Loading...</p>}
      {status === "success" && <p>Form submitted successfully!</p>}
      {status === "error" && <p>There was an error submitting the form.</p>}
    </form>
  );
};

export default ContactForm;
```

<div class="content-ad"></div>

# 향상된 정적 사이트 생성 (SSG) 및 증분적 정적 재생성 (ISR)

Next.js 15에서는 정적 사이트 생성과 증분적 정적 재생성을 강화하여 개발자가 동적으로 업데이트할 수 있는 고성능이고 SEO 친화적인 정적 사이트를 생성할 수 있습니다.

SSG 예시:

```js
// pages/index.js
import { getStaticProps } from "next";

export const getStaticProps = async () => {
  const res = await fetch("https://api.example.com/data");
  const data = await res.json();

  return {
    props: {
      data,
    },
  };
};

const HomePage = ({ data }) => {
  return (
    <div>
      <h1>SSG에서의 데이터:</h1>
      <pre>{JSON.stringify(data, null, 2)}</pre>
    </div>
  );
};

export default HomePage;
```

<div class="content-ad"></div>

ISR을 사용한 예시:

```js
// pages/index.js
import { getStaticProps } from "next";

export const getStaticProps = async () => {
  const res = await fetch("https://api.example.com/data");
  const data = await res.json();

  return {
    props: {
      data,
    },
    revalidate: 10, // 10초마다 최대 유효화
  };
};

const HomePage = ({ data }) => {
  return (
    <div>
      <h1>ISR에서 가져온 데이터:</h1>
      <pre>{JSON.stringify(data, null, 2)}</pre>
    </div>
  );
};

export default HomePage;
```

# 결론

Next.js 15와 React 19는 개발 경험을 혁신적으로 향상시키고 새로운 효율적인 디자인 패턴을 제공하는 강력한 기능과 훅을 소개했습니다. Jotai, Recoil, Zustand와 같은 라이브러리를 사용한 원자적 상태 관리는 상태를 다루는 현대적인 방법을 제공하여 섬세한 제어와 향상된 성능을 제공합니다. useOptimistic, useFormStatus, useTransition, useDeferredValue와 같은 새로운 훅은 일반적인 작업을 간소화하고 사용자 경험을 향상시킵니다. 또한, Next.js 15의 새로운 패턴인 서버 액션, 미들웨어, 개선된 정적 사이트 생성 등은 개발 프로세스를 더욱 효율적으로 만듭니다.

<div class="content-ad"></div>

이러한 새로운 디자인 패턴을 채택하고 최신 기능을 활용함으로써, 개발자들은 계속 발전하는 웹 개발 분야에서 선도적인 위치를 유지하면서 더 모듈화되고 효율적이며 유지보수가 쉬운 Next.js 애플리케이션을 구축할 수 있습니다. 이러한 진전은 Next.js가 동적이고 고성능 사용자 인터페이스를 구축하는 다재다능하고 견고한 선택지로 남아있을 수 있도록 보장합니다.

# Stackademic 🎓

마지막까지 읽어주셔서 감사합니다. 떠나시기 전에:

- 저자를 추천하고 팔로우해주시면 감사하겠습니다! 👏
- X 팔로우하기 | LinkedIn | YouTube | Discord
- 다른 플랫폼에서도 방문해주세요: In Plain English | CoFeed | Differ
- 더 많은 컨텐츠는 Stackademic.com에서 확인할 수 있습니다.
