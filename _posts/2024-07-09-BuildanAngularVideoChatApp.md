---
title: "Angular로 비디오 채팅 앱 만드는 방법"
description: ""
coverImage: "/assets/img/2024-07-09-BuildanAngularVideoChatApp_0.png"
date: 2024-07-09 12:49
ogImage:
  url: /assets/img/2024-07-09-BuildanAngularVideoChatApp_0.png
tag: Tech
originalTitle: "Build an Angular Video Chat App"
link: "https://dev.to/amosgyamfi/build-an-angular-video-chat-app-3g9c"
---

인기있는 채팅 메시징 앱인 WhatsApp과 Telegram은 실시간 비디오 통화를 제공하며, Zoom과 Google Meet과 같은 비디오 회의 앱은 회의 중 그룹 채팅을 제공합니다. 비디오 통화를 제공하는 채팅 앱과 채팅을 지원하는 비디오 회의 앱은 두 가지 유사한 커뮤니케이션 요구를 중점으로 삼습니다.

이 기사에서는 Angular 채팅 앱에 통합 비디오 회의 지원을 더하는 방법을 안내합니다. 최종 프로젝트를 사용하여 채팅을 보조 기능으로 하는 비디오 회의를 우선하도록 사용자 정의할 수 있습니다.

## 사전 준비 사항

이 자습서의 샘플 프로젝트의 두 가지 주요 기능은 채팅 메시징과 비디오 통화입니다. 메시징 기능에 대해 Stream Chat Angular SDK를 사용할 것이며, 비디오 통화 지원은 위 SDK의 동반 JavaScript Video SDK를 사용할 것입니다. 이 두 SDK를 앱 내에서 통합함으로써 개발자는 사람들이 채팅 메시지를 보내고 받을 수 있고, 라이브 스트리밍, 비디오 통화, 라이브 오디오 룸 대화에 참여하고 나갈 수 있는 체험을 빌드할 수 있습니다.

<div class="content-ad"></div>

당신이 좋아하는 IDE로 이 튜토리얼의 프로젝트를 재현할 수 있어요. 가급적이면 VS Code를 이용해보세요.

## 소스 코드 실행

![image](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fkj6azfygddj1kv1orv3i.gif)

예제 프로젝트를 테스트하기 위해 이 저장소의 앱을 클론하고 로컬 버전을 이용하거나 GitHub Codespace를 생성해보세요.

<div class="content-ad"></div>

## 채팅 기능으로 시작

![이미지](/assets/img/2024-07-09-BuildanAngularVideoChatApp_0.png)

이 섹션에서는 위 이미지와 비슷한 룩 앤 필을 가진 Angular 채팅 UI를 구축할 것입니다. 주요 채팅 기능에는 즉각 메시지, 미디어 첨부, 이모티콘, 스레드 및 답변, 그리고 메시지 상호작용이 포함됩니다. 새로운 Angular 프로젝트를 시작하고 다음 단계를 따라 시작해보세요.

### 새 Angular 프로젝트 생성

<div class="content-ad"></div>

새 Angular 프로젝트를 만들려면 Node.js를 설치해야 합니다. 터미널이나 선호하는 명령줄 도구를 열고 다음 명령어로 기기의 Node 버전을 확인해보세요:

node -v

Node가 설치되어 있지 않은 경우 위 링크를 클릭하여 최신 안정(LTS) 버전을 설치하세요.

다음 명령어를 실행하여 Angular CLI를 기기 전역으로 설치하세요.

<div class="content-ad"></div>

npm install -g @angular/cli

참고: macOS에서는 위 명령어 앞에 sudo를 추가해야 할 수 있습니다.

sudo npm install -g @angular/cli

Angular CLI를 설치했으므로 이제 다음을 사용하여 새 프로젝트를 만들 수 있습니다:

<div class="content-ad"></div>

ng new video-chat-angular --routing false --style css --ssr false

