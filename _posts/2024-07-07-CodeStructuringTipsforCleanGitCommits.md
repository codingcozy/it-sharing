---
title: "깨끗한 Git 커밋을 위한 코드 구조화 팁"
description: ""
coverImage: "/assets/img/2024-07-07-CodeStructuringTipsforCleanGitCommits_0.png"
date: 2024-07-07 02:07
ogImage:
  url: /assets/img/2024-07-07-CodeStructuringTipsforCleanGitCommits_0.png
tag: Tech
originalTitle: "Code Structuring Tips for Clean Git Commits"
link: "https://medium.com/@anasanjaria/code-structuring-tips-for-clean-git-commits-6aecef68739d"
---

## 코드 리뷰

저는 졸업한 지 얼마 안 된 신입생일 때, 제 Git 커밋은 엉망이었습니다. 여러 변경 사항을 결합하고 ‘readme 업데이트’나 ‘이슈 수정’과 같이 모호한 메시지를 가진 큰 커밋들이었습니다.

산업에 종사하면서 다른 사람들의 풀 리퀘스트를 검토하고 지난 몇 년 동안, 깔끔하고 조직적인 커밋의 중요성을 깨달았습니다.

이 기사에서 제 이야기와 다음과 같은 팁을 공유하겠습니다:

<div class="content-ad"></div>

- 코드를 유의미한 커밋으로 구성하세요.
- 깔끔한 커밋 이력을 유지하세요.

이 부분에 어려움이 있다면, 제 경험이 도움이 될지도 😉.

![CodeStructuringTipsforCleanGitCommits_0](/assets/img/2024-07-07-CodeStructuringTipsforCleanGitCommits_0.png)

# 🤔 왜 이것이 정말 중요한가요?

<div class="content-ad"></div>

'how'에 들어가기 전에 '왜'에 대해 알아보겠습니다. 왜 깔끔한 커밋 기록을 유지해야 하는 건 중요한 걸까요? 왜 우리는 업무를 효율적으로 구성해야 하는 걸까요?

## 효과적인 디버깅

자동화된 테스트도 모두 설치했는데도 당신의 변경 사항이 프로덕션 시스템을 망가뜨린다면 어떨까요? 이런 일이 저도 있었더라구요 😄.

구조화된 커밋과 효율적인 커밋 메시지는 디버깅을 쉽게 만들어 줍니다. 뒤죽박죽인 기록보다는 조직적인 기록에서 원인을 찾는 것이 훨씬 간단하죠.

<div class="content-ad"></div>

## 효과적인 피드백

![Code Structuring Tips for Clean Git Commits](/assets/img/2024-07-07-CodeStructuringTipsforCleanGitCommits_1.png)

풀 리퀘스트를 열 때 동료들에게 솔루션을 제시합니다.

신중하게 구성된 커밋은 리뷰어가 변경 사항을 더 잘 이해하게 도와주어 더 효과적인 피드백을 받을 수 있습니다.

<div class="content-ad"></div>

## 생각 과정을 반영하는 중

의미 있는 커밋으로 코드를 구성하면 당신의 사고 과정과 문제 해결 방식이 반영됩니다. 각 커밋은 이야기의 일부를 전달하며, 다른 사람들(그리고 미래의 본인)이 변경 사항을 이해하기 쉽게 만들어줍니다.

# 💻 더 나은 커밋을 위해 코드 구조화하기

모든 경우에 적합한 공식은 없지만, 내 경험을 바탕으로 여기 몇 가지 팁이 있습니다. 이를 통해 코드를 더 나은 커밋을 위해 구조화하는 데 도움을 줄 수 있습니다.

<div class="content-ad"></div>

```js
📝 중요 사항: 각 커밋이 컴파일되고 CI/CD 파이프라인을 통과하는지 확인하세요.
```

## 원자적 커밋

각 커밋은 단일하고 일관된 작업 단위를 나타내야 하며, 각 변경 사항이 독립적임을 보장합니다. 이것이 원자적 커밋이라는 의미입니다.

예시

<div class="content-ad"></div>

- Refactoring Only: 코드를 리팩터링하는 경우, 버그 수정이나 새로운 기능을 섞지 말아주세요.
- 성능 최적화: 다른 변경 사항 없이 알고리즘을 최적화하는 데 집중해주세요.

이렇게 하면 커밋이 문제를 일으킬 경우 쉽게 되돌릴 수 있고 다른 프로젝트 부분에 영향을 주지 않습니다.

## 인터렉티브 리베이스 — 게임 체인저

커밋을 수정할 수 있다는 사실을 알고 계셨나요? 솔직히 말해서, 저는 몰랐습니다. 깃 인터렉티브 리베이스를 사용하면 깔끔한 기록을 유지하기 위해 커밋의 순서를 바꾸거나 편집하거나 결합할 수 있습니다.

