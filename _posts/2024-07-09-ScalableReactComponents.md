---
title: "확장 가능한 React 컴포넌트 만드는 방법"
description: ""
coverImage: "/assets/img/2024-07-09-ScalableReactComponents_0.png"
date: 2024-07-09 13:04
ogImage:
  url: /assets/img/2024-07-09-ScalableReactComponents_0.png
tag: Tech
originalTitle: "Scalable React Components"
link: "https://medium.com/@johnsonkow/scalable-react-components-d3935330f2cb"
---

![Scalable React Components](/assets/img/2024-07-09-ScalableReactComponents_0.png)

리액트 개발자로서, 코드 리뷰 중에 중요한 피드백을 받는 경우가 많았어요. 때때로 컴포넌트를 생성할 때 관심사를 분리하지 않았는데, 그러면 '재사용 가능한가?' 라는 게임을 하게 되더라구요.

지금은 이 플랫폼의 특정 기능이나 페이지에 결합된 컴포넌트들을 만들었지만, 항상 확장 가능한 컴포넌트에 대한 인터뷰 질문을 받아요. 여러 부분에서 사용되고 다양한 방법으로 업데이트될 수 있는 컴포넌트를 만들어야 한다는 거죠. 성공 토스트에 대해 생각해보죠.

개발할 때, 먼저 로그인/가입 페이지와 같은 곳에 성공 토스트를 보고 싶어하는 페이지에서 시작할 거에요. 사용자가 자신을 인증하면 성공 또는 오류 토스트를 볼 수 있어야 해요. 따라서 아래와 같은 컴포넌트를 만들겠죠.

<div class="content-ad"></div>

```js
import React from "react";

const Toast = ({ user, type, children }) => {
  const backgroundColor = type == "success" ? "#21D375" : "#ff7f7f";

  if (!user || user.registrationStatus == "unverified") return null;

  return <div style={backgroundColor}>{children}</div>;
};

export default Toast;
```

이 코드는 솔직히 잘못된 것이 없습니다. 수용 가능한 코드이지만 '재사용 가능한가' 게임에서 패배했지만 왜냐하면 UI/UX 디자이너가 성공 토스트를 사용하는 다른 기능을 구현한다면 어떨까요? 또 다른 질문은 왜이 토스트 컴포넌트가 사용자 인증과 연결되어 있는가에 관한 것입니다. 때로는 특정 사용 사례에 대응하기 위해 코드를 작성하기도 하지만, 개발자로서成장하기 위해 스스로에게 이러한 질문을 해야 합니다. 반응이 빠르고 차이점을 확인할 수 있도록 간단한 추가를 해보겠습니다.

```js
import React from "react";

const Toast = ({ type, children }) => {
  const backgroundColor = type == "success" ? "#21D375" : "#ff7f7f";

  return (
    <div className="Toast" style={backgroundColor}>
      {children}
    </div>
  );
};

export default Toast;

const VerificationToast = ({ user, type, children }) => {
  if (!user || user.registrationStatus == "unverified") return null;

  return <Toast type={type} children={children} />;
};
```

작은 변경사항처럼 느껴질 수 있지만, 그만큼 세계가 달라지게 됩니다. 이제 확장 가능합니다. 로그인/가입 페이지에서 Toast 컴포넌트를 사용하는 대신 VerificationToast를 사용할 수 있습니다. 이것이 확장 가능하게 만드는 요소입니다. 새로운 토스트가 필요한 경우에도 Toast 컴포넌트를 기반으로 개발할 수 있습니다. 이와 같은 사고방식을 적용할 수 있는 다른 컴포넌트로 배너와 모달 등이 있습니다.

<div class="content-ad"></div>

어쨌든, 이 내용이 도움이 되었으면 좋겠어요. 관심사를 분리하고 확장성을 고려하여 프로그래밍하는 개념을 이해하는 데 도움이 되길 바라요. 즐거운 코딩하세요!
