---
title: "HTML 5 게임 만들기 - 등각 투영 보드 게임 튜토리얼"
description: ""
coverImage: "/assets/img/2024-07-06-MakeHTML5GamesIsometricBoardGameTutorial_0.png"
date: 2024-07-06 09:59
ogImage:
  url: /assets/img/2024-07-06-MakeHTML5GamesIsometricBoardGameTutorial_0.png
tag: Tech
originalTitle: "Make HTML 5 Games — Isometric Board Game Tutorial"
link: "https://medium.com/gitconnected/make-html-5-games-isometric-board-game-tutorial-439783c1d35d"
---

## 모두 필요한 것

이 튜토리얼과 매칭 비디오 튜토리얼에서는 HTML 5 Canvas와 JavaScript를 사용하여 등장형 보드 게임을 코딩하는 단계를 살펴볼 것입니다.

![이미지](/assets/img/2024-07-06-MakeHTML5GamesIsometricBoardGameTutorial_0.png)

만들 게임은 Orbs of Order라고 하며 플레이어가 마우스나 키로 네 개의 등대로 이동할 수 있는 게임입니다. 등대에 도착하면 플레이어는 색상 구체를 공개하기 위해 눌러야 합니다. 플레이어는 오른쪽 하단에 표시된 색상 순서대로 구체를 공개해야 합니다. 경로 탐색(A\*)은 플레이어가 사진 위쪽의 중간 회색 색상인 경로에 머무를 수 있도록 도와줍니다.

<div class="content-ad"></div>

## 준비

누구나 이 단계를 따라가며 게임을 만들 수 있어야 하지만, 이러한 유형의 게임을 직접 만드는 데 사용되는 많은 기술과 기법이 있습니다. 이곳은 이 기적 같은 세계를 소개하는 ◎ 캔버스에서 코딩 창조력 가이드입니다.

/assets/img/2024-07-06-MakeHTML5GamesIsometricBoardGameTutorial_1.png

우리는 ZIM JavaScript 캔버스 프레임워크를 사용하여 게임을 만들 것입니다. 게임 모듈 및 Board() 클래스를 갖춘 ZIM은 등장판 게임을 만드는 데 사용됩니다. ZIM에는 온라인 편집기와 간단한 등장판 예제가 모두 있습니다...
