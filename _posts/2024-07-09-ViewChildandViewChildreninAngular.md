---
title: "2024년 Angular의 View Child와 View Children 기능 비교 및 사용방법"
description: ""
coverImage: "/assets/img/2024-07-09-ViewChildandViewChildreninAngular_0.png"
date: 2024-07-09 17:36
ogImage:
  url: /assets/img/2024-07-09-ViewChildandViewChildreninAngular_0.png
tag: Tech
originalTitle: "View Child and View Children in Angular"
link: "https://medium.com/@jaydeepvpatil225/view-child-and-view-children-in-angular-fa19b77740d5"
---

![image](/assets/img/2024-07-09-ViewChildandViewChildreninAngular_0.png)

Angular에서 View Child 및 View Children 데코레이터에 대해 실제 구현을 통해 알아보겠습니다.

개요

- 동일한 또는 하위 컴포넌트의 HTML 템플릿 내에 있는 DOM 요소에 액세스하려면 View Child 및 View Children 데코레이터를 사용할 수 있습니다.
- 또한 View Child 및 View Children 데코레이터를 사용하여 자식 컴포넌트의 속성 및 메서드에 액세스하고 싶은 경우 가능합니다.

<div class="content-ad"></div>

구현

자식 컴포넌트 HTML 파일

```js
<div class="alert alert-secondary" role="alert" style="padding: 30px; margin: 40px;">
    <h3 #childheading>자식 컴포넌트</h3>
</div>
```

- 보다시피 여기에서는 제목 요소와 해시와 이름이 "childheading"로 지정된 참조 변수가 있습니다.
- 이제 이 제목 DOM 요소에 접근하려면 자식 컴포넌트 유형의 TypeScript 파일 내에서 아래와 같이 View Child 데코레이터를 사용해야 합니다

<div class="content-ad"></div>

자식 구성 요소 TypeScript 파일

```js
import { Component, ElementRef, Renderer2, ViewChild} from '@angular/core';

@Component({
  selector: 'app-child',
  templateUrl: './child.component.html',
  styleUrls: ['./child.component.css']
})
export class ChildComponent {
  // ViewChild
  @ViewChild("childheading") childheading!: ElementRef;

  constructor(private renderer: Renderer2) {

  }

  ngOnInit(): void {

  }

  ngAfterViewInit(): void {
    console.log(this.childheading)

    // ViewChild
    this.renderer.setStyle(this.childheading.nativeElement, 'background-color', 'red');

  }
}
```

- 1번 줄 - Angular 코어 라이브러리에서 ViewChild, ElementRef 및 Renderer2를 가져옵니다.
- 10번 줄 – ViewChild 요소를 생성하고 ElementRef 유형을 사용하여 ViewChild 내의 참조 변수를 더블 쿼트 내부에 넣습니다.
- 12번 줄 – DOM 요소의 속성을 변경하는 데 사용할 Renderer2를 생성자 내에 주입합니다.
- 그 후에 ngAfterViewInit Angular 라이프사이클 후크를 사용하여 HTML DOM 요소에 액세스할 때 해당 요소가 렌더링되고 초기화될 때 사용합니다.
- 21번 줄 – 확인 섹션 내에서 해당 속성에 대한 세부 정보를 확인하려면 콘솔에 자식 헤딩을 기록합니다.

![이미지](/assets/img/2024-07-09-ViewChildandViewChildreninAngular_1.png)

<div class="content-ad"></div>

- 여기 콘솔 섹션 안에 우리 애플리케이션을 실행하면 ElementRef와 네이티브 엘리먼트를 볼 수 있습니다. 다양한 속성을 액세스하고 수정할 수 있습니다. 아래에 제가 보여준 것처럼요.
- 라인 24 - renderers를 사용하여 네이티브 엘리먼트에 다른 스타일(글꼴, 배경색 등)을 설정할 수 있습니다.

같은 참조 변수를 사용하여 다른 헤딩에 액세스하려면(예: 자식 헤딩) View Child로는 불가능합니다. 대신 View Children을 사용해야 합니다. 왜냐하면 View Child는 첫 번째 일치하는 DOM 참조 엘리먼트만 가리키기 때문입니다.

View Children의 구현을 시작해보겠습니다.

자식 컴포넌트 HTML 파일

<div class="content-ad"></div>

```js
<div class="alert alert-secondary" role="alert" style="padding: 30px; margin: 40px;">
    <h3 #childheading>Child Component</h3>
    <h3 #childheading> Demo </h3>
</div>
```

- 여기에서는 동일한 템플릿 참조 변수를 가진 두 개의 제목을 볼 수 있습니다. View Children을 사용하여 각각에 대해 다른 속성을 설정할 것입니다.

Child Component TypeScript 파일

