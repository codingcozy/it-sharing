---
title: "AI 모델의 유효 기간  지속적 학습이 해결책일까"
description: ""
coverImage: "/assets/img/2024-08-03-AImodelshaveanexpirydateContinualLearningmaybeananswer_0.png"
date: 2024-08-03 21:09
ogImage: 
  url: /assets/img/2024-08-03-AImodelshaveanexpirydateContinualLearningmaybeananswer_0.png
tag: Tech
originalTitle: "AI models have an expiry date  Continual Learning may be an answer"
link: "https://medium.com/towards-data-science/ai-models-have-expiry-date-9a6e2c9c0a9f"
---


## 변화가 유일한 상수인 세상에서, 우리가 AI 모델에 지속적 학습 접근 방식이 필요한 이유

![image](/assets/img/2024-08-03-AImodelshaveanexpirydateContinualLearningmaybeananswer_0.png)

당신이 정원을 돌아다니며 식물에 물을 주는 작은 로봇을 가지고 있다고 상상해봅시다. 처음에는 몇 주 동안 데이터를 수집하여 로봇을 훈련하고 테스트하기 위해 상당한 시간과 자원을 투자합니다. 이 로봇은 초반에는 잔디와 흙으로 덮인 정원을 효율적으로 이동할 수 있게 배우게 됩니다.

그러나 시간이 지남에 따라 꽃들이 피어나고 정원의 모습이 크게 변화합니다. 다른 계절의 데이터로 훈련된 이 로봇은 이제 주변을 정확하게 인식하지 못하고 작업을 완료하기 어려워 합니다. 이를 해결하기 위해 개화하는 정원의 새로운 예시를 모델에 추가해주어야 합니다.

<div class="content-ad"></div>

첫 번째 생각은 훈련 데이터에 새로운 예제를 추가하고 모델을 처음부터 다시 훈련시키는 것이었죠. 하지만 이 방법은 비용이 많이 들 뿐만 아니라 환경이 변경될 때마다 매번 이 작업을 수행하고 싶지 않습니다. 게다가, 여러분이 모든 역사적 훈련 데이터를 사용할 수 없다는 것을 방금 깨달았습니다.

이제 새로운 샘플로 모델을 미세 조정하는 것을 고려해보고 있습니다. 하지만 이 방법은 모델이 이전에 학습한 능력 중 일부를 잃을 수 있기 때문에 위험합니다. 이는 새로운 정보를 학습할 때 모델이 이전에 습득한 지식과 기술을 잊어버리는 상황인 치명적인 잊혀짐(catastrophic forgetting)으로 이어질 수 있습니다.

..그렇다면 대안이 있을까요? 네, 계속적인 학습(Continual Learning)을 사용하는 것입니다!

물론, 정원에서 식물을 물주는 로봇은 문제를 설명하는 쪽과 관련이 있는 단순한 예시에 불과합니다. 텍스트의 후반부에서는 더 현실적인 응용 사례들을 살펴볼 수 있을 겁니다.

<div class="content-ad"></div>

## Continuous Learning (CL)로 적응력을 가지고 배우세요

모델이 앞으로 직면할 수 있는 모든 시나리오를 예견하고 준비하는 것은 불가능합니다. 따라서 많은 경우, 새로운 샘플이 도착할 때 모델의 적응적인 학습이 좋은 선택일 수 있습니다.

CL에서는 모델의 안정성과 가변성 사이의 균형을 찾으려 합니다. 안정성은 모델이 이전에 학습한 정보를 유지하는 능력이고, 가변성은 새로운 작업이 도입될 때 새로운 정보에 적응하는 능력입니다.

## 하지만 안정성과 가변성을 어떻게 조절할까요?

<div class="content-ad"></div>

연구자들은 적응 모델을 구축하는 여러 방법을 확인했습니다. [3]에서는 다음과 같은 카테고리가 설정되었습니다:

- 정칙화 기반 접근 방법

- 이 접근 방법에서는 모델 구조에 대한 이전 및 새로운 작업의 영향을 균형있게 유지해주는 정칙화 용어를 추가합니다.
- 예를 들어, 가중치 정칙화는 매개변수의 변동을 제어하기 위해 손실 함수에 페널티 용어를 추가하여 매개변수의 변경에 벌점을 부여하며, 이전 작업에 얼마나 많은 기여를 했는지 고려합니다.

2. 재생 기반 접근 방법

<div class="content-ad"></div>

