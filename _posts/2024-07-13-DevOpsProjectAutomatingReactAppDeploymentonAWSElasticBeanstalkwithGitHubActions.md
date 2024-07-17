---
title: "DevOps 프로젝트 GitHub Actions로 React 앱을 AWS Elastic Beanstalk에 자동 배포하기"
description: ""
coverImage: "/assets/img/2024-07-13-DevOpsProjectAutomatingReactAppDeploymentonAWSElasticBeanstalkwithGitHubActions_0.png"
date: 2024-07-13 18:48
ogImage:
  url: /assets/img/2024-07-13-DevOpsProjectAutomatingReactAppDeploymentonAWSElasticBeanstalkwithGitHubActions_0.png
tag: Tech
originalTitle: "DevOps Project — Automating React App Deployment on AWS Elastic Beanstalk with GitHub Actions"
link: "https://medium.com/dev-genius/devops-project-automating-react-app-deployment-on-aws-elastic-beanstalk-with-github-actions-e2c371e1dac8"
---

![이미지](/assets/img/2024-07-13-DevOpsProjectAutomatingReactAppDeploymentonAWSElasticBeanstalkwithGitHubActions_0.png)

# 프로젝트 설명

이 프로젝트에서는 React 애플리케이션을 AWS Elastic Beanstalk에 배포하는 것이 목표입니다. Elastic Beanstalk는 애플리케이션을 쉽게 배포, 관리 및 확장할 수 있도록 해주는 완전히 관리되는 서비스입니다. GitHub Actions를 활용하여 CI/CD 파이프라인을 설정하여, GitHub 리포지토리에서 새로운 커밋이나 풀 리퀘스트가 있을 때마다 React 앱을 Elastic Beanstalk에 자동으로 빌드 및 배포할 수 있습니다.

# 실전 프로젝트: AWS Elastic Beanstalk에서 React 앱 배포 자동화

<div class="content-ad"></div>

이번 실습 프로젝트에 오신 것을 환영합니다. 이 프로젝트에서는 Docker 및 GitHub Actions를 사용하여 React 앱을 AWS Elastic Beanstalk에 자동으로 배포하는 방법을 안내합니다. 이 프로젝트를 마치면 React 앱을 위한 완벽하게 작동하는 배포 파이프라인을 갖게 될 것입니다.

# 단계 1: 소스 코드 복제

만약 Ubuntu 머신을 사용 중이라면, Git이 이미 설치되어 있을 것입니다. 저장소를 복제하고 프로젝트 디렉토리로 이동하세요.

```js
git clone https://github.com/estebanmorenoit/AWS_Elastic_BeanStalk_On_EC2.git
```

<div class="content-ad"></div>

<img src="/assets/img/2024-07-13-DevOpsProjectAutomatingReactAppDeploymentonAWSElasticBeanstalkwithGitHubActions_1.png" />

```js
cd AWS_Elastic_BeanStalk_On_EC2
```

<img src="/assets/img/2024-07-13-DevOpsProjectAutomatingReactAppDeploymentonAWSElasticBeanstalkwithGitHubActions_2.png" />

# 단계 2: Docker 설정하기

<div class="content-ad"></div>

코드를 복제한 후에 docker_install.sh 셸 스크립트를 찾아 Docker를 설치하고 활성화합니다. 실행 가능하게 만들고 스크립트를 실행하여 Docker를 설치하세요.

```js
chmod +x docker_install.sh
sh docker_install.sh
```

![이미지](/assets/img/2024-07-13-DevOpsProjectAutomatingReactAppDeploymentonAWSElasticBeanstalkwithGitHubActions_3.png)

# 단계 3: Multi-Stage Dockerfile 생성

<div class="content-ad"></div>

React 애플리케이션을 실행하기 위해 Node.js가 필요하고 배포 후 요청을 제공하기 위해 NGINX가 필요합니다. 이러한 요구 사항을 해결하기 위해 다중 단계 Dockerfile을 만들어 보겠습니다.

Dockerfile:

```js
FROM node:14-alpine as builder
WORKDIR /app
COPY package.json .
RUN npm install
COPY . .
RUN npm run build

FROM nginx
EXPOSE 80
COPY --from=builder /app/build /usr/share/nginx/html
```

<div class="content-ad"></div>

# 단계 4: Docker 이미지 빌드

Dockerfile을 작성했으니 이제 Docker 이미지를 빌드해봅시다.

```js
sudo docker build -t react-app-image .
```

