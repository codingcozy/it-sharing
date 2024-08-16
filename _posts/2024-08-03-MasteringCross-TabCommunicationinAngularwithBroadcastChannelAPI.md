---
title: "BroadcastChannel API를 사용하여 Angular에서 크로스-탭 통신하는 방법"
description: ""
coverImage: "/assets/img/2024-08-03-MasteringCross-TabCommunicationinAngularwithBroadcastChannelAPI_0.png"
date: 2024-08-03 20:49
ogImage: 
  url: /assets/img/2024-08-03-MasteringCross-TabCommunicationinAngularwithBroadcastChannelAPI_0.png
tag: Tech
originalTitle: "Mastering Cross-Tab Communication in Angular with BroadcastChannel API"
link: "https://medium.com/@md.mollaie/mastering-cross-tab-communication-in-angular-with-broadcastchannel-api-0e15ccef75bf"
isUpdated: true
updatedAt: 1723816506769
---



<img src="/assets/img/2024-08-03-MasteringCross-TabCommunicationinAngularwithBroadcastChannelAPI_0.png" />

# 소개

싱글 페이지 애플리케이션(SPAs) 세계에서 우리는 종종 동일한 애플리케이션의 여러 탭이나 창 간에 데이터를 동기화하는 독특한 도전에 직면합니다. 사용자가 Angular 애플리케이션을 여러 탭에서 열었을 때 한 탭에서 변경 사항을 만들었지만 다른 탭은 이를 모르고 있어 일관성이 없고 사용자의 답답함을 유발할 수 있는 시나리오를 상상해 보세요.

전통적인 해결책은 백엔드 서버의 지속적인 폴링이나 복잡한 WebSocket 설정을 포함합니다. 그러나 조금 더 간단하고 효율적으로 처리하는 방법이 있다고 말한다면 어떨까요? 바로 BroadcastChannel API입니다. 이는 교차 탭 통신에 강력하지만 underutilized된 도구입니다.

<div class="content-ad"></div>

이 기사에서는이 문제를 해결하기 위한 다양한 방법을 탐구하고 BroadcastChannel API에 대해 깊이 파고들고 Angular를 사용하여 실제 예제를 구축할 것입니다. 마침내 여러 탭 간에 Angular 앱을 완벽하게 동기화하는 튼튼한 해결책을 얻을 것입니다.

# 크로스 탭 통신의 과제

해결책에 대해 깊이 이해하기 전에 문제를 명확히 정의해 보겠습니다. 전형적인 SPA에서:

- 각 탭 또는 창은 응용 프로그램의 독립적인 인스턴스로 작동합니다.
- 하나의 탭에서 상태 변경(예: 사용자 조작, 데이터 업데이트)은 자동으로 다른 탭에 반영되지 않습니다.
- 여러 탭에서 작업하는 사용자는 오래된 데이터나 일관성 없는 데이터를 만날 수 있습니다.

<div class="content-ad"></div>

이것으로 인해 여러 문제가 발생할 수 있어요:

- 데이터 충돌
- 중복된 서버 요청
- 사용자 경험 저하
- 불필요한 폴링으로 인한 서버 부하 증가

이제 문제 해결을 위한 전통적인 방법들을 살펴보겠습니다.

# 교차 테이블 통신에 대한 전통적인 접근 방식

<div class="content-ad"></div>

# 1. 웹소켓

웹소켓은 클라이언트와 서버 간의 전이중, 양방향 통신 채널을 제공합니다.

## 장점:

- 실시간 업데이트
- 양방향 통신
- 널리 지원됨

<div class="content-ad"></div>

## 단점:

- 서버 측 구현이 필요합니다.
- 간단한 데이터 공유에는 과도할 수 있습니다.
- 많은 열린 연결로 인한 잠재적인 확장성 문제가 있을 수 있습니다.

## 간단한 WebSocket 예시:

```js
const socket = new WebSocket('ws://example.com/socket');

socket.onopen = (event) => {
  console.log('WebSocket에 연결되었습니다');
  socket.send('안녕하세요, 서버!');
};

socket.onmessage = (event) => {
  console.log('서버로부터 메시지:', event.data);
};

socket.onclose = (event) => {
  console.log('WebSocket 연결이 종료되었습니다');
};
```  

