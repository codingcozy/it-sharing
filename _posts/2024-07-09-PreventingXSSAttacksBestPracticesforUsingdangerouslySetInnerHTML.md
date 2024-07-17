---
title: "XSS 공격 방지하기 dangerouslySetInnerHTML 사용 시 최고의 보안 방법 "
description: ""
coverImage: "/assets/img/2024-07-09-PreventingXSSAttacksBestPracticesforUsingdangerouslySetInnerHTML_0.png"
date: 2024-07-09 08:38
ogImage:
  url: /assets/img/2024-07-09-PreventingXSSAttacksBestPracticesforUsingdangerouslySetInnerHTML_0.png
tag: Tech
originalTitle: "Preventing XSS Attacks: Best Practices for Using dangerouslySetInnerHTML"
link: "https://medium.com/@DanielAlvesOmnes/preventing-xss-attacks-best-practices-for-using-dangerouslysetinnerhtml-943d4897fb29"
---

![2024-07-09-PreventingXSSAttacksBestPracticesforUsingdangerouslySetInnerHTML_0.png](/assets/img/2024-07-09-PreventingXSSAttacksBestPracticesforUsingdangerouslySetInnerHTML_0.png)

안녕하세요,

웹 개발 분야에서 효율을 위해 단축키를 사용하기 쉽습니다. 하나의 예는 dangerouslySetInnerHTML을 사용하여 HTML 문자열을 웹 애플리케이션에 직접 주입하는 것입니다. 이 방법은 HTML을 렌더링하는 데 편리할 수 있지만 크로스사이트 스크립팅(XSS) 공격과 같은 심각한 보안 위험을 초래할 수 있습니다.

이 글에서는 dangerouslySetInnerHTML과 관련된 위험에 대해 자세히 살펴보고 XSS 공격의 본질을 이해하며 이러한 위험을 완화하는 효과적인 전략에 대해 알아볼 것입니다.

<div class="content-ad"></div>

# 위험을 이해해 봅시다

## dangerouslySetInnerHTML의 본질

dangerouslySetInnerHTML을 사용하면, 데이터를 샌드박스화하지 않고 직접적으로 웹페이지에 렌더링할 수 있습니다. 간단한 해결책처럼 보일 수 있지만, 이는 중요한 보안 취약점을 노출시킵니다.

## XSS 공격의 위협

<div class="content-ad"></div>

크로스 사이트 스크립팅(XSS) 공격은 악성 스크립트가 웹 응용 프로그램에 삽입될 때 발생합니다. 이러한 스크립트는 사용자의 브라우저 상에서 실행되어 쿠키, 세션 토큰 또는 다른 개인 데이터와 같은 중요 정보에 무단 액세스로 이어질 수 있습니다.

XSS 취약점은 주로 사용자가 제공한 콘텐츠(예: 폼 입력 또는 dangerouslySetInnerHTML에 전달된 HTML 문자열)에서 발생합니다. 공격자는 이러한 취약점을 이용하여 악성 스크립트를 삽입하여 웹사이트의 보안과 신뢰를 침해할 수 있습니다.

# 위험 완화 방법

위험성을 감소시키기 위해 dangerouslySetInnerHTML과 관련된 위험을 완화하는 조치를 취하는 것이 중요합니다. 여기에 일부 유용한 전략이 있습니다:

<div class="content-ad"></div>

# 1. HTML 살균 처리하기

HTML 살균 처리는 XSS 공격을 예방하는 가장 효과적인 방법 중 하나입니다. DOMPurify와 같은 라이브러리는 HTML 문자열을 살균 처리하여 잠재적으로 해로운 스크립트를 제거하는 데 도움이 될 수 있습니다.

```js
import React from "react";
import DOMPurify from "dompurify";
import "./style.css";

export default function App() {
  const html = `<button onclick="alert('hack');">click</button>`;
  // DOMPurify는 alert('hack')을 제거합니다.
  const sanitizedHtml = DOMPurify.sanitize(html);

  return <div dangerouslySetInnerHTML={{ __html: sanitizedHtml }} />;
}
```

# 2. 신뢰할 수 있는 제3자 패키지 사용하기

<div class="content-ad"></div>

dangerouslySetInnerHTML에만 의존하는 대신, 더 안전한 대안을 제공하는 신뢰할 수 있는 제삼자 패키지를 사용하는 것을 고려해보세요. 예를 들어, htmr은 HTML을 안전하게 렌더링하고 HTML 문자열을 React 컴포넌트로 변환할 수 있습니다.

```js
import React from "react";
import htmr from "htmr";
import "./style.css";

export default function App() {
  const html = `<button onclick="alert('hack');">click</button>`;

  return <>{htmr(html)}</>;
}
```

htmr은 HTML 문자열을 다른 컴포넌트로 변환하는 기능을 제공하여 HTML을 다루는 동적이면서 안전한 방법을 제공합니다.

# 3. 데이터베이스 보안하기

<div class="content-ad"></div>

HTML 데이터를 저장하는 데이터베이스를 안전하게 유지하고 악의적인 스크립트로부터 보호하세요. 데이터를 데이터베이스에 저장하기 전에 항상 데이터를 유효성 검사하고 정제하세요. 또한 외부 소스로부터 수신한 HTML 콘텐츠에 대해 주의하고, 브라우저에서 렌더링하기 전에 서버에서 안전하게 처리해야 합니다.

# 결론

dangerouslySetInnerHTML은 HTML을 렌더링하는 데 유용한 도구일 수 있지만, 특히 XSS 취약점과 같은 보안 위험에 대해 인식할 필요가 있습니다. HTML을 정제하고 신뢰할 수 있는 타사 패키지를 사용하며 데이터베이스를 안전하게 관리함으로써 이러한 위험을 완화하고 웹 응용 프로그램을 잠재적인 공격으로부터 보호할 수 있습니다.

웹 개발에서 보안은 결코 후순위가 되어서는 안됩니다. 애플리케이션을 안전하게 보호하기 위한 적극적인 조치를 취하는 것은 사용자를 보호할 뿐만 아니라 웹 사이트의 신뢰성과 평판을 향상시킬 수 있습니다.

<div class="content-ad"></div>

안전하고 즐거운 코딩하세요!
