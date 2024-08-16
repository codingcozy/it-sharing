---
title: "Tomcat 커넥터 설계 방식 핵심 개념과 원리 완벽 정리"
description: ""
coverImage: "/assets/img/2024-07-19-HowIstheTomcatConnectorDesigned_0.png"
date: 2024-07-19 13:32
ogImage: 
  url: /assets/img/2024-07-19-HowIstheTomcatConnectorDesigned_0.png
tag: Tech
originalTitle: "How Is the Tomcat Connector Designed"
link: "https://medium.com/@cstoppgmr/how-is-the-tomcat-connector-designed-9589e2a58da6"
isUpdated: true
updatedAt: 1723812355907
---





![이미지](/assets/img/2024-07-19-HowIstheTomcatConnectorDesigned_0.png)

오늘은 Tomcat의 설계 철학을 단계별로 분석하여, 설계자들이 이러한 시스템을 만드는 질문에 어떻게 대답했는지 이해해 보려고 합니다. 이 분석을 통해 우리는 Tomcat의 전체 아키텍처, 거시적 관점에서 복잡한 시스템을 설계하는 방법, 그리고 최상위 모듈과 그들의 관계를 설계하는 방법에 대해 배울 수 있을 것입니다.

# Tomcat의 전체 아키텍처

시스템을 설계하기 위해서는 먼저 요구 사항을 이해해야 합니다. Tomcat은 두 가지 핵심 기능을 달성하기 위해 목표를 두고 있습니다:


<div class="content-ad"></div>

- 소켓 연결을 처리하며, 네트워크 바이트 스트림을 요청(Request)과 응답(Response) 객체로 변환하는 역할을 맡고 있습니다.
- 서블릿을 로딩하고 관리하며, 특히 요청(Request) 객체를 처리합니다.

그래서 Tomcat은 이러한 작업을 각각 처리하기 위해 커넥터(Connector)와 컨테이너(Container)라는 두 가지 핵심 구성 요소를 설계했습니다. 커넥터는 외부 통신을 담당하고, 컨테이너는 내부 처리를 다룹니다.

따라서 커넥터와 컨테이너는 Tomcat 구조에서 가장 중요한 부분으로 간주될 수 있습니다. 오늘은 커넥터가 어떻게 설계되었는지 분석해보겠으며, 다음 기사에서는 컨테이너의 설계를 소개하겠습니다.

커넥터에 대해 자세히 알아보기 전에, Tomcat이 지원하는 다양한 I/O 모델과 응용 계층 프로토콜에 대한 기초를 다지겠습니다.

<div class="content-ad"></div>

## Tomcat에서 지원하는 I/O 모델

- NIO: Java NIO 라이브러리를 사용하여 구현된 Non-blocking I/O.
- NIO2: JDK 7에서 제공되는 최신 NIO2 라이브러리를 사용하여 구현된 비동기 I/O.
- APR: C/C++로 작성된 네이티브 라이브러리인 Apache Portable Runtime 라이브러리를 사용하여 구현됨.

## Tomcat에서 지원하는 응용 계층 프로토콜

- HTTP/1.1: 대부분의 웹 응용 프로그램에서 사용되는 프로토콜.
- AJP: 웹 서버(예: Apache)와의 통합에 사용됨.
- HTTP/2: 웹 성능을 크게 향상시킴.

<div class="content-ad"></div>

여러 I/O 모델 및 응용 프로그램 레이어 프로토콜을 지원하기 위해 컨테이너는 여러 커넥터에 연결될 수 있습니다. 이는 한 방에 여러 개의 문이 있는 것과 유사합니다. 그러나 개별 커넥터나 컨테이너 자체는 서비스를 제공할 수 없습니다. 기능을 제공하기 위해 함께 조립돼야 하며, 이 조립을 "서비스 구성 요소"라고 합니다. 서비스 자체는 중요한 작업을 수행하지 않지만, 단순히 커넥터와 컨테이너를 둘러싼 것입니다. 톰캣에는 유연성을 위해 여러 서비스가 있을 수 있습니다. 톰캣에서 여러 서비스를 구성함으로써 동일한 기계에 배포된 다양한 애플리케이션에 다른 포트 번호를 통해 액세스할 수 있습니다.

다음과 같은 관계 다이어그램이 제공됩니다:

![릴레이션 다이어그램](/assets/img/2024-07-19-HowIstheTomcatConnectorDesigned_1.png)