프로젝트 이름은 video-chat-angular입니다. 스타일링은 CSS를 사용하며 라우팅을 끕니다. 프로젝트에 원하는 어떤 이름을 사용할 수 있습니다.

IDE에서 앱을 열고 아래와 같이 필요한 종속성을 설치해주세요.

![Building an Angular Video Chat App](/assets/img/2024-07-09-BuildanAngularVideoChatApp_1.png)

<div class="content-ad"></div>

위의 예시는 VS Code에서 열린 앱을 보여줍니다. VS Code에서 통합 터미널을 열고 다음과 같이 Stream Chat Angular을 종속 항목으로 설치하세요.

npm install stream-chat-angular stream-chat @ngx-translate/core

- Stream-chat-angular: Stream Chat Angular은 재사용 가능한 Angular 채팅 컴포넌트로 구성되어 있습니다.
- stream-chat: UI 컴포넌트 없는 핵심 Angular SDK입니다. 이것을 사용하여 완전히 사용자 정의된 채팅 메시징 경험을 구축할 수 있습니다.
- ngx-translate/core: Angular 프로젝트용 국제화 (i18n) 라이브러리입니다.

## Angular 채팅 SDK 구성

<div class="content-ad"></div>

Angular SDK를 설정하기 전에는 프로젝트의 src/app/app.config.ts 및 src/app/app.component.ts에 필요한 임포트를 추가해야 합니다.

src/app/app.config.ts 파일을 열고 다음을 appConfig 안에 넣어 TranslateModule을 임포트하세요.

providers: [importProvidersFrom(TranslateModule.forRoot())],

그다음 src/app/app.component.ts 파일을 열어 @Component 데코레이터 안에 TranslateModule, StreamAutocompleteTextareaModule, StreamChatModule을 임포트하세요.

<div class="content-ad"></div>

이 파일의 수정된 내용은 다음과 같아야 합니다:

```js
import { Component, OnInit } from '@angular/core';
import { TranslateModule } from '@ngx-translate/core';
import {
  ChatClientService,
  ChannelService,
  StreamI18nService,
  StreamAutocompleteTextareaModule,
  StreamChatModule,
} from 'stream-chat-angular';

// 비디오 imports
@Component({
  selector: 'app-root',
  standalone: true,
  imports: [TranslateModule, StreamAutocompleteTextareaModule, StreamChatModule],
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss'],
})
export class AppComponent implements OnInit {
  constructor(
    private chatService: ChatClientService,
    private channelService: ChannelService,
    private streamI18nService: StreamI18nService,
  ) {
    const apiKey = 'CREATE_WITH_YOUR_STREAM_APIKEY';
    const userId = 'CREATE_WITH_YOUR_STREAM_USERID';
    const userToken = 'CREATE_WITH_TOKEN';
    this.chatService.init(apiKey, userId, userToken);
    this.streamI18nService.setTranslation();
  }

  async ngOnInit() {
    const channel = this.chatService.chatClient.channel('messaging', 'talking-about-angular', {
      // 필요한 만큼 사용자 지정 필드 추가
      image: 'https://upload.wikimedia.org/wikipedia/commons/thumb/c/cf/Angular_full_color_logo.svg/2048px-Angular_full_color_logo.svg.png',
      name: 'Talking about Angular',
    });
    await channel.create();
    this.channelService.init({
      type: 'messaging',
      id: { $eq: 'talking-about-angular' },
    });
  }
}
```

위의 코드에서 정상 사용자는 채팅 기능에 액세스하기 위해 SDK의 백엔드 인프라에 연결되어야 합니다. 우리는 Angular Chat Tutorial에서 하드 코딩된 사용자 자격 증명을 사용하고 있습니다. 이 SDK를 사용하여 프로덕션 앱에 채팅 기능을 구현하려면 대시보드 계정에 등록해야 합니다. Stream 대시보드에서 API 키를 찾아 User JWT Generator를 사용하여 토큰을 생성해야 합니다. 사용자 자격 증명을 작성한 후, 새 채널을 생성하고 앱을 실행할 때 팝업시킬 조건을 정의합니다.

