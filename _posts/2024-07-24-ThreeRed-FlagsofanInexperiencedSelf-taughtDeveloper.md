---
title: "경험 없는 독학 개발자의 위험 신호 3가지"
description: ""
coverImage: "/assets/img/2024-07-24-ThreeRed-FlagsofanInexperiencedSelf-taughtDeveloper_0.png"
date: 2024-07-24 12:11
ogImage: 
  url: /assets/img/2024-07-24-ThreeRed-FlagsofanInexperiencedSelf-taughtDeveloper_0.png
tag: Tech
originalTitle: "Three Red-Flags of an Inexperienced Self-taught Developer"
link: "https://medium.com/gitconnected/three-red-flags-of-an-inexperienced-self-taught-developer-d9f72e054224"
---



![image](/assets/img/2024-07-24-ThreeRed-FlagsofanInexperiencedSelf-taughtDeveloper_0.png)

처음 시작할 때 대부분의 공부 중인 개발자들은 어려움을 겪습니다.

다른 사람들처럼, 제가 발전을 방해한 몇 가지 이상한 습관이 있었어요.

- 지루한 원리를 무시하지 않기
- 간단함을 위한 최적화하지 않기
- "완료"와 같은 커밋 메시지 작성하기


<div class="content-ad"></div>

여기서는 대부분의 자기 학습 개발자들이 경력 초반에 하는 몇 가지 실수를 발견했습니다.

이를 통해 당신이 개발자로서 살아남을 뿐만 아니라 번창할 수 있도록 도와줄 것입니다.

## 1. DRY 원칙을 무시하지 않는다

DRY.

<div class="content-ad"></div>

DRY.

DRY.

DRY.

프로그래밍 비디오를 보면 "반복하지 말아라"라는 용어를 듣게 될 거예요.

<div class="content-ad"></div>

프로그래밍 기사를 읽어보세요. 항상 같은 용어를 반복해서 볼 수 있을 거에요.

시니어 개발자에게 조언을 구해보세요. 항상 이 조언을 받게 될 거에요.

네 해 전, 프로그래밍 관련 일반적인 수업 몇 가지를 공유하기 시작했을 때 DRY에 대한 제한적 경험을 가지고 있었습니다.

프로그래밍과 관련된 많은 기사를 읽은 후, 모든 프로그래머가 DRY 원칙을 따라야 한다고 믿어왔었죠.

<div class="content-ad"></div>

