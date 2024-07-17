---
title: "AcademiaHUB 만들기 학생들을 위한 종합 가이드"
description: ""
coverImage: "/assets/img/2024-07-07-CreatingAcademiaHUBAComprehensiveStudentCompanion_0.png"
date: 2024-07-07 19:02
ogImage:
  url: /assets/img/2024-07-07-CreatingAcademiaHUBAComprehensiveStudentCompanion_0.png
tag: Tech
originalTitle: "Creating AcademiaHUB: A Comprehensive Student Companion"
link: "https://medium.com/@mahesh.paul.j/creating-academiahub-a-comprehensive-student-companion-81d9c2186299"
---

# 소개:

![AcademiaHUB](/assets/img/2024-07-07-CreatingAcademiaHUBAComprehensiveStudentCompanion_0.png)

AcademiaHUB에 오신 것을 환영합니다! 학업 생활을 간편하게 만들기 위해 설계된 만능 학생 동반자입니다. AcademiaHUB는 벙커 관리자, 알림 및 학점 평균 계산기를 특징으로 하며, 모든 기능이 Google 계정과 심리스하게 통합되어 장치 간 쉬운 데이터 동기화가 가능합니다.

# AcademiaHUB를 만든 이유

<div class="content-ad"></div>

## 필요성 파악

학생으로써 자주 출석률을 추적하고 CGPA를 계산하며 중요한 마감일을 알림하려고 여러 앱을 오가면서 사용해야 했습니다. 이러한 앱 간의 불편한 전환은 시간이 오래 걸리는 것 뿐만 아니라 비효율적이기도 했습니다. 이 모든 작업을 한 곳에서 처리할 수 있는 통합 플랫폼의 필요성을 깨달았고, 그래서 AcademiaHUB가 탄생하게 되었습니다.

## 비전과 목표

AcademiaHUB에 대한 내 비전은 학생들이 학사 책임을 더 효과적으로 관리할 수 있도록 도와주는 포괄적인 도구를 만드는 것이었습니다. 주요 목표는 다음과 같습니다:

<div class="content-ad"></div>

- 출석을 쉽게 추적할 수 있는 방법을 제공합니다.
- CGPA 계산을 간단하게 만듭니다.
- 제때 알림을 통해 중요한 마감일을 놓치지 않도록 보장합니다.
- Google 계정 통합을 통해 기기 간의 데이터 동기화를 신속하게 제공합니다.

# 개발 과정

## 계획 및 연구

AcademiaHUB의 개발 여정은 철저한 계획과 연구로 시작되었습니다. 먼저, 학생들에게 가장 유익한 핵심 기능을 식별하는 것부터 시작했습니다.

<div class="content-ad"></div>

## 기술 스택 선택

AcademiaHUB를 만들기 위해, 프론트엔드로는 유연성과 성능 때문에 ReactJS를 선택했습니다. 백엔드로는 Google의 Firebase를 선택했는데, 데이터 관리에 견고하고 확장 가능한 솔루션을 제공했습니다. Firebase는 Google 계정과의 원활한 통합으로, 여러 기기 간 데이터 동기화를 더 잘 할 수 있었습니다.

## 디자인과 사용자 경험

사용자 친화적인 디자인을 만드는 것이 중요했습니다. AcademiaHUB에 현대적이고 세련된 외관을 줄 수 있는 글래스모피즘 UI 스타일을 채택했습니다. 디자인은 시각적으로 매력적이고 직관적이어야 했고, 사용자가 앱을 쉽게 탐색할 수 있도록 보장했습니다.

<div class="content-ad"></div>

# 구현

## 통근 매니저

통근 매니저는 학생들이 출석률을 쉽게 추적할 수 있도록 합니다. 사용자는 수업과 빠진 세션을 기록할 수 있으며, 앱은 자동으로 출석률을 계산합니다. 이 기능은 학생들이 출석 상태를 알고 필요한 기준을 충족하지 못하도록 방지하는 데 도움을 줍니다.

![이미지](/assets/img/2024-07-07-CreatingAcademiaHUBAComprehensiveStudentCompanion_1.png)

<div class="content-ad"></div>

## 알림

알림 기능을 통해 학생들은 중요한 마감일, 시험 및 과제에 대한 알림을 설정할 수 있습니다. 이러한 알림은 사용자 정의가 가능하며 적시에 제공되어 중요한 날짜를 놓치지 않도록 합니다.

![image](/assets/img/2024-07-07-CreatingAcademiaHUBAComprehensiveStudentCompanion_2.png)

## CGPA 계산기

<div class="content-ad"></div>

CGPA 계산기는 학점 추적과 예측을 간단하게 해줍니다. 학생들은 자신의 성적을 입력할 수 있고, 앱이 누적 학점 평균을 계산하여 학업 성적을 추적하고 미래 개선을 위한 계획을 세우는 데 도움을 줍니다.

![CreatingAcademiaHUBAComprehensiveStudentCompanion](/assets/img/2024-07-07-CreatingAcademiaHUBAComprehensiveStudentCompanion_3.png)

## Google 계정 통합

AcademiaHUB의 주요 기능 중 하나는 Google 계정과의 통합입니다. 이를 통해 사용자는 Google 자격 증명으로 로그인하여 데이터를 모든 디바이스에 동기화할 수 있습니다. Firebase는 이를 위한 백엔드 인프라를 제공하여 데이터가 안전하게 저장되고 언제 어디서나 접근 가능하게 합니다.

<div class="content-ad"></div>

![이미지](/assets/img/2024-07-07-CreatingAcademiaHUBAComprehensiveStudentCompanion_4.png)

# 론칭 및 그 이후

AcademiaHUB를 론칭하는 것은 흥미로운 이정표였어요. 학생 커뮤니티로부터 긍정적인 반응을 받아 필요한 도구임이 입증되었고, 이는 매우 격려되는 일이었어요. 프로젝트 소스 코드를 확인하고 개발에 기여하려면 GitHub의 AcademiaHUB 저장소를 방문해보세요.

게다가, 실제로 AcademiaHUB를 체험하려면 실제 사이트를 방문해보세요: AcademiaHUB 라이브 사이트.

<div class="content-ad"></div>

전반적으로, 사용자 피드백을 기반으로 앱을 계속 개선하고 더 많은 기능을 추가할 계획입니다.

# 결론

AcademiaHUB를 만드는 것은 보람찬 여정이었습니다. 학업 책임을 더 효율적으로 관리하는 데 도움이 되었을 뿐만 아니라 동료 학생들에게 가치 있는 도구를 제공했습니다. AcademiaHUB를 시도해보시고 여러분의 생각과 제안을 공유해주세요. 더 나은 앱을 만드는 데 도움을 주세요.

AcademiaHUB 라이브 사이트를 방문하여 학업 생활을 더 효율적으로 관리하세요. AcademiaHUB GitHub 저장소를 방문하여 소스 코드를 확인하고 기여할 수 있습니다. 계속 발전하고 앱을 개선할 수 있도록 여러분의 피드백과 제안을 환영합니다.