<div class="content-ad"></div>

위의 구성에 추가로, Stream Chat SDK는 TypeScript의 "Allow Synthetic Default Imports" 개념을 사용하여 기본 가져 오기를 더 효율적으로 작성합니다. 이를 활성화하려면 프로젝트 루트 폴더의 tsconfig.json에 다음을 추가하십시오.

```json
"allowSyntheticDefaultImports": true
```

## 채팅 UI 구조 및 스타일 구성

생성된 Angular 프로젝트에서 UI 구조는 src/app/app.component.html에 정의되며, 스타일은 src/app/styles.css에 선언됩니다. 앱이 시작될 때 SDK의 채팅 구성요소 인 Channel, Message List, Message Input 및 Thread를 로드하려면 src/app/app.component.html의 HTML 콘텐츠를 다음과 같이 대체하십시오.

<div class="content-ad"></div>

```js
<div id="root">
  <stream-channel-list></stream-channel-list>
  <stream-channel>
    <stream-channel-header></stream-channel-header>
    <stream-message-list></stream-message-list>
    <stream-notification-list></stream-notification-list>
    <stream-message-input></stream-message-input>
    <stream-thread name="thread">
      <stream-message-list mode="thread"></stream-message-list>
      <stream-message-input mode="thread"></stream-message-input>
    </stream-thread>
  </stream-channel>
</div>
```

다음과 같은 CSS 스타일을 `src/app/styles.css`에 정의해 봅시다. 여기에서 지정하는 스타일에 비디오 콜 통합 스타일은 포함되어 있지 않습니다. 나중 섹션에서 해당 파일을 수정하여 이에 대한 내용을 다룰 것입니다.

```js
@import 'stream-chat-angular/src/assets/styles/v2/css/index.css';

html {
    height: 100%;
}

body {
    margin: 0;
    height: 100%;
}

#root {
    display: flex;
    height: 100%;

    stream-channel-list {
        width: 30%;
    }

    stream-channel {
        width: 100%;
    }

    stream-thread {
        width: 45%;
    }
}
```

## 채팅 기능을 테스트해 보세요.

<div class="content-ad"></div>

아래 Markdown 형식으로 표 태그를 변경해 주세요.

<img src="/assets/img/2024-07-09-BuildanAngularVideoChatApp_2.png" />

VS Code에서 개발 서버를 시작하여 채팅 버전을 실행하고 테스트하려면 통합 터미널을 실행하고 다음을 입력하세요:

ng serve or npm start

서버가 성공적으로 실행되면 아래와 유사한 화면이 표시됩니다.

<div class="content-ad"></div>

<img src="/assets/img/2024-07-09-BuildanAngularVideoChatApp_3.png" />

원하는 브라우저에서 localhost http://localhost:4200/ 링크를 열어 채팅 인터페이스를 테스트해보세요.

이 튜토리얼의 나머지 부분에서는, 채팅 창의 메시지 목록 맨 위 오른쪽에 있는 버튼을 클릭하여 비디오 통화 기능을 구현해보겠습니다.

## 비디오 통화 기능 통합

<div class="content-ad"></div>

![image](/assets/img/2024-07-09-BuildanAngularVideoChatApp_4.png)

채팅 UI에서 사용자가 비디오 통화를 시작할 수 있게 하려면, Stream Video의 low-level JavaScript 클라이언트를 사용할 것입니다. 이 클라이언트는 다른 SDK나 플랫폼과 함께 구현할 수 있습니다.

최종 프로젝트에서 GitHub Codespace를 생성하여 앱의 비디오 통화 기능을 테스트할 수 있습니다. Codespace를 사용하면 GitHub 저장소에서 복제하고 설치하지 않고도 브라우저에서 앱을 실행하고 테스트할 수 있습니다.

