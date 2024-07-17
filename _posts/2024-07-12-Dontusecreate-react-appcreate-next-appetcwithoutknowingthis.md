---
title: "create-react-app, create-next-app 등을 사용하기 전에 꼭 알아야 할 사항"
description: ""
coverImage: "/assets/img/2024-07-12-Dontusecreate-react-appcreate-next-appetcwithoutknowingthis_0.png"
date: 2024-07-12 18:39
ogImage:
  url: /assets/img/2024-07-12-Dontusecreate-react-appcreate-next-appetcwithoutknowingthis_0.png
tag: Tech
originalTitle: "Don’t use create-react-app, create-next-app, etc., without knowing this."
link: "https://medium.com/javascript-in-plain-english/dont-use-create-react-app-create-next-app-etc-without-knowing-this-ea69b41c5393"
---

![image](/assets/img/2024-07-12-Dontusecreate-react-appcreate-next-appetcwithoutknowingthis_0.png)

대부분의 초보자들은 YouTube 동영상과 무료 콘텐츠를 활용하여 코딩을 배웁니다. 만들어진자들은 자신이 선택한 초기 명령어를 사용하고, 그 이유를 잘 설명하지 않습니다.

일부는 목적과 사용법을 설명하지만, 대부분은 그렇지 않습니다. 이전 지식이 없는 초보자들은 지침을 따라 산업 표준으로 받아들입니다.

그 후, 이러한 명령어를 기반 템플릿 위에 모두 구축하기 때문에 프로젝트를 시작하는 더 나은 옵션이 있는 것을 깨닫게 됩니다.

<div class="content-ad"></div>

소프트웨어 엔지니어로서 회사들은 당신이 정확하고 최신 도구를 사용하여 일을 더 쉽게하고 더 나은 결과물을 제공하는 데 지불합니다.

더 나은 결과물을 제공하고 기본을 더 잘 이해하기 위해서는 올바르게 기초부터 시작해야합니다.

이 CLI 명령어들을 시작으로, 우리가 왜 사용하는지와 목적에 대해 설명하겠습니다. 시작해봅시다.

# 글로벌 CLI 명령어란 무엇인가요?

<div class="content-ad"></div>

글로벌 CLI 명령어는 각 프로젝트에서 필요한 패키지를 다운로드하지 않고 실행할 수 있는 명령어입니다. 한 번만 다운로드하면 여러 번 사용할 수 있습니다.

CLI는 명령줄 인터페이스를 의미합니다. 네이티브 또는 소프트웨어 기반 터미널을 사용하여 컴퓨터와 상호 작용하고 특정 명령을 실행합니다.

<img src="/assets/img/2024-07-12-Dontusecreate-react-appcreate-next-appetcwithoutknowingthis_1.png" />

글로벌은 해당 명령을 실행하는 데 필요한 코드가 컴퓨터의 모든 디렉토리나 터미널에서 액세스할 수 있는 위치에 있는 것을 나타냅니다.

<div class="content-ad"></div>

따라서, create-react-app은 전역 CLI 명령어로 자격이 있습니다. 어디에서나 사용할 수 있습니다. 해당 패키지 내용은 컴퓨터나 NPM의 중앙에 위치해 있기 때문에 전역적입니다.

기다려, 뭐라고요?! NPM?

## NPM의 역할

네, NPM입니다. create-react-app이 NPM 패키지라는 것을 알고 계셨나요? 네, NPM 레지스트리에 해당 패키지가 등록되어 있습니다.

<div class="content-ad"></div>

![image](/assets/img/2024-07-12-Dontusecreate-react-appcreate-next-appetcwithoutknowingthis_2.png)

만약 create-react-app을 사용하려 한다면, 해당 패키지를 전역으로 다운로드하거나 npx 명령어를 사용하여 다운로드 없이 사용할 수 있습니다.

패키지를 다운로드하면 컴퓨터의 디렉토리에 저장되어 어떤 터미널에서든 간편하게 액세스할 수 있습니다.

그러나 npx 접두사를 사용하여 create-react-app, create-vite-app 등을 사용하면 해당 패키지를 다운로드하지 않고 사용할 수 있습니다.

<div class="content-ad"></div>

## NPX의 역할

npx는 npm 기반의 명령어로, npm 레지스트리에서 패키지를 원격으로 가져와 다운로드하는 대신 실행할 수 있게 해줍니다.

컴퓨터에 패키지를 다운로드할 필요가 없습니다. 패키지를 사용하려면 npx 뒤에 패키지 이름을 붙이면 됩니다. 단, 프로젝트를 시작하는 것과 같이 무언가를 실행해야만 합니다.

대부분의 창조자들은 npx 명령어로 시작하지만 그 목적을 설명하지 못합니다. 개발자들을 위한 해리 포터의 지팡이처럼 작동합니다.

<div class="content-ad"></div>

