---
title: "배트맨-ComicCSS 사용 방법 및 예시"
description: ""
coverImage: "/assets/img/2024-07-12-Batman-ComicCSS_0.png"
date: 2024-07-12 18:19
ogImage:
  url: /assets/img/2024-07-12-Batman-ComicCSS_0.png
tag: Tech
originalTitle: "Batman-Comic.CSS"
link: "https://dev.to/alvaromontoro/batman-comiccss-hh1"
---

지난 주에는 말라가에서 열린 오픈 사우스 코드 행사에 참석했어요. comiCSS의 창조적인 과정을 설명했죠. 컨퍼런스 일환으로 어린이 이벤트가 있었는데, 그 중 하나로 봉사했어요.

주최측에서 제 발표와 관련된 어떤 것을 준비해 달라고 요청했고, 그렇게 하여 새로운 CSS 유틸리티 클래스 라이브러리인 batman-comic.css가 탄생했어요. 이 라이브러리는 누구나 배트맨 만화 스트립을 만들고 싶을 때 사용할 수 있어요.

지난 두 주 동안 만들어진 이 라이브러리는 이미 두 번의 아이들 컨퍼런스에서 사용되었어요. 이 라이브러리를 통해 아이들은 HTML로 놀며 CSS의 힘을 빠르게 체험할 수 있어요 — 때로는 최선의 방법이 아닐 수 있더라도요. 아이들은 텍스트를 추가하거나 HTML 클래스를 바꾸는 방법을 보고 순식간에 완전히 다른 만화가 나타나는 것을 즐기곤 해요.

## 원천

<div class="content-ad"></div>

도서관에 대한 원래 아이디어는 Batman과 Robin이 Bruce Wayne Batman이 검소한 사람임을 두고 논쟁하는 "웨딩 초청"라는 코믹 CSS 만화 스트립에서 왔어요.

![이미지](/assets/img/2024-07-12-Batman-ComicCSS_0.png)

그 만화 아이디어가 마음에 들었고, 이 과정을 간소화하고 싶었어요. 이 캐릭터들과 관련된 계획이 없더라도요. 이 과정을 간소화하면 코믹 CSS가 더 일관성 있고 빠르게 진행될 거라고 생각했어요. 이 도서관 외에도 다른 작업을 하고 있어요. 하지만 주제에서 벗어나죠.

이 캐릭터를 생성하는 CSS 라이브러리를 만들면 행사 준비가 간단했어요. 이미 많은 얼굴 표현을 완료했기 때문이었죠.

<div class="content-ad"></div>

HTML 및 CSS의 즉각적인 만족감은 아이들과 함께 긴 여정을 떠날 수 있다. 그들은 코딩을 하고 만화가 즉시 업데이트되는 것을 보게 됐어요. 그래서 그런 것이었죠.

## 도서관

세부 정보, 색상 및 클래스가 포함된 온라인 문서 페이지가 있으며, 또 다른 스페인어 버전이 있습니다. 이것은 저가 어린이 이벤트용으로 만든 것이에요.

캐릭터들의 그림은 단일 HTML 요소, 가상 요소 및 많은 그라데이션을 사용한 CSS로 이루어져 있어요. 이 간결함은 만화에 캐릭터를 추가하기 쉽게 만들어 줘요. 예를 들어, 이것은 웃는 배트맨을 추가할 거에요:

<div class="content-ad"></div>

```js
<div class="배트맨"></div>
```

그런 다음 다른 눈과 입 모양을 설정할 클래스가 있습니다. 모든 캐릭터에는 최대 864가지 다른 조합이 생성되는 동일한 표정 클래스가 있습니다 (12가지 눈 조합 _ 24가지 입 조합 _ 3가지 추가 기능). 예를 들어, 이것은 화난 배트맨을 추가합니다:

```js
<div class="배트맨 눈-화남 입-화남"></div>
```

각 캐릭터가 가질 수 있는 클래스 목록입니다. 일부 클래스는 다른 클래스와 결합할 수 있습니다 ("결합 가능"으로 표시됨)

