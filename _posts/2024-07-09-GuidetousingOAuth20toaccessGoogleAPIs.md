---
title: "OAuth 20을 사용하여 Google API에 접근하는 방법 가이드"
description: ""
coverImage: "/assets/img/2024-07-09-GuidetousingOAuth20toaccessGoogleAPIs_0.png"
date: 2024-07-09 16:36
ogImage:
  url: /assets/img/2024-07-09-GuidetousingOAuth20toaccessGoogleAPIs_0.png
tag: Tech
originalTitle: "Guide to using OAuth 2.0 to access Google APIs"
link: "https://medium.com/@tony.infisical/guide-to-using-oauth-2-0-to-access-google-apis-dead94d6866d"
---

<img src="/assets/img/2024-07-09-GuidetousingOAuth20toaccessGoogleAPIs_0.png" />

Google API에 인증하면 웹 응용 프로그램이 사용자를 대신하여 서비스에 액세스 할 수 있습니다. 이 글에서는 Google API와 OAuth 2.0을 사용하여 응용 프로그램을 인증하는 방법을 보여드립니다. 많은 예시는 인피시칼과 Google 간 통합 설정을 중심으로 제시되지만, 귀하의 사용 사례에 맞게 조정할 수 있어야 합니다.

간단히 말하면, Google Cloud Platform (GCP)에서 OAuth 응용 프로그램을 구성하고 응용 프로그램에서 OAuth 2.0 흐름을 설정하여 작동하도록해야 합니다.

OAuth가 무엇인지 잘 모르겠나요? 여기서 기본 내용을 확인해보세요.

<div class="content-ad"></div>

# 고수준 개요

Google API에 액세스하기 위해 OAuth 2.0을 구현하는 방법에 대해 자세히 알아보기 전에, 다룰 내용에 대한 고수준 흐름을 이해하는 것이 도움이 됩니다.

![이미지](/assets/img/2024-07-09-GuidetousingOAuth20toaccessGoogleAPIs_1.png)

- (A) 브라우저에서 사용자를 Google로 리디렉션: 사용자가 브라우저에서 버튼을 누르면 Google로 리디렉션되어 애플리케이션이 사용자의 Google 계정에 대한 액세스를 허용할 수 있습니다.
- (B) Google에서 사용자를 브라우저로 반환: 허용한 후 사용자는 코드와 함께 브라우저로 다시 리디렉션됩니다.
- (C) 코드-토큰 교환 수행: 브라우저에서 서버로 코드를 보내 Google과 교환합니다. 교환 후 서비스로부터 access_token 및 종종 refresh_token을 다시 받아야 합니다.
- (D) Google API에 대한 요청에 액세스 토큰 사용: access_token을 사용하여 사용자를 대신해 Google API에 요청을 보낼 수 있습니다. access_token이 만료되면 refresh_token을 사용하여 새로운 access_token을 얻을 수 있습니다.

<div class="content-ad"></div>

좋아요! 이제 Google API에 액세스하려면 GCP에서 OAuth 2.0을 구현할 준비가 되었습니다.

# GCP에서 OAuth 애플리케이션 구성하기

- 다음 링크에서 GCP 계정을 만드세요: https://cloud.google.com
- GCP에서 프로젝트 API 및 서비스 `Credentials`로 이동하여 새 OAuth 애플리케이션을 만드세요.

![OAuth application](/assets/img/2024-07-09-GuidetousingOAuth20toaccessGoogleAPIs_2.png)

<div class="content-ad"></div>

아래는 Markdown 형식으로 표를 변경하십시오.

3. OAuth 어플리케이션 생성

양식의 일부로, 승인된 리다이렉트 URI에 다음과 같은 리다이렉트 URL을 추가하십시다. 예를 들어 https://your-domain.com/integrations/gcp-secret-manager/oauth2/callback을 사용할 수 있습니다. 물론 원하는 라우팅 구조에 따라 자체 리다이렉트 URL을 선택할 수 있습니다. 이 URL은 사용자가 어플리케이션이 자신의 Google 계정에 액세스할 수 있도록 승인한 후 Google이 사용자를 리디렉션할 URL입니다.