```js
import { Component, QueryList, Renderer2, ViewChildren } from '@angular/core';

@Component({
  selector: 'app-child',
  templateUrl: './child.component.html',
  styleUrls: ['./child.component.css']
})
export class ChildComponent {
  //View Children
  @ViewChildren("childheading") childheading!: QueryList<any>;

  constructor(private renderer: Renderer2) {

  }

  ngOnInit(): void {

  }

  ngAfterViewInit(): void {
    console.log(this.childheading);

    //ViewChildren
    this.renderer.setStyle(this.childheading.first.nativeElement, 'color', 'red');
    this.renderer.setStyle(this.childheading.last.nativeElement, 'color', 'blue');

  }
}
```

<div class="content-ad"></div>

- 첫 번째 줄 — angular core 라이브러리에서 QueryList, Renderer2 및 ViewChildren을 가져옵니다.
- 열 번째 줄 — QueryList`any` 타입의 ViewChildren 객체를 생성하고 그 안에 템플릿 참조 변수를 넣습니다.
- 십 이 번째 줄 — 생성자 내부에 Renderer2를 주입합니다.
- 스무 번째 줄 — ngAfterViewInit Angular 라이프사이클 훅을 사용하여 요소가 초기화될 때 호출됩니다.
- 이십 일 번째 줄 — 콘솔에 자식 헤딩을 기록하며 애플리케이션을 실행하면 해당 내용을 볼 수 있습니다.

![이미지](/assets/img/2024-07-09-ViewChildandViewChildreninAngular_2.png)

여기서 첫 번째와 마지막 네이티브 엘리먼트의 세부 정보를 담은 쿼리 리스트를 볼 수 있습니다.

- 24, 25번째 줄 — 첫 번째와 마지막 요소에 대한 속성을 기반으로 서로 다른 스타일이나 기타 것들을 설정합니다.

<div class="content-ad"></div>

DOM 요소에 ViewChild 및 View Children을 사용하여 접근하는 방법에 대한 모든 것입니다.

이제 View Child 데코레이터를 사용하여 부모 컴포넌트에서 자식 컴포넌트 메서드에 액세스할 예정입니다.

자식 컴포넌트 HTML 파일

```js
<div class="alert alert-secondary" role="alert" style="padding: 30px;margin: 40px;">
  <h3>자식 컴포넌트</h3>
  <h5>{message}</h5>
</div>
```

<div class="content-ad"></div>

- 위에서 보시다시피, 우리는 문자열 보간을 사용하여 메서드가 실행될 때 메시지를 표시합니다.

자식 컴포넌트 TypeScript 파일

```js
import { Component } from "@angular/core";

@Component({
  selector: "app-child",
  templateUrl: "./child.component.html",
  styleUrls: ["./child.component.css"],
})
export class ChildComponent {
  message: string = "";

  constructor() {}

  ngOnInit(): void {}

  //자식 메서드
  childMethod() {
    this.message = "안녕, 부모 컴포넌트가 자식 메서드를 호출했어요...";
  }
}
```

- 자식 컴포넌트 내에서 하나의 메시지 속성과 메시지 속성에 어떤 문자열 메시지를 넣는 자식 메서드를 선언하고, 이제 이 자식 메서드를 부모 컴포넌트에서 실행할 것입니다.

<div class="content-ad"></div>

부모 컴포넌트 TypeScript 파일

```js
import { Component, ViewChild } from '@angular/core';
import { ChildComponent } from './child/child.component';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'viewchildandchildrendemo';

  @ViewChild("childMethod") method! : ChildComponent

  parentFunction()
  {
    this.method.childMethod();
  }

}
```

- 여기서는 View Child 객체를 생성하고 사용하려는 메서드명을 입력합니다.
- 그 후에 하나의 부모 함수를 생성하고 해당 함수 내에서 자식 컴포넌트의 메서드를 호출합니다.

부모 컴포넌트 HTML 파일

<div class="content-ad"></div>

<nav class="navbar navbar-light bg-light" style="padding: 30px; margin: 40px; justify-content:center">
  <span class="navbar-brand mb-0 h1">Angular Component Communication</span>
</nav>

<div class="alert alert-primary" role="alert" style="padding: 30px; margin: 40px;">
  <h3>Parent Component</h3>
  <button (click)="parentFunction()">Call child component method</button>
</div>

<app-child #childMethod></app-child>

- HTML 파일 내에서는 Angular 셀렉터와 자식 메소드 참조 변수를 사용하여 자식 컴포넌트를 호출합니다.
- 또한, 이벤트 바인딩을 사용하여 부모 함수를 호출하는 버튼을 하나 생성하고 해당 함수는 자식 메소드를 가리킵니다.

![ViewChild and ViewChildren in Angular](/assets/img/2024-07-09-ViewChildandViewChildreninAngular_3.png)

그래서 이것이 ViewChild와 ViewChildren에 관한 모든 것입니다.

<div class="content-ad"></div>

GitHub 링크

[https://github.com/Jaydeep-007/ViewChildAndChildrenDemo](https://github.com/Jaydeep-007/ViewChildAndChildrenDemo)

코딩 즐거움!
