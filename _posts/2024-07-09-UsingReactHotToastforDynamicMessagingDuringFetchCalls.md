---
title: "Fetch 호출 중 동적 메시징을 위해 React Hot Toast 사용하는 방법"
description: ""
coverImage: "/it-shar.ing/assets/no-image.jpg"
date: 2024-07-09 12:59
ogImage:
  url: /it-shar.ing/assets/no-image.jpg
tag: Tech
originalTitle: "Using React Hot Toast for Dynamic Messaging During Fetch Calls"
link: "https://medium.com/@gconnorpage/using-react-hot-toast-for-dynamic-messaging-during-fetch-calls-3b152e4a9f79"
---

웹 사이트의 가장 중요한 부분 중 하나는 사용자에게 정보를 효과적으로 전달하는 것입니다. 결국, 그것이 우리가 웹 사이트를 만드는 이유이죠! 이 커뮤니케이션의 중요한 부분 중 하나는 사용자에게 백그라운드에서 무슨 일이 일어나고 있는지 알려주는 것입니다. 그들이 개발자 도구를 열거나 네트워크 요청을 확인하지 않아도 되도록 사용자에게 메시지를 보내는 것입니다.

특히 fetch 호출에 대해서는 일반적으로 console.log()에만 의존하지 말고 요청 상태에 대한 알림을 최종 사용자에게 표시해야 합니다. 물론 setTimeout()을 사용하여 직접 디자인 및 스타일링한 알림을 만들 수 있지만, React를 사용한다면 그렇게 할 필요가 없습니다. 왜냐하면 완벽한 알림 핸들러가 단 한 번의 npm 설치로 제공되기 때문입니다!

React Hot Toast는 앱에 알림을 추가하는 간단하고 사용자 친화적인 방법이며, 사용하면 웹 사이트가 보다 전문적으로 보이도록 알림을 레벨업할 수 있습니다.

## 시작하기

<div class="content-ad"></div>

시작하려면 npm install react-hot-toast 또는 yarn add react-hot-toast를 사용하여 설치한 후 `Toaster` 컴포넌트를 앱 상단에 추가하십시오 (파일 맨 위에서 import ' Toaster ' from “react-hot-toast”를 사용하여 Toaster를 가져오는 것을 기억하십시오). 앱 컴포넌트는 다음과 같이 보일 것입니다:

```js
import Header from "./components/main/Header";
import Footer from "./components/main/Footer";
import { Outlet } from "react-router-dom";
import { Toaster } from "react-hot-toast";
import NavBar from "./components/main/NavBar";

function App() {
  return (
    <>
      <Toaster />
      <Header />
      <NavBar />
      <Outlet />
      <Footer />
    </>
  );
}

export default App;
```

이렇게 설정하면 토스팅을 시작할 수 있습니다!

React Hot Toast를 활용하는 여러 방법이 있지만, 이 블로그에서는 Fetch API와의 구현에 초점을 맞추어 사용자에게 동적 메시지를 제공하는 방법에 중점을 둘 것입니다.

<div class="content-ad"></div>

가장 간단한 Hot Toast 구현은 오류 처리에 사용됩니다. 앱에서는 toast.error()를 사용하여 .catch() 문에서 오류를 사용자에게 표시할 수 있습니다. 또한 두 번째 .then()에서 toast.success()를 사용하여 사용자에게 성공 메시지를 표시할 수 있습니다.

그러나 Hot Toast의 더 강력한 사용 방법 중 하나는 toast.promise() 함수의 사용입니다. 이 함수는 프라미스가 처리 중일 때 로딩 아이콘을 표시하고 성공 또는 실패 시 사용자 정의 메시지를 표시합니다.

이 기능의 간단한 구현 예는 아래 POST 요청에서 보여집니다:

```js
export const fetchPostSession = (url, validatedData, addSession, navigate) => {
  const fetchPromise = fetch(url, {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(validatedData),
  })
    .then((res) => {
      if (res.ok) {
        return res.json();
      } else {
        throw new Error(res.statusText);
      }
    })
    .then((session) => {
      addSession(session);
      //toast.success("스터디 세션 생성됨")
      navigate("/sessions");
    })
    .catch((err) => {
      //handleError(`스터디 세션 생성에 실패했습니다. \n${err.message || err}`)
      throw err;
    });

  toast.promise(fetchPromise, {
    loading: "스터디 세션 생성 중...",
    success: "스터디 세션이 성공적으로 생성되었습니다!",
    error: "스터디 세션 생성에 실패했습니다.",
  });
};
```

<div class="content-ad"></div>

보시는 바와 같이, 변수 fetchPromise를 선언하고 POST 요청으로 설정합니다. fetch() 호출 내에서 Hot Toast가 작동하도록 특별히 처리할 것은 없으므로 모두 표준입니다. 성공 및 오류 처리에 대한 이전 시도에서 내가 주석 처리한 코드를 볼 수 있을 거에요. toast.promise 함수는 두 개의 인수를 가져야 합니다 — fetch 함수와 로딩 중 및 성공 또는 실패 시 표시할 메시지가 담긴 객체입니다.

그리고 정말로 그렇게 쉬워요! 몇 초만에 React 앱을 업그레이드하고 전문적인 오류 처리를 추가할 수 있어요. Hot Toast는 매우 많은 사용자 정의가 가능하므로 Hot Toast의 스타일 및 사용자 정의 방법의 더 많은 예제를 확인하십시오.
