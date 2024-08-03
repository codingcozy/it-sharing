---
title: "대형 언어 모델, ALBERT  자가 지도 학습을 위한 경량화된 BERT"
description: ""
coverImage: "/assets/img/2024-08-03-LargeLanguageModelsALBERTALiteBERTforSelf-supervisedLearning_0.png"
date: 2024-08-03 21:21
ogImage: 
  url: /assets/img/2024-08-03-LargeLanguageModelsALBERTALiteBERTforSelf-supervisedLearning_0.png
tag: Tech
originalTitle: "Large Language Models, ALBERT  A Lite BERT for Self-supervised Learning"
link: "https://medium.com/towards-data-science/albert-22983090d062"
---


## BERT 아키텍처 선택의 주요 기법을 이해하여 간결하고 효율적인 모델을 생성하는 방법

![이미지](/assets/img/2024-08-03-LargeLanguageModelsALBERTALiteBERTforSelf-supervisedLearning_0.png)

# 소개

최근 몇 년간 대형 언어 모델의 발전이 급속히 증가했습니다. BERT는 다양한 NLP 작업을 높은 정확도로 해결할 수 있는 가장 인기 있는 효율적인 모델 중 하나가 되었습니다. BERT 이후, 뒤이어 다른 일련의 모델들이 등장하여 놀라운 결과를 보여주고 있습니다.

<div class="content-ad"></div>

쉽게 관찰할 수 있는 분명한 경향은 시간이 지남에 따라 대형 언어 모델(LLMs)이 매개변수 및 학습된 데이터 양을 기하급수적으로 증가시키면서 더 복잡해진다는 사실입니다. 딥러닝 연구에 따르면 이러한 기술들이 보통 더 나은 결과를 이끌어낸다는 것이 밝혀졌습니다. 불행하게도, 기계 학습 세계에서는 LLMs에 관한 몇 가지 문제에 이미 직면했으며, 확장성은 이를 효과적으로 훈련하고 저장하여 사용하는 데 주요 장애물이 되었습니다.

그 결과, 최근에는 확장성 문제에 대처하기 위해 새로운 LLMs가 개발되었습니다. 본 문서에서는 2020년에 고안된 BERT 매개변수의 상당한 축소를 목표로 한 ALBERT에 대해 논의할 것입니다.

# ALBERT

ALBERT의 기본 메커니즘을 이해하려면 해당 공식 논문을 참조해야 합니다. 대부분의 경우, ALBERT는 BERT에서 동일한 아키텍처를 파생합니다. 아래에서 해당 모델 아키텍처의 선택에 대한 세 가지 주요 차이점을 다루고 설명하겠습니다.

<div class="content-ad"></div>

# 1. 요소별 매개변수 임베딩

입력 시퀀스가 토큰화되면, 각 토큰은 어휘 임베딩 중 하나로 매핑됩니다. 이러한 임베딩은 BERT의 입력에 사용됩니다.

어휘 크기 V(가능한 임베딩의 총 수)와 임베딩 차원 H을 가정해 봅시다. 그러면 V개의 임베딩마다 H값을 저장해야 하므로 V x H 임베딩 행렬이 필요합니다. 실제로 이 행렬은 대부분 크기가 매우 크며 많은 메모리를 필요로 합니다. 하지만 더 큰 문제는 임베딩 행렬의 원소 대부분이 학습 가능하고 모델이 적절한 매개변수를 학습하는 데 많은 리소스가 필요하다는 것입니다.

예를 들어, BERT 베이스 모델을 살펴보겠습니다. 30,000개의 토큰으로 구성된 어휘가 있고, 각 토큰은 768개 요소의 임베딩으로 표현됩니다. 이 경우, 총 2300만 개의 가중치가 저장되고 학습되어야 합니다. 더 큰 모델의 경우, 이 숫자는 더욱 커집니다.

<div class="content-ad"></div>

이 문제는 행렬 인수분해를 사용하여 피할 수 있어요. 원래 어휘 행렬 V x H는 V x E 및 E x H 크기의 작은 행렬 쌍으로 분해될 수 있어요.

![image](/assets/img/2024-08-03-LargeLanguageModelsALBERTALiteBERTforSelf-supervisedLearning_1.png)

결과적으로, O(V x H) 매개변수 대신 분해는 오직 O(V x E + E x H) 가중치를 필요로 해요. 당연히, 이 방법은 H `` E일 때 효과적이에요.

행렬 인수분해의 또 다른 위대한 측면은 토큰 임베딩을 얻기 위한 조회 과정을 변경하지 않는다는 사실이에요: 왼쪽 분해된 행렬 V x E의 각 행은 토큰을 해당하는 임베딩으로 매핑합니다. 이렇게 하면 임베딩의 차원이 H에서 E로 감소해요.

<div class="content-ad"></div>

그럼에도 불구하고, 행렬이 분해된 경우에는 BERT의 입력을 얻기 위해 매핑된 임베딩을 숨겨진 BERT 공간으로 변환해야 합니다. 이 작업은 왼쪽 행렬의 해당 행을 오른쪽 행렬의 열과 곱함으로써 수행됩니다.

# 2. 계층 간 매개변수 공유

모델의 매개변수를 줄이는 한 가지 방법은 매개변수를 공유할 수 있게 하는 것입니다. 이는 모든 매개변수가 동일한 값을 공유하도록 하는 것을 의미합니다. 대부분의 경우, 이는 가중치를 저장하는 데 필요한 메모리를 줄이는 것으로 이어집니다. 그러나 역전파나 추론과 같은 표준 알고리즘은 여전히 모든 매개변수에 대해 실행되어야 합니다.

