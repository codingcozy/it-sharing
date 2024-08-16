---
title: "최신 RAG 10 교정 검색 증강 생성CRAG에 대해 알아보기"
description: ""
coverImage: "/assets/img/2024-07-26-AdvancedRAG10CorrectiveRetrievalAugmentedGenerationCRAG_0.png"
date: 2024-07-26 12:05
ogImage: 
  url: /assets/img/2024-07-26-AdvancedRAG10CorrectiveRetrievalAugmentedGenerationCRAG_0.png
tag: Tech
originalTitle: "Advanced RAG 10 Corrective Retrieval Augmented Generation CRAG"
link: "https://medium.com/ai-advances/advanced-rag-10-corrective-retrieval-augmented-generation-crag-3f5a140796f9"
isUpdated: true
updatedAt: 1723816817515
---



이 기사는 흔한 시나리오인 개방형 시험에 참가하는 것으로 시작합니다. 일반적으로 세 가지 전략이 있습니다:

- 방법 1: 익숙한 주제에 대해 빠르게 답변하십시오. 익숙하지 않은 경우 참고서를 참조하십시오. 관련 섹션을 빠르게 찾아내고 정리하여 요약한 후, 답안을 시험지에 작성하십시오.
- 방법 2: 각 주제에 대해 책을 참조하십시오. 관련 섹션을 식별하고 마음속으로 요약한 후, 시험지에 답변을 쓰십시오.
- 방법 3: 각 주제에 대해 책을 참고하고 관련 섹션을 식별하십시오. 의견을 형성하기 전에 수집된 정보를 옳은것, 틀린것, 모호한 것의 세 가지 그룹으로 분류하십시오. 각 유형의 정보를 개별적으로 처리하십시오. 그런 다음, 이 처리된 정보를 기반으로 요약하여 정리하여 마음속으로 작성하십시오. 시험지에 답변을 쓰십시오.

방법 1은 자기-RAG를 포함하며, 방법 2는 고전적인 RAG 프로세스입니다.

마지막으로, 이 기사에서는 말씀하신 대로 올바른 복구 강화 생성(CRAG)이라는 방법 3이 소개됩니다.

<div class="content-ad"></div>

# CRAG의 취지

![이미지](/assets/img/2024-07-26-AdvancedRAG10CorrectiveRetrievalAugmentedGenerationCRAG_0.png)