![image](/assets/img/2024-07-09-BuildanAngularVideoChatApp_5.png)

<div class="content-ad"></div>

## JavaScript 비디오 SDK 설치 및 구성하기

비디오 SDK를 설치하려면, VS Code에서 프로젝트의 루트 폴더로 이동하여 새 터미널 인스턴스를 열고 다음을 실행하세요.

npm i @stream-io/video-client

그런 다음 개발 서버를 다음과 같이 시작하세요:

<div class="content-ad"></div>

npm start

### 비디오 통화 서비스 만들기

프로젝트의 앱 폴더인 /video-chat-angular/src/app 안에 있는지 확인하세요. 그런 다음, 아래의 Angular CLI 명령어를 사용하여 통화 서비스를 생성해보세요.

```bash
ng generate service calling
```

<div class="content-ad"></div>

위 작업을 실행한 후 앱 디렉토리를 확장하면 두 개의 추가 파일이 생성된 것을 발견할 겁니다.

- calling.service.spec.ts
- calling.service.ts

참고: 위 링크에는 원래 생성된 콘텐츠가 포함되어 있지 않습니다. 저희 콜링 서비스를 제공하기 위해 수정되었습니다.

calling.service.ts를 열어서 파일 내용을 다음과 같이 대체해 보겣습니다.

<div class="content-ad"></div>

위의 샘플 코드로 생성한 서비스는 비디오 통화를 관리하는 데 사용됩니다. 비디오 및 오디오가 활성화된 통화를 만들고 참여하며 통화를 종료할 수 있습니다. 상태 변경을 반응적으로 관리하기 위해 Angular 시그널을 활용한 반응형 프로그래밍 패턴을 사용합니다. Angular 시그널을 활용한 비디오 통화 앱을 만드는 방법을 자세히 알고 싶다면 YouTube 튜토리얼을 확인해보세요.

먼저, JavaScript 비디오 SDK를 가져와서 비디오 클라이언트 인스턴스를 생성하고 API 키 및 토큰으로 초기화합니다. 사용자 유형과 프로젝트에 사용할 사용자 토큰을 생성하는 방법에 대한 자세한 내용은 문서의 "클라이언트 및 인증" 섹션을 참조하시기 바랍니다.

마지막으로, call.join 메서드를 사용하여 통화를 만들고 참여합니다. call.join 메서드는 실시간 오디오 및 비디오 전송을 가능하게 합니다.

<div class="content-ad"></div>

다음으로, calling.service.spec.ts 파일을 열고 원래 코드를 아래 예시 코드로 교체해주세요.

```js
import { TestBed } from "@angular/core/testing";

import { CallingService } from "./calling.service";

describe("CallingService", () => {
  let service: CallingService;

  beforeEach(() => {
    TestBed.configureTestingModule({});
    service = TestBed.inject(CallingService);
  });

  it("should be created", () => {
    expect(service).toBeTruthy();
  });
});
```

위의 예시 코드는 CallingService를 위한 기본 단위 테스트 구성을 수행합니다. 호출 서비스가 생성되거나 성공적으로 초기화될 수 있는지 확인합니다.

## 오디오/비디오 통화 관리 방법

<div class="content-ad"></div>

이 섹션에서는 마이크와 카메라를 켜고 끄는 기능, 통화 수락 및 종료, 세션 ID로 참가자 식별과 같은 오디오 및 비디오 통화 관련 기능을 관리하는 구성 요소를 정의해야 합니다. 앱의 src/app 디렉터리로 이동하여 call이라는 새 폴더를 만듭니다.

![이미지](/assets/img/2024-07-09-BuildanAngularVideoChatApp_6.png)

call 폴더 내에 다음 파일을 추가하고 GitHub 프로젝트 링크에서 해당 내용을 복사하여 채워 넣습니다.

