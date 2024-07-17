---
title: "멀티 페이지 앱 뷰 전환 기능이 드디어 도입됐습니다"
description: ""
coverImage: "/assets/img/2024-07-12-Multi-PageAppViewTransitionsAreHere_0.png"
date: 2024-07-12 18:31
ogImage:
  url: /assets/img/2024-07-12-Multi-PageAppViewTransitionsAreHere_0.png
tag: Tech
originalTitle: "Multi-Page App View Transitions Are Here!"
link: "https://medium.com/itnext/multi-page-app-view-transitions-are-here-9bf29b966355"
---

이제 전적으로 정적 HTML 페이지로 전환할 수 있습니다

![image](/assets/img/2024-07-12-Multi-PageAppViewTransitionsAreHere_0.png)

같은 문서에 대한 뷰 전환, 즉 싱글페이지 앱의 경우 현재 Chrome, Edge 및 Safari에서 지원되지만 이제 Chrome 126도 문서 간 뷰 전환이 지원됩니다. 즉, 이제 동일 도메인에 호스팅된 두 정적 HTML 페이지 간의 전환을 할 수 있습니다.

웹 앱에 대한 큰 이점인데요, 페이지 전환을 구현하는 것이 더욱 간단해졌습니다. 동일 문서 뷰 전환이 이미 싱글페이지 앱에서 페이지 전환을 구현하는 것을 훨씬 쉽게 만들었지만, 문서 간 뷰 전환은 이를 한 단계 업그레이드한 기능입니다.

<div class="content-ad"></div>

SPA를 완전히 제거하고 앱을 정적 HTML 페이지 세트로 구축하면서 멋진 페이지 전환 효과를 유지할 수 있습니다.

## 작동 방식

웹 앱에서 문서 간 뷰 전환을 활성화하려면 페이지가 이러한 전환을 선택해야 합니다. 처음 구현할 때는 각 HTML 페이지에 `meta` 태그를 추가해야 했지만 이제 @view-transition at-rule로 이동되었습니다:

```js
@view-transition {
  navigation: auto;
}
```

<div class="content-ad"></div>

여기에 전환하려는 모든 HTML 페이지에 추가해야 할 내용이 거의 모두 있습니다. 기본적으로 멋진 페이딩 전환 효과를 얻게 될 거에요. 이겁니다. 하지만, 말 그대로, 페이딩 전환은, 음, 다소 지루합니다. 그러니까 이를 사용자 정의해 봅시다!

많은 네이티브 앱에서 찾을 수 있는 'What PWA Can Do Today'의 슬라이딩 전환에 대해 알아봅시다. 이렇게 생겼답니다.

여기에 대한 CSS 일부가 있습니다:

```js
[data-transition="slide"]::view-transition-old(root) {
  animation-name: slide-out;
}

[data-transition="slide"]::view-transition-new(root) {
  animation-name: slide-in;
  mix-blend-mode: normal;
}
```

<div class="content-ad"></div>

옛 페이지는 ::view-transition-old(root) 가상 요소로 표현되고, 새 페이지는 ::view-transition-new(root) 가상 요소로 표현됩니다. 앱에는 페이지 간 전환 데모가 포함되어 있어 페이지에 애니메이션을 선택할 수 있습니다. 이를 위해 애니메이션 이름을 `html` 요소의 data-transition 속성으로 설정하여 선택합니다. "slide" 애니메이션을 사용하려면:

```js
<html data-transition="slide">
```

그리고 CSS에서는 [data-transition="slide"] 속성 선택자를 사용합니다.

기타 CSS 애니메이션 속성은 두 가상 요소에 모두 설정됩니다. 예를 들어:

<div class="content-ad"></div>

```css
::view-transition-new(root),
::view-transition-old(root) {
  animation-duration: 300ms;
  animation-timing-function: cubic-bezier(0.465, 0.183, 0.153, 0.946);
  animation-direction: normal;
}
```

슬라이딩 페이지 전환 효과가 추가됩니다. 그러나 뒤로 이동할 때 새 페이지는 화면 오른쪽으로 다시 슬라이드해 나가야 합니다. 그러나 이전 예제의 CSS로는 사용자가 다시 이동하는 페이지를 포함해 모든 새 페이지가 오른쪽에서 슬라이드되며 우리가 원하는 바가 아닙니다.

