---
title: "OutSystems에서 JavaScript 3rd-party 라이브러리를 사용한 고급 그래프 구현 방법"
description: ""
coverImage: "/assets/img/2024-07-07-AdvancedgraphsinOutSystemsusingJavaScript3rd-partylibraries_0.png"
date: 2024-07-07 01:57
ogImage:
  url: /assets/img/2024-07-07-AdvancedgraphsinOutSystemsusingJavaScript3rd-partylibraries_0.png
tag: Tech
originalTitle: "Advanced graphs in OutSystems using JavaScript 3rd-party libraries"
link: "https://medium.com/@catafgomes/advanced-graphs-in-outsystems-using-javascript-3rd-party-libraries-975a834eed55"
---

OutSystems에서는 내장 차트 구성 요소를 제공하지만 (https://charts.outsystems.com/에서 사용 가능), 개발자들은 때로는 더 고급 그래픽 기능을 찾을 수 있습니다. 이런 경우에는 AmCharts와 같은 서드파티 JavaScript 라이브러리를 활용하여 바퀴를 다시 발명할 필요 없이 견고한 솔루션을 제공할 수 있습니다.

이 문서에서는 OutSystems 내에서 이러한 JavaScript 외부 라이브러리를 통합하여 다양한 비즈니스 요구를 충족하는 더 복잡한 그래프를 구축하는 방법을 살펴볼 것입니다.

이 예시로 폭발식 파이 차트(파이 차트 옆에 파이 차트가 표시되는 방식)를 사용할 것입니다.

![예시 이미지](/assets/img/2024-07-07-AdvancedgraphsinOutSystemsusingJavaScript3rd-partylibraries_0.png)

<div class="content-ad"></div>

# 단계 1: JavaScript 라이브러리 탐색

원하는 차트 유형을 제공하는 JavaScript 라이브러리를 조사해보세요. 예를 들어, AmCharts 웹사이트에서 폭발하는 파이 차트 예제를 찾을 수 있습니다.

![exploing-piechart-example](/assets/img/2024-07-07-AdvancedgraphsinOutSystemsusingJavaScript3rd-partylibraries_1.png)

원하는 차트 유형을 식별한 후, 제공된 JavaScript 소스 코드의 다양한 섹션을 이해해보세요.

<div class="content-ad"></div>

- 스타일: 차트 컨테이너에 대한 CSS 스타일을 정의합니다.

![image](/assets/img/2024-07-07-AdvancedgraphsinOutSystemsusingJavaScript3rd-partylibraries_2.png)

2. 리소스: AmCharts에서 필요한 JavaScript 라이브러리를 가져옵니다.

![이미지](/assets/img/2024-07-07-AdvancedgraphsinOutSystemsusingJavaScript3rd-partylibraries_3.png)

<div class="content-ad"></div>

3. 차트 코드: 데이터 설정, 시리즈 구성 및 이벤트 처리를 포함한 파이 차트를 생성하고 구성하는 JavaScript 코드입니다.

![Chart Code](/assets/img/2024-07-07-AdvancedgraphsinOutSystemsusingJavaScript3rd-partylibraries_4.png)

4. HTML: 차트가 렌더링될 div 요소인 "chartdiv"의 id를 만드는 HTML 코드입니다.

![HTML Code](/assets/img/2024-07-07-AdvancedgraphsinOutSystemsusingJavaScript3rd-partylibraries_5.png)

<div class="content-ad"></div>

마지막으로, 다음 단계에서 OutSystems에 가져올 필요한 JavaScript 리소스를 다운로드해 주세요.

# 단계 2: 스크립트 가져오기

OutSystems를 사용하여 구현 작업을 진행해 봅시다.

모듈로 JavaScript 스크립트를 가져오는 것부터 시작해 주세요 (인터페이스 탭 아래의 스크립트 폴더를 마우스 오른쪽 클릭하여 "스크립트 생성" 또는 "스크립트 가져오기"를 선택하세요).

<div class="content-ad"></div>

<img src="/assets/img/2024-07-07-AdvancedgraphsinOutSystemsusingJavaScript3rd-partylibraries_6.png" />

그런 다음 인터페이스 탭에서 아직도 파이 차트를 표시하려는 화면/블록을 선택하고 필요한 스크립트에서 2개의 스크립트를 선택하십시오.

<img src="/assets/img/2024-07-07-AdvancedgraphsinOutSystemsusingJavaScript3rd-partylibraries_7.png" />

# 단계 3: 사용자 인터페이스 구성

<div class="content-ad"></div>

코드를 검토하여 차트에 필요한 컨테이너 요소를 확인하세요. 코드는 100% 너비와 500 픽셀 높이를 가진 chartdiv라는 컨테이너를 예상하므로 해당 컨테이너를 만들어주세요.

```js
#chartdiv {
  width: 100%;
  height: 500px;
}
```

```js
var container = am4core.create("chartdiv", am4core.Container);
```

<img src="/assets/img/2024-07-07-AdvancedgraphsinOutSystemsusingJavaScript3rd-partylibraries_8.png" />

<div class="content-ad"></div>

# 단계 4: 데이터 구조 정의하기

JavaScript 라이브러리에서 필요로하는 데이터 형식을 이해하세요. 데이터의 구조를 파악하기 위해 예시 JavaScript 코드를 살펴보세요. 나중에 이 더미 데이터를 동적 데이터로 대체할 것이며, 입력 매개변수로 전달됩니다.

```js
chart.data = [
  {
    country: "리투아니아",
    litres: 500,
    subData: [
      { name: "A", value: 200 },
      { name: "B", value: 150 },
      { name: "C", value: 100 },
      { name: "D", value: 50 },
    ],
  },
  {
    country: "체코 공화국",
    litres: 300,
    subData: [
      { name: "A", value: 150 },
      { name: "B", value: 100 },
      { name: "C", value: 50 },
    ],
  },
  {
    country: "아일랜드",
    litres: 200,
    subData: [
      { name: "A", value: 110 },
      { name: "B", value: 60 },
      { name: "C", value: 30 },
    ],
  },
  {
    country: "독일",
    litres: 150,
    subData: [
      { name: "A", value: 80 },
      { name: "B", value: 40 },
      { name: "C", value: 30 },
    ],
  },
  {
    country: "호주",
    litres: 140,
    subData: [
      { name: "A", value: 90 },
      { name: "B", value: 40 },
      { name: "C", value: 10 },
    ],
  },
  {
    country: "오스트리아",
    litres: 120,
    subData: [
      { name: "A", value: 60 },
      { name: "B", value: 30 },
      { name: "C", value: 30 },
    ],
  },
];
```

OutSystems의 "JSON에서 구조 추가" 기능을 사용하여 필요한 구조를 자동으로 생성할 수 있습니다.

<div class="content-ad"></div>

아래에 표시된 구조로 변경해야 합니다: 데이터와 하위 데이터. '데이터' 구조에는 주요 카테고리(예: 작업 상태)와 그 총합이 포함됩니다. '하위 데이터' 구조에는 각 주요 카테고리(예: 각 상태 내의 작업 등급)에 관련된 하위 카테고리나 하위 세그먼트가 포함됩니다.

특정 사용 사례에서, 두 구조 모두에 color 속성을 추가했습니다. 이는 두 파이차트의 각 파이의 색상을 지정하고 싶기 때문입니다.

<div class="content-ad"></div>

화면/블록에 데이터 구조의 목록을 저장할 로컬 변수를 만들어주세요. 이 변수는 차트에 필요한 데이터를 최종적으로 저장할 것입니다.

<img src="/assets/img/2024-07-07-AdvancedgraphsinOutSystemsusingJavaScript3rd-partylibraries_11.png" />

# 단계 5: 차트용 데이터 가져오기

이제 집계나 고급 SQL을 사용하여 가져올 수 있는 데이터로 로컬 변수를 채워넣는 시간입니다. 이 단계는 사용 사례와 비즈니스 컨셉에 따라 다를 수 있습니다. 이 기사에서는 프로젝트 관리 시스템 내에서 작업 상태에 초점을 맞추고 있습니다.

<div class="content-ad"></div>

데이터 모델을 살펴보면서 시작해 봅시다. 작업을 저장하는 Task라는 엔티티가 있습니다. Rating과 Status라는 정적 엔티티와 링크하는 2개의 외래 키가 있습니다.

<img src="/assets/img/2024-07-07-AdvancedgraphsinOutSystemsusingJavaScript3rd-partylibraries_12.png" />

Status 정적 엔티티에는 'Open,' 'In Progress,' 'Done,' 'Cancelled'라는 4개의 레코드가 포함되어 있습니다. 마찬가지로 'Rating' 정적 엔티티에는 'No Risk,' 'Risk,' 'Problem,' 'Not Applicable'이라는 4개의 레코드가 있습니다.

화면에는 GetTasksStatus (페치 설정이 At Start로 되어 있는)와 On After Fetch 이벤트가 있는 GetRatingByStatus라는 2개의 어그리게이트가 있습니다.

<div class="content-ad"></div>

<img src="/assets/img/2024-07-07-AdvancedgraphsinOutSystemsusingJavaScript3rd-partylibraries_13.png" />

<img src="/assets/img/2024-07-07-AdvancedgraphsinOutSystemsusingJavaScript3rd-partylibraries_14.png" />

GetTasksStatus 집계는 상태별로 그룹화된 작업의 색상, 레이블 및 개수를 선택하며, GetRatingByStatus는 등급별로 그룹화된 작업의 색상, 레이블 및 개수를 선택하여 Done 상태로 설정된 작업에 초점을 맞춥니다.

<img src="/assets/img/2024-07-07-AdvancedgraphsinOutSystemsusingJavaScript3rd-partylibraries_15.png" />

<div class="content-ad"></div>

![이미지](/assets/img/2024-07-07-AdvancedgraphsinOutSystemsusingJavaScript3rd-partylibraries_16.png)

GetTasksStatus 데이터는 왼쪽 파이 차트에 나타난 것처럼 상태별 작업의 전체 분포를 표시하는 데 사용되고, GetRatingByStatus 데이터는 Done 상태의 등급 분포를 나타내는 용도로 오른쪽 파이 차트에 표시됩니다.

GetTasksStatus 집계의 OnAfterFetch 이벤트에서 로컬 변수를 채우는 로직을 구축합니다.

![이미지](/assets/img/2024-07-07-AdvancedgraphsinOutSystemsusingJavaScript3rd-partylibraries_17.png)

<div class="content-ad"></div>

# 단계 6: 차트 생성하기

컨테이너와 데이터가 준비되었으니 이제 차트를 생성할 시간입니다. 예를 들어 데이터를 검색한 후에 클라이언트 액션인 GeneratePieChart를 생성할 수 있습니다. 이 액션은 데이터 쿼리의 'On After Fetch' 이벤트 내에서 호출할 수 있습니다.

<img src="/assets/img/2024-07-07-AdvancedgraphsinOutSystemsusingJavaScript3rd-partylibraries_18.png" />

이 클라이언트 액션에서는 데이터를 JSON 형식으로 직렬화하고, 이를 차트를 생성하는 JavaScript 노드로 전달하세요.

<div class="content-ad"></div>

![이미지](/assets/img/2024-07-07-AdvancedgraphsinOutSystemsusingJavaScript3rd-partylibraries_19.png)

라이브러리 문서에서 얻은 JavaScript 차트 코드를 붙여 넣어 JavaScript 노드를 만들고 더미 데이터 대신 입력 매개변수에서 데이터를 가져오도록 조정합니다.

![이미지](/assets/img/2024-07-07-AdvancedgraphsinOutSystemsusingJavaScript3rd-partylibraries_20.png)

원하는대로 파이 차트를 더 맞춤화하려면 색상, 범례, 크기 등을 위한 추가 변경 작업이 필요할 수 있습니다.

<div class="content-ad"></div>

지금은 모듈을 게시하고 최종 결과물을 확인할 때입니다:

<img src="/assets/img/2024-07-07-AdvancedgraphsinOutSystemsusingJavaScript3rd-partylibraries_21.png" />

# 결론

폭발하는 원형 차트를 실제로 보고 싶으신가요? 지금 Exploding Pie Chart 데모 앱을 시도해보세요! 더 간편하게 개발 과정을 진행하기 위해 OutSystems Forge에서 제공되는 Exploding Pie Chart 컴포넌트를 활용할 수도 있습니다.

<div class="content-ad"></div>

위의 단계를 따르면 JavaScript 라이브러리를 사용하여 동적이고 시각적으로 매력적인 차트로 OutSystems 애플리케이션을 향상시킬 수 있습니다. 이제 다양한 차트 유형을 탐험해 보는 것이 좋겠군요!

읽어 주셔서 감사합니다! 귀하의 피드백은 귀중히 받아들이겠습니다. 의견을 남겨주시거나 LinkedIn 또는 OutSystems 프로필을 통해 저에게 연락해 주세요.

# 감사의 글

Nuno Reis와 Heriberto Filho에게 이 기사를 검토하고 커뮤니티와 나눌 지식을 공유하도록 격려해 준 점에 감사드립니다.

<div class="content-ad"></div>

# 고지사항

AmCharts와 같은 제3자 JavaScript 라이브러리를 사용하실 경우 해당 저작권 및 라이선스 조건이 적용될 수 있습니다. 라이브러리 제공업체가 지정한 조건 및 규정을 검토하고 준수하는 것이 중요합니다.
