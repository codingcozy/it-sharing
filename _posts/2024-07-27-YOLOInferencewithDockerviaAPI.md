---
title: "Docker와 API를 사용한 YOLO 추론 방법"
description: ""
coverImage: "/assets/img/2024-07-27-YOLOInferencewithDockerviaAPI_0.png"
date: 2024-07-27 13:58
ogImage: 
  url: /assets/img/2024-07-27-YOLOInferencewithDockerviaAPI_0.png
tag: Tech
originalTitle: "YOLO Inference with Docker via API"
link: "https://medium.com/towards-data-science/yolo-inference-with-docker-via-api-cd6757ba614b"
---


<img src="/assets/img/2024-07-27-YOLOInferencewithDockerviaAPI_0.png" />

# 소개

이 글에서는 Docker를 사용하여 YOLOv8 객체 검출 모델에서 추론을 실행하는 방법과 추론 프로세스를 조율하는 데 사용할 REST API를 생성하는 방법을 설명합니다. 이 글은 YOLOv8 추론 실행 방법, API 구현 방법 및 Docker 컨테이너에서 두 가지를 실행하는 방법으로 세 가지 섹션으로 나뉩니다.

글 내내 프로젝트에 필요한 모든 개념과 구성 요소의 코드 구현이 표시됩니다. 전체 코드는 제 GitHub 저장소에서도 찾을 수 있습니다.

<div class="content-ad"></div>

더 깊이 들어가서 코드와 구조를 살펴보고 몇 가지 명령어로 간단하게 도커를 통해 REST API를 통해 추론을 실행할 수 있으며, 저장소의 README 파일에는 따를 단계, API 문서 가져오는 방법, 프로젝트 구조 등을 자세히 설명하고 있습니다.

# YOLOv8 추론

YOLO는 훈련 시간과 정확도를 균형있게 유지하는 어려움을 해결하고, 물체 탐지를 달성하기 위해 탄생했습니다...