- call.component.css
- call.component.html
- call.component.spec.ts
- call.component.ts

<div class="content-ad"></div>

위의 파일은 오디오/비디오 통화 중 참가자의 상태를 관리하고 마이크 및 카메라 상태를 제어하는 데 필요한 UI 및 로직을 제공합니다.

## 통화 참가자 표시 및 관리

이 섹션에서는 비디오 통화 중 참가자의 비디오를 표시하고 오디오를 재생하기 위한 필수 바인딩을 설정하는 컴포넌트를 정의해야 합니다. 다음 디렉토리 src/app/participant을 생성해 보겠습니다.

![링크 텍스트](/assets/img/2024-07-09-BuildanAngularVideoChatApp_7.png)

<div class="content-ad"></div>

위 이미지의 참가자 파일을 호출 디렉토리에 추가하고 각 내용을 GitHub 프로젝트의 링크에서 복사하여 교체하세요.

- participant.component.css
- participant.component.html
- participant.component.spec.ts
- participant.component.ts

## 채팅 UI에 시작 전화 버튼 추가하기

![Start Call Button](/assets/img/2024-07-09-BuildanAngularVideoChatApp_8.png)

<div class="content-ad"></div>

지금까지 채팅 UI의 메시지 목록 헤더는 아바타와 채팅 채널 제목으로 구성되어 있습니다. 메시지 목록의 헤더를 수정하여 시작 전화 버튼을 추가해보겠습니다. 또한 시작 버튼에 대한 추가적인 스타일을 구현할 것입니다.

프로젝트 루트 폴더에 있는 style.css 파일을 열어서 아래의 코드 스니펫을 @import 아래에 추가해주세요.

```js
:root {
  --custom-blue: hsl(218, 100%, 50%);
  --custom-blue-hover: hsl(218, 100%, 70%);

  --custom-red: hsl(0, 80%, 60%);
  --custom-red-hover: hsl(0, 80%, 50%);
}
```

수정된 style.css의 내용은 아래와 같습니다.

<div class="content-ad"></div>

```js
@import "stream-chat-angular/src/assets/styles/v2/css/index.css";

:root {
  --custom-blue: hsl(218, 100%, 50%);
  --custom-blue-hover: hsl(218, 100%, 70%);

  --custom-red: hsl(0, 80%, 60%);
  --custom-red-hover: hsl(0, 80%, 50%);
}

html {
  height: 100%;
}

body {
  margin: 0;
  height: 100%;
}

#root {
  display: flex;
  height: 100%;

  stream-channel-list {
    width: 30%;
  }

  stream-channel {
    width: 100%;
  }

  stream-thread {
    width: 45%;
  }
}
```

Angular 프로젝트 생성 시 함께 제공되는 빈 CSS 파일인 app.component.css를 찾아주세요. 다음 스타일 스니펫으로 해당 파일을 작성해주세요.

```js
.header-container {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 1rem;
}

button {
  background: #005fff;
  color: white;
  padding: 0.5rem 1rem;
  border-radius: 0.25rem;
  border: none;
  cursor: pointer;
}
```

이제 채팅 UI의 HTML 구조에 콜 서비스를 추가해보겠습니다.

<div class="content-ad"></div>

```js
<ng-container>
  <ng-container *ngIf="callingService.callId()">
    <app-call [call]="callingService.call()!"></app-call>
  </ng-container>
</ng-container>
```

위의 코드 스니펫을 앱 디렉토리의 app.component.html 파일에 추가해야 합니다.

```js
<div id="root">
 <stream-channel-list></stream-channel-list>
 <ng-container *ngIf="!callingService.callId()">
 <stream-channel>
 <div class="header-container">
 <stream-channel-header></stream-channel-header>
 <button (click)="startCall()">Start call</button>
 </div>
 <stream-message-list></stream-message-list>
 <stream-notification-list></stream-notification-list>
 <stream-message-input></stream-message-input>
 <stream-thread name="thread">
 <stream-message-list mode="thread"></stream-message-list>
 <stream-message-input mode="thread"></stream-message-input>
 </stream-thread>
 </stream-channel>
 </ng-container>
 <ng-container *ngIf="callingService.callId()">
 <app-call [call]="callingService.call()!"></app-call>
 </ng-container>
</div>
```

