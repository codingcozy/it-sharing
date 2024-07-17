---
title: "TypeScript의 any vs unknown vs never 언제 그리고 어떻게 사용해야 할까"
description: ""
coverImage: "/assets/img/2024-07-09-TypeScriptsanyvsunknownvsneverWhenandHowtoUseThem_0.png"
date: 2024-07-09 17:26
ogImage:
  url: /assets/img/2024-07-09-TypeScriptsanyvsunknownvsneverWhenandHowtoUseThem_0.png
tag: Tech
originalTitle: "TypeScript’s any vs unknown vs never: When and How to Use Them"
link: "https://medium.com/@rahuulmiishra/typescripts-any-vs-unknown-vs-never-when-and-how-to-use-them-35d81ea57c01"
---

## TypeScript에서 안전하고 정확한 타이핑을 위한 정보 기반 결정을 내리는 방법

![이미지](/assets/img/2024-07-09-TypeScriptsanyvsunknownvsneverWhenandHowtoUseThem_0.png)

any와 unknown은 값을 어떤 타입으로 지정해야 할지 확실하지 않을 때 사용하는 두 가지 방법입니다. 반면에 never는 모든 데이터 유형을 고려한 후 아무 값도 남지 않았을 때 사용됩니다.

오늘은 이러한 방법들의 사용법을 살펴보고 더 자세히 이해해보겠습니다.

<div class="content-ad"></div>

# any

사용하면 단순히 타입 체커를 끕니다. 'any' 유형으로 표시된 변수에는 아무 것이나 할당할 수 있습니다.

![image](/assets/img/2024-07-09-TypeScriptsanyvsunknownvsneverWhenandHowtoUseThem_1.png)

단점:

1. undefined에서 메서드를 호출해도 TypeScript가 오류를 표시하지 않습니다.

<div class="content-ad"></div>

<img src="/assets/img/2024-07-09-TypeScriptsanyvsunknownvsneverWhenandHowtoUseThem_2.png" />

알 수 없음으로 이 문제를 해결하는 방법을 살펴보겠습니다.

# 알 수 없음

타입스크립트 여행을 시작할 때 모든 사용하는 것에 대해 타입을 명시하는 것은 매우 어렵습니다. 때로는 구글 검색도 실패하거나 시간이 오래 걸리는 경우가 있습니다.

<div class="content-ad"></div>

위험한 경우에는 unknown을 사용할 수 있어요.

unknown이 무엇을 하는지 아시나요?

unknown은 익명의 것을 뜻하는 any와 같지만 더 강력한 역할을 한답니다.

unknown 타입의 변수를 만들면 any처럼 어떤 값이든 할당할 수 있지만, 그 값을 가져오려고 할 때 TypeScript가 오류를 반환합니다. unknown 타입의 변수에 접근할 때는 해당 값이 존재하지 않을 수 있다고 말해줘요.

<div class="content-ad"></div>

알 수 없는 타입의 변수에서 무언가를 사용하기 전에 체크를 추가하라는 요구사항입니다.

![image](/assets/img/2024-07-09-TypeScriptsanyvsunknownvsneverWhenandHowtoUseThem_3.png)

이제 에러를 보면, TypeScript는 알 수 없는 타입을 사용하기 전에 타입/값 체크를 강제하도록 요구하고 있습니다.

any를 unknown으로 대체함으로써, TypeScript가 발생시키는 에러를 얻을 수 있는 이점이 있습니다.

<div class="content-ad"></div>

지금은 알 수 없는 유형에 대해 작업을 수행하려면 추가적인 확인을 추가해야 합니다.

![image 4](/assets/img/2024-07-09-TypeScriptsanyvsunknownvsneverWhenandHowtoUseThem_4.png)

함수의 경우, 확인사항이 더 엄격합니다.

![image 5](/assets/img/2024-07-09-TypeScriptsanyvsunknownvsneverWhenandHowtoUseThem_5.png)

<div class="content-ad"></div>

알 수 없음은 데이터 유형의 올바른 작동을 보장하도록 합니다.

# 결코

결코는 모든 유형과 반대로 간주될 수 있습니다.

"모든"은 아무 것이나 주라는데 반해, "결코"는 아무 것도 주라고 말합니다.

<div class="content-ad"></div>

`never`은 모든 가능한 값을 모두 사용하고 할당할 것이 없을 때 사용됩니다.

할당할 것이 없는 상황을 몇 가지 살펴보겠습니다.

- if else 블록 안에서

![이미지](/assets/img/2024-07-09-TypeScriptsanyvsunknownvsneverWhenandHowtoUseThem_6.png)

<div class="content-ad"></div>

else 블록에서는 문자열이나 숫자가 아닌 다른 값이 오면 오류가 발생합니다.

또한 누군가가 함수 시그니처를 객체를 받도록 변경하고, 객체 유형을 처리하는 if가 없는 경우 컴파일 시간에 오류를 볼 수 있습니다.

2. switch 문 안

![이미지](/assets/img/2024-07-09-TypeScriptsanyvsunknownvsneverWhenandHowtoUseThem_7.png)

<div class="content-ad"></div>

3. 오류를 throw 하는 함수에서

![image](/assets/img/2024-07-09-TypeScriptsanyvsunknownvsneverWhenandHowtoUseThem_8.png)

우리는 이 함수가 실행되지 않을 것이고 아무것도 반환하지 않을 것을 알고 있습니다. 그리고 이는 반환 유형으로 never를 할당하는 완벽한 경우입니다.

4. never는 종종 조건문과 함께 사용됩니다.

<div class="content-ad"></div>

<img src="/assets/img/2024-07-09-TypeScriptsanyvsunknownvsneverWhenandHowtoUseThem_9.png" />

이 기사를 통해 새로운 것을 배워 보았으면 좋겠어요.

웹 개발과 관련된 개념을 이해하거나 의문이 생기면, topmate에서 저와 상담을 예약해보세요. https://topmate.io/frontendmaster
