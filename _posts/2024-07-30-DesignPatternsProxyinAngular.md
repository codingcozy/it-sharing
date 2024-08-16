---
title: "Angular에서 Proxy 디자인 패턴 사용 방법"
description: ""
coverImage: "/assets/img/2024-07-30-DesignPatternsProxyinAngular_0.png"
date: 2024-07-30 17:02
ogImage: 
  url: /assets/img/2024-07-30-DesignPatternsProxyinAngular_0.png
tag: Tech
originalTitle: "Design Patterns  Proxy in Angular"
link: "https://medium.com/design-patterns-proxy-in-angular/design-patterns-proxy-in-angular-8661c1049252"
isUpdated: true
updatedAt: 1723816478198
---



마크다운 형식으로 표 태그를 변경합니다.

![Proxy Pattern](/assets/img/2024-07-30-DesignPatternsProxyinAngular_0.png)

구조 패턴은 클래스와 객체를 더 큰 구조로 결합하는 방법을 설명합니다.

구조 패턴은 다음과 같습니다:
1. 어댑터
2. 컴포지트
3. 프록시
4. 플라이웨이트
5. 퍼사드
6. 브릿지
7. 데코레이터

프록시 디자인 패턴(중재자로도 알려짐)은 객체를 생성하는 데 시간이 많이 소요되고 리소스를 많이 사용하는 경우에 유용합니다. 이 패턴을 사용하면 해당 객체를 실제로 필요한 시점까지 생성을 연기할 수 있습니다.

<div class="content-ad"></div>

프록시 디자인 패턴은 다음 상황에서 사용될 수 있습니다:

- 성능 최적화: 파일이 오래 걸리는 경우
- 원격 객체: 객체가 다른 주소 공간(예: 서버)에 있고 네트워크를 통해로드하는 데 오랜 시간이 걸리거나 네트워크 혼잡 기간에 특히 시간이 오래 걸리는 경우에 사용할 수 있습니다. 프록시는 클라이언트와 원격 객체 간의 통신에서 중계자 역할을 할 수 있습니다.
- 접근 제어: 객체에 대한 액세스 권한을 제한하고자 하는 경우, 프록시 디자인 패턴은 사용자의 권한을 확인한 후 요청을 대상 객체로 전달할 수 있습니다.
- 자원 관리: 네트워크 연결, 파일, 모듈 또는 기능과 같은 자원을 관리하려는 경우, 프록시 디자인 패턴을 사용하여 이러한 자원에 대한 액세스를 감시하고 제어할 수 있습니다.
- 네트워크 인터페이스: 로컬 객체에 네트워크 인터페이스를 제공하고자 할 때, 프록시 디자인 패턴을 사용하여 메서드 호출을 네트워크 메시지로 변환하거나 그 반대로 할 수 있습니다.

전체 데모를 보고 싶다면 데모 프로젝트를 확인해보세요.

프록시 디자인 패턴은 다양한 방식으로 설명할 수 있습니다. 여기서는 텍스트를 JSON으로 매핑하는 컨버터 예시를 들어 설명하겠습니다. 이 예시에서는 추가로 두 라이브러리를 설치해야 합니다.

<div class="content-ad"></div>

그 다음에는 다음 명령어를 사용하여 서버를 실행해야 합니다:

<div class="content-ad"></div>

```js
node server.js
```

server.js 스크립트가 있는 디렉토리에서 실행해주세요.

서버로부터 데이터를 가져오는 메서드가 포함된 서비스는 다음과 같이 제공됩니다:

```js
import { HttpClient } from '@angular/common/http';
import { Injectable } from '@angular/core';
import { Observable } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class DataService {

  private apiUrl = 'http://localhost:3000/text-data';

  constructor(private http: HttpClient) {}

  getTextData(): Observable<string> {
    return this.http.get(this.apiUrl, { responseType: 'text' });
  }
}
```

<div class="content-ad"></div>

여기에 Proxy 디자인 패턴의 전체 구현이 있습니다:

```js
import { Component } from '@angular/core';
import { DataService } from '../../common/service/data.service';
import { map, take } from 'rxjs';


export abstract class JsonParser {
  abstract parse(text: string): any;
}
export interface ConvertStringToObject {
  description: string;
}

@Component({
  selector: 'app-proxy',
  template:`
  <h3>Proxy 디자인 패턴은 중개자로도 알려져 있습니다.
    두 가지 추상화 사이의 중개 기능을 합니다.
  </h3>
  <div>{ (textData$| async)?.description }</div>`,
  styleUrl: './proxy.component.scss',
  providers: [DataService]
})
export class ProxyComponent extends JsonParser {
  textData$ = this.dataService.getTextData()
    .pipe(
      take(1),
      map(val => this.parse(val))
    );

  constructor(private dataService: DataService) { super() }

  parse(value: string): ConvertStringToObject {
    if (typeof value === 'string') {
      return { description: value };
    }
    return value;
  }
}
```

핵심은 문자열을 ConvertStringToObject 객체로 변환하는 변환 클래스인 JsonParser 추상 클래스입니다. 물론, 객체는 더 복잡할 수 있습니다. ProxyComponent 클래스는 JsonParser를 상속하며 parse 메소드의 전체 구현이 이미 포함되어 있습니다. 이 메소드에서 텍스트가 지정된 인터페이스의 객체로 변환됩니다.

## 요약:

<div class="content-ad"></div>

프록시 디자인 패턴의 이념을 소개하고 싶어요. 이 패턴은 하나의 추상화와 다른 추상화 사이에서 중재자 역할을 합니다. 여기서 하나의 추상화는 큰 파일, 잘못된 데이터 형식, 사용자 권한 확인 등의 여러 가지 것들일 수 있어요. 이 패턴의 목적은 사용자 상호작용을 지원하는 기능을 수행하는 것입니다.

자료:

- 자바. Wzorce 프로젝토베. 번역: Piotr Badarycz. 원제: 자바 디자인 패턴. 사용자 지침서. 저자: James William Cooper

제가 다른 Angular와 관련된 디자인 패턴에 관한 기사들도 있어요:

<div class="content-ad"></div>

구조적 패턴:
- 디자인 패턴 - 앵귤러에서 어댑터
- 디자인 패턴 - 앵귤러에서 컴포지트

창조적 패턴:
- 디자인 패턴 - 앵귤러에서 빌더

저는 영어를 잘하지 못해요. 영어 실력 향상을 위해 글을 쓰고 있어요.