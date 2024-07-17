---
title: "ChatGPT가 Cypress 퀴즈를 해결할 수 있을까"
description: ""
coverImage: "/assets/img/2024-07-09-CanChatGPTsolveCypressQuiz_0.png"
date: 2024-07-09 08:20
ogImage:
  url: /assets/img/2024-07-09-CanChatGPTsolveCypressQuiz_0.png
tag: Tech
originalTitle: "Can ChatGPT solve Cypress Quiz?"
link: "https://medium.com/@elaichenkov/can-chatgpt-solve-cypress-quiz-e1d377ca1a17"
---

![CanChatGPTsolveCypressQuiz](/assets/img/2024-07-09-CanChatGPTsolveCypressQuiz_0.png)

요즘, 저는 재미있고 도전적인 방식으로 Cypress 지식을 테스트하는 Cypress 퀴즈를 출시했어요. 아직 들어보지 못했다면, 이 기회를 놓치지 마세요. 한편으로, OpenAI는 최신 주력 모델인 GPT-4o를 소개했어요. AI 기술과 테스트 도구의 열렬한 지지자로서, 저는 ChatGPT가 Cypress 퀴즈의 문제들과 어떻게 해결되는지 확인하기로 결정했어요.

이로써 흥미로운 실험이 시작됩니다: ChatGPT가 고급 언어 처리 능력을 갖고 있기 때문에 Cypress 테스트의 복잡성을 다룰 수 있을까요? 더 중요한 것은, QA 엔지니어들이 AI가 그들의 일자리를 대체할까 염려해야 할까요? 이러한 질문들을 탐색하기 위해, 우리는 ChatGPT를 Cypress 퀴즈의 일련의 질문들로 테스트하고 그 대답을 분석할 거에요. 함께 알아보도록 하죠.

## 질문 1

<div class="content-ad"></div>

![CanChatGPTsolveCypressQuiz](/assets/img/2024-07-09-CanChatGPTsolveCypressQuiz_1.png)

ChatGPT 설명:
Cypress에서 .should()은 단언문(assertions) 및 단언문이 충족될 때까지 재시도하는 데 사용되며, .then()은 단순히 콜백(callback)으로 명령 체인이 완료될 때 즉시 실행됩니다.

테스트 #1에서 cy.wrap(`foo`).should((foo) => cy.log(foo))를 사용하면 실패합니다. 왜냐하면 should는 조건이 참(true)이어야 하며 조건이 충족될 때까지 계속 재시도하기 때문입니다. 그러나 cy.log(foo)는 확인할 수 있는 값을 반환하지 않기 때문에 조건을 충족시키지 못하고 테스트가 실패합니다.

테스트 #2에서 cy.wrap(`foo`).then((foo) => cy.log(foo)); 는 `foo`를 성공적으로 기록할 것입니다. 왜냐하면 then은 충족되어야 하는 조건을 필요로 하지 않고 제공된 함수를 단순히 실행하기 때문입니다.

<div class="content-ad"></div>

ChatGPT 답변: D — 정답 ✅

## 질문 #2

![이미지](/assets/img/2024-07-09-CanChatGPTsolveCypressQuiz_2.png)

ChatGPT 설명:
테스트는 연속적으로 두 가지 다른 URL을 방문합니다. 두 번째 cy.visit(`https://cypressquiz.com/`)는 https://cypress.io를 방문한 것을 덮어씁니다. 따라서 단언문은 https://cypressquiz.com의 콘텐츠를 기반으로 수행되며, `h1` 요소가 `Quiz` 텍스트를 포함하는지 확인합니다.

<div class="content-ad"></div>

cypressquiz.com의 'h1' 요소에 '퀴즈' 텍스트가 포함되어 있으므로 시험이 통과해야 합니다.

ChatGPT 답변: A — 올바르지 않음 ❌

## 문제 #3

<img src="/assets/img/2024-07-09-CanChatGPTsolveCypressQuiz_3.png" />

<div class="content-ad"></div>

이번 문제에서는 음수 인덱스로 배열에 액세스하면 undefined가 반환됩니다. 음수 인덱스는 유효하지 않기 때문입니다. 따라서 `cy.wrap([`JS`, `TS`]).its(-1)`는 undefined를 반환할 것입니다.

단언문인 `should('eq', 'TS')`는 음수 인덱스의 값이 `TS`이라고 예상하지만, 실제로는 그렇지 않기 때문에 단언 오류가 발생합니다.

ChatGPT 답변: C — 정답 ✅

## 문제 #4

<div class="content-ad"></div>

마크다운 형식으로 변경된 내용입니다:

![이미지](/assets/img/2024-07-09-CanChatGPTsolveCypressQuiz_4.png)

