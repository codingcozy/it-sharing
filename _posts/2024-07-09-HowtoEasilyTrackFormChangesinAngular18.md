---
title: "Angular 18에서 폼 변경사항을 쉽게 추적하는 방법"
description: ""
coverImage: "/it-shar.ing/assets/no-image.jpg"
date: 2024-07-09 16:22
ogImage:
  url: /it-shar.ing/assets/no-image.jpg
tag: Tech
originalTitle: "How to Easily Track Form Changes in Angular 18"
link: "https://medium.com/gitconnected/how-to-easily-track-form-changes-in-angular-18-843c295530ee"
---

## 안녕하세요 👋

Angular 18가 나왔어요! 그리고 새로운 흥미로운 기능 중 하나는 폼에서의 통합 된 컨트롤 상태 변경 이벤트입니다. 이는 폼 컨트롤 값의 상태 변화를 추적하는 전역 observable로, 변경된, 터치된, 터치되지 않은, 더러운, 원형 등과 같은 상태를 구독합니다.

빠르게 한 바퀴 돌며 어떻게 사용하는지 배워봅시다! 😃

# Angular 18 이전에...

<div class="content-ad"></div>

이 새로운 기능을 사용하기 전에는 양식 이벤트를 추적했습니다:

- 값 변경 관찰 가능을 통해 양식 값의 변경을 감시할 수 있습니다.
- 또는 유효성 상태를 추적하기 위해 상태 변경 관찰 가능을 통해 추적할 수 있었습니다.

# Angular 18에서...

v18에서는 FormControl 클래스에 추가된 (혹은 더 정확히는 AbstractControl에서 상속된) 새로운 events 속성 (읽기 전용 events: Observable`ControlEvent`TValue``)을 받았습니다. 이 속성은 양식 내의 변경을 추적하는 관찰 가능한 속성입니다. 다른 관찰 가능한 것과 마찬가지로, 구독만 하면 양식의 변경 사항에 대한 알림을 받을 수 있습니다.

<div class="content-ad"></div>

이 기능의 PR은 여기에서 확인하실 수 있어요!

## ▶️ 사용 가능한 이벤트 유형:

- touchchange: 이 이벤트는 컨트롤의 터치 상태가 변경될 때 트리거됩니다.
- pristinechange: 이 이벤트는 컨트롤의 원천 상태가 변경될 때 트리거됩니다.
- valuechange: 이 이벤트는 컨트롤값이 변경될 때마다 트리거됩니다.
- statuschange: 이 이벤트는 컨트롤의 상태가 변경될 때마다 트리거됩니다.
- FormSubmittedEvent: 이 이벤트는 폼이 제출될 때 트리거됩니다.
- FormResetEvent: 이 이벤트는 폼을 재설정할 때마다 트리거됩니다.

## ▶️ 이벤트 활용사례:

<div class="content-ad"></div>

아래와 같이 이벤트를 구독하고 관심 있는 이벤트를 캐치하면 됩니다:

```js
@Component({
  selector: 'app-root',
  standalone: true,
  imports: [ReactiveFormsModule],
  template: `
    <h2>Unified Control State Change Events</h2>
    <form [formGroup]="fGroup">
      <input formControlName="unifiedCtrlDemo" />
      <button type="submit">제출</button>
      <button type="reset">초기화</button>
    </form>
    <p>{ events() }</p>
  `,
})
export class AComponent implements OnInit {
```

```js
fGroup = new FormGroup({
  unifiedCtrlDemo: new FormControl(''),
});
events = signal('');
...
 ngOnInit() {
    // 컨트롤 이벤트
    this.fGroup.get('unifiedCtrlDemo')?.events.subscribe((event) => {
      if (event instanceof ValueChangeEvent) {
        this.events.set(`값 변경됨: ${event.value}`);
        console.log(this.events);
      }
      if (event instanceof TouchedChangeEvent) {
        this.events.set(`터치됨: ${event.touched}`);
        console.log(this.events);
      }
      if (event instanceof PristineChangeEvent) {
        this.events.set(`원본 상태: ${event.pristine}`);
        console.log(this.events);
      }
      if (event instanceof StatusChangeEvent) {
        this.events.set(`상태 변경됨: ${event.status}`);
        console.log(this.events);
      }
    });
    // 폼 이벤트
    this.fGroup?.events.subscribe((event) => {
      if (event instanceof FormSubmittedEvent) {
        this.events.set('폼 제출됨');
        console.log(this.events);
      }
      if (event instanceof FormResetEvent) {
        this.events.set('폼 초기화됨');
        console.log(this.events);
      }
    });
  }
...
}
```

위의 코드 전체를 여기서 찾을 수 있습니다:

<div class="content-ad"></div>

부수고, 놀며, 배워보세요!

# 마지막으로...

폼 이벤트를 처리하는 표준화된 방법이 있어 정말 좋습니다. 이는 Angular 초보 개발자들에게 도움이 될 것이며, 애플리케이션에서 Angular 폼을 사용하는 사람들에게 코드를 간소화해줄 것입니다.

오늘은 여기까지, 나중에 또 봐요 🙋

<div class="content-ad"></div>

질문이나 피드백이 있으면 언제든 댓글을 달거나 LinkedIn을 통해 연락해 주세요. 제가 듣고 있을게요!

제게 커피를 사주고 싶으시다구요? ☕️

제 글이 마음에 들었다면 👏 좋아요를 눌러 주시고 🔗 공유하고 🔔 구독해서 제 최신 글을 받아보세요.

저랑은 Medium, LinkedIn, Facebook, Instagram, YouTube, 혹은 Twitter에서 소통할 수 있어요.
