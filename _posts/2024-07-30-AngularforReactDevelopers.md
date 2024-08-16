---
title: "React 개발자를 위한 Angular 가이드"
description: ""
coverImage: "/assets/img/2024-07-30-AngularforReactDevelopers_0.png"
date: 2024-07-30 16:58
ogImage: 
  url: /assets/img/2024-07-30-AngularforReactDevelopers_0.png
tag: Tech
originalTitle: "Angular for React Developers"
link: "https://medium.com/@tellmejeffrey/angular-for-react-developers-424c0295f694"
isUpdated: true
updatedAt: 1723816453197
---



![Angular for React Developers](/assets/img/2024-07-30-AngularforReactDevelopers_0.png)

안녕하세요! 오늘부터 Angular에 대해 이야기하는 시리즈 기사를 시작하려고 합니다. Angular는 JavaScript를 사용하여 구성 요소를 쉽고 빠르게 구축하는 UI 프레임워크입니다. 주로 React 개발자를 대상으로 하지만 JavaScript UI 프레임워크로 전환을 고려 중인 누구에게나 유용할 것으로 기대합니다.

저는 여러 해간 React 개발자로 일해왔으며 React는 UI 개발을 사랑하게 만들었습니다. 몇 년 전에 새로운 회사에 합류하면서 수년간 쌓인 코드베이스 위에 빌드해야 했기 때문에 Angular로 전환해야 했습니다. 이 기사들은 제가 그 전환이 시작된 때 손에 쥐고 있었으면 했던 가이드입니다.

두 프레임워크 간의 주요 차이점, Angular에서 좋아하는 점과 React에는 없어 아쉬운 부분, 그리고 Angular에 있었으면 했던 React의 부분 등을 이야기해보고 싶습니다. 논의해야 할 다양한 주제들이 많아서 각각을 세부적으로 다루는 부분으로 나눠 보려고 합니다.

<div class="content-ad"></div>

# React로의 나의 경험

우리가 React에 대해 이야기하는 것으로 시작해봅시다. 나는 Allstate의 딜러 서비스 부서에서 Extended Care 제품을 작업하기 시작했을 때, 대략 여섯 년 전(이 글을 쓰는 시점으로부터) React를 시작했습니다. 이전에는 Red Hat JBoss와 Java Server Pages (JSPs)로 작성된 HTML 사용자 인터페이스 같은 백엔드 서버에서 작업한 적이 있었지만, React를 사용한 UI 개발은 처음이었습니다. JQuery와 같은 프레임워크를 포함한 JavaScript에 대한 경험이 많았지만, 실제 페이지 로직의 대부분은 Java 코드로 작성되었습니다.

멘토를 찾는 것은 새로운 것을 배우는 좋은 방법이라는 것을 알게 되었고, 내가 React를 빠르게 익힐 수 있도록 도와준 Allstate의 멘토가 있어서 운이 좋았습니다. React를 배우는 것 뿐만 아니라 Redux와 같은 다른 프레임워크들도 사용되었기 때문에 제대로 개발할 수 있는 수준에 이르는 데 약 한 달이 걸렸습니다. Allstate의 팀은 페어 프로그래밍을 사용했는데, 이는 많이 도와줬습니다. 페어 프로그래밍에서는 한 명이 드라이버(실제로 키보드를 다루는 사람)이 되고, 다른 한 명이 네비게이터(드라이버를 안내하고 함께 고민하는 역할)였습니다. 새로 온 나는 드라이버로서, 이는 나를 배우는 과정에서 개념을 강화하는 데 도움이 되었습니다.

JSP 페이지를 이전에 작성했던 것과 비교했을 때, React는 상쾌한 바람같았고, UI 페이지를 작성하는 것이 정말 즐겁게 느껴졌습니다. 나는 종종 JavaScript를 싫어했었는데, 그 이유는 주로 타입이 없고 이해하기 어려운 방식으로 스크립트를 작성할 수 있기 때문이었습니다. React는 JavaScript에 대한 제약을 해소해주는 구조를 제공했고, TypeScript를 사용하여 타입 안전성을 확보할 수 있었습니다.

<div class="content-ad"></div>

나는 React 여정 몇 년을 거쳐 NextJS를 발견했다. NextJS는 React를 기반으로 만들어졌지만, 설정의 복잡성을 제거합니다. 설정 파일 보일러플레이트 대신 설정 설정을 재정의하기만 하면 되므로 고급 스타일시트 프레임워크인 SASS와 SCSS와 같은 고급 기능과 같이 정적 및 서버 측 렌더링을 지원합니다. NextJS와 같은 프레임워크로 프로젝트를 시작할 수 있다는 것은 개발 과정을 훨씬 원활하게 만들어 줍니다.