- 이 방법 그룹은 모델이 이전 작업을 여전히 신뢰할 수 있도록 일부 히스토리컬 데이터를 복구하는 데 집중합니다. 이 방법의 제한 사항 중 하나는 항상 히스토리컬 데이터에 액세스해야 한다는 것이며 이는 항상 가능한 것은 아닙니다.
- 예를 들어 경험 재생(Experience replay)은 과거의 훈련 데이터 샘플을 보존하고 재생한다는 개념입니다. 새로운 작업을 훈련할 때 이전 작업의 몇 가지 예시를 추가하여 모델이 이전과 새로운 작업 유형의 혼합에 노출되게 하여 재앙적인 잊혀짐을 제한하는데 도움이 됩니다.

3. 최적화 기반 접근법

- 여기서는 모든 작업에 대한 성능을 유지하면서 재앙적인 잊혀짐의 영향을 줄이기 위해 최적화 방법을 조작하고자 합니다.
- 예를 들어, 그래디언트 투사(Gradient projection)는 새로운 작업을 위해 계산된 그래디언트가 이전 그래디언트에 영향을 미치지 않도록 투사하는 방법입니다.

4. 표현 기반 접근법

<div class="content-ad"></div>

- 이 방법 그룹은 재앙적인 잊혀짐을 피하기 위해 견고한 특징 표현을 얻고 활용하는 데 중점을 둡니다.
- 예를 들어, 자기 감독 학습은 모델이 특정 작업에 대해 훈련되기 전에 데이터의 견고한 표현을 학습할 수 있는 방법입니다. 이 아이디어는 미래에 모델이 직면할 수 있는 다양한 작업 간에 우수한 일반화를 반영하는 고품질 특징을 학습하는 것입니다.

5. 구조 기반 방법

- 이전 방법들은 단일 모델과 단일 매개변수 공간을 전제로 하지만, CL에서 모델 아키텍처를 활용하는 여러 기술도 있습니다.
- 예를 들어, 매개변수 할당은 각 새 작업이 네트워크의 전용 부분 공간을 제공받아 매개변수 파괴적 간섭 문제를 해결하는 방법입니다. 그러나 네트워크가 고정되어 있지 않으면 새로운 작업 수에 따라 네트워크 크기가 증가할 수 있습니다.

## 그리고 CL 모델의 성능을 어떻게 평가합니까?

<div class="content-ad"></div>

CL 모델의 기본 성능은 여러 가지 각도로 측정할 수 있어요 [3]:

- 전반적인 성능 평가: 모든 작업에서의 평균 성능
- 메모리 안정성 평가: 지속적인 훈련 이후 특정 작업의 최대 성능과 현재 성능의 차이를 계산
- 학습 가소성 평가: 모든 데이터를 사용하여 훈련한 경우의 공동 훈련 성능과 CL을 사용하여 훈련한 경우의 성능 차이 측정

## 그럼 모든 AI 연구자들이 왜 즉시 Continual Learning으로 전환하지 않을까요?

만약 과거 훈련 데이터에 접근할 수 있고 계산 비용에 대해 걱정할 필요가 없다면, 처음부터 새롭게 훈련시키는 것이 더 간단해 보일 수 있어요.

<div class="content-ad"></div>

이는 계속적인 훈련 중 모델에서 일어나는 일의 해석 가능성이 아직 제한되어 있기 때문 중 하나의 이유입니다. 처음부터 훈련하는 것이 계속적인 훈련보다 동일하거나 더 나은 결과를 제공한다면, 사람들은 CL 방법의 성능 문제를 이해하는 데 시간을 낭비하는 대신 더 쉬운 방법인 처음부터 재훈련하는 것을 선호할 수 있습니다.

또한 현재 연구는 주로 모델과 프레임워크의 평가에 초점을 맞추는데, 이는 비즈니스가 가질 수 있는 실제 사용 사례를 잘 반영하지 않을 수 있습니다. [6]에서 언급된 대로, 많은 합성 증분 벤치마크는 작업이 자연스럽게 진화하는 실제 상황을 잘 반영하지 않습니다.

마지막으로, [4]에서 지적했듯이, CL 주제에 관한 많은 논문들은 계산 비용보다는 저장 비용에 초점을 두고 있으며, 실제로 과거 데이터를 저장하는 것이 모델을 다시 훈련시키는 것보다 훨씬 더 저렴하고 에너지 소비가 적다는 것입니다.

## CL 모델을 개선해야 하는 이유는 무엇일까요?

<div class="content-ad"></div>

지속적 학습은 현재 AI 모델 중 가장 어려운 병목 현상 중 하나를 해결하기 위해 노력합니다 — 데이터 분포가 시간이 지남에 따라 변화한다는 사실입니다. 재교육은 비용이 많이 들며 대량의 계산이 필요하므로 경제적 및 환경적 관점에서 지속가능한 접근 방식은 아닙니다. 따라서, 미래에는 잘 개발된 지속적 학습 방법이 있으면 모델을 더 많은 사용자 집단이 접근하고 재사용할 수 있게 할 수도 있습니다.

