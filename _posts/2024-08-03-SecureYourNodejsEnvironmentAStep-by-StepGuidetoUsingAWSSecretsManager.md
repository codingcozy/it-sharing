---
title: "Nodejs 환경 보안을 위한 AWS Secrets Manager 사용법"
description: ""
coverImage: "/assets/img/2024-08-03-SecureYourNodejsEnvironmentAStep-by-StepGuidetoUsingAWSSecretsManager_0.png"
date: 2024-08-03 20:53
ogImage: 
  url: /assets/img/2024-08-03-SecureYourNodejsEnvironmentAStep-by-StepGuidetoUsingAWSSecretsManager_0.png
tag: Tech
originalTitle: "Secure Your Nodejs Environment A Step-by-Step Guide to Using AWS Secrets Manager"
link: "https://medium.com/@abhi28005/secure-your-node-js-environment-a-step-by-step-guide-to-using-aws-secrets-manager-b3ef72e41d24"
isUpdated: true
updatedAt: 1723816612980
---



<img src="/assets/img/2024-08-03-SecureYourNodejsEnvironmentAStep-by-StepGuidetoUsingAWSSecretsManager_0.png" />

요즘 테크 개발 문화에서는 기술 애플리케이션을 빠른 속도로 개발하는 것이 중요합니다. 때로는 민감한 키토를 보호하는 것을 잊어버리고 경쟁 업체 또는 해커들이 사용할 수 있도록 공개하는 경우가 있습니다. 이로 인해 데이터가 유출될 수 있습니다. 때로는 이를 환경(env) 변수, 구성 세부 정보 및 다른 표기법으로 부르기도 합니다.

이러한 환경 변수를 안전하게 사용하기 위해 AWS는 Secret Manager라는 서비스를 제공합니다.

그럼 Secret Manager가 뭐길래요? 😕

<div class="content-ad"></div>

시크릿 매니저는 당신의 모든 세부사항을 저장하는 은행 금고와 같습니다. 그리고 당신에게 그 금고에 접근할 수 있는 독특하게 디자인된 열쇠를 제공받게 될 것입니다. 당신을 식별할 수 있도록 당신을 위해 생성된 IAM 사용자와 연결될 것입니다.

기능과 사용법에 대한 특징은 몇 가지 있지만 그것들은 유용합니다...

- 시크릿 회전
- 세밀한 정책으로 액세스 관리
- 중앙에서 안전하고 감사할 수 있는 비밀 정보
- 이용한 만큼 지불하기

## 이제 우리가 여기 온 이유에 대해 시작해 봅시다...

<div class="content-ad"></div>

필요 사항: 

- AWS 계정 (없다면 여기서 만들기)
- Node JS (없다면 여기서 설정하기)
- AWS cli (없다면 이 안내에 따라 설치하기)

AWS 콘솔에 로그인하고 시크릿 매니저 서비스로 이동해봅시다.

그 후 "새 시크릿 저장"을 클릭하고 "다른 유형의 시크릿"을 선택한 후 시크릿의 키-값 쌍을 추가해주세요.

<div class="content-ad"></div>

JWT_SECRET: 'DjnenjfbeiKMNJDjkemdknfejfepe3939nckjfejknef=='

![Secure Your Node.js Environment: A Step-by-Step Guide to Using AWS Secrets Manager](/assets/img/2024-08-03-SecureYourNodejsEnvironmentAStep-by-StepGuidetoUsingAWSSecretsManager_1.png)

그런 다음 '알카다브라'와 같은 이름의 secret을 지정하고 저장하세요.

이제 게이트키퍼가 정리되었으니 Node JS를 사용하여 Pathway를 만들어 봅시다.

<div class="content-ad"></div>

기본적인 노드 애플리케이션을 설정하고 aws-sdk npm 모듈을 활용하여 시크릿 매니저의 강력한 기능을 사용할 수 있습니다.

```js
npm install aws-sdk
```

<img src="/assets/img/2024-08-03-SecureYourNodejsEnvironmentAStep-by-StepGuidetoUsingAWSSecretsManager_2.png" />

서버 파일인 app.js 또는 index.js와 같은 애플리케이션의 서버 파일에서 이 파일을 가져옵니다.

<div class="content-ad"></div>

서버를 시작하고 시크릿을 검색하여 응용 프로그램을 안전하게 사용하세요.

여기에요! 🎉 AWS Secret Manager의 도움으로 Node.js 애플리케이션을 포트 녹스로 변신시켰어요. 위험도 큰 능력을 가지고 있기 때문에 비밀을 안전하게 지켜야 해요!

즐거운 코딩 하시고, 여러분의 비밀이 항상 비밀로 유지되기를 바랄게요! 🤫🔐

## 마지막으로

<div class="content-ad"></div>

테이블 태그를 마크다운 형식으로 변경해보세요.