<div class="content-ad"></div>

이것은 특히 커밋이 원자적이지 않은 것을 깨닫게 될 때 유용합니다.

예시 - 만약 실수로 커밋한 내용을 바로 취소하고 싶다면, 리베이스를 사용하여 해당 커밋을 제거하고 기록을 깔끔하게 유지하세요.

## 구현 전 스켈레톤

복잡한 기능을 작업할 때는 작업을 작은 커밋이나 여러 개의 풀 리퀘스트(PR)로 나누세요.

<div class="content-ad"></div>

예시: 비즈니스 로직을 세부 정보 없이 개요화한 스켈레톤 커밋으로 시작하세요:

```js
def process(email: String): Unit = {
  val user = getUser(email)
  val score = calculateScore(user.id)
  storeScore(score)
}

def getUser(email: String): User = ???

def calculateScore(userId: Int): Int = ???

def storeScore(score: BigDecimal): Unit = ???
```

그 후에 각 메서드와 해당 유닛 테스트를 구현하는 개별 커밋을 하세요.

## 설정 변경 사항을 표시하세요

<div class="content-ad"></div>

설정 변경 사항은 중요하며 주의 깊게 검토해야 합니다. 그것들을 별도로 커밋하여 시각적으로 만드세요. 이렇게 함으로써 리뷰어들은 주요 변경 사항에 집중할 수 있게 해줍니다.

## 효과적인 커밋 메시지 작성

효과적인 커밋 메시지는 다른 사람들이 당신의 변경 사항을 이해하는 데 도움이 됩니다. 이 템플릿을 사용하세요.

```js
TICKET-ID 여기에 메시지를 입력하세요

Reason - 이 변경을 왜 하고 있나요?
```

<div class="content-ad"></div>

예시

```js
NOX-123 GitHub Actions로 CI/CD 파이프라인 이전

Jenkins를 GitHub Actions로 변경하여 유지보수 부담을 줄입니다. Jenkins는 자체 관리되는 시스템이며, 이 구성 요소를 제거하려고 합니다.
```

이유를 추가하면 다른 사람들(그리고 미래의 여러분)에게 더 나은 맥락을 제공합니다. 변경 사항이 추가된 이유가 과거에만 관련이 있고 지금은 의미가 없을 수도 있습니다.

# 🌐 실제 시나리오

<div class="content-ad"></div>

다음은 실제 시나리오와 해당 처리 방법에 대한 설명입니다.

## 유닛 테스트 없이 버그 수정하기

상황: 버그를 수정해야 하지만 전혀 유닛 테스트가 없는 경우.

적어도 2개의 커밋으로 작업을 조직화합니다. 다음과 같이:

<div class="content-ad"></div>

- 커밋 #1 — 기능을 수정하지 않고 누락된 단위 테스트(들) 추가
- 커밋 #2 — 버그 수정

이유: 회귀(Regression)를 피하고 기존 기능의 동작을 문서화합니다.

## 제3자 도구의 기본 구성 수정

상황: 로드 밸런서를 사용하여 AWS 클라우드에서 실행되는 웹 애플리케이션이 있습니다. 안정성을 위해 nginx 액세스 로그의 기본 로그 형식을 수정해야 합니다.

<div class="content-ad"></div>

여기 코드의 구조입니다.

- 커밋 #1 — 기본 nginx 구성을 수정하지 않고 버전 관리에 추가합니다. 스냅샷 역할을 합니다.
- 커밋 #2 — 원하는대로 구성을 조정합니다.

이유: 리뷰어가 변경 사항을 정확히 이해할 수 있도록 하기 위함입니다.

## 작업 중인 커밋

<div class="content-ad"></div>

상황: 작업이 아직 끝나지 않았지만 오늘은 여기까지 하려고 합니다.

진행 중인 모든 작업을 TICKET-ID WIP로 커밋하고 원격 저장소에 푸시합니다.

이유: 1) 작업을 잃을 걱정이 없습니다. 2) 제 부재 시 다른 사람이 긴급한 경우 이어서 할 수 있습니다.

읽어주셔서 감사합니다! 이 조언들이 깔끔하고 조직화된 커밋 기록을 유지하는 데 도움이 되기를 바랍니다. 궁금한 사항이 있거나 추가 팁이 있다면 아래 댓글에서 자유롭게 공유해 주세요. 즐거운 코딩하세요!

<div class="content-ad"></div>

만약 이 게시물을 즐기셨다면, 다음 내용도 마음에 드실 수 있을 거에요:

- 코드 리뷰어로 배운 교훈
- 강력한 코드 리뷰 문화 구현하기
