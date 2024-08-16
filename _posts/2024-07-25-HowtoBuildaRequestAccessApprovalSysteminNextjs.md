---
title: "Nextjs에서 접근 권한 제한 승인 시스템 구축 방법"
description: ""
coverImage: "/assets/img/2024-07-25-HowtoBuildaRequestAccessApprovalSysteminNextjs_0.png"
date: 2024-07-25 11:57
ogImage: 
  url: /assets/img/2024-07-25-HowtoBuildaRequestAccessApprovalSysteminNextjs_0.png
tag: Tech
originalTitle: "How to Build a Request Access Approval System in Nextjs"
link: "https://dev.to/arindam_1729/how-to-build-a-request-access-approval-system-using-nextjs-p3p"
isUpdated: true
updatedAt: 1723813231909
---



## 소개

Next.js는 서버 사이드 렌더링 및 정적 사이트 생성을 통해 빠르고 확장 가능한 웹 애플리케이션을 만들기 위한 강력한 React 프레임워크입니다. 애플리케이션이 성장함에 따라 민감한 데이터 및 자원에 대한 액세스 관리는 보안과 통제를 유지하는 데 중요합니다. 여기서 요청 액세스 승인 시스템이 매우 유용합니다.

요청 액세스 승인 시스템은 조직의 디지털 인프라 내에서 세밀한 액세스 제어를 가능하게 합니다. 직원들은 기밀 파일이나 특수 소프트웨어와 같은 특정 자원에 대한 액세스를 요청할 수 있으며, 지정된 승인자는 이러한 요청을 검토하고 관리할 수 있습니다.

Next.js에는 이 시스템에 대한 내장 기능이 제공되지는 않지만 견고한 권한 부여 도구인 Permit.io를 사용하여 통합할 수 있습니다. 이 도구는 권한을 구성하고 관리하는 작업을 간편화하여 복잡한 액세스 제어를 Next.js 애플리케이션에 구현하는 이상적인 솔루션입니다.

<div class="content-ad"></div>

안녕하세요! 이 가이드에서는 Next.js와 Permit.io를 사용하여 Request Access Approval 시스템을 구축하는 방법을 안내하겠습니다. 접근 요청을 처리하고 승인을 관리하며 권한을 시행하여 귀하의 조직 내에서 민감한 정보에 대한 액세스를 인가된 개인만 가능하도록 보장하는 데 어떻게 적용하는지 보여드리겠습니다.

## Request Access Approval(요청 액세스 승인)란?

자세히 들어가기 전에 RAA(Request Access Approval)가 무엇인지 알아보겠습니다. 예를 들어, 개발자가 본 프로덕션 데이터베이스에 액세스해야 하는 경우, 그들은 시스템을 통해 요청을 제출합니다. 이 요청은 담당자나 인가된 담당자에게 도착하여 검토됩니다. 요청자가 역할에 따라 액세스가 필요한지와 요구 사항을 충족하는지를 확인합니다.

모든 것이 올바르다면 요청을 수락합니다. 그렇지 않으면 거부할 수 있습니다. 이를 통해 옳은 사람들만 이해할 수 있는 데이터에 액세스할 수 있도록 합니다. 이 실천은 Request Access Approval(요청 액세스 승인)이라고 합니다.

<div class="content-ad"></div>

![image](/assets/img/2024-07-25-HowtoBuildaRequestAccessApprovalSysteminNextjs_0.png)

## 이것이 왜 필요한가요?

지금까지 RAA(요청 액세스 승인)를 살펴보았는데, 왜 이것이 필요한 걸까요? 이것은 민감한 데이터를 무단 액세스로부터 보호하는 강력한 방법으로, 필요한 사람만이 데이터를 볼 수 있도록 보장합니다. 이로써 데이터 침해의 위험을 줄이고 조직이 다양한 보안 규정을 준수하도록 돕습니다. 또한 액세스 부여 프로세스를 더 빠르고 효율적으로 만드는 데 도움이 됩니다.

RAA 시스템은 중요한 정보를 보호하기 위한 추가적인 보안 계층을 추가합니다. 조직은 민감한 데이터에 대한 무단 사용자의 액세스를 제한할 수 있어 고객 정보 및 기타 기밀 데이터를 보호하는 데 필수적입니다.

<div class="content-ad"></div>

RAA 시스템은 액세스 요청을 관리하는 프로세스를 간소화합니다. 관리자는 여러 요청을 효율적으로 검토 및 승인할 수 있고, 직원들은 빠르게 액세스 요구를 제출할 수 있습니다. 이는 조직 내 보안과 효율성을 높입니다.

