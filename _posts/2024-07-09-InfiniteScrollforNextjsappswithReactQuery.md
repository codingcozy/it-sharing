---
title: "React Query를 활용한 Nextjs 앱의 무한 스크롤 구현 방법"
description: ""
coverImage: "/assets/img/2024-07-09-InfiniteScrollforNextjsappswithReactQuery_0.png"
date: 2024-07-09 08:30
ogImage:
  url: /assets/img/2024-07-09-InfiniteScrollforNextjsappswithReactQuery_0.png
tag: Tech
originalTitle: "Infinite Scroll for Next.js apps with React Query"
link: "https://medium.com/@mateogalic112/infinite-scroll-for-next-js-apps-with-react-query-97ec4c66cefb"
---

![Infinite Scroll](/assets/img/2024-07-09-InfiniteScrollforNextjsappswithReactQuery_0.png)

대량의 목록을 렌더링하면 앱이 느리고 반응이 둔해질 수 있습니다. 모든 종류의 애플리케이션을 구축할 때 이는 매우 일반적인 사용 사례이므로, 간단하고 가벼운 무한 스크롤 구성 요소를 만들기로 결정했습니다. 코드로 빠르게 이동하려면, Github 리포지토리 링크를 확인해주세요. 좋았다면 별을 주는 것을 망설이지 마세요.

# 기술 스택

이 데모에서는 베틀 테스트된 기술을 사용하기로 결정했습니다:

<div class="content-ad"></div>

- Next.js
- TailwindCSS
- TypeScript
- 물론, React Query도 사용합니다.

저장소에는 UI 라이브러리로 shadcn이 포함되어 있지만 완전히 선택 사항입니다. 데이터 원본으로는 Github의 사용자 API를 사용할 것이며, 사용자 목록을 페이징된 형태로 반환합니다. API는 커서 페이징을 사용하므로 사용자가 목록의 끝에 도달할 때 새 데이터를로드하는 우리의 사용 사례에 적합합니다.

# API에서 데이터 가져오기

가장 먼저, 외부 서버에서 데이터를 가져와야 합니다. Axios 대신에 fetch와 같은 일반 웹 API를 많이 선호합니다.

<div class="content-ad"></div>

![image](/assets/img/2024-07-09-InfiniteScrollforNextjsappswithReactQuery_1.png)

React Query를 사용한 API 요청은 매우 간단하며, useEffect 내에서 데이터를 가져오는 것을 피하는 것을 강력히 추천합니다.

# React.js 및 TypeScript로 데이터 표시

무한 스크롤 동작의 주된 로직을 설명하기 전에, 가져온 데이터를 표시할 HTML을 준비해야 합니다.

<div class="content-ad"></div>

![image](/assets/img/2024-07-09-InfiniteScrollforNextjsappswithReactQuery_2.png)

이제 useInfiniteScroll 커스텀 훅에 대해 설명하겠습니다. 하지만 먼저 레이아웃을 빠르게 확인해 보겠습니다.

첫 번째 div 요소는 고정 높이를 가진 메인 컨테이너로 사용되며 y-축으로 스크롤이 가능합니다. 사용자가 화면을 아래까지 스크롤할 때만 데이터를 가져오도록 하여 불필요한 요청을 방지합니다.

내부에는 항목을 렌더링하는 ul 요소와 lastElementRef가 첨부된 다른 div 요소가 있습니다.

<div class="content-ad"></div>

lastElementRef가 뷰포트 내에 있는지 추적해야 합니다. 이는 목록의 끝에 도달했음을 의미합니다. 그런 경우 API에서 데이터 청크를 더 가져와야 합니다.

# 중요한 부분 - useInfiniteScroll 훅 사용하기

여기서 마법이 일어납니다. 부드러운 무한 스크롤 동작을 위한 주요 도구를 먼저 이해해야 합니다: IntersectionObserver API.

lastElement div 요소가 그 상위 요소와 교차하는 것을 관찰하는 것이 정확히 우리가 원하는 것입니다!

<div class="content-ad"></div>

- observerElem은 IntersectionObserver 인스턴스를 추적하기 위한 useRef이며, 한 번만 생성되도록 합니다.
- observerCallback은 관찰 대상 요소가 뷰포트와 교차할 때 실행되는 useCallback 훅입니다. 요소가 교차하고 가져올 페이지가 더 있을 경우 데이터의 다음 페이지를 가져옵니다.
- observer는 observer 설정 및 연결 해제를 처리하는 또 다른 useCallback입니다. 불필요한 다시 렌더링을 피하기 위해 isFetchingNextPage 및 observerCallback에 의존합니다.
- useEffect 훅은 observer 리스너를 설정하고 정리하는 역할을 담당합니다.

![이미지](/assets/img/2024-07-09-InfiniteScrollforNextjsappswithReactQuery_3.png)

# 데모 시간

![데모 이미지](https://miro.medium.com/v2/resize:fit:1200/1*pZO7JltZKgV9iZluzLfzLw.gif)

<div class="content-ad"></div>

# 결론

React Query를 활용하여 무한 스크롤을 구현하면 React 애플리케이션의 데이터 가져오기와 상태 관리를 높일 수 있어요. 이를 통해 사용자 경험을 원활하고 효율적으로 만들 수 있습니다.

React Query의 useInfiniteQuery와 Intersection Observer API를 이용하여 사용자가 스크롤할 때 쉽게 추가 데이터를 로드할 수 있어요. 이렇게 하면 성능과 사용자 참여도를 향상시킬 수 있습니다. 이 접근 방식은 대량 데이터 집합을 처리를 단순화하고 동적이고 반응적인 인터페이스를 만들어냅니다.

다음 SaaS 제품을 개발할 계획이 있다면 Alpha Code에서 React로 시작해보세요!

<div class="content-ad"></div>

오늘 새로운 것을 배우셨거나 이 기사를 즐겁게 읽으셨다면, LinkedIn이나 Twitter에서 저를 팔로우해주세요. 다음에 또 만나요!