![이미지](/assets/img/2024-07-13-DevOpsProjectAutomatingReactAppDeploymentonAWSElasticBeanstalkwithGitHubActions_5.png)

<div class="content-ad"></div>

# 단계 5: Docker 컨테이너 실행하기

빌드된 이미지로 Docker 컨테이너를 실행하세요:

```js
sudo docker run -d -p 80:80 react-app-image
```

도커 ps 명령어를 사용하여 컨테이너가 실행 중인지 확인하고 EC2의 공개 IP 및 포트 80을 통해 응용 프로그램의 활동을 확인하세요.

<div class="content-ad"></div>

![image](/assets/img/2024-07-13-DevOpsProjectAutomatingReactAppDeploymentonAWSElasticBeanstalkwithGitHubActions_6.png)

# 단계 6: AWS Elastic Beanstalk 구성하기

AWS Elastic Beanstalk 서비스로 이동하여 "애플리케이션 생성"을 클릭합니다. 애플리케이션의 이름을 포함한 필수 정보를 제공하고, 플랫폼으로 "Docker"를 선택하고 시작점으로 "샘플 코드"를 선택합니다.

![image](/assets/img/2024-07-13-DevOpsProjectAutomatingReactAppDeploymentonAWSElasticBeanstalkwithGitHubActions_7.png)

<div class="content-ad"></div>

"애플리케이션 생성"을 클릭한 후 환경 설정을 기다려주세요.

![이미지](/assets/img/2024-07-13-DevOpsProjectAutomatingReactAppDeploymentonAWSElasticBeanstalkwithGitHubActions_8.png)

배포된 앱에 액세스하려면 "도메인" 섹션으로 이동하여 제공된 URL을 따라주세요. 이 링크를 통해 AWS Elastic Beanstalk에서 실행 중인 라이브 React 앱으로 이동할 수 있습니다.

![이미지](/assets/img/2024-07-13-DevOpsProjectAutomatingReactAppDeploymentonAWSElasticBeanstalkwithGitHubActions_9.png)

<div class="content-ad"></div>

# 단계 7: GitHub Actions를 통해 CI/CD 활성화하기

CI/CD에 GitHub Actions를 사용할 것입니다. "deploy-react.yaml" 파일을 찾아서 ".github/workflows" 디렉토리 아래로 이동시키세요. 해당 파일을 브랜치 이름, 애플리케이션 이름, 환경 이름, AWS 액세스 키, AWS 비밀 액세스 키, 지역 등과 같은 관련 매개변수로 업데이트해야 합니다.

```yaml
name: BeanStalk에 React 애플리케이션 배포
on:
  push:
    branches:
      - "main"
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: 소스 코드 체크아웃
        uses: actions/checkout@v2

      - name: 배포 패키지 생성
        run: zip -r deploy.zip . -x '*.git*'

      - name: EB에 배포
        uses: einaregilsson/beanstalk-deploy@v21
        with:
          aws_access_key: ${ secrets.AWS_ADMIN_ACCESS_KEY_ID }
          aws_secret_key: ${ secrets.AWS_ADMIN_SECRET_ACCESS_KEY_ID }
          application_name: react-app
          environment_name: React-app-env-2
          version_label: ${ github.sha }
          region: eu-west-2
          deployment_package: deploy.zip
```

# 단계 8: GitHub Actions CI/CD를 트리거하기

<div class="content-ad"></div>

"AWS_Elastic_BeanStalk_On_EC2" 폴더 아래의 모든 코드를 GitHub 저장소의 "main" 브랜치로 푸시해주세요. GitHub Actions가 자동으로 CI/CD 프로세스를 트리거할 것입니다.

![이미지](/assets/img/2024-07-13-DevOpsProjectAutomatingReactAppDeploymentonAWSElasticBeanstalkwithGitHubActions_10.png)

# 단계 9: 결과 확인

이전에 받은 Elastic Beanstalk 링크를 확인해보세요. 샘플 애플리케이션이 React 앱으로 대체되어 성공적으로 배포된 것을 확인할 수 있어야 합니다."

<div class="content-ad"></div>

수고하셨습니다! 이 핸즈온 프로젝트를 완료해 주셔서 축하드립니다! 이번 도전은 GitHub Actions를 사용하여 AWS Elastic Beanstalk에 React 애플리케이션을 배포하는 자동화에 관한 것이었습니다. 이 CI/CD 파이프라인을 설정함으로써 배포 프로세스를 최적화하고 앱을 항상 최신 상태로 유지하여 사용자가 항상 쉽게 이용할 수 있도록 보장했습니다.