마지막으로, 본문 채팅 섹션 중 하나에 추가된 app.component.ts 파일을 수정하여 CallingService 및 CallComponent를 import하고 startCall 메서드를 추가해야 합니다. app.component.ts에서 수정된 샘플 코드는 다음과 같아야 합니다.

<div class="content-ad"></div>

```js
import { Component, OnInit } from '@angular/core';
import { TranslateModule } from '@ngx-translate/core';
import {
  ChatClientService,
  ChannelService,
  StreamI18nService,
  StreamAutocompleteTextareaModule,
  StreamChatModule,
} from 'stream-chat-angular';
import { CallingService } from './calling.service';
import { CommonModule } from '@angular/common';

// Video imports
import { CallComponent } from './call/call.component';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [
    TranslateModule,
    StreamAutocompleteTextareaModule,
    StreamChatModule,
    CommonModule,
    CallComponent,
  ],
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss'],
})
export class AppComponent implements OnInit {
  callingService: CallingService;

  constructor(
    private chatService: ChatClientService,
    private channelService: ChannelService,
    private streamI18nService: StreamI18nService,
    callingService: CallingService
  ) {
    this.callingService = callingService;
    const apiKey = '5cs7r9gv7sny';
    const userId = 'floral-meadow-5';
    const userName = 'Floral Meadow';
    const userToken =
      'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyX2lkIjoiZmxvcmFsLW1lYWRvdy01In0.uASF95J3UqfS8zMtza0opjhtGk7r7xW0SJACsHXF0Do';
    this.chatService.init(apiKey, userId, userToken);
    this.streamI18nService.setTranslation();
  }

  async ngOnInit() {
    const channel = this.chatService.chatClient.channel(
      'messaging',
      'talking-about-angular',
      {
        // add as many custom fields as you'd like
        image:
          'https://upload.wikimedia.org/wikipedia/commons/thumb/c/cf/Angular_full_color_logo.svg/2048px-Angular_full_color_logo.svg.png',
        name: 'Talking about Angular',
      }
    );
    await channel.create();
    this.channelService.init({
      type: 'messaging',
      id: { $eq: 'talking-about-angular' },
    });
  }

  startCall() {
    const channelId = this.channelService.activeChannel?.id;
    this.callingService.setCallId(channelId);
  }
}
```

## 통합된 채팅 및 비디오 통화 기능 테스트

<img src="https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fck4u4rthp0twi0dmbo12.gif" />

개발 서버를 다시 시작하려면:

<div class="content-ad"></div>

npm start

방금 구현한 시작 호출 버튼은 메시지 목록 헤더의 오른쪽 상단에 나타날 것입니다. 버튼을 클릭하면 로컬 참가자의 실시간 비디오가 표시됩니다. 실시간 비디오 또는 활성 통화 화면에서는 카메라 전환, 마이크 음소거, 통화 종료 등과 같은 표준 통화 작업을 수행할 수 있는 버튼이 있습니다.

## 결론

축하합니다! 👏 Angular 비디오 채팅 애플리케이션을 실시간으로 완전히 구축하기 위한 모든 단계를 따라오셨습니다. 이 앱을 사용하면 사용자가 풍부한 텍스트 채팅을 보내고 받을 수 있으며 미디어를 첨부하고 상대방과 얼굴을 마주한 비디오 통화를 할 수 있습니다. 관련 링크를 탐색하여 Angular 채팅 및 JavaScript 비디오 SDK의 고급 기능과 기능에 대해 자세히 알아보세요.
