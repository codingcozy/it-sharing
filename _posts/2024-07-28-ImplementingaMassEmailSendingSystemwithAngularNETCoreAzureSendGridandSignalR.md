---
title: "Angular, NET Core, Azure, SendGrid, SignalR를 활용한 대량 이메일 발송 시스템 구현하는 방법"
description: ""
coverImage: "/assets/img/2024-07-28-ImplementingaMassEmailSendingSystemwithAngularNETCoreAzureSendGridandSignalR_0.png"
date: 2024-07-28 13:56
ogImage: 
  url: /assets/img/2024-07-28-ImplementingaMassEmailSendingSystemwithAngularNETCoreAzureSendGridandSignalR_0.png
tag: Tech
originalTitle: "Implementing a Mass Email Sending System with Angular, NET Core, Azure, SendGrid, and SignalR"
link: "https://medium.com/@balramchavan/implementing-a-mass-email-sending-system-with-angular-net-core-azure-sendgrid-and-signalr-c4fe96877aa4"
---


저희 이전 블로그에서는 대량 이메일 발송 시스템의 디자인 고려 사항 및 작업 흐름에 대해 논의했습니다. 이제 전체 프론트엔드에 Angular를 사용하여 실제 구현에 대해 살펴보겠습니다. 백엔드 API에는 .NET Core를 사용하고, 대기 및 처리에는 Azure 클라우드 서비스를 사용하며, 이메일 발송에는 SendGrid를 사용하고 실시간 푸시 알림에는 SignalR을 사용합니다. 또한 이메일 발송 전 임의의 지연 추가, 실패한 이메일을 위한 재시도 논리, 데이터베이스 상태 업데이트와 같은 모범 사례도 다룰 것입니다.

![이미지](/assets/img/2024-07-28-ImplementingaMassEmailSendingSystemwithAngularNETCoreAzureSendGridandSignalR_0.png)

## 1. Angular를 사용한 프론트엔드 설정

먼저, 사용자가 이메일 캠페인을 생성하고 관리할 수 있는 Angular 프론트엔드를 설정해 봅시다.

<div class="content-ad"></div>

캠페인을 생성하고 관리할 수 있는 컴포넌트를 작성해보세요.

```javascript
import { Component } from '@angular/core';
import { CampaignService } from './campaign.service';

@Component({
  selector: 'app-campaign',
  template: `
    <div>
      <h1>캠페인 생성</h1>
      <form (submit)="createCampaign()">
        <!-- form… -->


```