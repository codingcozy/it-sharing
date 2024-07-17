---
title: "AWS Lambda, API Gateway, AWS Amplify를 사용한 서버리스 애플리케이션 구축 방법"
description: ""
coverImage: "/assets/img/2024-07-07-ServerlessApplicationusingAWSLambdaApiGatewayAWSAmplify_0.png"
date: 2024-07-07 01:50
ogImage: 
  url: /assets/img/2024-07-07-ServerlessApplicationusingAWSLambdaApiGatewayAWSAmplify_0.png
tag: Tech
originalTitle: "Serverless Application using AWS Lambda ,Api Gateway,AWS Amplify"
link: "https://dev.to/albine_peter_c2ffb10b422f/serverless-application-using-aws-lambda-api-gatewayaws-amplify-4kmg"
---


<img src="/assets/img/2024-07-07-ServerlessApplicationusingAWSLambdaApiGatewayAWSAmplify_0.png" />

- 프로젝트 초기화: npm init으로 프로젝트를 설정하고 Amplify CLI를 설치합니다.
- Amplify 설정: amplify configure를 실행하고 AWS 자격 증명을 설정하기 위해 안내에 따릅니다.
- Amplify 초기화: amplify init을 사용하여 프로젝트 설정 및 환경을 구성합니다.
- 람다 함수 추가: amplify add function을 실행하여 런타임(예: Node.js)을 선택하고 함수 코드를 사용자 정의합니다.
- API Gateway 설정: amplify add api로 REST API를 추가하고 람다 함수에 연결하고 엔드포인트를 정의합니다.
- 백엔드 배포: amplify push를 사용하여 람다 함수와 API 구성을 배포합니다.
- 호스팅 추가: amplify add hosting으로 호스팅을 구성하고 수동 또는 지속적인 배포를 선택합니다.
- 프론트엔드 빌드: 프론트엔드 코드를 생성하고 빌드하여 적절한 디렉토리에 배치합니다.
- 프론트엔드 배포: amplify publish를 사용하여 프론트엔드를 배포합니다.
- 액세스 및 테스트: 제공된 URL을 통해 배포된 애플리케이션에 액세스하고 API 엔드포인트를 테스트합니다.