<div class="content-ad"></div>

# 2. WebRTC (Web Real-Time Communication)

WebRTC은 브라우저 간 직접적인 피어 투 피어 통신을 가능하게 합니다.

## 장점:

- 서버 중개 없이 직접 통신
- 낮은 지연 시간
- 오디오, 비디오 및 데이터 채널 지원

<div class="content-ad"></div>

## 단점:

- 설정 및 관리가 복잡합니다
- 주로 음성/영상 통신을 위해 설계되었습니다
- NAT 트래버셜을 위해 STUN/TURN 서버가 필요할 수 있습니다

## 기본 WebRTC 데이터 채널 예제:

```js
const peerConnection = new RTCPeerConnection();
const dataChannel = peerConnection.createDataChannel("myChannel");

dataChannel.onopen = (event) => {
  console.log('데이터 채널이 열렸습니다');
  dataChannel.send('안녕하세요, 상대방!');
};

dataChannel.onmessage = (event) => {
  console.log('메시지가 수신되었습니다:', event.data);
};
```

<div class="content-ad"></div>

# 3. 로컬 저장소 이벤트

로컬 저장소 API를 사용하면 탭 간에 localStorage의 변경 사항을 감지할 수 있습니다.

## 장점:

- 구현하기 간단합니다
- 널리 지원됩니다
- 서버 통신이 필요하지 않습니다

<div class="content-ad"></div>

## 단점:

- 문자열 데이터에 제한됨
- 대량의 데이터에 대해 느릴 수 있음
- 실시간이 아님 (약간의 지연 발생)

## 로컬 스토리지 이벤트 예시:

```js
// 탭 A에서
window.addEventListener('storage', (event) => {
  if (event.key === 'myKey') {
    console.log('다른 탭에서 값이 변경됨:', event.newValue);
  }
});

// 탭 B에서
localStorage.setItem('myKey', '새 값');
```

<div class="content-ad"></div>

이러한 방법들은 모두 작동할 수 있지만 각각 단점이 있습니다. WebSockets와 WebRTC는 구현하기 복잡할 수 있으며 간단한 데이터 공유에 과도할 수 있습니다. 로컬 저장소 이벤트는 더 단순하지만 성능 및 데이터 유형 측면에서 제한이 있을 수 있습니다.

여기서 BroadcastChannel API가 등장하는 것입니다. 이 API는 다양한 탭 간 통신 시나리오에 대해 더 간단하고 효율적인 솔루션을 제공합니다.

# BroadcastChannel API 소개

BroadcastChannel API는 서로 다른 브라우징 컨텍스트(창, 탭, 아이프레임) 간에 통신할 수 있는 방법을 제공합니다. 이는 동일한 출처의 다른 창/탭으로 메시지를 방송하는 데 특히 설계되었습니다.

<div class="content-ad"></div>

# 역사 및 브라우저 지원

BroadcastChannel API는 2015년에 처음 소개되었으며 현재는 주요 브라우저에서 널리 지원되고 있습니다. 2023년 현재, 다음 브라우저에서 지원됩니다:

- Chrome (54+)
- Firefox (38+)
- Edge (79+)
- Safari (15.4+)
- Opera (41+)

(최신 브라우저 지원 정보는 항상 caniuse.com에서 확인해 주세요.)

<div class="content-ad"></div>

# 브로드캐스트 채널 작동 방식

API를 사용하는 것이 의외로 간단합니다. 다음은 기본 예제입니다:

```js
// 채널 생성 또는 조인
const channel = new BroadcastChannel('my-channel');

// 메시지 수신 대기
channel.onmessage = (event) => {
  console.log('받은 메시지:', event.data);
};

// 메시지 전송
channel.postMessage('안녕하세요!');

// 사용이 완료되면 채널 닫기
channel.close();
```

## 주요 개념:

<div class="content-ad"></div>