예를 들어, 직원이 시스템을 통해 요청을 제출할 수 있고, 해당 요청은 관리자나 인가된 개인이 검토합니다. 그들은 직원의 역할과 정보의 민감도와 같은 요소를 고려하여 액세스 권한을 부여하거나 거부합니다.

## 준비 사항

이 주제에 대해 더 알고 싶다면 Next.js 구조에 대한 기본 지식이 있어야 합니다. Next.js에 대해 더 알아보려면 여기를 참조하세요.

<div class="content-ad"></div>

사용자 인증 및 승인을 알아야 합니다. 세션 관리, OAuth 통합, 역할 기반 액세스 제어 (RBAC), 안전한 비밀번호 해싱 및 저장 방법 구현과 같은 중요한 내용들이 있습니다.

## 단계별 안내:

### Next.js 프로젝트 설정

시작하려면 Next.js 프로젝트를 만들어야 합니다. 시간을 절약하기 위해 이미 설정된 스타터 템플릿을 사용할 수 있습니다.

<div class="content-ad"></div>

터미널을 열고 GitHub 저장소를 복제하세요.


git clone <https://github.com/Arindam200/nextjs-starter>


이렇게 하면 Next.js 프로젝트가 생성됩니다. 그 후에 새로 생성된 프로젝트 폴더로 이동하세요:


cd nextjs-starter


<div class="content-ad"></div>

이제 필요한 종속성을 설치하겠습니다.

```js
npm install @permitio/permit-js permitio dotenv
```

다음으로, 프로젝트의 루트에 .env 파일을 생성하여 환경 변수를 저장할 것입니다.

```js
//.env

JWT=your_jwt_token
ENV=your_environment_key
```

<div class="content-ad"></div>

위에 나열된 모든 것을 준비했네요. 이제 기본적인 Next.js 프로젝트 설정을 완료했습니다.

### 사용자 관리 요소 생성:

액세스 요청 요소를 만들려면 먼저 사용자 관리 요소를 만드는 것이 중요합니다. 이를 먼저 만들어 봅시다.

- Elements 섹션으로 이동
- 사용자 관리 아래에서 "요소 생성"을 클릭하세요

<div class="content-ad"></div>

![이미지](/assets/img/2024-07-25-HowtoBuildaRequestAccessApprovalSysteminNextjs_1.png)

- 역할을 할당하여 사용자 관리 요소를 만들어보세요:
이름을 추가해주세요, 예를 들어, “사용자 관리”
RBAC 역할을 선택해주세요
“숨은 역할” 섹션에서 관리자 역할을 “레벨 1 - 워크스페이스 소유자”로 끌어다 놓아주세요
요소를 생성해주세요.
- 이름을 추가해주세요, 예를 들어, “사용자 관리”
- RBAC 역할을 선택해주세요
- “숨은 역할” 섹션에서 관리자 역할을 “레벨 1 - 워크스페이스 소유자”로 끌어다 놓아주세요
- 요소를 생성해주세요.

### 액세스 요청 요소 생성

- Elements 옵션을 클릭해주세요
- 액세스 요청 아래에 위치한 “요소 생성”을 클릭해주세요

<div class="content-ad"></div>

<img src="/assets/img/2024-07-25-HowtoBuildaRequestAccessApprovalSysteminNextjs_2.png" />

- 연결하고 싶은 사용자 관리 요소를 선택합니다.
- 선택에 따라 요소를 사용자 정의하세요.

### 접근 요청 및 사용자 관리 요소 구현

이제 Permit.io의 접근 요청 요소 및 사용자 관리 요소를 Next.js 애플리케이션에 통합하겠습니다.

<div class="content-ad"></div>

- 먼저 components 폴더 아래에 액세스 요청 요소용 컴포넌트인 AccessRequest.tsx를 생성하세요:

```js
import React from 'react'

function AccessRequest() {
  return (
    <iframe
            title="허가 요소 AR"
            src="https://embed.permit.io/ar?envId=e0ffba3632274e8085d92c793d34f0c1&darkMode=false&tenantKey=default"
            width="100%"
            height="100%"
            style={ border: 'none' }
    />
  )
}

export default AccessRequest;
```

- 다음으로, 사용자 권한을 관리하는 사용자 관리 요소용 컴포넌트를 만드세요. UserManagement.tsx에 다음 코드를 추가하세요:

```js
import React from 'react'

function UserManagement() {
  return (
    <iframe
        title="허가 요소 사용자 관리"
        src="https://embed.permit.io/user-management?envId=0ce2a34be877476f958a43b354e06756&darkMode=false"
        width="100%"
        height="100%"
        style="border: none;"
    />
  )
}

export default UserManagement;
```

<div class="content-ad"></div>

- 이제 Dashboard.tsx 컴포넌트를 생성하세요. 이 컴포넌트는 Permit.io를 사용하여 로그인 프로세스를 처리하고 Access Request와 User Management 요소를 모두 내장합니다.

```js
'use client';
import permit, { LoginMethod } from '@permitio/permit-js';
import AccessRequest from './AccessRequest';
import UserManagement from './UserManagement';
import 'dotenv/config';
import { useEffect } from 'react';

const Dashboard = () => {
    useEffect(() => {
        const login = async () => {
            try {
                await permit.elements.login({
                    loginMethod: LoginMethod.frontendOnly,
                    userJwt: process.env.JWT,
                    tenant: 'default',
                    envId: process.env.ENV,
                });
                console.log('로그인 성공');
            } catch (err) {
                console.error('로그인 오류:', err);
            }
        };

        if (process.env.JWT && process.env.ENV) {
            login();
        } else {
            console.error('JWT 토큰 또는 ENV 변수가 정의되지 않았습니다.');
        }
    }, []);

    return (
        <div>
            <AccessRequest />
            <UserManagement />
        </div>
    );
};

export default Dashboard;
```

이 컴포넌트는 useEffect 훅을 사용하여 컴포넌트가 마운트될 때 로그인 함수를 실행합니다.

로그인 함수는 permit.elements.login()을 사용하여 사용자를 인증합니다:

<div class="content-ad"></div>

- 클라이언트 측 인증에 LoginMethod.frontendOnly를 사용합니다.
- JWT 토큰과 환경 ID를 환경 변수에서 검색합니다.
- 로그인에 성공하면 성공 메시지를 로깅하고, 그렇지 않으면 오류를 로깅합니다.

이 구성 요소는 Permit.io 액세스 요청 및 사용자 관리 요소를 내장한 두 개의 iframe을 렌더링합니다.

- 대시보드 구성 요소를 사용하려면, 액세스 요청 요소와 사용자 관리 요소를 표시하려는 위치에 구성 요소를 추가해야 합니다. 이 예시에서는 page.tsx에 다음을 추가해주세요:

```js
import Dashboard from "@/components/Dashboard";

export default function Home() {
  return (
    <main>
      <div>
        <Dashboard />
      </div>
    </main>
  );
}
```

<div class="content-ad"></div>

다 끝났어요. 다음은 Next.js를 사용하여 Permit.io Request Access 기능을 구현하는 방법입니다. 이것은 클라이언트 측 구현입니다. 서버 측 구현도 사용할 수 있어요.

### 어플리케이션 실행하기

프로젝트를 시작하려면 터미널에서 다음 명령어를 실행하세요:

```js
npm run dev
```

<div class="content-ad"></div>

프로젝트를 시작해보겠습니다.

이제, 로컬호스트:3000 으로 이동하면 다음 페이지를 볼 수 있습니다:

![이미지](/assets/img/2024-07-25-HowtoBuildaRequestAccessApprovalSysteminNextjs_3.png)

이제 액세스를 요청하면 관리자가 다음을 받게 됩니다:

<div class="content-ad"></div>

<img src="/assets/img/2024-07-25-HowtoBuildaRequestAccessApprovalSysteminNextjs_4.png" />

그리고 만약 관리자가 우리에게 접근 권한을 주면, 우리는 제한된 또는 비공개 페이지에 접근할 수 있게 됩니다.

이를 통해, 우리 애플리케이션의 제한된 영역에는 인가된 사용자만 접근할 수 있으며, 우리가 권한을 부여하면 그들만이 해당 내용을 볼 수 있습니다.

## 결론

<div class="content-ad"></div>

이 튜토리얼에서는 Permit.io를 사용하여 Next.js에서 요청 액세스 승인 기능을 구현하는 방법을 살펴보았습니다. 이 단계를 따라가면 응용 프로그램의 액세스 관리의 보안성과 효율성을 향상시킬 수 있습니다.

만약 이 포스트가 도움이 되었다면, 혜택을 받을 수 있는 사람들과 공유해보세요.

권한 부여를 구현하는 방법에 대해 더 배우고 싶으신가요? 궁금한 점이 있으신가요? 팀에 문의하기 위해 Slack 커뮤니티에 가입해보세요.

읽어 주셔서 감사합니다 : )