돌아가는 내비게이션은 달라야 하며, 전진 또는 후진으로 이동하는 여부에 따라 `html` 요소에 클래스를 설정하여 다른 애니메이션을 적용할 수 있습니다.

다음과 같이 할 수 있습니다:

<div class="content-ad"></div>

```js
// "back-navigation" 시 back-transition 클래스를 추가합니다.
if (isBackNavigation()) {
  document.documentElement.classList.add("back-transition");
}

// 새 페이지를 어떻게 가져와야 할지 설정합니다.
const template = getNewPageHTML();

const transition = document.startViewTransition(async () => {
  document.body.innerHTML = template;

  try {
    // 전환을 기다립니다.
    await transition.finished;
  } catch (e) {
    console.error("전환 중 에러 발생:", e);
  } finally {
    // "back-transition" 클래스를 제거합니다.
    document.documentElement.classList.remove("back-transition");
  }
});
```

그리고 이제 우리는 "back" 네비게이션에서 전환을 변경할 수 있습니다. 다음과 같이:

```js
[data-transition="slide"].back-transition::view-transition-new(root) {
  animation-name: slide-out-reverse;
}

[data-transition="slide"].back-transition::view-transition-old(root) {
  animation-name: slide-in-reverse;
  mix-blend-mode: normal;
  z-index: 1;
}
```

이 방법은 작동하지만, 항상 HTML 요소에 클래스를 추가한 후 전환이 완료되면 제거해야 합니다. 이를 잊어버리면 전환을 망칠 수 있습니다.

<div class="content-ad"></div>

다행히도, 새로운 사양에는 더 나은 방법이 추가되었어요.

## 뷰 전환 유형

뷰 전환 유형을 사용하면 활성 뷰 전환에 하나 이상의 유형을 할당할 수 있어요. 따라서 `html`에 클래스를 추가하는 대신 전환에 종속된 유형을 할당할 수 있어요.

이렇게 하면 HTML을 수정할 필요가 없고, 전환이 완료되면 해당 유형이 자동으로 제거됩니다.

<div class="content-ad"></div>

```js
// 전환 유형 결정
const direction = isBackNavigation() ? "뒤로" : "앞으로";

// 새 페이지 가져오기
const template = getNewPageHTML();

document.startViewTransition({
  update: () => {
    document.body.innerHTML = template;
  },
  // 전환 유형 설정
  types: [direction],
});
```

document.startViewTransition는 이제 뷰를 업데이트하는 update 속성을 포함하는 객체를 받고 types 속성에 전환 유형의 배열을 포함합니다. 앞으로 이동할 때는 전환 유형이 적용되고 뒤로 이동할 때는 뒷면 유형이 적용됩니다.

CSS에서는 이제 :active-view-transition-type 가상 클래스를 사용하여 활성 전환 유형에 따라 규칙을 정의할 수 있습니다:

```js
/* 앞으로 이동 */
html:active-view-transition-type(forward) {
  &[data-transition="slide"]::view-transition-old(root) {
    animation-name: slide-out;
  }

  &[data-transition="slide"]::view-transition-new(root) {
    animation-name: slide-in;
    mix-blend-mode: normal;
  }
}

/* 뒤로 이동 */
html:active-view-transition-type(back) {
  &[data-transition="slide"]::view-transition-new(root) {
    animation-name: slide-out-reverse;
  }

  &[data-transition="slide"]::view-transition-old(root) {
    animation-name: slide-in-reverse;
    mix-blend-mode: normal;
    z-index: 1;
  }
}
```

<div class="content-ad"></div>

과거의 예제들은 모두 동일 문서 뷰 전환에 적용되었지만 이제는 문서 간 뷰 전환에 전환 유형을 사용할 수도 있어요.

## 페이지 전환(pageswap) 및 페이지 공개(pagereveal) 이벤트

문서 간 뷰 전환에 전환 유형을 사용하려면, 페이지 전환(pageswap) 및 페이지 공개(pagereveal) 이벤트가 명세에 추가되었어요.

