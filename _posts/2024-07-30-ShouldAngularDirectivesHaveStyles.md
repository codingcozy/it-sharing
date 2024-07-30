---
title: " Angular 디렉티브에 스타일을 적용해야 할까"
description: ""
coverImage: "/assets/img/2024-07-30-ShouldAngularDirectivesHaveStyles_0.png"
date: 2024-07-30 17:03
ogImage: 
  url: /assets/img/2024-07-30-ShouldAngularDirectivesHaveStyles_0.png
tag: Tech
originalTitle: " Should Angular Directives Have Styles"
link: "https://medium.com/@tomaszs2/should-angular-directives-have-styles-62cd7e566542"
---


![image](/assets/img/2024-07-30-ShouldAngularDirectivesHaveStyles_0.png)

# Angular의 속성 지시자와 스타일에 관한 토론이 7년 동안 이어지고 있습니다. 그러나 이것이 변할 수도 있습니다.

평소처럼 Angular에서 보고된 이슈를 찾아보다가, 2017년에 시작된 흥미로운 토론을 발견했습니다. 7년 전부터 계속되어온 이야기죠.

토론이 상당히 길어서 주요 이슈를 이해하는 것이 실제로 어렵기 때문에 요약해 드리겠습니다.

<div class="content-ad"></div>

간단히 말하면 Angular 개발자들 중에 지시문에 styles 및 styleUrl을 정의할 수 있는 기능에 대한 큰 수요가 있습니다.

이해를 돕기 위해 이것을 분석해보겠습니다. Angular 애플리케이션은 주로 구성 요소와 조합으로 구축됩니다.

이것은 기능의 동작과 외관을 캡슐화하는 구성 요소를 구축한다는 의미입니다. 예를 들어 버튼. 그러면 해당 버튼으로 사용자 인터페이스를 구성할 수 있고, 이제는 내부 구현과 스타일에 더 이상 신경 쓰지 않아도 됩니다.

외관과 부분적인 동작은 CSS(또는 SCSS 등)를 사용하여 달성되며, @Component 지시문의 styles 속성이나 styleUrl에 내부적으로 정의된 스타일 시트로 이루어집니다. 이 속성은 스타일을 포함한 CSS 또는 SCSS 파일을 가리킵니다.

<div class="content-ad"></div>

하지만 구성 요소 이외에도 Angular에는 디렉티브라는 개체도 있습니다. 공식 Angular 문서에 따르면:

문서에는 컴포넌트의 호스트 요소의 색을 변경하는 디렉티브의 예가 제시되어 있습니다:

```js
import {Directive, ElementRef} from '@angular/core';
@Directive({
  standalone: true,
  selector: '[appHighlight]',
})
export class HighlightDirective {
  constructor(private el: ElementRef) {
    this.el.nativeElement
      .style.backgroundColor = 'yellow';
  }
}
```

그런데 99.9%의 작업을 컴포넌트와 표준 구성으로 수행할 수 있기 때문에 디렉티브가 필요한 이유가 무엇일까요?

<div class="content-ad"></div>

우리가 지시문을 적용하려는 컴포넌트가 자체 하이라이트 속성을 가질 수 없는 이유는 무엇인가요?

물론 가능합니다. 그러나 컴포넌트 라이브러리를 구축할 때와 같이 경우에 따라 그렇게 할 수 없는 경우가 있습니다.

예를 들어 Angular Material은 네이티브 입력 값을 Angular Material mat-form-field 컴포넌트와 함께 사용할 수 있도록 matInput 지시문을 사용합니다:

```js
<mat-form-field class="example-full-width">
    <mat-label>Favorite food</mat-label>
    <input matInput placeholder="Ex. Pizza" value="Sushi">
</mat-form-field>
```

<div class="content-ad"></div>

Angular Material과 기본 요소 간의 간격을 좁히는 목적으로 사용됩니다.

"mat-input"과 같이 자체 입력 구성 요소를 제공하지 않는 이유가 궁금할 수 있습니다. 이 동작을 캡슐화하기 위해 지시문을 사용하는 대신 왜 그런지 궁금할 수 있습니다.

그러나 Angular Material 개발자가 입력 API를 확장하고 싶지 않았다는 점이 합리적으로 보입니다. 즉, 입력 기본 속성을 변경하고 싶지 않았다는 것을 의미합니다. 따라서 지시문은 입력 요소의 동작을 좁게 변경하면서 입력 태그 이름을 포함한 입력 기능의 나머지에 영향을 주지 않는 가벼운 방법입니다.

결론적으로 우리가 얻을 수 있는 결론은 다음과 같습니다:

<div class="content-ad"></div>

- 지시문은 컴포넌트의 일부를 변경할 수 있도록 해주지만 나머지 부분에 영향을 미치지 않습니다.
- 이것은 구현을 변경하지 않고 다양한 컴포넌트에 적용할 수 있습니다.

그래서 지시문은 컴포넌트 조합이 불편한 상황에서 탈출구 역할을 합니다.

