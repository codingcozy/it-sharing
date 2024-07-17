---
title: "Axios Interceptor란 ReactJS에서 리프레시 토큰 다루는 방법"
description: ""
coverImage: "/assets/img/2024-07-07-WhatisAxiosInterceptorHowtohandlerefreshtokensinReactJS_0.png"
date: 2024-07-07 21:04
ogImage:
  url: /assets/img/2024-07-07-WhatisAxiosInterceptorHowtohandlerefreshtokensinReactJS_0.png
tag: Tech
originalTitle: "What is Axios Interceptor? How to handle refresh tokens in ReactJS"
link: "https://medium.com/@sanchit0496/what-is-axios-interceptor-how-to-handle-refresh-tokens-in-frontend-7e8bbdbb8ac9"
---

인기 있는 JavaScript 라이브러리인 Axios는 서버로의 HTTP 요청을 보내는 과정을 간단하게 만들어줍니다. 그러나 Axios를 정말 특별하게 만드는 것은 인터셉터 기능입니다. 이 도구를 사용하면 개발자들이 then() 또는 catch()에 의해 처리되기 전에 전역적으로 HTTP 요청과 응답을 가로채고 조작할 수 있습니다.

Axios 인터셉터의 개념, 그 이점, 그리고 토큰 갱신 메커니즘 구현과 같이 효과적으로 사용하는 방법에 대해 알아봅시다.

![image](/assets/img/2024-07-07-WhatisAxiosInterceptorHowtohandlerefreshtokensinReactJS_0.png)

# Axios 인터셉터란 무엇인가요?

<div class="content-ad"></div>

Axios interceptors는 Axios가 모든 요청 또는 응답에 대해 호출하는 함수들입니다. 이러한 인터셉터는 기본적으로 요청이 전송되기 전이나 프라미스가 클라이언트 코드에 반환되기 전에 코드를 실행하거나 요청/응답을 수정할 수 있는 기능을 제공합니다. 이 기능은 모든 API 호출에 대해 균일하게 로깅, 인증 및 오류 처리를 수행하는 데 특히 유용합니다.

![Axios Interceptor](/assets/img/2024-07-07-WhatisAxiosInterceptorHowtohandlerefreshtokensinReactJS_1.png)

# Axios Interceptors 사용의 장점

- 중앙 집중식 요청/응답 처리: 모든 HTTP 호출에 대해 코드를 반복하는 대신 (헤더 설정 또는 응답 로깅과 같은 작업), 한 곳에서 처리할 수 있습니다. 이것은 코드를 DRY (Don't Repeat Yourself)하게 유지하고 유지 보수를 쉽게 할 수 있습니다.
- 자동 토큰 갱신: 인터셉터는 인증 토큰이 만료되었을 때 감지할 수 있습니다. 실패한 요청이 응용 프로그램에서 오류를 발생시키기 전에, 인터셉터가 토큰을 자동으로 갱신하고 원래 요청을 다시 시도할 수 있습니다. 그러나 이를 위해 백엔드에서도 쉽게 토큰을 갱신할 수 있는 API를 만들어야 합니다.
- 오류 처리: Axios 인터셉터를 사용하여 전역적으로 오류를 처리할 수 있습니다. 예를 들어, 401 Unauthorized 응답을 가로채 사용자를 로그인 페이지로 리디렉션하거나 리프레시 토큰을 호출하여 사용자가 새 토큰을 사용하여 응용 프로그램을 원활하게 탐색할 수 있도록 할 수 있습니다.
- 데이터 향상: 요청과 응답이 최종 목적지에 도달하기 전에 수정할 수 있습니다. 예를 들어, 응답에서 날짜를 자동으로 형식화하거나 요청에 추가 헤더를 추가할 수 있습니다.

<div class="content-ad"></div>

# 코드

토큰을 새로 고침하는 인터셉터를 개발해 봅시다. 실제로 새로 고침 토큰 로직의 구현은 백엔드 API에 따라 다를 수 있지만, 개념은 일관성을 유지합니다: 백엔드로부터 401 Unauthorized 오류를 받으면 토큰을 새로고침합니다.

## 인터셉터 생성

```js
import axios from "axios";

const axiosInstance = axios.create({
  baseURL: "https://jsonplaceholder.typicode.com",
  // 다른 구성
});

axiosInstance.interceptors.response.use(
  (response) => response,
  (error) => {
    if (error.response && error.response.status === 401) {
      console.log("여기서 새로 고침 토큰 API를 호출합니다.");
      // 401 오류 처리, 예를 들어 로그인 화면으로 리디렉션하거나 토큰 새로고침
    }
    return Promise.reject(error);
  }
);

export default axiosInstance;
```

<div class="content-ad"></div>

위 예시에서 axios의 인스턴스를 axiosInstance.interceptors.response.use를 사용하여 생성했습니다. 이 인스턴스는 전역 수준에서 API 호출을 처리할 것입니다. 이제 'axios'에서 axios를 가져와 API를 호출하는 대신, 방금 생성한 인터셉터를 가져와 사용할 것입니다.

위에서 만든 인터셉터를 사용하여 API를 호출하는 방법의 예시는 다음과 같습니다.

```js
import axiosInstance from "./path-to-your-axios-instance";

async function fetchUserData() {
  try {
    const response = await axiosInstance.get("/users/1");
    console.log("사용자 데이터:", response.data);
  } catch (error) {
    console.error("오류 발생:", error.message);
    // 여기서 백엔드에서 오는 오류를 처리할 수 있습니다.
  }
}

// 사용자 데이터를 가져오는 함수 실행
useEffect(() => {
  fetchUserData();
}, []);
```

이제 라이브러리에서 직접 Axios를 가져오는 대신, 이전에 설정한 사용자 정의 인스턴스를 가져올 것입니다. 이 인스턴스에는 이미 baseURL이 포함되어 있으므로 전체 API URL을 매번 지정하는 대신 get 또는 post 요청에 특정 엔드포인트를 추가하기만 하면 됩니다. 이 접근 방식은 사전 정의된 구성을 활용하여 코드를 간소화합니다.

<div class="content-ad"></div>

## 읽어 주셔서 감사합니다. 만약 이 기사가 도움이 되었다면, 더 많은 유익한 콘텐츠를 위해 저를 Medium과 LinkedIn에서 팔로우해주세요.