지금은 npm, npx, 전역 명령어, CLI 등에 대해 이해하셨습니다. 하지만 이것들을 사용해야 할까요?

# 전역 CLI 명령어를 사용해야 할까요?

왜 개발자가 이러한 CLI 명령어를 사용하고 싶어하지 않을까 생각할 수 없어요. 이러한 명령어는 우리 삶을 편하게 만들어줍니다.

이러한 CLI 명령어들은 미리 작성된 코드로 폴더 구조를 구축하여 시간을 절약해줍니다.

<div class="content-ad"></div>

이외의 일반적이지 않은 CLI(Command Line Interface) 명령어 중에는 create-mf-app과 같은 명령어가 있습니다. 이러한 명령어는 서버, 프론트엔드 애플리케이션 등을 위한 미리 작성된 코드로 폴더 구조를 생성합니다.

![image](/assets/img/2024-07-12-Dontusecreate-react-appcreate-next-appetcwithoutknowingthis_3.png)

올바른 명령어나 도구를 사용 중이라면 사용하지 않을 이유가 없습니다.

그러나 적절한 명령어나 도구를 찾는 것은 대부분 경험과 실험을 통해 얻어집니다.

<div class="content-ad"></div>

저는 Theo나 ThePrimagen과 같은 유튜브 크리에이터들의 영상을 재미로 보면서 대부분의 명령어를 배우고 많은 도구들을 알게 되었습니다.

지능적인 개발자는 늘 더 나은 결과물을 만드는 동안 자신의 삶을 더 편하게 만들 방법을 찾을 수 있어요.

하지만 어떻게 하면 사용 중인 것이 업계 표준인지, 혹은 유용한지 알 수 있을까요?

# 대안은 무엇일까요?

<div class="content-ad"></div>

특정 명령어를 사용 중이라면 대안을 찾아보세요. Discord나 Reddit와 같은 다른 플랫폼의 대화에 참여하여 해당 명령어의 적절성에 대해 논의해보세요.

![image](/assets/img/2024-07-12-Dontusecreate-react-appcreate-next-appetcwithoutknowingthis_4.png)

다른 개발자들과 이야기해보세요. 구글에서 표준 명령어의 대안을 검색하고 올바른 방향으로 안내 받을 수 있는 다양한 자료들이 있습니다.

무료 Notion 문서와 명령어 대안, 장단점, 그리고 다양한 자료와 도구를 모두 한 곳에서 확인할 수 있는 뉴스레터를 구독해보세요.

<div class="content-ad"></div>

<img src="/assets/img/2024-07-12-Dontusecreate-react-appcreate-next-appetcwithoutknowingthis_5.png" />

대안들을 살펴보신 후에는, 사용하시고 실험해보면서 평가해보세요.

대안들을 실험하고 사용한 뒤에, 여러분은 다른 사람들과 차이를 만들어내게 될 거예요. 여러분은 코드와 명령어를 충동적으로 사용하는 것을 그만두고, 여러분과 여러분의 프로젝트에 맞는 방법을 사용하게 될 거예요.

# 결론

<div class="content-ad"></div>

글로벌 CLI 명령어가 NPM과 함께 패치되는 것은 필수적인 도구와 파일로 프로젝트를 시작하는 데 중요합니다.

이 도구 없이 프로젝트를 구축하는 것은 상상할 수 없습니다.

이 도구들을 사용하고 있기 때문에, 그들의 목적, 더 나은 대안이 있는지, 그리고 이 명령어를 실행하기 위해 내부적으로 무엇이 일어나는지 알아야 합니다.

인식과 실험은 경험으로 이어집니다. 모든 개발자는 기술을 향상시키고, 더 많은 지식을 습득하고, 더 많은 경험을 쌓아야 합니다. 경험은 항상 가치가 있습니다.

<div class="content-ad"></div>

여전히 함께 계신다면, 아마 소프트웨어 공학 및 개발 콘텐츠에 더 깊이 파고들고 싶으실 것입니다. 그래서 저는 여러분을 위해 기사 목록을 준비했습니다. 즐거운 독서되세요!

참여하고 싶다면, 의견을 남기고 변경해야 할 사항이 있는지 알려주세요. 이메일 director@afankhan.com (Afan Khan LLC)로도 언제든지 연락할 수 있습니다. 그게 편하시다면 Twitter (X)로도 저에게 연락할 수 있어요 — @whyafan.

# 간단하고 쉬운 언어로 🚀

갑작스런 커뮤니티에 함께해 주셔서 감사합니다! 떠나시기 전 한 마디만 더:

<div class="content-ad"></div>

- 작가에게 박수를 보내고 팔로우하며 함께하십시오! 👏
- 저희를 팔로우하세요: X | 링크드인 | 유튜브 | 디스코드 | 뉴스레터
- 다른 플랫폼에서도 만나보세요: CoFeed | Differ
- PlainEnglish.io에서 더 많은 콘텐츠를 확인해보세요.