페이지 전환 이벤트는 이전 뷰의 스냅샷이 촬영되기 직전에 발생하고 페이지 공개 이벤트는 새 뷰의 스냅샷이 촬영되기 직전에 발생해요. 이를 통해 두 페이지 모두에 마지막 변경 사항을 가할 수 있어요.

<div class="content-ad"></div>

What PWA Can Do Today의 페이지 전환에 대해 알아보았는데, 앞으로나 뒤로 이동할 때 올바른 전환 유형을 설정하기 위해 pagereveal을 사용해야 함을 발견했어요:

```js
window.addEventListener("pagereveal", async (e) => {
  // 만약 뷰 전환이 실행 중이면, 이벤트의 "viewTransition" 속성은 "true"가 됩니다
  if (e.viewTransition) {
    // 네비게이션 API를 사용하여 이동 중인 페이지의 URL을 가져오기
    const fromURL = navigation.activation.from.url.replace(base, "");

    // 홈페이지에서 이동할 경우 앞으로 이동, 그 외에는 뒤로 이동
    const navigationType = fromURL === "/" ? "forward" : "back";

    // 올바른 전환 유형 추가
    e.viewTransition.types.add(navigationType);

    // 뒤로 이동할 때 홈페이지를 맨 위로 스크롤
    if (navigationType === "back") {
      window.scrollTo(0, 0);
    }
  }
});
```

그런 다음, 이전 예제와 같은 CSS를 사용하여 이전 및 새로운 뷰의 전환을 사용자 정의할 수 있지만, 사용된 전환 유형을 @view-transition at-rule에 추가해야 합니다:

```css
@view-transition {
  navigation: auto;
  types: back, forward;
}
```

<div class="content-ad"></div>

## 가능한 빠르게 페이지를 로드하는 방법

물론, 서로 다른 문서 간의 뷰 전환을 처리할 때 페이지를 이동할 때 전체 페이지가 다시 로드되므로 모든 페이지가 빠르게 로드되도록 해야 합니다.그렇지 않으면 전환 시 반응이 없어보일 수 있습니다.

페이지가 충분히 빠르게 로드되지 않을 때 이것은 어려울 수 있습니다. 이 경우 웹 앱에 Service Worker를 추가하고 특히 스트리밍 응답을 제공하도록 허용하면 좋은 방법이라고 생각했습니다.

기본적으로 브라우저의 HTML 파서는 스트리밍 방식으로 HTML을 렌더링합니다. 즉, 브라우저가 페이지를 다운로드하는 동안 HTML을 렌더링할 수 있습니다. 브라우저가 전체 페이지를 기다리지 않고 다운로드하는 동안 렌더링을 시작할 수 있습니다.

<div class="content-ad"></div>

당연히 앱의 모든 HTML 페이지를 IndexedDB에 캐시하고 서비스 워커가 이를 제공하도록 할 수 있지만, 더 나은 방법이 있습니다.

웹 앱에 서비스 워커를 추가하고 Response의 body 속성을 사용하면(이 속성은 ReadableStream입니다) 전체 HTML 페이지를 빠르게 로드할 수 있습니다.

페이지를 가져올 때마다 Response의 body 속성에 있는 ReadableStream에 액세스하여 HTML을 다운로드하는 동안 HTML을 렌더링할 수 있습니다:

```js
fetch('/some/url')
.then(response => response.body)
.then(body => {
  const reader = body.getReader(); // 이제 스트림을 읽을 수 있습니다!
}
```

<div class="content-ad"></div>

싱글 페이지 앱에서는 일반적으로 콘텐츠가 삽입되는 단일 페이지인 앱 쉘을 사용합니다. 일반적으로 헤더, 푸터 및 각 페이지의 콘텐츠가 배치되는 콘텐츠 영역으로 구성됩니다.

문제는 HTML 페이지에 로드된 후에 추가되는 모든 콘텐츠가 스트리밍 HTML 파서를 우회하고 따라서 렌더링이 느리다는 것입니다. 모든 페이지를 헤더, 푸터 및 콘텐츠로 분할하고 서비스 워커가 해당 부분을 스트리밍하도록 하면 페이지를 놀라울 정도로 빠르게 렌더링할 수 있습니다.