- 채널 생성: BroadcastChannel을 만들 때는 새 채널을 만들거나 동일한 이름의 기존 채널에 가입하는 것입니다.
- 메시지 브로드캐스팅: postMessage()를 통해 보낸 메시지는 동일한 채널에 가입한 다른 모든 탭/창으로 브로드캐스트됩니다.
- 동일 출처 정책: BroadcastChannel은 동일한 출처(프로토콜, 도메인, 포트)의 페이지에 대해서만 작동합니다.
- 데이터 유형: 객체, 배열, 심지어 ArrayBuffer와 같은 다양한 데이터 유형을 보낼 수 있습니다.

# 제한 사항과 고려 사항

강력하지만, BroadcastChannel에는 몇 가지 제한 사항이 있습니다:

- 동일 출처 제한: 서로 다른 도메인 간 통신에 사용할 수 없으므로 동일한 출처에서만 작동합니다.
- 영속성 없음: 메시지는 저장되지 않습니다. 메시지를 보낼 때 다른 탭이 듣고 있지 않으면 메시지가 손실됩니다.
- 전송 보장 없음: 메시지 전달이나 순서를 보장하는 내장 메커니즘이 없습니다.
- 성능: 일반적으로 효율적이지만 자주 대량의 데이터를 브로드캐스트하는 경우 성능에 영향을 줄 수 있습니다.

<div class="content-ad"></div>

# 실제 적용 사례

BroadcastChannel은 다음과 같은 시나리오에서 특히 유용합니다:

- 협업 도구: 문서 편집을 실시간으로 여러 탭 간에 동기화합니다.
- 다중창 애플리케이션: 금융 거래 플랫폼이나 분석 대시보드에서 상태를 일관되게 유지합니다.
- 채팅 애플리케이션: 여러 개의 오픈된 채팅 창 간에 메시지를 동기화합니다.
- 전자 상거래: 탭 간에 쇼핑 카트 내용을 업데이트합니다.

Google과 같은 기업은 실시간 협업을 위해 Google Docs와 같은 제품에서 비슷한 개념을 사용하지만, 아마도 특정 요구 사항에 맞는 더 복잡한 맞춤형 솔루션을 사용할 것입니다.

<div class="content-ad"></div>

이제 BroadcastChannel의 기본을 이해했으니, Angular 애플리케이션에서 그것을 구현해 보겠습니다.

# Angular에서 Cross-Tab 통신 구현하기

우리는 Angular과 BroadcastChannel API를 사용하여 여러 탭 간에 작동하는 간단한 채팅 애플리케이션을 만들 것입니다. 이를 통해 실시간 업데이트와 타이핑 인디케이터가 탭 간에 공유되는 것을 보여줄 것입니다.

# 프로젝트 설정하기

<div class="content-ad"></div>

먼저, 우리 Angular 프로젝트를 설정해 봅시다:

```js
ng new cross-tab-chat
cd cross-tab-chat
ng add @angular/material
```

다음으로, 라우팅을 설정해 봅시다. src/app/app-routes.ts 파일을 생성하세요:

```js
import { Routes } from '@angular/router';

export const routes: Routes = [
  {
    path: 'page-one',
    loadComponent: () =>
      import('./page-one.component').then((m) => m.PageOneComponent),
  },
  {
    path: 'page-two',
    loadComponent: () =>
      import('./page-two.component').then((m) => m.PageTwoComponent),
  },
];
```

<div class="content-ad"></div>

# 공유 서비스 생성하기

이제 브로드캐스트 채널 통신을 관리하는 서비스를 만들어보겠습니다. src/app/shared.service.ts 파일을 생성해주세요:

```js
import { Injectable, NgZone } from '@angular/core';
import { BehaviorSubject, Subject } from 'rxjs';
import { Message } from './message.interface';

@Injectable({
  providedIn: 'root',
})
export class SharedService {
  private messagesSubject = new BehaviorSubject<Message[]>([]);
  private typingSubjects: { [key: string]: Subject<boolean> } = {
    'page-one': new Subject<boolean>(),
    'page-two': new Subject<boolean>(),
  };

  messages$ = this.messagesSubject.asObservable();

  private bc = new BroadcastChannel('shared-service');

  constructor(private ngZone: NgZone) {
    this.bc.onmessage = (event) => {
      this.ngZone.run(() => {
        const { type, payload } = event.data;
        switch (type) {
          case 'addMessage':
            this.addMessageLocally(payload);
            break;
          case 'setTyping':
            this.setTypingLocally(payload.source, payload.isTyping);
            break;
          default:
            console.log('Unknown message type:', type);
            break;
        }
      });
    };
  }

  addMessage(message: Message) {
    this.addMessageLocally(message);
    this.bc.postMessage({ type: 'addMessage', payload: message });
  }

  private addMessageLocally(message: Message) {
    const messages = this.messagesSubject.getValue();
    messages.push(message);
    this.messagesSubject.next(messages);
  }

  setTyping(source: string, isTyping: boolean) {
    this.setTypingLocally(source, isTyping);
    this.bc.postMessage({ type: 'setTyping', payload: { source, isTyping } });
  }

  private setTypingLocally(source: string, isTyping: boolean) {
    this.typingSubjects[source].next(isTyping);
  }

  isTyping$(source: string) {
    return this.typingSubjects[source].asObservable();
  }
}
```

이 서비스를 자세히 살펴보겠습니다:

<div class="content-ad"></div>

- 채팅 메시지를 정의하기 위해 Message 인터페이스를 사용합니다.
- BehaviorSubject를 사용하여 메시지 목록을 관리하고 구성 요소가 변경 사항을 구독할 수 있도록 합니다.
- `shared-service`라는 BroadcastChannel을 만듭니다.
- 생성자에서는 NgZone을 사용하여 BroadcastChannel의 메시지 핸들러를 설정하여 Angular의 변경 감지가 이러한 외부 이벤트를 잡을 수 있도록 합니다.
- 메시지 추가 및 입력 상태 설정 메서드를 사용하여 로컬 상태를 업데이트하고 다른 탭으로 변경 사항을 브로드캐스트합니다.
- 각 페이지의 입력 상태를위한 별도의 Subjects를 사용합니다.

# 채팅 컴포넌트 개발

이제 채팅 컴포넌트를 만들어봅시다. 먼저 PageOneComponent부터 시작해봅시다. `src/app/page-one/page-one.component.ts`를 만들어주세요:

```js
import { CommonModule } from '@angular/common';
import { Component, OnDestroy, OnInit, NgZone } from '@angular/core';
import { FormsModule } from '@angular/forms';
import { MatCardModule } from '@angular/material/card';
import { MatFormFieldModule } from '@angular/material/form-field';
import { MatInputModule } from '@angular/material/input';
import { MatButtonModule } from '@angular/material/button';
import { MatIconModule } from '@angular/material/icon';
import { Message } from './message.interface';
import { SharedService } from './shared.service';
import { debounceTime, Subscription, Subject } from 'rxjs';

@Component({
  selector: 'app-page-one',
  template: `
    <mat-card class="chat-container">
      <mat-card-header>
        <mat-card-title>채팅 (페이지 1)</mat-card-title>
      </mat-card-header>
      <mat-card-content class="chat-messages">
        @for (message of messages; track message.time) {
        <div
          class="message"
          [ngClass]="{
            'user-message': message.source === 'page-one',
            'other-message': message.source === 'page-two'
          }"
        >
          <div class="message-content">{ message.text }</div>
          <div class="message-timestamp">
            { message.time | date : 'short' }
          </div>
        </div>
        } @if (isOtherTyping) {
        <div class="typing-indicator">페이지 2가 입력 중...</div>
        }
      </mat-card-content>
      <mat-card-actions class="chat-input">
        <mat-form-field appearance="outline" class="full-width">
          <input
            matInput
            [(ngModel)]="newMessage"
            placeholder="메시지를 입력하세요..."
            (keyup.enter)="sendMessage()"
            (ngModelChange)="onTyping()"
          />
        </mat-form-field>
        <button
          mat-fab
          color="primary"
          (click)="sendMessage()"
          [disabled]="!newMessage.trim()"
        >
          <mat-icon>send</mat-icon>
        </button>
      </mat-card-actions>
    </mat-card>
  `,
  standalone: true,
  imports: [
    CommonModule,
    FormsModule,
    MatCardModule,
    MatFormFieldModule,
    MatInputModule,
    MatButtonModule,
    MatIconModule,
  ],
})
export class PageOneComponent implements OnInit, OnDestroy {
  messages: Message[] = [];
  newMessage = '';
  isOtherTyping = false;
  private subscription$!: Subscription;
  private typingSubscription$!: Subscription;
  private typingSubject$ = new Subject<void>();

  constructor(private sharedService: SharedService, private ngZone: NgZone) {}

  ngOnInit() {
    this.subscription$ = this.sharedService.messages$.subscribe((messages) => {
      this.ngZone.run(() => {
        this.messages = messages;
      });
    });

    this.typingSubscription$ = this.sharedService
      .isTyping$('page-two')
      .subscribe((isTyping) => {
        this.ngZone.run(() => {
          this.isOtherTyping = isTyping;
        });
      });

    this.typingSubject$.pipe(debounceTime(300)).subscribe(() => {
      this.sharedService.setTyping(
        'page-one',
        this.newMessage.trim().length > 0
      );
    });
  }

  ngOnDestroy() {
    if (this.subscription$) {
      this.subscription$.unsubscribe();
    }
    if (this.typingSubscription$) {
      this.typingSubscription$.unsubscribe();
    }
  }

  sendMessage() {
    if (this.newMessage.trim()) {
      this.sharedService.addMessage({
        source: 'page-one',
        text: this.newMessage,
        time: new Date(),
      });
      this.newMessage = '';
      this.sharedService.setTyping('page-one', false);
    }
  }

  onTyping() {
    this.typingSubject$.next();
  }
}
```