# Angular로의 전환

React 개발을 하면서 나는 React보다 더 나은 UI 개발 프레임워크를 찾고 있었습니다. 내 관심을 끈 몇 가지 흥미로운 프레임워크가 있었고 Angular도 그 중 하나였지만, 문법이 이상하고 React의 문법처럼 명확하지는 않았습니다.

React는 JSX라는 자체 비표준 구문을 갖고 있어 기본적으로 HTML과 JavaScript를 결합한 것이었는데, 처음에는 이것이 이상하게 느껴졌지만, 결국 view와 비즈니스 로직을 결합하는 우아한 방법으로 보였습니다. 나중에 클래스 기반 컴포넌트 대 함수형 컴포넌트 및 React Hooks에 대해서도 이야기할 것이지만, 함수형 컴포넌트와 React Hooks가 내가 가장 좋아하는 React의 부분입니다.

<div class="content-ad"></div>

올스테이트(Allstate)처럼, 새로운 회사에서 멘토를 만나 세계를 가르쳐 주신 분이 있어서 Angular에 대한 학습을 빠르게 적응하고 이제는 React보다 거의 선호합니다. 거의라고 말하는 이유는 React가 더 잘 처리하는 부분이 여전히 있기 때문입니다. 두 프레임워크를 대조하고 비교하는 것이 이 시리즈의 실제 주제입니다.

한 문서로 모든 것을 다루는 것은 불가능할 것이며, UI 개발의 여러 측면에 대해 React와 Angular가 다르게 다루는 방식에 대해 자세히 살펴볼 예정입니다. 또한 이 프레임워크와 함께 작동하여 UI 개발을 매력적으로 만드는 몇 가지 다른 프레임워크에 대해 언급할 것입니다. 흥미로운 다른 UI 프레임워크에 대한 몇 가지 언급도 있을 수 있지만, 주력은 Angular와 React에 있을 것입니다.

# Angular와 React에서 HTML 비교

먼저 Angular와 React에서 HTML 처리 방식에 대해 이야기하겠습니다. 어차피 이 두 프레임워크가 하는 일은 HTML 생성입니다. 나는 구시대적이어서 모든 HTML을 손으로 작성했던 시절에 소프트웨어 개발을 시작했습니다. 요즘에는 인터넷에서 보는 대부분의 HTML이 사용자가 페이지와 상호작용할 때마다 동적으로 생성됩니다.

<div class="content-ad"></div>

React에서 JSX를 사용하면 HTML이 JavaScript 코드와 통합됩니다. 많은 개발자들이 JSP를 작성하거나 PHP를 사용하는 것에 익숙한 경우, 조금 이상하게 느껴질 수 있습니다. JSP와 PHP 파일은 코드가 포함된 템플릿인 반면 JSX는 특정 위치에 HTML이 삽입된 JavaScript 파일과 같은 느낌입니다. 때로는 문자열, 숫자 또는 변수를 반환하는 대신 HTML 조각을 반환할 수도 있습니다:

```js
class UserComponent extends React.Component {
  function userToString(user) {
    return user.firstName + ' ' + user.lastName;
  }

  const user = {
    firstName: 'Manny',
    lastName: 'Ramirez'
  };

  render() {
    return (
      <div>
        <h1>Welcome</h1>
        <div>Greetings, {userToString(user)}!</div>
      </div>
    );
  }
}
```

React의 모든 컴포넌트는 HTML 객체 중 하나를 반환하며(렌더 메서드를 통해 또는 기능 컴포넌트의 반환 값으로), 그 안에 일부 코드가 포함될 수 있습니다.

Angular에서는 JavaScript 파일에 HTML을 지정할 수 있지만 일반적으로 HTML은 별도의 파일에 제공됩니다. HTML 파일에는 동적 부분을 위한 특별한 구문이 있으며, JSP나 PHP를 사용한 적이 있는 사람들에게 익숙한 느낌입니다.

<div class="content-ad"></div>