이는 콘텐츠와 푸터가 아직 다운로드 중일 때 페이지의 헤더를 렌더링하기 시작할 수 있어서 성능상의 큰 이점을 제공합니다.

코드를 살펴봅시다. 특히, 서비스 워커에 의해 가로채진 외부 요청이 발생할 때 호출되는 fetch 이벤트 핸들러를 살펴봅시다.

<div class="content-ad"></div>

```js
const fetchHandler = async (e) => {
  const { request } = e;
  const { url } = request;

  // 만약 페이지가 정적이라면, 스트리밍합니다
  if (isStaticHTML(url)) {
    e.respondWith(getStreamedHtmlResponse(url));
  } else {
    e.respondWith(caches.match(request).then((response) => (response ? response : fetch(request))));
  }
};
self.addEventListener("fetch", fetchHandler);
```

isStaticHTML() 함수는 가져온 URL이 스트리밍해야 하는 정적 HTML 페이지를 가리키는지 확인하기 위한 로직일 수 있습니다.

만약 그렇다면, 서비스 워커는 getStreamedHtmlResponse()로부터 나오는 결과 스트림으로 응답할 것입니다. 아래와 같이 생겼습니다:

```js
const templateFolder = "/src/templates";

// 헤더 템플릿
const header = `${templateFolder}/header.html`;

// 푸터 템플릿
const footer = `${templateFolder}/footer.html`;

const getStreamedHtmlResponse = (url) => {
  const stream = new ReadableStream({
    async start(controller) {
      const pushToStream = (stream) => {
        const reader = stream.getReader();

        return reader.read().then(function process({ value, done }) {
          if (done) {
            return;
          }
          controller.enqueue(value);
          return reader.read().then(process);
        });
      };

      const templates = [
        caches.match(header),
        // 내용, 예를 들어 /src/templates/home.html 같은
        caches.match(`${templateFolder}${url}.html`),
        caches.match(footer),
      ];

      const responses = await Promise.all(templates);

      for (const template of responses) {
        await pushToStream(template.body);
      }

      controller.close();
    },
  });

  // 스트림이 반환된 응답의 본문입니다
  return new Response(stream, {
    headers: { "Content-Type": "text/html; charset=utf-8" },
  });
};
```

<div class="content-ad"></div>

getStreamedHtmlResponse 함수 내부에서는 새 ReadableStream을 구성합니다. 이 스트림은 start 메서드를 포함한 underlyingSource 객체가 전달되며, 이 메서드는 스트림이 구성된 직후 즉시 호출됩니다.

start는 controller 인수가 전달되는데, 이는 ReadableStream의 내부 상태 및 큐를 제어할 수 있는 ReadableStreamDefaultController입니다.

start 메서드 내부에서는 HTML 페이지의 템플릿을 가져와 pushToStream 함수를 사용하여 해당 템플릿의 내용을 개별 스트림으로 메인 스트림에 삽입합니다.

이 함수는 템플릿의 개별 스트림을 청크 단위로 읽고 controller.enqueue()를 사용하여 큐에 넣습니다.

<div class="content-ad"></div>

시작 함수는 비동기 함수이기 때문에 새로운 Response가 만들어지고 즉시 응답의 본문으로 ReadableStream이 반환됩니다.

이제 브라우저는 응답을 스트리밍하여 페이지가 거의 즉시 화면에 나타납니다.

## 하나 더…

크로스 문서 보기 전환은 Android의 Chrome 126에서 잘 작동하지만, 크로스 문서 보기 전환을 사용하는 웹 앱이 PWA로 설치될 때 문제가 발생합니다.

<div class="content-ad"></div>

설치한 후 다른 페이지로 이동하면 브라우저 UI가 화면 상단에 잠깐 나타나므로 전환 과정이 원활하지 않습니다. 이 화면 녹화에서 확인할 수 있습니다:

이 문제에 대한 버그 보고서가 이미 있으므로 곧 수정될 것으로 기대합니다.

현대 웹 플랫폼의 Progressive Web Apps (PWA), 웹 컴포넌트 및 새로운 기능에 대한 주간 업데이트를 원하신다면 제 이메일 목록에 가입해주세요. 현대 웹 플랫폼의 기능을 평이한 영어로 설명한 내용을 확인할 수 있습니다.