<div class="content-ad"></div>

- 눈
  eyes-no: 눈 없음.
  eyes-think: 위에서 약간 감겨진 눈.
  eyes-doubt: 위에서 아래로 약간 감겨진 눈.
  eyes-sad: 안쪽으로 기울어진 슬픈 표정의 눈.
  eyes-angry: 바깥쪽으로 기울어진 화난 표정의 눈.
  eyes-suspicious: 왼쪽 눈은 생각 중이고, 오른쪽 눈은 화가난 표정입니다.
  eyes-surprise (combinable): 큰 눈.
  eyes-shock (combinable): 오른쪽 눈이 더 강조됨.
- mouth-no: 입 없음.
- mouth-sad: 찡그린 입.
- mouth-angry: mouth-sad 참조.
- mouth-talk: 대화하는 캐릭터의 입.
- mouth-round: 동그란 모양
- mouth-whisper: 작은 타원 모양
- mouth-right (combinable): 입을 약간 오른쪽으로 움직임.
- mouth-left (combinable): 입을 약간 왼쪽으로 움직임.
- mouth-to-right (combinable): 입을 오른쪽으로 기울임.
- mouth-to-left (combinable): 입을 왼쪽으로 기울임.
- Others
  blush: 얼굴에 붉은 빛이 나타남.
  scare: 얼굴에 푸레색 빛이 나타남.
  shame: 얼굴에 (더 연한?) 붉은 빛이 나타남.
- blush: 얼굴에 붉은 빛이 나타남.
- scare: 얼굴에 푸레색 빛이 나타남.
- shame: 얼굴에 (더 연한?) 붉은 빛이 나타남.

이 클래스 이름에 완전히 납득하지는 못하겠어요. 이 라이브러리는 "빠르고 거칠게" 개발했기 때문에 이름과 기본 값은 더 일관성 있게 변경될 가능성이 높습니다.

또한, 각 캐릭터는 다양한 CSS 사용자 정의 속성을 사용하여 색상을 정의하며 (자세한 정보는 문서를 확인하세요), 만화 패널은 쉬운 사용자 정의를 위해 CSS Grid를 레이아웃으로 사용합니다.

## 예제

<div class="content-ad"></div>

현재 라이브러리로 만들 수 있는 몇 가지 예시가 있습니다 (아직 매우 제한적입니다):

- ![Batman ComicCSS_1 image](/assets/img/2024-07-12-Batman-ComicCSS_1.png)
- ![Batman ComicCSS_2 image](/assets/img/2024-07-12-Batman-ComicCSS_2.png)
- ![Batman ComicCSS_3 image](/assets/img/2024-07-12-Batman-ComicCSS_3.png)

<div class="content-ad"></div>

몇 개는 같은 아이디어를 다르게 구현한 것이에요. 더 많은 아이디어가 필요했지만, 이 예시들은 라이브러리 옵션들을 잘 보여줄 거예요.

## 다음은 뭐가 있을까요?

위에서 말했듯이, 나는 라이브러리를 사용해서 새로운 CSS 만화를 생성하는 데 사용할 수도 있겠죠. 하지만 솔직히 말해서, 그것이 어떻게 적용될지 아직 잘 모르겠어요.

나는 이를 이벤트에 재사용할 수도 있어요 — 특히 어린이나 초심자들과 함께, 그들은 코드를 좀 사용함으로써 무엇을 달성할 수 있는지에 놀랍게 반응하기 때문이에요. 하지만 조금의 업데이트가 필요할 거예요:

<div class="content-ad"></div>

- 새로운 캐릭터(슈퍼맨? 베인? 조커? 캣우먼?)
- 새로운 얼굴 표정
- 정확한 얼굴 표정(로빈은 조금 버그가 있음)
- 소품 추가

앞으로의 계획은 이 라이브러리를 GitHub에 공유하고 세계에 공개하여 다른 사람들이 사용하고 새로운 콘텐츠(특히 소품)를 기여할 수 있도록 하는 것입니다.
