---
title: "JSON 웹 토큰 JWT 이해하기"
description: ""
coverImage: "/assets/img/2024-07-28-UnderstandingJSONWebTokensJWTforSecureInformationSharing_0.png"
date: 2024-07-28 13:50
ogImage: 
  url: /assets/img/2024-07-28-UnderstandingJSONWebTokensJWTforSecureInformationSharing_0.png
tag: Tech
originalTitle: "Understanding JSON Web Tokens JWT for Secure Information Sharing"
link: "https://dev.to/vyan/understanding-json-web-tokens-jwt-for-secure-information-sharing-5c3a"
isUpdated: true
updatedAt: 1723816951353
---



소프트웨어 아키텍처의 세계에서, 특히 다수의 당사자 및 안전한 인증/승인을 다룰 때, 신원 증명을 공유하기 위한 강력한 메커니즘이 중요합니다. 가장 안전하고 널리 사용되는 방법 중 하나는 JSON 웹 토큰(JWT) 🔏 입니다.

이 블로그에서는 JWT가 무엇인지, 어떻게 작동하는지, 그리고 안전한 데이터 전송에 탁월한 선택인 이유에 대해 깊이 파헤쳐 보겠습니다.

### JSON 웹 토큰 (JWT)이란?

JSON 웹 토큰 (JWT)은 JSON 객체로 정보를 안전하게 전송하는 간결하고 자체 포함된 방식을 정의한 오픈 표준(RFC 7519)입니다. 이 정보는 디지털로 서명되기 때문에 확인하고 신뢰할 수 있습니다. JWT는 비밀을 사용하여 서명할 수 있으며(HMAC 알고리즘 사용), RSA 또는 ECDSA를 사용하여 공개/개인 키 쌍으로 서명할 수도 있습니다.

<div class="content-ad"></div>

### JWT를 사용하는 이유

JWT는 stateless하다는 것을 의미합니다. 이는 서버가 세션에 토큰 정보를 저장할 필요가 없다는 것을 의미합니다. 이 stateless한 특성은 JWT를 확장 가능하게 만들어주며, 클라이언트-서버 응용 프로그램 및 분산 시스템에 적합합니다. 여기에는 JWT가 선호되는 이유 중 일부가 있습니다:

- 보안: JWT는 디지털으로 서명되어 데이터가 조작되지 않도록 보장합니다.
- 확장성: stateless하므로 JWT는 서버 측 저장소가 필요하지 않아 분산 시스템에 이상적입니다.
- 콤팩트함: JWT는 콤팩트하며 URL, POST 매개변수 또는 HTTP 헤더를 통해 쉽게 전송할 수 있습니다.
- 자체 포함: JWT의 페이로드에는 사용자가 원하는 모든 정보가 포함되어 있어 데이터베이스를 여러 번 쿼리할 필요가 없습니다.

![이미지](/assets/img/2024-07-28-UnderstandingJSONWebTokensJWTforSecureInformationSharing_0.png)

<div class="content-ad"></div>

### JWT의 구조

JWT는 세 부분으로 구성되어 있습니다: Header, Payload 및 Signature입니다. 이러한 부분은 점 (.)으로 구분됩니다.

```js
Header.Payload.Signature
```

각 구성 요소를 자세히 살펴보겠습니다:

<div class="content-ad"></div>

#### 1️⃣ 헤더

헤더는 일반적으로 두 부분으로 구성됩니다: JWT라는 토큰 유형과 HMAC, SHA256 또는 RSA와 같은 사용 중인 서명 알고리즘.

예시:

```js
{
  "alg": "HS256",
  "typ": "JWT"
}
```

<div class="content-ad"></div>

이 JSON 객체는 그런 다음 Base64Url로 인코딩되어 JWT의 첫 번째 부분을 형성합니다.

#### 2️⃣ 페이로드

페이로드에는 클레임이 포함되어 있습니다. 클레임은 개체(일반적으로 사용자)에 대한 문장과 추가 데이터입니다. 등록된, 공개 및 비공개 클레임의 세 가지 유형이 있습니다.

- 등록된 클레임: 이들은 필수는 아니지만 권장되는 미리 정의된 클레임 집합으로 발급자(iss), 만료 시간(exp), 주제(sub), 청중(aud) 등이 있습니다.
- 공개 클레임: 이들은 원하는 대로 정의할 수 있지만 충돌 방지가 되거나 이름 공간이 있어야 합니다.
- 비공개 클레임: 이들은 이를 사용하기로 합의한 당사자 간의 정보를 공유하기 위해 만든 사용자 정의 클레임입니다.

<div class="content-ad"></div>

예시:

```js
{
  "sub": "1234567890",
  "name": "John Doe",
  "admin": true
}
```

이 JSON 객체는 JWT의 두 번째 부분을 형성하기 위해 Base64Url로 인코딩됩니다.

#### 3️⃣ 서명

<div class="content-ad"></div>