```js
# 환영합니다!

안녕하세요, { user }님! 여기에는 여러분의 메시지가 있어요:

<div *ngFor="let message of messages">
  <h2>{ message.subject }</h2>
  <div>{ message.body }</div>
  <hr/>
</div>

이 컴포넌트의 비즈니스 영역은 JavaScript 클래스로 둘러싸여 있습니다:

@Component({
    selector: 'app-sample',
    templateUrl: './sample.html',
    styleUrls: ['./sample.scss'],
})
export class Sample {
    @Input user: User;
    @Input messages: Array<Message>;

    someMethod() {
       ...
    }
}

이 문서에서 문법의 세부 사항을 다루지는 않겠지만, 비즈니스 로직이 뷰와 분리되어 있다는 것을 알 수 있습니다.

<div class="content-ad"></div>

앵귤러와 리액트는 모두 많은 컴포넌트에서 하나의 HTML 페이지를 만듭니다. 결과적으로 생성되는 HTML은 하나의 루트 노드를 가진 XML 트리입니다. 리액트와 앵귤러의 큰 차이점 중 하나는 리액트에서 컴포넌트가 항상 하나의 루트 노드를 갖는 HTML을 반환한다는 것입니다. 이것은 큰 제약은 아니지만 가끔은 다른 HTML 주변에 임의의 div 태그를 추가해야 할 때가 있습니다. 이 요구 사항을 충족시키기 위해 단지 이 div 태그를 넣어야 하는 경우를 피하기 위해 React.Fragment라는 특별한 컴포넌트가 만들어졌습니다. 반면 앵귤러는 반환된 HTML에 대해 이러한 제한을 두지 않습니다. 본질적으로, 앵귤러의 각 HTML 파일은 적절한 위치에 삽입된 프래그먼트입니다.

앵귤러와 리액트 HTML 사이의 또 다른 기본적인 차이점은 데이터 바인딩과 이벤트의 구문입니다. 리액트에서는 이러한 바인딩이 일반 HTML 속성과 유사하게 보이며 컴포넌트에 속성으로 전달됩니다. 또한, 이벤트 바인딩은 클릭과 같은 이벤트에 대해 자바스크립트 코드를 실행하는 개발자들이 익숙한 구문을 사용하여 바인딩합니다. 다만 호출할 함수의 이름만 제공하면 되는 겁니다:

...
render() {
  return (
    <div>
      <UserForm user={user} />
      <button onclick="handleButtonClick">Click Me!</button>
    </div>
  );
}
...

반면 앵귤러는 데이터 바인딩에 사용되는 속성에 특수 문자를 넣으며, 이 바인딩은 단방향 또는 양방향일 수 있습니다. 또한 이벤트 바인딩에 다른 문자를 사용합니다.

<div class="content-ad"></div>

<input type="text" ([ngModel])="title" />
<userform [user]="user" (changed)="handleFormChange($event)"></userform>
<button (click)="submitForm()">Submit</button>

이 입력란은 양방향 데이터 바인딩을 보여줍니다. 입력란의 값은 title이라는 변수이며, 입력란 내에서 업데이트가 이루어지면 동일한 변수 title이 업데이트됩니다. userform 컴포넌트는 단방향 바인딩을 나타내며, user 변수가 입력으로 전달되지만 출력으로 전달되지 않습니다. userform에서 발생하는 changed 이벤트는 handleFormChange 메서드를 호출합니다 ($event는 이벤트에서 전달된 데이터를 나타내는 특별한 구문입니다). 버튼 클릭 이벤트 핸들러에 표시된 것처럼 이벤트 구문은 마우스 및 키보드 이벤트에 사용됩니다.

React와 Angular 간의 차이 중 일부이며, 이후의 기사에서 더 다루겠지만, 지금까지 다룬 내용은 Angular와 React에서 페이지가 어떻게 구축되는지의 차이에 대한 통찰력을 제공하며, 한쪽에서 다른 쪽으로 옮겨갈 때 조금 익숙해지는 데 약간의 시간이 필요한 이유를 설명했습니다.

# 결론
```

<div class="content-ad"></div>

UI 개발 세계에서의 내 여정은 Java에서 시작되었습니다. 그곳은 무겁고 효율적이지 않았지만, 그 후 React를 발견하게 되면서 상황이 바뀌었습니다. React에서 앱을 개발하는 것을 정말 즐겼지만, 새 직장으로 전환하면서 Angular를 알게 되었고, 이제는 그것을 사용하고 있어요. 이러한 프레임워크에서 어떻게 앱이 개발되는지에는 많은 차이가 있고, HTML이 작성되는 방식만 해도 그것의 한 예시입니다. 이후에는, 상태 관리, 모듈화, 다른 프레임워크와의 통합, 비동기 이벤트, 그리고 각각의 프레임워크에서 독특한 기능과 같은 다양한 차이점에 대해 살펴볼 것입니다.