---
title: "나의 HNG 여정 제로 단계 Nginx를 사용하여 정적 웹페이지 배포하는 방법"
description: ""
coverImage: "/assets/img/2024-07-06-MyHNGjourneyStageZeroHowtoDeployaStaticWebpageUsingNginx_0.png"
date: 2024-07-06 09:47
ogImage:
  url: /assets/img/2024-07-06-MyHNGjourneyStageZeroHowtoDeployaStaticWebpageUsingNginx_0.png
tag: Tech
originalTitle: "My HNG journey. Stage Zero: How to Deploy a Static Webpage Using Nginx"
link: "https://dev.to/ravencodess/my-hng-journey-stage-zero-how-to-deploy-a-static-webpage-using-nginx-55ij"
---

## 소개

지난 몇 년 동안, HNG 인턴십은 항상 어떤 수준의 두려움과 존경을 받는 주제였습니다. 어떤 사람은 "힘든 경험으로서 용서하지 않는 경험"이라고 말합니다.
올해, 저는 데브옵스 트랙에 도전하기로 결심했습니다. 10 단계의 작업을 완료해야 하며, 전체 여정을 문서화할 것입니다.

### 요구 사항

요구 사항은 명확합니다.

<div class="content-ad"></div>

- 웹페이지는 클라우드 기반 가상 머신에 호스팅되어야 합니다.
- 웹페이지는 공개적으로 접속 가능해야 합니다.
- 웹페이지는 Nginx 또는 Apache와 같은 웹 서버를 활용해야 합니다.
- 웹페이지에는 인턴의 이름, Slack 사용자이름, 이메일 주소가 포함되어야 합니다.
- 웹페이지에는 공식 HNG 웹사이트로 돌아가는 버튼이 포함되어야 합니다.

### 준비 사항

기술/지식: Docker, Nginx, Linux CLI의 기본적인 이해.

도구/소프트웨어: Docker, Nginx, AWS.

<div class="content-ad"></div>

시작해 봅시다

가상 머신 설정
이 단계에서는 AWS EC2를 사용할 예정이지만 원하는 클라우드 플랫폼을 자유롭게 사용해도 괜찮습니다.

간단한 정적 웹 서버로 이 EC2 사양을 선택했습니다:

- 인스턴스 이름: My Web Server
- AMI: Amazon Linux 2023 (무료 티어)
- 인스턴스 유형: t2 micro
- 저장소: 기본 8GB gp3
- 네트워킹: 기본 VPC 및 서브넷
- 보안 그룹: HTTP, HTTPS 액세스를 어디서나 허용하고 내 IP에서 SSH 액세스를 허용합니다

<div class="content-ad"></div>

새 키페어를 생성하고 인스턴스에 SSH로 연결하고 싶을 때 안전하게 저장하세요.

![image](/assets/img/2024-07-06-MyHNGjourneyStageZeroHowtoDeployaStaticWebpageUsingNginx_0.png)

고급 세부정보 패널을 확장하고, 맨 아래로 스크롤하여 User Data를 찾아 아래 스크립트를 붙여넣으세요.

```js
#!/bin/bash
# 소프트웨어 업데이트
yum update -y

# Docker 설치
yum install docker -y
service docker start
usermod -aG docker ec2-user

# Git 설치
yum install git -y

# 저장소 복제
git clone https://github.com/Ravencodess/static-webpage-stage-0.git
cd static-webpage-stage-0

# Docker 이미지 빌드
docker build -t my-web-app .

# 컨테이너 실행 및 서버의 포트 80을 컨테이너 내부의 포트 80에 매핑
docker run -d -p 80:80 my-web-app
```

<div class="content-ad"></div>

이 스크립트는 내 공개 Github 레포지토리에서 애플리케이션 코드를 가져와 Nginx 기반 도커 컨테이너에서 애플리케이션을 실행하며 서버의 포트 80을 컨테이너 내부의 포트 80에 매핑합니다. 이렇게 하면 브라우저에서 서버의 공인 IP 주소를 사용하여 정적 웹페이지에 액세스할 수 있습니다.

저장소의 내용을 자세히 살펴보겠습니다.

![image](/assets/img/2024-07-06-MyHNGjourneyStageZeroHowtoDeployaStaticWebpageUsingNginx_1.png)

HTML, CSS 및 JS 폴더에는 기능적인 웹페이지를 만들기 위해 필요한 파일, 스타일시트 및 스크립트가 포함되어 있으며 원하는 대로 편집하고 구성할 수 있습니다.

<div class="content-ad"></div>

Dockerfile

![Dockerfile](/assets/img/2024-07-06-MyHNGjourneyStageZeroHowtoDeployaStaticWebpageUsingNginx_2.png)

도커 파일과 nginx.conf 파일 내용에 더 자세하게 관심을 기울이고 싶어요.

<div class="content-ad"></div>

- 어플리케이션을 위한 기본 이미지로 Docker Hub의 최신 nginx 이미지를 사용합니다.
- 기본 nginx.conf 설정 파일을 제거하고 수정된 conf 파일로 교체합니다.
- 그런 다음 html, css 및 javascript 파일을 Nginx가 웹 페이지를 제공하는 데 사용하는 /usr/share/nginx 폴더로 복사합니다.
- 마지막으로 포트 80이 노출되어 있음을 문서화하고 컨테이너를 시작합니다.

nginx.conf

![이미지](/assets/img/2024-07-06-MyHNGjourneyStageZeroHowtoDeployaStaticWebpageUsingNginx_3.png)

이 파일은 Nginx를 localhost 포트 80에서 수신 대기하도록 설정하고 웹 페이지가 필요로 하는 모든 정적 자산에 대한 리디렉션을 설정합니다.

<div class="content-ad"></div>

인스턴스가 성공적으로 실행되어 상태 확인을 통과했으므로 이제 공개 IP 주소 또는 DNS를 찾아서 새 브라우저 탭에 붙여넣으세요.

웹 페이지가 작동 중입니다.

![이미지 1](/assets/img/2024-07-06-MyHNGjourneyStageZeroHowtoDeployaStaticWebpageUsingNginx_4.png)

![이미지 2](/assets/img/2024-07-06-MyHNGjourneyStageZeroHowtoDeployaStaticWebpageUsingNginx_5.png)

<div class="content-ad"></div>

행복한 런칭 🚀