시그니처를 만들려면 인코딩된 헤더, 인코딩된 페이로드, 비밀키, 그리고 헤더에서 지정된 알고리즘이 필요합니다. 이 시그니처는 JWT의 발신자가 자기 자신이라고 말한 대로임을 확인하고 메시지가 중간에 변경되지 않았는지 확인하는 데 사용됩니다.

예를 들어, HMAC SHA256 알고리즘을 사용하려면 다음과 같이 시그니처를 생성합니다:

```js
HMACSHA256(
  base64UrlEncode(헤더) + "." +
  base64UrlEncode(페이로드),
  비밀키)
```

이후 시그니처는 Base64Url로 인코딩되어 JWT의 세 번째 부분을 형성합니다.

<div class="content-ad"></div>

<img src="/assets/img/2024-07-28-UnderstandingJSONWebTokensJWTforSecureInformationSharing_1.png" />

### JWT 동작 방법

클라이언트가 보호된 리소스에 액세스하려면 HTTP 요청의 Authorization 헤더에 JWT를 포함시킵니다. 이는 Bearer Token과 유사합니다. 서버는 JWT를 유효한지 확인하기 위해 일련의 단계를 수행합니다. 모든 유효성 검사 단계를 통과하면 서버는 JWT를 유효하다고 간주하고 요청된 리소스에 액세스 권한을 부여합니다.

다음은 단계입니다:

<div class="content-ad"></div>

1️⃣ JWT 디코딩: 서버는 JWT를 디코딩하여 헤더와 페이로드를 추출합니다.

![image](/assets/img/2024-07-28-UnderstandingJSONWebTokensJWTforSecureInformationSharing_2.png)

3️⃣ 만료 여부 확인: 서버는 토큰이 만료되지 않았는지 확인하기 위해 페이로드의 만료 시간을 확인합니다.

4️⃣ 추가적인 확인: 어플리케이션의 요구 사항에 따라, 서버는 토큰 발급자를 확인하거나 폐기된 토큰 목록에 대한 확인과 같은 추가적인 확인을 수행할 수 있습니다.

<div class="content-ad"></div>

![JWT](/assets/img/2024-07-28-UnderstandingJSONWebTokensJWTforSecureInformationSharing_3.png)

### 현실 세계에서의 JWT 활용 사례

2024년에 JWT의 실용적인 응용 사례를 살펴보겠습니다:

- Single Sign-On (SSO) 원더랜드:
한 번 회사 생태계에 로그인하고 여러 서비스에 쉽게 접근할 수 있다고 상상해보세요. JWT를 통해 교차 도메인에서 인증 상태를 안전하게 전달하여 이 꿈을 실현할 수 있습니다.
- 마이크로서비스 조정:
마이크로서비스 세계에서 JWT는 완벽한 메신저 역할을 합니다. 서비스 간에 인증 및 권한 정보를 안전하고 효율적으로 전달합니다.
- IoT 기기 인증:
스마트 냉장고가 스마트 토스터와 대화할 때 (2024년에는 그 정도까지 도달했습니다), JWT는 인가된 기기만 데이터를 교환할 수 있도록 보장합니다.
- 서버리스 함수 권한부여:
서버리스 아키텍처가 부상함에 따라 JWT는 지속적인 세션 없이 함수 호출을 승인하는 무상태 방법을 제공합니다.

<div class="content-ad"></div>

### 2024년을 위한 JWT 최적의 사용 방법

- 비밀은 지키고 안전하게 보호하세요:
Signing 키를 하나의 반지처럼 다루세요. 키 관리 서비스를 사용하고 정기적으로 키를 교체하세요.
- 토큰을 너무 붐비게 하지 마세요:
페이로드를 작게 유지하세요. JWT는 데이터베이스를 대체할 목적이 아닙니다!
- 합리적인 만료 시간 설정:
짧은 수명을 갖는 토큰(분 단위로 생각하세요)은 토큰이 노출되었을 때의 위험을 최소화합니다.
- 토큰 폐기 구현:
필요할 때 토큰을 무효화하기 위해 토큰 블랙리스트나 Redis 캐시를 사용하세요.
- 모두 HTTPS를 통해:
언제나 암호화된 연결로 JWT를 전송하세요. 어떤 예외도 허용되지 않습니다!

### 실제 JWT 활용

JWT는 매우 다목적이며 다양한 시나리오에서 사용할 수 있습니다. 예를 들어, 백엔드 API를 인가하면서 동일한 권한 세트를 유지하는 데 일반적으로 사용됩니다. Appwrite를 포함한 많은 현대 애플리케이션은 안전하고 효율적인 인증 프로세스를 보장하기 위해 JWT 기반 인증을 지원합니다.

<div class="content-ad"></div>

### 결론

JSON Web Tokens (JWT)은 현대 웹 애플리케이션에서 안전한 정보 공유에 강력한 도구입니다. 데이터를 전송하는 간결하고 자체 포함되어 안전한 방법을 제공하여 상태를 보존하는 인증부터 API 보안까지 다양한 사용 사례에 매우 적합합니다. JWT를 이해하고 구현함으로써 개발자는 애플리케이션의 보안성과 확장성을 향상시킬 수 있습니다.