[4]에서 찾아본 결과, 잘 개발된 지속적 학습 방법이 필요하거나 그에 이반할 수 있는 애플리케이션 목록이 있습니다:

- 모델 편집

- 모델의 오류가 자주 발생하는 부분을 선택적으로 편집하여 모델의 다른 부분을 손상시키지 않습니다. 지속적 학습 기술을 사용하면 모델 오류를 훨씬 적은 계산 비용으로 계속 수정할 수 있습니다.

<div class="content-ad"></div>

2. 개별화 및 특수화

- 일반 목적 모델은 때로 특정 사용자에게 더 개별화되도록 조정되어야 합니다. Continual Learning을 통해 모델에 치명적인 망각을 유발하지 않으면서 일부 파라미터만 업데이트할 수 있습니다.

3. 장치 내 학습

- 작은 장치는 제한된 메모리와 계산 자원을 가지고 있으므로, 새로운 데이터가 도착할 때 실시간으로 모델을 효율적으로 학습시킬 수 있는 방법이 있다면, 이 영역에 유용할 수 있습니다.

<div class="content-ad"></div>

4. 웜 스타트를 통한 빠른 재학습

- 모델은 새로운 샘플이 제공되거나 분포가 크게 변할 때 업데이트해야 합니다. 계속적인 학습을 통해 기존 방법보다 효율적으로 진행할 수 있습니다. 이는 새로운 샘플로 영향을 받는 부분만 업데이트하고 처음부터 재학습하지 않기 때문입니다.

5. 강화 학습

- 강화 학습은 에이전트가 종종 정상적이지 않은 환경과 상호 작용하는 것을 포함합니다. 따라서 효율적인 계속적인 학습 방법과 접근법은 이러한 경우에 잠재적으로 유용할 수 있습니다.

<div class="content-ad"></div>

## 더 알아보기

여러분이 알 수 있듯이, 계속적 학습 방법의 영역에서는 아직 많은 개선의 여지가 있습니다. 관심이 있으시다면 아래 자료들로 시작해보세요:

- 소개 강의: [계속적 학습 코스] 강의 #1: ContinualAI의 소개와 동기부여 YouTube에서 시청하기 [강의 링크](https://youtu.be/z9DDg2CJjeE?si=j57_qLNmpRWcmXtP)
- 계속적 학습에 대한 동기에 관한 논문: Continual Learning: Application and the Road Forward [4]
- 계속적 학습의 최신 기술에 관한 논문: Comprehensive Survey of Continual Learning: Theory, Method and Application [3]

질문이나 의견이 있으시면 언제든지 댓글 섹션에서 공유해주세요.

<div class="content-ad"></div>

안녕하세요!

![이미지](/assets/img/2024-08-03-AImodelshaveanexpirydateContinualLearningmaybeananswer_1.png)

# 참고 자료

[1] Awasthi, A., & Sarawagi, S. (2019). Continual Learning with Neural Networks: A Review. In Proceedings of the ACM India Joint International Conference on Data Science and Management of Data (pp. 362–365). Association for Computing Machinery.

<div class="content-ad"></div>

[2] 연속적 AI 위키 소개: 연속 학습 https://wiki.continualai.org/the-continualai-wiki/introduction-to-continual-learning

[3] 왕, 르, 장, 스, 주. (2024). 연속 학습에 대한 포괄적인 조사: 이론, 방법 및 응용. IEEE Transactions on Pattern Analysis and Machine Intelligence, 46(8), 5362–5383.

[4] Eli Verwimp, Rahaf Aljundi, Shai Ben-David, Matthias Bethge, Andrea Cossu, Alexander Gepperth, Tyler L. Hayes, Eyke Hüllermeier, Christopher Kanan, Dhireesha Kudithipudi, Christoph H. Lampert, Martin Mundt, Razvan Pascanu, Adrian Popescu, Andreas S. Tolias, Joost van de Weĳer, Bing Liu, Vincenzo Lomonaco, Tinne Tuytelaars, & Gido M. van de Ven. (2024). 연속 학습: 적용 및 앞으로의 도전과제 https://arxiv.org/abs/2311.11908

[5] 아와스티, 아., & 사라와기, 에스. (2019). 신경망을 활용한 연속 학습: 리뷰. ACM 인도 합동 국제 데이터 과학 및 데이터 관리 컨퍼런스 논문집 (pp. 362–365)에서 발표된 내용. Association for Computing Machinery.

<div class="content-ad"></div>

[6] Saurabh Garg, Mehrdad Farajtabar, Hadi Pouransari, Raviteja Vemulapalli, Sachin Mehta, Oncel Tuzel, Vaishaal Shankar, & Fartash Faghri. (2024). TiC-CLIP: CLIP 모델의 지속적 학습.