ChatGPT 설명:
이 테스트에서는 cy.wrap(`foo`)가 문자열 foo를 래핑하고 Cypress 명령 체인을 통과시킵니다. .should() 메서드는 단언을 위해 사용되며 제공된 함수가 통과할 때까지 재시도합니다. 단언에서 expect(foo).to.eq(`foo`)는 래핑된 값이 `foo`인지 확인하여 통과합니다.

.should() 콜백 내의 return 문은 boo를 반환하며 다음 명령인 .then() 메서드로 전달되어, .then() 메서드에서 expect(boo).to.eq(`boo`)가 값이 boo인지 확인하여 통과합니다.

ChatGPT 대답: A — 부정확 ❌

<div class="content-ad"></div>

## 질문 #5

<img src="/assets/img/2024-07-09-CanChatGPTsolveCypressQuiz_5.png" />

ChatGPT 설명:
Cypress의 .contains() 메서드는 대소문자를 구분합니다. 이는 요소 내 텍스트 콘텐츠를 검색할 때 대문자와 소문자를 구별한다는 것을 의미합니다. 따라서 cy.contains(`capital sentence`)는 cy.contains(`Capital Sentence`)와 같지 않을 것입니다.

ChatGPT 답변: A — 정답 ✅

<div class="content-ad"></div>

## 질문 #6

![이미지](/assets/img/2024-07-09-CanChatGPTsolveCypressQuiz_6.png)

ChatGPT 설명:
Cypress의 .go() 메소드는 브라우저의 기록을 탐색하는 데 사용됩니다. .go()에 음수를 전달하면 브라우저의 기록을 지정된 페이지 수만큼 뒤로 이동합니다.

따라서 cy.go(-1)은 브라우저의 기록에서 한 페이지를 뒤로 이동합니다.

<div class="content-ad"></div>

ChatGPT 답변: A — 정답입니다 ✅

## 질문 #7

<img src="/assets/img/2024-07-09-CanChatGPTsolveCypressQuiz_7.png" />

ChatGPT 설명:
Cypress.env() 메서드는 환경 변수를 가져오거나 설정하는 데 사용됩니다. 이 코드에서 Cypress.env(`Not a number`, NaN)은 환경 변수 `Not a number`를 NaN으로 설정합니다. Cypress.env(`Not a number`)를 사용하여 이 변수를 검색하면 NaN이 반환됩니다.

<div class="content-ad"></div>

JavaScript에서 typeof 연산자는 피연산자의 타입을 반환합니다. NaN의 타입은 숫자입니다.

ChatGPT 답변: A — 정답 ✅

## 질문 #8

![이미지](/assets/img/2024-07-09-CanChatGPTsolveCypressQuiz_8.png)

<div class="content-ad"></div>

ChatGPT 설명:
제공된 코드에서 .wrap() 메서드를 사용하여 함수 addToStack을 포함하는 배열을 래핑합니다. 그런 다음 .invoke() 메서드가 인수 0과 4로 호출됩니다. .invoke() 메서드는 래핑된 배열의 인덱스 0에 있는 함수를 인수로서 호출합니다.

이는 addToStack(4)가 호출되어 4를 stack 배열에 푸시하게 됨을 의미합니다. 따라서 .invoke() 호출 후에는 stack 배열이 [4]가 됩니다.

마지막으로 console.log(stack)은 stack 배열인 [4]를 로깅할 것입니다.

ChatGPT 정답: B — 부적절한 답 ❌

<div class="content-ad"></div>

## 질문 #9

![이미지](/assets/img/2024-07-09-CanChatGPTsolveCypressQuiz_9.png)

ChatGPT 설명:
Cypress에서 여러 요소를 선택할 때 cy.get(`li`)를 사용하면 선택한 모든 요소가 표시될 경우 .should(`be.visible`) 어서션은 통과됩니다. Cypress 어서션인 .should(`be.visible`)은 기본적으로 선택된 모든 요소에 적용됩니다.

ChatGPT 답변: C — 잘못된 답 ❌

<div class="content-ad"></div>

## 질문 #10

![이미지](/assets/img/2024-07-09-CanChatGPTsolveCypressQuiz_10.png)

ChatGPT 설명:
Cypress에서 beforeEach는 테스트 내부에서 this를 사용할 수 있는 별명을 설정합니다. 그러나 화살표 함수에서 this의 값은 일반 함수와는 다릅니다. 화살표 함수에는 자체 this 컨텍스트가 없으며, 주변 렉시컬 컨텍스트로부터 this를 상속받습니다.

테스트 #1에서 화살표 함수는 자체 this를 갖지 않으므로 this.text는 정의되지 않아 테스트가 실패합니다.

<div class="content-ad"></div>

