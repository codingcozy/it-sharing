---
title: "LLM 기반 RAG를 활용한 최신 GENAI 기법 소개"
description: ""
coverImage: "/assets/img/2024-08-03-ItstimetoupdatehowwedogenAILLM-basedRAG_0.png"
date: 2024-08-03 20:54
ogImage: 
  url: /assets/img/2024-08-03-ItstimetoupdatehowwedogenAILLM-basedRAG_0.png
tag: Tech
originalTitle: "Its time to update how we do genAI LLM-based RAG"
link: "https://medium.com/@paul.k.pallaghy/its-time-to-update-how-we-do-genai-llm-based-rag-8b8528d71bb8"
isUpdated: true
updatedAt: 1723816643147
---



요즘 RAG에는 몇 개의 청크(크기도)를 사용해야 할까요? (RAG 아키텍처 아카이브의 다른 변형들은 일단 제외하고 생각해 봅시다).

확실히 더 크고 저렴한 컨텍스트 창을 사용하면 두 가지를 동시에 열 수 있는 가능성이 있습니다.

![링크](/assets/img/2024-08-03-ItstimetoupdatehowwedogenAILLM-basedRAG_0.png)

저는 항상 10x 512 토큰을 사용해 왔었어요 (대부분의 사이트가 권장하는 약 3배 크기보다 훨씬 좋다고 생각합니다).

<div class="content-ad"></div>

요점은:

1. 사용자 요청은 주로 넓은 비교 또는 *목록*을 원합니다. LLM에게 3개의 청크만 반환하는 것은 매우 낮습니다. 이는 중요하지 않은 쿼리의 상당 부분에 대해 거의 쓸모 없는 응답으로 이어질 수 있습니다.

2. 임베딩 기반 벡터 검색 점수에서 매우 높은 의미론적 정확도를 기대하지 않아도 됩니다. 임베딩 일치는 물론 LLM 자체만큼 똑똑하지 않습니다. 이는 순위가 반드시 완벽하지 않으므로 '상위 3개'는 조금 나이브할 수 있습니다.

그리고:

<div class="content-ad"></div>

3. 우리는 LLM에 더 많은 맥락을 제공하고 싶어요. 

하지만 벡터 검색 시 더 큰 청크로 의미를 희석시킬 가능성을 고려해야 합니다.

따라서 청크 크기를 너무 크게 증가시키지는 못해요.

하지만 더 많은 청크를 반환하면 희석 요인이 줄어들 것으로 보입니다.

<div class="content-ad"></div>

LLM 비용을 줄이면, 우리는 청크 크기도 증가시킬 수 있어요. LLM에 충분한 청크를 전달한다면요.

요즘에는 RAG에서 10배 크기의 1K로 훨씬 나은 결과를 얻고 있어요.

(사실, 제가 원하는 건 512 크기의 청크를 기반으로 검색하지만 그 주변에 LLM에 2K 크기의 청크를 반환하는 거예요).

아무튼, 이 1–3 전략은 LLM에게 더 많은 맥락과 콘텐츠가 있는 각 청크(그리고 더 다양한 소스에서 나온 청크)를 가지게 해줘요 (그리고 선택과 불러내기를 할 수 있어요).

<div class="content-ad"></div>

그래도 LLM은 의사 결정 단계에서 정말 잘 작동해. 벡터 검색 보다 확실히 더 나은 결과를 얻어.

비용 중심 팀원들로부터 약간의 저항을 받고 있어, 하지만 내 의견은 비용이 매우 빨리 하락하고 결과가 확연히 향상되고 있다는 점이야.

의견이 있나요?

네 실험은?