<div class="content-ad"></div>

이 컴포넌트의 주요 부분을 살펴보겠습니다:

- 변경 감지 전략: 성능 향상을 위해 ChangeDetectionStrategy.OnPush를 사용하고 있습니다. 이는 Angular가 입력이 변경될 때 또는 명시적으로 변경 검사를 요청했을 때에만 변경 사항을 확인한다는 것을 의미합니다.
- ngOnInit에서의 구독:

- SharedService에서 messages$를 구독하여 로컬 메시지 배열을 업데이트합니다.
- 'page-two'의 타이핑 상태를 구독하여 다른 페이지가 입력 중임을 표시합니다.
- 너무 많은 타이핑 상태 업데이트를 피하기 위해 자체 타이핑에 대해 디바운스된 subject를 사용합니다.

- NgZone 및 ChangeDetectorRef:

<div class="content-ad"></div>

- Angular에서 BroadcastChannel 이벤트로 인해 발생하는 변경 사항이 Angular에 알려지도록 NgZone.run()을 사용합니다.
- OnPush 변경 감지를 사용할 때 뷰를 업데이트하기 위해 detectChanges()를 수동으로 호출합니다.

- ngOnDestroy에서 정리: 메모리 누수를 방지하기 위해 옵저버를 구독해제합니다.
- sendMessage 및 onTyping 메소드:

- sendMessage()는 새로운 메시지를 공유 서비스에 추가하고 타이핑 상태를 재설정합니다.
- onTyping()은 디바운스된 타이핑 subject를 트리거합니다.

이제 PageTwoComponent를 만들어 봅시다. PageOneComponent와 매우 유사하지만 몇 가지 주요 차이점이 있습니다. src/app/page-two/page-two.component.ts를 생성하세요.

<div class="content-ad"></div>