앵귤러에서는 특정한 경우에만 드물게 사용되는 매우 구체적인 요소입니다.

이제 우리 모두가 알고 있는 것은 앵귤러에서의 지시문의 목적입니다. 이는 2017년에 보고된 이슈의 주제를 이해하는 데 도움이 될 것입니다.

<div class="content-ad"></div>

앵귤러 문서의 지시문을 다시 살펴봅시다:

```js
import {Directive, ElementRef} from '@angular/core';
@Directive({
  standalone: true,
  selector: '[appHighlight]',
})
export class HighlightDirective {
  constructor(private el: ElementRef) {
    this.el.nativeElement
      .style.backgroundColor = 'yellow';
  }
}
```

이 코드에서 앵귤러스러운 부분이 있지 않은 것 같습니다. 훈련받은 눈은 즉시 알아차릴 것입니다.

앵귤러에서 컴포넌트에 스타일을 적용하려면 styles 또는 styleUrl 속성을 사용해야 합니다:

<div class="content-ad"></div>


```js
@Component({
  selector: 'profile-photo',
  template: `<img src="profile-photo.jpg" alt="Your profile photo">`,
  styles: ` img { border-radius: 50%; } `,
})
export class ProfilePhoto { }
```

앵귤러에서 스타일을 적용하는 방법이에요.

그러나 지시문에서는 지원하지 않기 때문에 그 Angular 기능을 사용할 수 없어요.

2017년 이슈가 다른 개발자들로부터 많은 지원을 받아 고쳐져야 한다는 합리적인 요구로 등장했어요:


<div class="content-ad"></div>

<img src="/assets/img/2024-07-30-ShouldAngularDirectivesHaveStyles_1.png" />

기능 요청에 대한 토론이 진행되면 의사 소통에 많은 오해가 있다는 느낌을 받습니다.

이 기능이 분명히 누락되어 있다고 생각하는 개발자들이 많습니다. 또한 다른 방법을 제안하고 필요성을 이해하지 못하는 개발자들도 있습니다.

Angular 문서에 표시된 것처럼, 디렉티브는 일반적인 구성과 부품 사용이 불편한 경우에 사용되는 이스케이프 햇치입니다.

<div class="content-ad"></div>

@Directive를 사용한 모든 예제들은 다른 방법으로 해결할 수 있습니다. 제안된 모든 해결책과 해결책을 모두 읽어 보았어요. 하지만 먼저, 특정한 상황에서 왜 누군가가 @Directive를 모두 필요로 하는지 전체 맥락을 설명하는 것이 매우 어렵다고 생각해요.

두 번째로, 제안된 해결책들은 지루합니다. 지시자들은 바로 이러한 경우를 처리하는 깔끔하고 편리한 방법을 제공하기 위해 존재하는 이유이기 때문에요.

현재 형태를 보면, 디렉티브를 사용하여 스타일을 적용하는 방식이 nativeElement (또는 글로벌 스타일 등)을 통해 이루어지는 것은 디자인 결정인 것처럼 느껴져요.

@Directive는 hacky하고 사용해서는 안 되는 것이므로, 생으로 남겨두어 사용을 자제하도록 해요. 정글에 있으니 자신의 힘으로 해결해보세요.

<div class="content-ad"></div>

그러나 Angular Material 팀과 기타 사용자는 지시문을 사용하며, 이들이 합리적인 사용 사례를 갖고 있는 것으로 나타났습니다.

그러므로 지시문은 그리 해킹적이지 않을 수도 있습니다. 아마도 우리는 지시문을 Angular 프레임워크의 중요한 부분으로 취급할 수 있을지도 모릅니다. 정의된 지시문은 동작을 변경하는 데 사용될 수 있습니다. 때로는 동작을 변경하는 데 CSS를 통해 변경을 필요로 할 수 있습니다.

이 경우, 실제로 Angular 기능을 제공할 수도 있습니다. 이 경우에는 styles 및 styleUrl에 대한 지원이 합리적입니다.

부가적인 혜택으로, 속성 지시문 API가 구성요소와 더 일치할 수 있습니다. 속성 지시문은 결국 가벼운 구성요소와 유사하기 때문에 이는 합리적인 일로 보입니다.

<div class="content-ad"></div>

그래서 directive를 스타일링해야 할까요? 주석을 달아보세요!

그리고, Angular 치트 시트 카드를 디자인했어요. 지금 주문하세요!

만약 이 글을 좋아하신다면, 박수를 치거나 이메일로 구독하고 공유하고 좋아요를 눌러주세요! 

소프트웨어 엔지니어링에 대해 최신 소식을 받고 싶으신가요? 이미 18,000명 이상의 구독자들이 Tom Smykowski의 기사를 구독하고 있어요. 이제 당신도 그들 중 하나가 되어보세요. 매월 $5만 내면 Tom Smykowski뿐만 아니라 Medium의 모든 기사에도 액세스할 수 있고, Tom Smykowski가 더 많은 흥미로운 이야기를 만들어 내게 됩니다! 지금 멤버십 가입하세요!