테스트 #2에서는 beforeEach에서 설정된 별칭값을 올바르게 참조하는 자체 this 컨텍스트를 가진 일반 함수가 사용되어, cy.log(this.text)를 통해 `some text`를 출력할 수 있습니다.

ChatGPT 답변: D — 정답 ✅

## 질문 #11

<img src="/assets/img/2024-07-09-CanChatGPTsolveCypressQuiz_11.png" />

<div class="content-ad"></div>

ChatGPT 설명:
Cypress에서 .should(`not.be.visible`) 어설션은 선택한 모든 요소에 적용됩니다. 이는 이 어설션이 통과하려면 선택한 모든 요소가 보이지 않는 조건을 만족해야한다는 것을 의미합니다.

따라서 만약 여러 개의 li.hidden 요소가 있다면, 이 어설션이 통과되려면 그 모든 요소가 보이지 않아야 합니다.

ChatGPT 답변: C — 정확함 ✅

## 질문 #12

<div class="content-ad"></div>

![이미지](/assets/img/2024-07-09-CanChatGPTsolveCypressQuiz_12.png)

ChatGPT 설명:
Cypress에서 .eq() 메서드는 특정 인덱스의 요소를 선택하는 데 사용됩니다. .eq() 메서드에 음수를 전달하면 목록 끝부터 요소를 선택합니다.

따라서 cy.get(`li`).eq(-1)은 마지막 `li` 요소를 가져옵니다.

ChatGPT 답변: C — 정답 ✅

<div class="content-ad"></div>

## 질문 #13

![이미지](/assets/img/2024-07-09-CanChatGPTsolveCypressQuiz_13.png)

ChatGPT 설명:
사이프레스에서 훅과 테스트 블록의 실행 순서는 다음과 같습니다:

- before 훅은 모든 테스트 이전에 한 번 실행됩니다.
- beforeEach 훅은 각 테스트 이전에 실행됩니다.
- it 테스트 자체가 실행됩니다.
- afterEach 훅은 각 테스트 이후에 실행됩니다.
- after 훅은 모든 테스트 이후에 한 번 실행됩니다.

<div class="content-ad"></div>

주문이 있어요:

- beforeEach(() =` cy.log(1));는 각 테스트가 실행되기 전에 실행되어서 1을 기록합니다.
- before(() =` cy.log(2));는 모든 테스트를 실행하기 전에 한 번 실행되어서 2를 기록합니다.
- 해당 테스트는 3을 기록합니다.
- after(() =` cy.log(4));는 모든 테스트를 실행한 후에 한 번 실행되어서 4를 기록합니다.
- afterEach(() =` cy.log(5));는 각 테스트가 실행된 후에 실행되어서 5를 기록합니다.

따라서, 결과는 2, 1, 3, 5, 4가 될 것입니다.

<div class="content-ad"></div>

## 질문 #14

![이미지](/assets/img/2024-07-09-CanChatGPTsolveCypressQuiz_14.png)

ChatGPT 설명:
Cypress에서 cy.wrap('some value').as('value')은 'some value'에 대한 별칭인 'value'를 할당합니다. 이 별칭은 cy.get('@value')를 사용하여 후속 테스트에서 참조할 수 있습니다.

before() 훅은 모든 테스트 이전에 한 번 실행되며 별칭을 설정합니다.

<div class="content-ad"></div>

테스트 #1과 테스트 #2는 둘 다 alias를 가져와서 assert 하는데 cy.get('@value').should('eq', 'some value');를 사용해요. before() 훅에서 alias를 올바르게 설정했기 때문에 두 테스트 모두 문제없이 통과해야 해요.

ChatGPT 답변: A — 잘못됨 ❌

## 퀴즈 완료

<img src="/assets/img/2024-07-09-CanChatGPTsolveCypressQuiz_15.png" />

<div class="content-ad"></div>

퀴즈가 완료되었어요! ChatGPT는 9개의 문제를 정확히 대답하고 5개의 실수를 했어요. 꽤 괜찮지 않나요? 그래도 더 많은 것을 기대했었어요. 전반적으로, ChatGPT가 많은 주제에 대해 잘 파악하고 있지만, 핵심 개념과 대부분의 방법들의 기본을 완전히 이해하지 못하고 Cypress의 특이한 동작들을 완전히 이해하지 못한다는 것이 분명해요.

이 실험은 항상 공식 문서를 참조하고 실습과 지속적인 학습을 통해 이해를 더 깊게 하는 것이 얼마나 중요한지를 강조합니다. 인공지능은 도움이 되지만 철저한 지식과 실무 경험으로 대체할 수 없어요. 따라서, 문서를 꼭 읽고 실력을 향상시켜 검증된 Cypress 사용자가 되도록 노력해주세요. 게다가, 인공지능이 여러분을 대체하지는 않을 거에요; 여러분의 일은 안전해요, 적어도 지금 😁
