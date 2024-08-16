---
title: "LLM 기반 애플리케이션 개발 LLM 모델 사용하는 방법"
description: ""
coverImage: "/assets/img/2024-07-29-DiveintoLLM-basedapplicationdevelopmentLearntouseLLMmodels-Part1_0.png"
date: 2024-07-29 13:56
ogImage: 
  url: /assets/img/2024-07-29-DiveintoLLM-basedapplicationdevelopmentLearntouseLLMmodels-Part1_0.png
tag: Tech
originalTitle: "Dive into LLM-based application development Learn to use LLM models-Part 1"
link: "https://medium.com/@rahulveettil/dive-into-llm-based-application-development-learn-to-use-llm-models-part-1-79152353f350"
isUpdated: true
updatedAt: 1723817003044
---



Huggingface model, OpenAI model, Ollama model

![Image](/assets/img/2024-07-29-DiveintoLLM-basedapplicationdevelopmentLearntouseLLMmodels-Part1_0.png)

# 1. 소개

대형 언어 모델 (LLMs)은 자연어 처리 (NLP) 작업을 혁신적으로 변화시켰습니다. 이러한 모델들은 언어 번역, 질문 및 답변, 감정 분석, 텍스트 생성 및 기타 여러 영역에서 발전을 이루도록 만들어졌습니다. 이 모델들은 대량의 데이터 코퍼스에서 학습되어 다양한 용도로 적용할 수 있는 기초 모델이 되었습니다. 이러한 모델의 주요 제공 업체는 Hugging Face, OpenAI 및 Ollama이며, 각각이 언어 모델의 민주화에 기여하고 있습니다.

<div class="content-ad"></div>

이 블로그에서는 LLM 공간에서 시작하는 방법과 API 호출을 사용하여 Hugging Face, OpenAI 및 Ollama 모델과 상호 작용하는 방법을 배우게 됩니다.

LLM에 대한 블로그 포스트 시리즈입니다. 아래에서 다른 기사를 찾을 수 있습니다:

- LLM 모델 사용 방법 학습하기: LLM 모델 사용 방법-파트 1
- LLM 기반 응용 프로그램 개발에 뛰어들기: 코드 개발 자동화 -파트 2

# 2. Hugging Face — Mixtral-Instruct LLM 사용 방법 배우기

<div class="content-ad"></div>

허깅페이스는 트랜스포머를 통해 다양한 사전 훈련 모델을 제공하는 주목할만한 플랫폼으로 등장했습니다.