<div class="content-ad"></div>

4. 구글 OAuth2 애플리케이션용 클라이언트 ID 및 클라이언트 비밀번호를 얻어서 손쉽게 이용하세요.

![이미지](/assets/img/2024-07-09-GuidetousingOAuth20toaccessGoogleAPIs_5.png)

# GCP OAuth 애플리케이션과 웹 애플리케이션을 설정하는 방법

5. 사용자를 웹 애플리케이션 프론트엔드에서 GCP로 리디렉션하세요.

<div class="content-ad"></div>

당신의 웹 애플리케이션 프론트엔드에서 Google로 사용자를 리디렉트하여 OAuth 2.0 플로우를 시작하도록 하는 버튼을 만들어보세요. 그래서 사용자가 Google에 로그인하고 웹 애플리케이션이 사용자의 Google 계정에 액세스할 수 있도록 허용할 수 있습니다. 버튼 핸들러는 다음 로직을 포함해야 합니다:

```js
// GCP로부터 받은 클라이언트 ID
const client_id = "";

// CSRF 토큰 생성 및 로컬에 저장
const state = crypto.randomBytes(16).toString("hex");
localStorage.setItem("latestCSRFToken", state);

// 사용자를 Google 로 리다이렉트
const link = `https://accounts.google.com/o/oauth2/auth?scope=https://www.googleapis.com/auth/cloud-platform&response_type=code&access_type=offline&state=${state}&redirect_uri=${window.location.origin}/integrations/google/oauth2/callback&client_id=${client_id}`;
window.location.assign(link);
```

여기서는 CSRF 공격을 방지하기 위한 CSRF 토큰을 생성하여 지역 저장소에 저장하고 GCP로 상태 매개변수로 보내는 것과 관련이 있으며, 이는 본 문서의 범위를 벗어난 주제입니다.

'링크'의 redirect_uri를 GCP 단계 3에서 등록한 것으로 교체해야 합니다. 사용자가 버튼을 누르면 아래와 같이 Google로 리디렉트해야 합니다:

<div class="content-ad"></div>

![OAuth 2.0 Flow Step 6](/assets/img/2024-07-09-GuidetousingOAuth20toaccessGoogleAPIs_6.png)

6. 사용자가 웹 응용 프로그램으로 리디렉션된 후 state 매개 변수를 유효성 검사하고 코드를 백엔드로 전송합니다.

Google은 사용자가 로그인하고 웹 응용 프로그램이 Google 계정에 액세스 권한을 부여한 후 리디렉션할 것이므로 OAuth 2.0 플로의 다음 단계를 처리할 페이지를 만들어야 합니다. 여기서 리디렉션 URI는 코드와 state 매개 변수를 가지고 올 것이며, 이러한 매개 변수를 URL에서 파싱하고 state 매개 변수가 4단계에서 로컬 저장소에 저장된 CSRF 토큰과 일치하는지 확인해야 합니다. 유효하다면 코드를 웹 응용 프로그램 백엔드로 전송하여 코드 토큰 교환을 수행할 수 있습니다.

```js
const { code, state } = queryString.parse(router.asPath.split("?")[1]);

