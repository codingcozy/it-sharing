---
title: "Git Rebase 군단, 일어서라"
description: ""
coverImage: "/assets/img/2024-07-23-GitRebaseGangRiseUp_0.png"
date: 2024-07-23 22:06
ogImage: 
  url: /assets/img/2024-07-23-GitRebaseGangRiseUp_0.png
tag: Tech
originalTitle: "Git Rebase Gang, Rise Up"
link: "https://medium.com/@seniorbrogrammer/git-rebase-gang-rise-up-ae31487d9436"
---


음, 나는 Crowdstrike에서 Windows를 사용하지 않아서 안전 모드에서 머신을 부팅하는 서버 룸에 가둬져 있지 않아. 따라서 이제 소프트웨어 엔지니어링에서 가장 중요한 것을 논의할 시간이다: 버전 관리.

git을 즐기는 사람으로서, 다른 사람들이 리베이스에 좀 더 존중을 베풀어주기 시작하는 건 참 좋은 일이야. "나는 여러 작은 커밋을 사용하고 병합 충돌을 싫어해"에 관한 기술적인 심층 탐구는 다 필요 없어. 여기서 진짜 개발자들에 의한 실질적인 소프트웨어 개발이야.

함께 몇 단락을 나란히 나아가 보자.

<div class="content-ad"></div>

알겠어요. 이 문제에 대해 솔직하게 이야기해볼게요. Rebase를 로컬에서만 사용하고, 변경 사항을 푸시합니다. 병합 충돌이 발생하면 최신 변경 사항을 병합하기 위해 메인에서 내려받고(이명이 master였던 시절을 겪었습니다), 그리고 장황할지언정 충돌을 처리합니다.

맞아요, 병합 충돌은 삶의 일부죠. 모든 것을 강력하고 강력하다고 해서 어떤 명령을 피하려고 하지 마세요. Crowdstrike은 방금 세계에 수십 억 달러를 소비시켰어요.

강력한 명령어를 다룰 수 있을 거예요, 작은 형제.

저는 왜 사람들이 이 명령을 싫어하거나 두려워할지 이해가 안 돼요. 실행한 다음 단순히 git push --force-with-lease를 실행하면 되는데 이게 왜 어렵다고 생각하는지 몰라요. --force-with-lease 명령은 원격 변경 사항을 식별하고 푸시를 차단합니다. 그렇지 않으면 푸시할 수 있어요.

<div class="content-ad"></div>

그렇게 쉬워요.

Git의 각종 복잡성을 두려워하지 마세요. Git 게임을 플레이하고 Git에서 편안해지세요. 리베이스가 세상의 끝은 아니니까, 언제든지 Git 작업에서 도움을 요청하세요. 다른 명령어처럼, 리베이스도 조심하지 않으면 위험할 수 있어요. 배우고 있을 때는 항상 백업 브랜치를 만들어두세요.

독서해 주셔서 감사합니다.