---
title: "PyTorch 초보자를 위한 정리"
description: ""
coverImage: "/assets/img/2024-07-29-PyTorchLearntoWalkBeforeYouRunPart1_0.png"
date: 2024-07-29 13:56
ogImage: 
  url: /assets/img/2024-07-29-PyTorchLearntoWalkBeforeYouRunPart1_0.png
tag: Tech
originalTitle: "PyTorch Learn to Walk Before You Run  Part 1"
link: "https://medium.com/@rahulveettil/pytorch-learn-to-walk-before-you-run-part-1-ee7f42683de9"
isUpdated: true
updatedAt: 1723817012608
---



파이토치 초보자를 위한 안내서

![PyTorch](/assets/img/2024-07-29-PyTorchLearntoWalkBeforeYouRunPart1_0.png)

# 소개

파이토치(PyTorch)는 Facebook AI Research에서 개발한 오픈 소스 딥러닝 프레임워크입니다. 동적 계산 그래프, 자동 그래디언트 계산 및 사용자 친화적 인터페이스로 인공 지능 분야를 혁신하였습니다.

<div class="content-ad"></div>

생물학 및 화학 AI 분야에서 PyTorch의 유연성은 연구자들에게 필수적인 도구로 자리 잡았어요. 이는 단백질 구조 예측, 유전체 서열 분석, 의학 이미지 처리, 교란 모델링, 단백질 표현 학습과 같은 복잡한 모델 개발을 가능하게 합니다.

이것은 4부작 시리즈로, 우리는 다음을 배울 거예요:

- PyTorch: 달리기 전에 걷는 법 배우기 — 파트 1 — PyTorch의 텐서 및 다양한 연산
- PyTorch: 달리기 전에 걷는 법 배우기 — 파트 2 — 첫 번째 모델 클래스 작성 및 복잡도 조금씩 증가시키기
- PyTorch: 달리기 전에 걷는 법 배우기 — 파트 3 — 자동 미분
- PyTorch: 달리기 전에 걷는 법 배우기 — 파트 4 — 모델 훈련

코드 중심의 실용적인 시리즈이므로, 필요한 경우가 아니면 일일이 설명하지는 않을 거예요.

<div class="content-ad"></div>

# 텐서 작업