// state 매개 변수를 유효성 검사
if (state !== localStorage.getItem("latestCSRFToken") {
  localStorage.removeItem("latestCSRFToken");
  // 코드를 백엔드로 보냄
  const res = await axios.post("/api/oauth-token", {
    code
  });
}
```

<div class="content-ad"></div>

7. 액세스 토큰 및 리프레시 토큰을 받아오는 코드를 교환해 주세요.

이제 애플리케이션 백엔드에서 Google API 엔드포인트와 코드-토큰 교환을 수행해야 합니다.

```js
const res = await axios.post(
  "https://oauth2.googleapis.com/token",
  new URLSearchParams({
    grant_type: "authorization_code",
    code,
    client_id: process.env.GCP_CLIENT_ID,
    client_secret: process.env.GCP_CLIENT_SECRET,
    redirect_uri: `your-domain/integrations/gcp-secret-manager/oauth2/callback`,
  })
);

const access_token = res.data.access_token; // 구글 API에 접근하는 데 사용됨
const refresh_token = res.data.refresh_token; // 액세스 토큰을 새로고침하는 데 사용됨
const expires_in = res.data.expires_in; // 액세스 토큰을 새로 고칠 때의 시간을 알려줌
```

8. 사용자를 대신하여 Google API에 액세스하기 위해 액세스 토큰을 사용하세요.

<div class="content-ad"></div>

이제 사용자를 대신해 Google API에 액세스할 수 있습니다. API로 보낸 요청에 access_token을 포함하여 이 작업을 수행할 수 있습니다.

9. access_token이 만료되면 refresh_token을 교환하여 새 access_token을 얻으세요.

7단계에서는 GCP가 반환한 두 가지 추가 필드를 변수 refresh_token과 expires_in에 바인딩하는 것을 알 수 있습니다. access_token은 Google API의 단기 인증 자격 증명이므로 expires_in 필드는 유효 기간의 만료 시간을 알려줍니다. 액세스 토큰이 만료되었다는 것을 알게 되면 refresh_token을 포함한 특별한 API 요청을 만들어 토큰 엔드포인트로 넘겨서 새 access_token을 받아 사용자를 대신해 Google API에 계속 액세스할 수 있습니다.

```js
const res = await standardRequest.post(
  "https://oauth2.googleapis.com/token",
  new URLSearchParams({
    client_id: process.env.GCP_CLIENT_ID,
    client_secret: process.env.GCP_CLIENT_SECRET,
    refresh_token,
    grant_type: "refresh_token",
  })
);

const access_token = res.data.access_token;
const expires_in = res.data.expires_in;
```

<div class="content-ad"></div>

그것이에요!

이제 Google API와 OAuth 2.0을 사용하여 응용 프로그램을 인증하는 방법을 알았어요.

# 팁

OAuth 2.0을 올바르게 구현하는 것은 응용 프로그램과 사용자 보안에 매우 중요합니다.

<div class="content-ad"></div>

다음은 염두에 두어야 할 실용적인 조언입니다:

- 코드베이스 어디에서도 응용 프로그램의 Client ID 및 Client Secret를 하드코딩하지 마세요. 대신, 응용 프로그램의 Client ID 및 Client Secret를 안전하게 저장해야 합니다. 이를 위해 응용 프로그램 환경 변수로 저장하는 것이 좋습니다. 더 나아가, 이러한 자격 증명을 Infisical과 같은 시크릿 매니저에 저장하고 런타임에 백엔드로 다시 가져오는 것을 고려해야 합니다.
- OAuth 코드-토큰 교환을 여러 번 수행하지 마세요. 교환은 한 번만 수행한 다음 리프레시 및 엑세스 토큰을 관리하여 이후 모든 요청을 서비스 API로 보내야 합니다. 이러한 토큰은 이전 팁과 마찬가지로 안전하게 다루어져야 합니다. 이를 위해 이를 시크릿 매니저에 저장하는 것을 고려해야 합니다.

— -

# Infisical — 오픈 소스 시크릿 관리 플랫폼

<div class="content-ad"></div>

![Infisical](/assets/img/2024-07-09-GuidetousingOAuth20toaccessGoogleAPIs_7.png)

Infisical (9.7K+ ⭐️)는 수천 개의 팀과 조직이 팀 및 인프라 전체에 걸쳐 비밀을 저장하고 동기화할 수 있도록 도와줍니다.

GitHub 저장소: https://github.com/Infisical/infisical
