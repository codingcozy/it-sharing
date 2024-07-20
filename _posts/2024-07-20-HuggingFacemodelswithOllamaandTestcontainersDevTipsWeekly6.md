---
title: " Ollama와 Testcontainers로 Hugging Face 모델 사용하기  DevTips 주간 소식 6"
description: ""
coverImage: "/assets/img/2024-07-20-HuggingFacemodelswithOllamaandTestcontainersDevTipsWeekly6_0.png"
date: 2024-07-20 11:30
ogImage: 
  url: /assets/img/2024-07-20-HuggingFacemodelswithOllamaandTestcontainersDevTipsWeekly6_0.png
tag: Tech
originalTitle: " Hugging Face models with Ollama and Testcontainers  DevTips Weekly 6"
link: "https://medium.com/@zarinfam/%EF%B8%8F-hugging-face-models-with-ollama-and-testcontainers-devtips-weekly-6-149e7cbb8848"
---


어서오십시오, DevTips 주간 소식(#6)을 소개합니다! 📰 이번 주에는 여러분을 최신 기술 속으로 안내해 줄 기사와 자료를 선별했습니다.

가장 흥미로운 것 중 하나는 공식 🐳 도커 팀 블로그의 글인 “How to Run Hugging Face Models Programmatically Using Ollama and Testcontainers”입니다. 우리는 또한 “웹 앱을 배포하는 다른 접근 방식”과 “자바 21에서 데이터 중심 프로그래밍”이라는 두 가지 훌륭한 📺 비디오도 살펴볼 것입니다. 지난 주에 발행된 이 기사들을 함께 살펴보세요.

![이미지](/assets/img/2024-07-20-HuggingFacemodelswithOllamaandTestcontainersDevTipsWeekly6_0.png)

# How to Run Hugging Face Models Programmatically Using Ollama and Testcontainers

<div class="content-ad"></div>

![이미지](/assets/img/2024-07-20-HuggingFacemodelswithOllamaandTestcontainersDevTipsWeekly6_1.png)

독커(🐳) 웹사이트의 블로그 포스트에 따르면, 🤗 Hugging Face 모델을 🐪 Ollama와 🚚 Testcontainers를 사용하여 실행하는 방법이 설명되어 있습니다. 이를 통해 개발자들에게 AI 통합이 더 간단해집니다. Ollama 컨테이너 설정, 모델 다운로드, 컨테이너 지속성 처리에 대해 다루고 있습니다. 더불어, 사용자 정의 컨테이너를 소개하여 다양한 응용 프로그램에서 Hugging Face 모델을 사용할 수 있도록 하며, 사용 편의성, 프로그래밍적 액세스, 재현성을 강조합니다. 이 접근 방식은 Docker의 컨테이너화를 활용하여 AI/ML 모델 배포를 간소화합니다.

# JEP 423: 오픈JDK에서 G1 가비지 컬렉터에 지역 핀 지원 도입

![이미지](/assets/img/2024-07-20-HuggingFacemodelswithOllamaandTestcontainersDevTipsWeekly6_2.png)

<div class="content-ad"></div>

InfoQ 기사에서는 JEP 423을 논의하고 있습니다. 이는 G1 가비지 컬렉터에 지역 피닝을 도입하며 스위프트 모드에서 비활성화할 필요 없이 가비지 컬렉션 작업 중 특정 메모리 영역을 고정하기 위해 ☕ Java 22로 통합되었습니다. 이 향상은 임계적인 객체가 이동되지 않도록 특정 메모리 영역을 고정시키는 것을 허용하므로 가비지 컬렉션 중단이 필요 없습니다. 이 개발은 대규모로 디스크된 Java와 C, C++와 같은 관리되지 않는 언어의 상호 운용성을 획기적으로 개선하여 JNI(Java Native Interface) 임계적인 영역을 더 효율적으로 처리합니다.

# 다음 프로젝트 용 최상의 아키텍처, 프레임워크는 중요하지 않습니다!

![이미지](/assets/img/2024-07-20-HuggingFacemodelswithOllamaandTestcontainersDevTipsWeekly6_3.png)

이 기사는 🌟 올바른 아키텍처를 선택하는 것이 프레임워크보다 프로젝트 성공에 더 중요하다는 점을 강조합니다🌟. 이는 'Frontend를 위한 Backend' (BFF) 패턴을 소개하여 백엔드 서비스를 다양한 클라이언트 요구에 맞게 사용자 경험을 향상시키는 것입니다. 백엔드 호출 및 데이터 변환을 처리함으로써 BFF는 복잡한 프론트엔드 요구를 단순화하여 확장 가능하고 유지 관리 가능한 프로젝트 개발을 촉진합니다.

<div class="content-ad"></div>

# 여러 방법을 사용하여 웹 앱을 배포하는 방법 (Azure, Render, MongoDB Atlas, Koyeb 등)

이 비디오는 다양한 플랫폼과 기술을 사용하여 🌐 웹 애플리케이션을 배포하는 포괄적인 가이드를 제공합니다. 강사는 Render, MongoDB Atlas, Koyeb, Azure와 같은 플랫폼에서 애플리케이션을 배포하는 단계별 프로세스를 시연합니다.

# Spring Tips: 자바 21+에서 데이터 중심 프로그래밍

이 비디오는 ☕ 자바 21 및 이후 버전에서 데이터 중심 프로그래밍을 탐구합니다. 이는 거대한 코드베이스에서 작은 서비스 지향 아키텍처로의 전환을 강조하며 데이터와 메시지에 반응하는 방식입니다. 비디오에서는 Java 21의 이 패러다임을 지원하는 네 가지 주요 기능인 sealed types, records, pattern matching, smart switch expressions을 소개합니다. 이러한 기능은 언어가 새로운 워크로드와 서비스 스타일을 다루는 능력을 향상시켜 코드를 보다 효율적이고 우아하며 유형 안전하게 만듭니다. 비디오에는 이러한 기능의 혜택을 보여주기 위해 대출과 같은 금융 상품을 다루는 등 실용적인 예제가 포함되어 있습니다.

<div class="content-ad"></div>

🙏 읽어 주셔서 감사합니다. 아래로 연락하세요:

🖊️ Medium | 🐦 Twitter

🗞️ DevTips Weekly의 이전 이슈