다이어그램에서 볼 수 있듯이 최상위에는 서버(Server)가 있으며, 이는 톰캣 인스턴스를 가리킵니다. 서버(Server)에는 하나 이상의 서비스가 포함되며, 각 서비스에는 여러 커넥터와 하나의 컨테이너가 포함됩니다. 커넥터와 컨테이너는 표준 ServletRequest 및 ServletResponse 객체를 사용하여 서로 통신합니다.

<div class="content-ad"></div>

# 커넥터

커넥터는 프로토콜 및 I/O 모델의 차이로부터 서블릿 컨테이너를 보호합니다. HTTP 또는 AJP인지 여부에 관계없이 컨테이너는 표준 ServletRequest 객체를 수신합니다.

커넥터의 기능 요구사항을 더 구체화할 수 있습니다. 예를 들어:

- 네트워크 포트에서 수신 대기.
- 네트워크 연결 요청 수락.
- 요청 네트워크 바이트 스트림 읽기.
- 바이트 스트림을 특정 응용 프로그램 계층 프로토콜(HTTP/AJP)에 따라 구문 분석하고 통합 Tomcat Request 객체를 생성.
- Tomcat Request 객체를 표준 ServletRequest로 변환.
- 서블릿 컨테이너를 호출하여 ServletResponse를 가져옵니다.
- ServletResponse를 Tomcat Response 객체로 변환.
- Tomcat Response를 네트워크 바이트 스트림으로 변환.
- 응답 바이트 스트림을 브라우저로 다시 보냅니다.

<div class="content-ad"></div>

요구 사항이 명확해지면 다음 고려해야 할 질문은 커넥터가 가져야 할 하위 모듈입니다. 훌륭한 모듈식 설계는 높은 응집력과 낮은 결합력을 고려해야 합니다.

- 높은 응집력은 관련성이 높은 기능이 가능한 한 집중되어 흩뿌려지지 않아야 한다는 것을 의미합니다.
- 낮은 결합력은 두 관련 모듈이 종속성을 최소화하고 가능한 한 의존도를 줄여야 하며 두 모듈 간의 강한 종속성을 피하기 위해 최대한 노력해야 합니다.

커넥터의 세부 기능 목록을 분석하면, 커넥터가 세 가지 높은 응집력 있는 기능을 수행해야 한다는 것을 알 수 있습니다:

- 네트워크 통신.
- 애플리케이션 레이어 프로토콜 구문 분석.
- Tomcat 요청/응답과 Servlet 요청/응답 간의 변환.

<div class="content-ad"></div>

따라서, Tomcat의 디자이너들은 세 가지 기능을 구현하기 위해 세 가지 구성 요소인 EndPoint, Processor 및 Adapter를 설계했습니다.

이러한 구성 요소는 추상 인터페이스를 통해 상호 작용합니다. 이 접근 방식은 변화를 캡슐화하는 장점도 가지고 있습니다. 이것이 객체 지향 설계의 본질인데, 시스템의 안정적인 부분과 빈번하게 변하는 부분을 격리시켜 재사용성을 높이고 시스템 결합을 줄이는 데 도움이 됩니다.

네트워크 통신의 I/O 모델은 변경 가능합니다. 블로킹 I/O, 비동기 I/O 또는 APR이 될 수 있습니다. 또한 응용 프로그램 계층 프로토콜도 변경 가능합니다. HTTP, HTTPS 또는 AJP일 수 있습니다. 브라우저가 보내는 요청 정보도 변경 가능합니다.

그러나 전반적인 처리 로직은 변경되지 않습니다. EndPoint는 Processor에 바이트 스트림을 제공하고, Processor는 Tomcat 요청 객체를 Adapter에 제공하며, Adapter는 ServletRequest 객체를 컨테이너에 제공하는 역할을 합니다.

<div class="content-ad"></div>

새 I/O 체계나 새 응용 계층 프로토콜을 지원해야 할 때 관련 구체 하위 클래스를 구현하기만 하면 됩니다. 상위 수준의 일반 처리 로직은 변경되지 않습니다. 