항상 DRY(Don't Repeat Yourself) 원칙을 적용하면 코드베이스가 더 간소화되고 효율적으로 작동할 것으로 생각했습니다.

그러나 시간이 지남에 따라 항상 DRY를 적용하는 것이 모든 경우에 유용하지는 않다는 사실을 깨달았습니다.

일부 프로그래머는 코드를 어떻게 DRY하게 만들 수 있을지 항상 고민합니다.

모두 DRY에 대해 말하는데, 그들의 코드는 반드시 DRY해야 한다고 생각합니다. 하지만 DRY에 대해 생각하기 전에 프로그래머들은 여기서 DRY가 의미 있는지 물어봐야 합니다.

<div class="content-ad"></div>

## DRY 원칙의 문제

전문적으로 코드를 작성하기 시작하는 대부분의 프로그래머들이 DRY 원칙을 너무 심각하게 받아들입니다.

DRY를 너무 심각하게 받아들여 필요할 때라도 무시하지 않습니다.

코드 중복을 줄이기 위해 복잡한 추상화를 유발하고 유지보수가 힘들어지게 만드는 방식으로 변경하는 경우가 많습니다.

<div class="content-ad"></div>

아무도 여기에 대해 이야기하지 않죠. 글을 쓰거나 비디오를 만드는 모든 프로그래머들은 좋은 부분에 대해서만 이야기하죠.

한 예를 들어서 살펴봅시다.

이렇게 두 개의 함수를 작성했다고 가정해봅시다:

```js
function showAlert(text) {
    console.log("Alert: " + text);
}

function showError(text) {
    console.error("Error: " + text);
}
```

<div class="content-ad"></div>

코드를 DRY하게 만들기로 결정했다면, 쉽게 변경하여 하나의 함수로 변환할 수 있다.

```js
function showText(type, text) {
    switch(type) {
        case 'alert':
            console.log("Alert: " + text);
            break;
        case 'error':
            console.error("Error: " + text);
            break;
        default:
            throw new Error("Invalid message type");
    }
}
```

DRY 원칙을 따라 원하는 변경 사항을 적용했습니다.

이제 나중에 사용자와 관리자 역할에 기반한 경고 메시지에 몇 가지 추가 텍스트를 코드베이스에 추가해야 한다고 상상해봅시다.

<div class="content-ad"></div>

코드를 변경해 봅시다.

```js
function showText(type, text, role) {
    switch(type) {
        case 'alert':
            if (role === 'admin') {
                console.log("[Admin] Alert: " + text);
            } else {
                console.log("Alert: " + text);
            }
            break;
        case 'error':
            if (role === 'admin') {
                console.error("[Admin] Error: " + text);
            } else {
                console.error("Error: " + text);
            }
            break;
        case 'warning':
            if (role === 'admin') {
                console.warn("[Admin] Warning: " + text);
            } else {
                console.warn("Warning: " + text);
            }
            break;
        default:
            throw new Error("Invalid message type");
    }
}
```

이 한 가지 함수가 이제 여러 가지 책임을 처리합니다. 어떻게 생각하세요?

앞의 코드는 앞으로 유지보수하기 어려울까요?

<div class="content-ad"></div>

이제 같은 경고 메시지를 첫 번째 세트에서 소개해 보겠습니다.

```js
function showAlert(text, role) {
    if (role === 'admin') {
        console.log("[Admin] Alert: " + text);
    } else {
        console.log("Alert: " + text);
    }
}

function showError(text, role) {
    if (role === 'admin') {
        console.error("[Admin] Error: " + text);
    } else {
        console.error("Error: " + text);
    }
}

function showWarning(text, role) {
    if (role === 'admin') {
        console.warn("[Admin] Warning: " + text);
    } else {
        console.warn("Warning: " + text);
    }
}
```

이제 이를 살펴봅시다.

위 방법에서 각 메시지 유형의 코드는 독립적입니다. 위의 세 가지 함수 코드를 유지하는 것은 하나의 함수에 비해 쉽고 관리하기 쉬울 것입니다.

<div class="content-ad"></div>

예시를 간단하게 유지했어요. 그러나 DRY 원칙을 따르는 코드가 점점 복잡해질 수 있다고 생각해보세요. 

따라서 남의 충고를 맹목적으로 따르는 것이 바로 옳은 일은 아니에요.

DRY 원칙을 따르는 것이 도움이 된다면서도 때로는 완전히 무시하는 것도 중요해요.

# 2. “나중에 돌아와서 처리할게요”

<div class="content-ad"></div>

코딩을 시작할 때 이상한 태도로 많은 정신 에너지를 낭비했어요.

미래에 이 태도가 심한 고통을 야기할 것을 알았다면, 피하려고 노력했을 텐데요.

처음부터 코딩을 배울 때 어려운 주제에 직면할 때마다 피하려고 했어요.

나중에 돌아와서 다시 해보겠다고 자기 자신에게 말했었죠.

<div class="content-ad"></div>

저는 CSS를 배우고 있어요.

버튼을 스타일링하는 방법과 마진을 추가하는 방법과 같은 개념을 완료했어요. 그 다음으로 Flexbox를 배우기 시작했어요.

Flexbox 개념이 이해하기 어렵다고 느꼈다면, 무시하고 과제를 완료하기 위해 해결책을 찾았어요. 이미 과제를 완료했기 때문에 Flexbox를 조금은 이해했다고 생각해요.

나중에 돌아와서 모든 것을 배우기로 결정할 거예요. 이어서 Flexbox에 대해 모든 것을 배울 거에요.

<div class="content-ad"></div>

하지만 이상적인 때가 되어서 돌아가서 개념을 배우는 것은 결코 다시 오지 않았어요.

저만의 경험이 아닙니다.

스스로 가르치며 프로그래밍하는 많은 분들이 이런 생각을 가지고 있답니다. "나중에 다시 돌아와서 할게."

이런 마음가짐으로 시작된 이른바 자신을 가르치는 시절이후에는 나중에 전문가로써 영향을 미치기 시작하지요.

<div class="content-ad"></div>

## 대부분의 프로그래머들이 "나중에 다시 하기"를 믿는 이유는 무엇인가요?

나는 예전에 "나중에 다시 하기"를 믿는 쪽이었어요.

지금 나에게 왜 그랬냐고 물으면, 그 이유는 하나 뿐이에요.

우리 프로그래머들이 어려운 개념에 막혔을 때, 새로운 관점으로 문제에 더 효과적으로 접근할 수 있다고 느끼기 때문이에요.

<div class="content-ad"></div>

하지만 이러한 마음가짐은 우리에게 도움이 되지 않아요.

문제에 나중에 다시 접하게 되더라도, 문제를 이해하기가 더 어려워져요. 왜냐하면 우리는 그 상황을 기억하지 못하기 때문이에요.

한 번 입력 필드와 관련된 문제를 해결하지 못해 고민한 적이 있어요. 입력 필드 다음의 버튼이 입력 필드의 길이와 일치하지 않았어요.

이는 작은 문제일 줄 알았어요. 코드베이스에 추가할 것이 더 많기 때문에, 그 문제는 뒤로 미루고 다른 기능을 먼저 추가한 뒤 나중에 돌아올 것으로 계획했어요.

<div class="content-ad"></div>

이제 다른 작업을 마치고 작은 문제를 해결하러 돌아왔을 때,

처음부터 문제가 왜 발생했는지와 관련된 것을 전혀 기억하지 못했어요. 처음 계획은 눈에 보이지 않게 되었어요.

그 문제를 해결하기 위해 코드베이스를 다시 훑어야 했어요. 코드베이스에 많은 새 코드를 추가했기 때문이죠.

어느 정도 10분 정도면 해결되어야 했던 문제가... 몇 시간 이상 소요되었었지요.

<div class="content-ad"></div>

## 스스로 가르친 개발자들은 이것을 알지 못합니다.

개인 프로젝트용 코드를 작성할 때 "나중에 돌아올게"라고 말하기가 쉽습니다.

대부분의 경우 우리는 개인 프로젝트를 가볍게 여기기 때문에 프로젝트로 돌아가지 않습니다. 아주 가끔 우리는 프로젝트를 되돌아가서 변경을 가합니다.

이는 프로페셔널한 세계에서 프로젝트로 돌아가서 변경을 가하기 쉬울 것이라고 가정해서는 안 된다는 뜻입니다.

<div class="content-ad"></div>

이걸 배우는 데 많은 시간이 걸렸어요.

이 여정 중 몇 차례, 나 자신에게 나중에 이 작은 문제들에 돌아올 것이라고 말하며 발견했어요.

하지만 코드베이스를 계속해서 추가한다면, 이 작은 문제가 심각한 문제로 점점 커져가는 것을 심지어 깨닫지 못할 수도 있어요.

어려운 문제에 직면했을 때 "나중에 다시 돌아와 볼 거야"라고 말하기보다요.

<div class="content-ad"></div>

"나중에 작은 문제로 돌아오지 않을 거에요. 그 문제를 바로 해결하거나, 몇 시간 안에 다시 돌아올 시간을 정해봐요.

미래의 자신이 더 다룰 문제가 많기 때문이죠.

# 3. 그들은 버전 관리를 별로 안 중요하다고 생각해."

몇 일 전에 개인 프로젝트를 하고 있었죠.

<div class="content-ad"></div>

그 프로젝트에서 제 목표는 JavaScript 개념 몇 가지를 다시 공부하는 것이었어요.

프로젝트를 구축하기 시작했어요.

이 프로젝트에 협력자가 없을 것이라 생각해서 버전 관리를 사용하지 않아야겠다고 생각했어요.

혼자 변경 사항을 쉽게 관리할 수 있는데 왜 불필요한 부담을 가지고 있어야 하죠? 몇 일 동안 제 여가 시간에 프로젝트를 완료할 계획이었어요.

<div class="content-ad"></div>

이 시점에서 말씀드리고 싶은 점은 이러한 태도가 나만의 문제가 아니라는 것입니다.

많은 자학 프로그래머, 혼자 개발하는 사람, 프리랜서들이 버전 관리에 대해 이러한 태도를 보일 것입니다.

개인 프로젝트를 진행하거나 1인 팀인 경우, 그들은 버전 관리를 심각하게 취하지 않습니다.

버전 관리를 부담으로 여기고, 자신의 코드에 신경을 쓰기 때문에 아무런 문제가 발생하지 않을 것이라고 여기는 것이죠.

<div class="content-ad"></div>

하지만 이런 종류의 태도는 항상 그들에게 해를 끼칩니다.

## 내 프로젝트에 무엇이 일어났나요?

처음 몇 일 동안 내 프로젝트는 잘 진행되고 있었습니다.

몇 가지 기능을 추가할 수 있었고, 몇몇 온라인 비디오를 시청하면서 꼬일 때도 있었습니다.

<div class="content-ad"></div>

하지만 어느 날, 전혀 예상하지 못한 일이 발생했어요.

마지막 버전에 몇 가지 변경을 하고 나서 되돌아가야 하는 상황이 발생했어요. 코드를 다시 한 다음 이전 버전으로 돌아가는 것은 어려운 일은 아니었지만, 진짜 문제는 시간이었어요.

나에게 남은 자유 시간은 몇 일밖에 없었어요. 그리고 이전 버전으로 돌아가려면 몇 일을 투자해야 했어요.

그 순간에 내 노트북 앞에 앉아 있었을 때, 저는 큰 후회에 사로잡혔어요. 왜 GitHub을 사용하지 않았을까요?

<div class="content-ad"></div>

만약 사용했다면 제 시간 내에 완료했을 텐데요. 추가 시간을 투자해서 전체 코드를 다시 작성해야겠어요.

그 순간에 우리는 우리의 최고의 날을 위해 버전 관리를 사용하지 않는다는 것을 깨달았어요. 하지만 최악의 날을 위해 사용한다는 거죠.

## 버전 관리를 심각하게 여기지 않는다는 징후는 무엇인가요?

버전 관리를 진지하게 여기지 않는 개발자들을 많이 봤어요.

<div class="content-ad"></div>

대부분은 이를 오버헤드로 간주합니다.

그들 중 일부는 프로젝트를 위해 몇 일 동안 버전 관리를 무시할 수 있다면 시간 내에 완료할 수 있다고 생각합니다.

그래서 그들은 큰 커밋을 하게 됩니다.

버전 관리를 사용하지 않는 것은 진지하게 대하지 않는 지표 중 하나입니다.

<div class="content-ad"></div>

버전 관리를 진지하게 다루지 않고 있는 사람들의 몇 가지 조짐을 발견했어요.

- 빈도가 낮은 커밋
- 대규모 커밋
- "완료"와 같은 부적절한 커밋 메시지
- 주 브랜치에 직접 커밋
- 근본적인 문제 해결을 하지 않음
- .gitignore를 사용하지 않음

프로그래머가 버전 관리를 진지하게 다루지 않는다는 조짐 중 일부에요.

언제나 코드의 오래된 버전들이 디버깅에 필요한 경우가 많으며, 프로젝트의 역사를 이해하는 것은 미래 버전을 작성하는 데 도움이 될 거예요.

<div class="content-ad"></div>

# 뉴스레터에서 더 많은 정보 확인해보세요:

- I Spent 17 Days Studying Two Programmers Who Built a $1 Billion Company — Here’re Their Rules To Build a Startup
- Meet a Programmer Who Rejected a $10,000,000,000 Acquisition Offer From Microsoft