언급된 아이디어는 ALBERT에 구현되어 있으며, 이는 구조가 동일한 일련의 트랜스포머 블록으로 구성되어 있어 매개변수 공유를 더 효율적으로 만듭니다. 실제로, 트랜스포머에서 계층 간에 여러 가지 매개변수 공유 방법이 존재합니다:

<div class="content-ad"></div>

- 주의를 기울일 파라미터만 공유;
- 순방향 신경망(FNN) 파라미터만 공유;
- ALBERT에서 사용되는 모든 파라미터를 공유.

![링크](/assets/img/2024-08-03-LargeLanguageModelsALBERTALiteBERTforSelf-supervisedLearning_2.png)

## 3. 문장 순서 예측

BERT는 사전 훈련 시 두 가지 목표에 중점을 두었습니다: 가려진 언어 모델링(MSM)과 다음 문장 예측(NSP). 보통 MSM은 BERT의 언어 지식 습득 능력을 향상시키는 데 사용되었고 NSP의 목표는 BERT의 특정 downstream 작업에서의 성능을 향상시키는 데 사용되었습니다.

<div class="content-ad"></div>

하지만 여러 연구에서 NSP 목표를 제거하는 것이 유익할 수도 있다는 것을 보여주었습니다. 그 이유는 그 간단함 때문에 MLM과 비교했을 때입니다. 이 아이디어에 따라, ALBERT 연구자들은 NSP 과제를 제거하고 문장 순서 예측(SOP) 문제로 대체하기로 결정했습니다. SOP의 목표는 두 문장이 올바른 순서인지 또는 역순인지를 예측하는 것입니다.

트레이닝 데이터셋에 대해 말씀드리자면, 모든 입력 문장의 양의 쌍은 동일한 텍스트 단락 내에서 순차적으로 수집됩니다(BERT와 같은 방법). 부정적 문장의 경우, 원칙은 동일하지만 두 문장이 역순으로 간다는 점만 다릅니다.

![이미지](/assets/img/2024-08-03-LargeLanguageModelsALBERTALiteBERTforSelf-supervisedLearning_3.png)

# BERT 대 ALBERT

<div class="content-ad"></div>

BERT와 ALBERT의 자세한 비교는 아래 다이어그램에서 설명되어 있습니다.

![ALBERT vs BERT](/assets/img/2024-08-03-LargeLanguageModelsALBERTALiteBERTforSelf-supervisedLearning_4.png)

가장 흥미로운 관찰은 다음과 같습니다:
- BERT large의 매개변수의 70%만 갖고 있는 ALBERT xxlarge 버전이 하류 작업에서 더 나은 성과를 달성합니다.
- ALBERT large는 BERT large와 비교할 때 유사한 성능을 달성하며, 매개변수 크기 압축으로 인해 1.7배 빠릅니다.
- 모든 ALBERT 모델은 128의 임베딩 크기를 갖습니다. 논문의 소므 조사에서 보여졌듯이 이것이 최적의 값입니다. 예를 들어 임베딩 크기를 768까지 증가시켜도, 지표가 개선되지만 모델의 증가하는 복잡성에 비해 1% 정도로 그다지 많이 개선되지는 않습니다.
- ALBERT xxlarge는 BERT large보다 데이터의 단일 반복 처리 속도가 3.3배 느리지만, 실험 결과 두 모델을 동일한 시간 동안 학습한다면 ALBERT xxlarge가 벤치마크에서 훨씬 뛰어난 평균 성능을 보여줍니다. (88.7% 대 87.2%)
- ALBERT 모델의 넓은 은닉 크기(≥ 1024)는 층 수 증가로 큰 이점을 얻지 못합니다. 이것이 ALBERT large의 24층에서 xxlarge 버전의 12층으로 감소된 이유 중 하나입니다.

<div class="content-ad"></div>


<img src="/assets/img/2024-08-03-LargeLanguageModelsALBERTALiteBERTforSelf-supervisedLearning_5.png" />

- 은닉 레이어 크기의 증가로 인한 유사한 현상이 발생합니다. 값이 4096보다 큰 값으로 증가시켜도 모델 성능이 저하됩니다.

<img src="/assets/img/2024-08-03-LargeLanguageModelsALBERTALiteBERTforSelf-supervisedLearning_6.png" />

# 결론


<div class="content-ad"></div>

ALBERT은 원래의 BERT 모델보다 선태할 만한 선택으로 보입니다. 왜냐하면 ALBERT는 하위 작업에서 그들을 능가하기 때문입니다. 그렇지만, ALBERT는 더 긴 구조 때문에 훨씬 더 많은 계산이 필요합니다. 이 문제의 좋은 예시는 235M 개의 파라미터와 12개의 인코더 레이어를 가진 ALBERT xxlarge입니다. 이 235M개의 가중치 중 대부분은 단일 트랜스포머 블록에 속합니다. 이 가중치들은 그 후 12개의 레이어 각각에 대해 공유됩니다. 따라서 훈련 또는 추론 중에 알고리즘은 20억 개 이상의 파라미터에서 실행되어야 합니다!

이러한 이유로 인해, ALBERT는 속도를 희생하여 더 높은 정확도를 달성할 수 있는 문제에 더 적합합니다. 결과적으로, NLP 분야는 결코 멈추지 않고 새로운 최적화 기술을 향해 끊임없이 발전하고 있습니다. ALBERT의 속도가 곧 향상될 가능성이 매우 높습니다. 논문의 저자들은 ALBERT 가속화를 위한 잠재적인 알고리즘으로 희소 어텐션과 블록 어텐션과 같은 방법을 이미 언급했습니다.

# Resources

- ALBERT: A Lite BERT for Self-supervised Learning of Language Representations

<div class="content-ad"></div>

모든 이미지는 별도로 표기되지 않는 한 저자가 촬영한 것입니다.