```js
import { CommonModule } from '@angular/common';
import {
  Component,
  OnDestroy,
  OnInit,
  NgZone,
  ChangeDetectionStrategy,
  ChangeDetectorRef,
} from '@angular/core';
import { FormsModule } from '@angular/forms';
import { MatCardModule } from '@angular/material/card';
import { MatFormFieldModule } from '@angular/material/form-field';
import { MatInputModule } from '@angular/material/input';
import { MatButtonModule } from '@angular/material/button';
import { MatIconModule } from '@angular/material/icon';
import { Message } from './message.interface';
import { SharedService } from './shared.service';
import { debounceTime, Subscription, Subject } from 'rxjs';

@Component({
  selector: 'app-page-two',
  template: `
    <mat-card class="chat-container">
      <mat-card-header>
        <mat-card-title>Chat (Page Two)</mat-card-title>
      </mat-card-header>
      <mat-card-content class="chat-messages">
        @for (message of messages; track message.time) {
        <div
          class="message"
          [ngClass]="{
            'user-message': message.source === 'page-two',
            'other-message': message.source === 'page-one'
          }"
        >
          <div class="message-content">{ message.text }</div>
          <div class="message-timestamp">
            { message.time | date : 'short' }
          </div>
        </div>
        } @if (isOtherTyping) {
        <div class="typing-indicator">Page One is typing...</div>
        }
      </mat-card-content>
      <mat-card-actions class="chat-input">
        <mat-form-field appearance="outline" class="full-width">
          <input
            matInput
            [(ngModel)]="newMessage"
            placeholder="Type a message..."
            (keyup.enter)="sendMessage()"
            (ngModelChange)="onTyping()"
          />
        </mat-form-field>
        <button
          mat-fab
          color="primary"
          (click)="sendMessage()"
          [disabled]="!newMessage.trim()"
        >
          <mat-icon>send</mat-icon>
        </button>
      </mat-card-actions>
    </mat-card>
  `,
  standalone: true,
  imports: [
    CommonModule,
    FormsModule,
    MatCardModule,
    MatFormFieldModule,
    MatInputModule,
    MatButtonModule,
    MatIconModule,
  ],
  changeDetection: ChangeDetectionStrategy.OnPush,
})
export class PageTwoComponent implements OnInit, OnDestroy {
  messages: Message[] = [];
  newMessage = '';
  isOtherTyping = false;
  private subscription$!: Subscription;
  private typingSubscription$!: Subscription;
  private typingSubject$ = new Subject<void>();

  constructor(
    private sharedService: SharedService,
    private ngZone: NgZone,
    private cdr: ChangeDetectorRef
  ) {}

  ngOnInit() {
    this.subscription$ = this.sharedService.messages$.subscribe((messages) => {
      this.ngZone.run(() => {
        this.messages = messages;
        this.cdr.detectChanges();
      });
    });

    this.typingSubscription$ = this.sharedService
      .isTyping$('page-one')
      .subscribe((isTyping) => {
        this.ngZone.run(() => {
          this.isOtherTyping = isTyping;
          this.cdr.detectChanges();
        });
      });

    this.typingSubject$.pipe(debounceTime(300)).subscribe(() => {
      this.sharedService.setTyping(
        'page-two',
        this.newMessage.trim().length > 0
      );
    });
  }

  ngOnDestroy() {
    if (this.subscription$) {
      this.subscription$.unsubscribe();
    }
    if (this.typingSubscription$) {
      this.typingSubscription$.unsubscribe();
    }
  }

  sendMessage() {
    if (this.newMessage.trim()) {
      this.sharedService.addMessage({
        source: 'page-two',
        text: this.newMessage,
        time: new Date(),
      });
      this.newMessage = '';
      this.sharedService.setTyping('page-two', false);
    }
  }

  onTyping() {
    this.typingSubject$.next();
  }
}
```

<div class="content-ad"></div>

이제 컴포넌트가 설정되었으니, 이 구현이 어떻게 작동하는지 자세히 살펴보겠습니다:

- 공유 상태: SharedService는 중앙 상태 관리 시스템으로 작동합니다. 메시지 목록과 페이지별 타이핑 상태를 보유합니다.
- BroadcastChannel: 크로스 탭 통신의 핵심입니다. 메시지가 추가되거나 타이핑 상태가 변경되면, 로컬 상태를 업데이트하는 것뿐만 아니라 BroadcastChannel을 통해 메시지를 전송합니다.
- 반응형 프로그래밍: RxJS 옵저버블을 적극적으로 활용합니다. messages$ 옵저버블은 컴포넌트가 메시지 목록의 변경에 반응할 수 있게 합니다. 타이핑 옵저버블은 실시간으로 타이핑 상태를 업데이트할 수 있습니다.
- 성능 최적화:

- 변경 감지 주기의 수를 줄이기 위해 OnPush 변경 감지를 사용합니다.
- 타이핑 상태는 너무 자주 업데이트되지 않도록 debounce를 사용합니다.
- 필요한 경우에만 수동으로 변경 감지를 트리거하며, 이는 NgZone 실행 내에서 수행합니다.

- 탭 간 일관성: 모든 상태 변경 사항이 모든 탭에 방송되고, 각 탭이 이러한 방송을 수신하므로, 모든 탭이 채팅 상태를 일관되게 볼 수 있도록 보장합니다.

<div class="content-ad"></div>

# 구현 테스트

이 구현을 테스트하려면:

- Angular 애플리케이션을 실행하세요 (ng serve).
- 두 개의 다른 브라우저 탭을 열고 하나는 /page-one으로 이동하고 다른 하나는 /page-two로 이동하세요.
- 하나의 탭에서 입력을 시작하고 다른 탭에서 입력 인디케이터를 관찰하세요.
- 두 탭에서 메시지를 보내고 실시간으로 두 페이지에 모두 나타나는 것을 확인하세요.

# 고려 사항과 잠재적인 개선 사항

<div class="content-ad"></div>

이 구현은 기본 채팅 애플리케이션에 대해 잘 작동하지만, 프로덕션 환경에서 몇 가지 고려해야 할 사항이 있습니다:

- 오류 처리: BroadcastChannel 통신을 위한 적절한 오류 처리를 추가해야 합니다.
- 메시지 지속성: 현재 모든 탭이 닫힐 때 메시지가 손실됩니다. 실제 응용 프로그램에서는 백엔드 API를 사용하여 메시지를 지속시키고 싶을 것입니다.
- 확장성: 많은 수의 메시지의 경우 페이지네이션 또는 가상 스크롤링을 구현하고 싶을 수 있습니다.
- 보안: 응용 프로그램의 모든 탭이 BroadcastChannel을 통해 메시지를 보낼 수 있다는 점을 기억해야 합니다. 더 복잡한 응용 프로그램에서는 메시지 유효성 검사 형태를 구현해야 할 수도 있습니다.
- 오프라인 지원: Service Workers 및 IndexedDB와 같은 기술을 사용하여 오프라인에서 작동하도록 이를 개선할 수 있습니다.

# 결론

BroadcastChannel API는 웹 응용 프로그램에서 크로스 탭 통신을 구현하는 강력하고 직관적인 방법을 제공합니다. Angular의 반응형 프로그래밍 모델과 강력한 변경 감지 시스템과 결합하여, 다중 탭 간 일관성을 유지하는 반응성이 뛰어난 실시간 애플리케이션을 만들 수 있습니다.

<div class="content-ad"></div>

이 접근 방식은 협업 문서 편집, 실시간 대시보드 또는 다중 창 애플리케이션과 같은 더 복잡한 시나리오로 확장할 수 있습니다. 핵심은 각 탭의 격리된 인스턴스로서가 아닌 모든 열린 인스턴스를 가로질러 확장되는 일관된 총체로서 당신의 애플리케이션을 생각하는 것입니다.

이러한 기술을 활용하여 더 견고하고 반응성이 뛰어나며 사용자 친화적인 웹 애플리케이션을 만들 수 있습니다. 이를 통해 현대 사용자들의 다중 탭 및 창을 효과적으로 활용하는 요구 사항을 더 잘 충족시킬 수 있습니다.

기억하세요, 웹 플랫폼은 지속적으로 발전하고 있으며 교차 탭 및 창 통신을 위한 새로운 API 및 기술이 나올 수 있습니다. 항상 최신 웹 표준과 최상의 실천 방법을 따르고 애플리케이션에서 가장 효율적이고 효과적인 방법을 사용하도록 업데이트되어 있는지 확인하세요.

이 구현의 완전한 작동 예제는 GitHub 저장소에서 확인할 수 있습니다:

<div class="content-ad"></div>

https://github.com/mollaie/angular-broadcasting

도움이 되었다면 저장소를 복제하거나 포크 또는 별표를 눌러 주세요!