I/O 모델 및 응용 계층 프로토콜은 자유롭게 조합될 수 있으므로, NIO + HTTP 또는 NIO2 + AJP와 같이, Tomcat의 디자이너들은 네트워크 통신과 응용 계층 프로토콜 구문 분석을 함께 고려하여 ProtocolHandler라는 인터페이스를 설계하여 이 두 가변 지점을 캡슐화했습니다. 각 프로토콜 및 통신 모델의 조합은 Http11NioProtocol 및 AjpNioProtocol과 같이 해당하는 구체 구현 클래스를 갖습니다.

이러한 가변 지점 외에도 시스템에는 비교적 안정적인 부분이 있으므로, Tomcat은 이러한 안정적인 부분을 캡슐화하기 위해 일련의 추상 기본 클래스를 설계했습니다. 추상 기본 클래스 AbstractProtocol은 ProtocolHandler 인터페이스를 구현합니다. 각 응용 계층 프로토콜은 AbstractAjpProtocol 및 AbstractHttp11Protocol과 같이 자체 추상 기본 클래스를 가지며, 구체적인 프로토콜 구현 클래스는 프로토콜 계층의 추상 기본 클래스를 확장합니다. 여기서 이들의 상속 관계를 정리했습니다.

![TomcatConnectorDesign](/assets/img/2024-07-19-HowIstheTomcatConnectorDesigned_2.png)

<div class="content-ad"></div>

위 다이어그램에서 상속 및 계층적 관계를 명확하게 확인할 수 있습니다. 이 설계의 목적은 가능한 한 안정적인 부분을 추상 기본 클래스에 배치하는 것이며, 각 I/O 모델 및 프로토콜 조합에는 해당하는 구체적인 구현 클래스가 있습니다. 우리는 필요할 때 자유롭게 선택하여 사용할 수 있습니다.

요약하면, 커넥터 모듈은 세 가지 핵심 구성 요소인 Endpoint, Processor 및 Adapter를 사용하여 세 가지 작업을 수행합니다. Endpoint와 Processor는 ProtocolHandler 구성 요소로 함께 추상화됩니다. 이들 간의 관계는 아래 다이어그램에 표시되어 있습니다.

![다이어그램](/assets/img/2024-07-19-HowIstheTomcatConnectorDesigned_3.png)

다음으로, 저는 두 가지 최상위 구성 요소인 ProtocolHandler와 Adapter를 자세히 소개하겠습니다.

<div class="content-ad"></div>

## ProtocolHandler 컴포넌트

이전에 언급한 대로, 커넥터는 네트워크 연결 및 응용 프로그램 계층 프로토콜을 처리하기 위해 ProtocolHandler를 사용합니다. 이것은 두 가지 중요한 부분을 포함합니다: EndPoint와 Processor. 아래에서는 그들의 작동 원리에 대해 자세히 설명하겠습니다.

EndPoint

EndPoint는 통신 엔드포인트로, 통신 수신 및 송신 처리를 위한 인터페이스, 특정 소켓 수신기 및 송신자 처리기, 그리고 전송 계층의 추상화입니다. 따라서 EndPoint는 TCP/IP 프로토콜을 구현하는 데 사용됩니다.

<div class="content-ad"></div>

EndPoint은 인터페이스이며, 해당 추상 구현 클래스는 AbstractEndpoint입니다. AbstractEndpoint의 구체적인 하위 클래스인 NioEndpoint 및 Nio2Endpoint 등에는 Acceptor 및 SocketProcessor와 같은 두 가지 중요한 하위 구성 요소가 있습니다.

Acceptor는 소켓 연결 요청을 수신하기 위해 사용됩니다. SocketProcessor는 수신된 소켓 요청을 처리하는 데 사용됩니다. 이는 Runnable 인터페이스를 구현하고 처리를 위해 Run 메서드에서 프로토콜 처리 구성 요소 Processor를 호출합니다. 처리 능력을 향상시키기 위해 SocketProcessor는 실행을 위해 스레드 풀에 제출됩니다. 이 스레드 풀은 executor라고 하며, 나준에 자세히 소개할 것입니다. 톰캣이 기본 Java 스레드 풀을 확장하는 방법에 관한 섹션에서 자세히 설명하겠습니다.

Processor

EndPoint는 TCP/IP 프로토콜을 구현하는 데 사용되는 반면 Processor는 HTTP 프로토콜을 구현하는 데 사용됩니다. Processor는 EndPoint로부터 소켓을 수신하고 바이트 스트림을 읽은 다음 Tomcat 요청 및 응답 객체로 구문 분석하여 이를 컨테이너로 제출하여 Adapter를 통해 처리합니다. Processor는 응용 프로그램 계층 프로토콜의 추상화입니다.

