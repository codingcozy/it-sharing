---
title: "라이브러리를 애플리케이션에 추가하기 전에 확인해야 할 체크리스트"
description: ""
coverImage: "/assets/img/2024-07-26-ChecklistBeforeAddingALibraryToYourApp_0.png"
date: 2024-07-26 12:05
ogImage: 
  url: /assets/img/2024-07-26-ChecklistBeforeAddingALibraryToYourApp_0.png
tag: Tech
originalTitle: "Checklist Before Adding A Library To Your App"
link: "https://medium.com/@elye-project/checklist-before-adding-a-library-to-your-app-5fbe123aee30"
isUpdated: true
updatedAt: 1723816824749
---



## 모바일 개발 배우기

![이미지](/assets/img/2024-07-26-ChecklistBeforeAddingALibraryToYourApp_0.png)

최근 iOS에서 애플이 앱 개발자들에게 Privacy Manifest와 라이브러리를 통합하도록 요구하는 중요한 조건을 도입했습니다. 이로 인해 앱이 의존하는 일부 라이브러리에는 아직 Privacy Manifest가 없으며, 이는 5월 1일 이후의 업데이트 출시를 위협할 수 있습니다!

한편, 안드로이드 개발 분야에서는 아직 적용되지는 않았지만 중요한 변화가 다가오고 있습니다. 안드로이드는 SDK Runtime이라는 새로운 SDK 프레임워크 도입을 검토 중입니다. 이 프로젝트는 라이브러리가 어플리케이션에 통합되기 전에 라이브러리의 안전성을 확인하는 것을 목표로 합니다.

<div class="content-ad"></div>

# 체크리스트

그러므로 이러한 고려 사항을 감안할 때, 라이브러리에 대한 종속성은 관련 비용이 따른다는 점을 상기시키는 용도로 쓰입니다. 채택하기 전에 철저한 심사가 필수적입니다.

나는 처음에 잠재적인 체크리스트에 대한 나의 생각을 Reddit에서 공유했고, 비교적 긍정적인 피드백을 받았습니다. 이제 보다 명확하게 설명하기 위해 여기서 더 확장하겠습니다.

## 비즈니스 영향