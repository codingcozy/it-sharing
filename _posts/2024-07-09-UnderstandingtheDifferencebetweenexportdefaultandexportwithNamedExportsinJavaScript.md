---
title: "JavaScript에서 export default와 named export의 차이점 이해하기"
description: ""
coverImage: "/it-shar.ing/assets/no-image.jpg"
date: 2024-07-09 17:11
ogImage:
  url: /it-shar.ing/assets/no-image.jpg
tag: Tech
originalTitle: "Understanding the Difference between export default and export with Named Exports in JavaScript"
link: "https://medium.com/@heshramsis/understanding-the-difference-between-export-default-and-export-with-named-exports-in-javascript-f0569c221a3"
---

JavaScript 모듈을 만들 때 파일에서 코드를 내보내는 두 가지 일반적인 방법이 있습니다: export default를 사용하는 방법과 named exports를 사용하는 방법입니다. 이 두 방법은 모둠의 코드를 다른 모듈에서 사용할 수 있도록 만드는 동일한 목표를 달성하지만, 문법 및 사용 방식에 차이가 있습니다. 이 글에서는 이 두 접근 방식 간의 차이와 언제 어느 쪽을 선택해야 할지에 대해 알아보겠습니다.

# Export Default

export default 구문을 사용하면 모듈에서 기본 내보내기로 하나의 값만 내보낼 수 있습니다. 다른 모듈이 export default를 사용하는 모듈을 가져올 때 가져온 값은 기본으로 내보낸 값이 됩니다. 모듈 당 기본 내보내기는 하나만 가질 수 있습니다.

다음은 인사 메시지를 내보내는 예시입니다:

<div class="content-ad"></div>

```js
// greetings.js
const greeting = "Hello, world!";
export default greeting;
```

그리고 다른 모듈에서 기본 내보내기를 가져오는 예시입니다:

```js
// app.js
import greeting from "./greetings.js";
console.log(greeting); // 결과는 "Hello, world!" 입니다.
```

보시다시피, 기본 내보내기의 이름을 명시할 필요는 없습니다. 기본 내보내기는 하나뿐이기 때문입니다.

<div class="content-ad"></div>

# 명명된 내보내기로 내보내기

명명된 내보내기와 함께 내보내기 구문을 사용하면 모듈에서 여러 값을 내보낼 수 있습니다. 다른 모듈이 명명된 내보내기를 사용하는 모듈을 가져올 때 가져온 값은 내보낸 값들이 속성으로 있는 객체가 됩니다. 모듈 당 여러 명명된 내보내기를 가질 수 있습니다.

다음은 인사말과 작별 메시지를 모두 내보내기 위해 명명된 내보내기를 사용하는 예시입니다:

```js
// greetings.js
export const greeting = "Hello, world!";
export const farewell = "Goodbye, world!";
```

<div class="content-ad"></div>

그리고 다른 모듈에서 명명된 내보내기를 가져오는 예시가 있어요:

```js
// app.js
import { greeting, farewell } from "./greetings.js";
console.log(greeting); // 출력: "Hello, world!"
console.log(farewell); // 출력: "Goodbye, world!"
```

명명된 내보내기를 가져올 때는 중괄호로 묶인 내보낼 값의 정확한 이름을 사용해야 한다는 점을 주의하세요.

# 어떤 것을 사용해야 할까요?

<div class="content-ad"></div>

export default을 사용할지, 명명된 내보내기를 사용할지 결정하는 것은 귀하의 프로젝트의 구체적인 내용 및 코드를 구성하는 방법에 달려 있습니다. 일반적인 지침은 다음과 같습니다:

- 모듈에서 단일 값만 내보내야 하는 경우 또는 모듈이 응용 프로그램의 주요 기능을 나타내는 경우 export default를 사용하십시오.
- 모듈에서 여러 값을 내보내야 하는 경우 또는 코드를 작은 재사용 가능한 구성 요소로 구성하려는 경우 명명된 내보내기를 사용하십시오.

일반적으로 export default 또는 명명된 내보내기 중 하나를 일관되게 사용하여 코드를 구성하고 이해하기 쉽도록 모든 모듈에 적용하는 것이 좋습니다.

# 결론

<div class="content-ad"></div>

한 마디로, export default와 명명된 내보내기를 사용하는 것은 JavaScript 모듈에서 코드를 내보내는 두 가지 방법입니다. export default는 기본 내보내기로 하나의 값을 내보내는 데 사용되는 반면, 명명된 내보내기는 복수의 값을 명명된 내보내기로 내보내는 데 사용됩니다. 이 두 문법 중 어떤 것을 선택할지는 코드 조직화와 재사용성에 대한 여러분의 특정 요구사항에 달려 있습니다.