<div class="content-ad"></div>

프로세서(Processor)는 요청 처리를 위한 메서드를 정의하는 인터페이스입니다. 이 외의 메서드도 정의되어 있어요. 추상 구현 클래스(AbstractProcessor)는 프로토콜의 일부 공통 속성을 캡슐화하지만 메서드는 구현되어 있지 않아요. AJPProcessor 및 HTTP11Processor와 같은 구체적인 구현은 특정 프로토콜에 대한 구문 분석 및 요청 처리 메서드를 구현합니다.

커넥터의 구성 요소 다이어그램을 살펴보죠:

![커넥터 다이어그램](/assets/img/2024-07-19-HowIstheTomcatConnectorDesigned_4.png)

다이어그램에서 볼 수 있듯이 EndPoint가 소켓 연결을 받은 후에는 소켓 프로세서(SocketProcessor) 작업을 생성하고 처리를 위해 스레드 풀에 제출합니다. 소켓 프로세서의 Run 메서드는 프로세서 컴포넌트를 호출하여 응용 계층 프로토콜을 구문 분석합니다. 프로세서가 구문 분석을 통해 요청 개체(Request object)를 생성한 후에는 어댑터의 Service 메서드를 호출할 것입니다.

<div class="content-ad"></div>

지금까지 ProtocolHandler의 전체 아키텍처와 작동 원리를 배웠습니다. EndPoint의 상세 설계에 대해서는 나중에 EndPoint가 Java NIO의 비차단 및 NIO2의 비동기 기능을 최대한 활용하여 고도의 동시성을 달성하는 방법을 구체적으로 소개할 것입니다.

어댑터 구성 요소

이전에 언급했듯이 다양한 프로토콜로 인해 클라이언트가 보낸 요청 정보도 다릅니다. Tomcat은 이 요청 정보를 "저장"하기 위해 자체 Request 클래스를 정의합니다. ProtocolHandler 인터페이스는 요청을 구문 분석하고 Tomcat Request 클래스를 생성하는 역할을 담당합니다. 그러나 이 Request 객체는 표준 ServletRequest가 아니므로 Tomcat Request를 컨테이너를 호출하기 위한 매개변수로 사용할 수 없습니다. Tomcat 설계자들이 고안한 해결책은 어댑터 패턴의 클래식한 응용 중 하나인 CoyoteAdapter를 도입하는 것입니다. 커넥터는 CoyoteAdapter의 Service 메서드를 호출하며 Tomcat Request 객체를 전달합니다. CoyoteAdapter는 Tomcat Request를 ServletRequest로 변환한 다음 컨테이너의 Service 메서드를 호출하는 역할을 합니다.

# 결론

<div class="content-ad"></div>

Tomcat의 전체 아키텍처에는 두 가지 핵심 구성 요소인 커넥터와 컨테이너가 포함됩니다. 커넥터는 외부 통신을 담당하고, 컨테이너는 내부 처리를 처리합니다. 커넥터는 통신 프로토콜 및 I/O 모델의 차이를 캡슐화하기 위해 ProtocolHandler 인터페이스를 사용합니다. ProtocolHandler 내에는 EndPoint와 Processor 두 모듈이 있습니다. EndPoint는 저수준 소켓 통신을 처리하며, Processor는 응용 프로그램 계층 프로토콜 해석을 담당합니다. 컨테이너는 Adapter를 통해 컨테이너를 호출합니다.

Tomcat의 전체 아키텍처를 공부함으로써 복잡한 시스템을 설계하는 데 기본적인 아이디어를 얻을 수 있습니다. 먼저 요구 사항을 분석하고 고응집도 및 낮은 결합도의 원리에 따라 하위 모듈을 결정합니다. 그런 다음 하위 모듈 내에서 가변성과 불변성 지점을 식별합니다. 인터페이스와 추상 기본 클래스를 사용하여 불변성을 캡슐화합니다. 추상 기본 클래스에서 템플릿 메서드를 정의하고 서브 클래스가 추상 메서드를 구현하도록하면 특정 서브 클래스가 가변성 지점을 처리하게 됩니다.

![Tomcat Connector Design](/assets/img/2024-07-19-HowIstheTomcatConnectorDesigned_5.png)