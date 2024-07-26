---
title: "블레임리스 포스트모템이 중요한 이유는"
description: ""
coverImage: "/assets/img/2024-07-25-WhatsthePointofBlamelessPostmortems_0.png"
date: 2024-07-25 12:14
ogImage: 
  url: /assets/img/2024-07-25-WhatsthePointofBlamelessPostmortems_0.png
tag: Tech
originalTitle: "Whats the Point of Blameless Postmortems"
link: "https://medium.com/better-programming/whats-the-point-of-blameless-postmortems-5d8c2ff519d7"
---



<img src="/assets/img/2024-07-25-WhatsthePointofBlamelessPostmortems_0.png" />

보브 로스를 기억하시나요?

보브 로스는 많은 미국인들에게 아이콘이었습니다 (지금도 그렇습니다). 그는 PBS에서 'The Joy of Painting'이라는 프로그램을 진행했는데, 매 에피소드마다 새로운 그림을 그렸는데, 보통은 평화로운 풍경을 그리곤 했습니다.

로스는 (적어도 TV에서는) 부드럽고 편안한 말투로 유명했으며, 자신의 회화에 대한 심신을 남긴 사람으로 기억됩니다. 그에게는 실수가 존재하지 않았습니다; "행복한 작은 우연"이었던 것입니다.


<div class="content-ad"></div>

저희 Policygenius 팀은 사고와 장애에 대해 엔지니어링 문화에서 이 구문을 도입하려고 노력하고 있습니다. 모든 것을 우연으로 간주하고 싶어합니다 — 누군가 고의로 행동하지 않았으며, 잘못을 지은 단일 인물도 없습니다.

이 아이디어는 소프트웨어 산업 내에서 "책임없는 문화"로 자주 언급됩니다. 장애의 원인을 찾아내기 위해 개인 또는 팀을 탓하는 대신, 장애는 한 명의 책임자가 아닌 연이어 발생한 다수의 실패로 인한 것으로 간주됩니다.

이 특징은 사고 조사 및 검토 중에 자주 나타납니다. 팀이 사고를 검토하고 그 후 다음 단계를 어떻게 진행하는지에 따라 당신의 팀이 어떻게 일을 수행하는지에 대해 많은 것을 알 수 있습니다.

책임없는 사후조사를 실시하는 팀을 구축하고 이를 통해 성공하는 방법에 대해 알아봅시다.

<div class="content-ad"></div>

# 포스트모템이란 무엇인가요?

소프트웨어에서 포스트모템은 사건을 조사하여 사건의 근본 원인을 이해하는 것을 말합니다. 어떤 일이 발생한 후에 사건의 원인을 식별함으로써 우리는 다음에 이를 피할 수 있는 방법을 배울 수 있을 것을 희망합니다.

전형적인 포스트모템은 몇 가지 중요한 정보를 논의합니다. 서로 다른 조직에서 서식은 다르겠지만 (GitHub에서 훌륭한 예시 모음을 볼 수 있습니다) 모두 아래에 개요된 이 분류를 대략 다룰 것입니다.

1. 사건의 영향: 누가 영향을 받았나요? 사건이 얼마나 오랫동안 계속되었나요? 돈이나 고객을 잃었나요? 
2. 사건의 시간 순서: 어떻게 사건에 대해 알림을 받았나요? 누가 당직인가요? 누가 대응했나요? 어떤 조치가 취해졌고 언제 있었나요? 
3. 근본 원인: 시스템에서 문제가 발생했나요? 요청 지연이 너무 길었나요? 벤더가 실패했나요, 아니면 데이터베이스가 다운되었나요? 
4. 해결책: 사건을 완화하거나 해결하기 위해 어떤 조치가 취해졌나요? 사건이 완전히 해결되었나요, 지속 중인가요, 아니면 일부만 완화된 상태인가요? 
5. 사후 조치: 향후 이와 같은 사건을 피하기 위해 각 팀이 취할 단계는 무엇인가요? 코드를 변경해야 하나요? 알림 절차를 개선해야 하나요? 배포 방법?



<div class="content-ad"></div>

사후 조치 문서는 주로 주요 사고 대응 담당 팀에 의해 작성되고 전체 엔지니어링 조직에서 검토됩니다. 대규모 조직(100명 이상의 엔지니어)은 관련 팀에만 제한적으로 검토할 수도 있습니다. 그러나 더 넓은 대상이 될수록 학습 기회가 많아진다는 것을 염두에 두세요.

본문에서는 훌륭한 사후 조치 문서를 작성하는 내부 작업에 대해서는 다루지 않겠습니다. Atlassian의 다양한 기사, GitHub의 실제 예시 및 다른 여러 자료가 있기 때문입니다.

대신, 비난을 받지 않는 것의 중요성과 그 가능성을 위해 필요한 문화적 전환에 초점을 맞추고자 합니다.

# 누구를 비난해야 할까요?

<div class="content-ad"></div>

지금까지 언급된 대로, 사후 조치 문서의 한 가지 문제점은 연기가 걷히고 모두가 무엇이 발생했는지 보게 되면, 개인이나 팀을 범인으로 찾으려는 유혹이 있을 수 있다.

사건의 심각성에 따라(예: 멀티 시간 중단), 리더들은 권위의 메시지를 전달하기 위해 책임자를 찾을 수도 있습니다. 다른 팀들은 실패를 다른 팀에게 전가하려고 하며, 시스템이 먼저 실패했다고 주장할 수도 있습니다.

모두가 책임을 전가하고 손가락질을 하는 것을 원합니다.

그러나 이런 일이 조직 내에서 시작되면, 끔찍한 일이 일어나기 시작합니다: 사람들이 거짓말을 시작합니다. 그들은 문제를 덮거나 보고 싶어도 입을 다물기 시작합니다. 그리고 이는 재앙으로 이어질 수 있습니다.

<div class="content-ad"></div>

팀들은 자신들의 연관성을 숨기기 위해 사건 세부 사항을 감추려고 할 것입니다. 문제를 수정할 아이디어를 갖고 있는 사고 대응자들은 그룹에 언급하지 않을 수도 있습니다. 그들은 비밀리에 문제를 해결하려고 할 것이며 아무도 주목하지 않기를 바랄 것입니다.

더 나쁜 것은 시간이 지남에 따라 이러한 두려움의 문화가 개발의 모든 측면으로 스며들기 시작한다는 것입니다. 이는 엔지니어들이 코드 리뷰에서 문제를 제기하는 것을 피하거나 새로운 아이디어를 제품에 제시하는 것도 피하게 만듭니다. 엔지니어와 아이디어를 잃게 되고 천천히 뒤쳐지게 될 것입니다.

가장 중요한 것은 비난 문화가 실제로 시간이 지남에 따라 더 많은 사건으로 이어진다는 것입니다. 기본적인 문제들이 해결되거나 수정되지 않습니다. 멀티 시간 장애를 일으킨 버그는 코드 속에 미래로 수개월 혹은 수년이 지나도 남아 있습니다. 다음으로 잘못 될 다음 일을 위한 티킹 타임 밤을 만들어 놓게 됩니다.

# 후속 조치 포스트모템에 비난을 제외하려면?

<div class="content-ad"></div>

포스트모템을 비난 없이 진행하려면 어떻게 시작해야 할까요? 사람들을 탓하지 않는 멋진 문서만으로는 충분하지 않습니다. 문화 변화가 필요합니다.

문화 변화는 다양한 방법으로 일어납니다. 소프트웨어 업계에서 많은 노력은 하향식으로 시작됩니다. 이러한 노력에는 관측 가능성을 향상하거나 오래된 시스템을 리팩토링하는 것이 포함됩니다. 하향식 문화 변화에서는 누구도 허락을 구하지 않고 그들이 더 나아지리라고 믿는 방식으로 행동하기 시작하며, 다른 이들도 이를 따르기 시작합니다.

비난 없는 문화나 안전을 중시하는 문화는 주로 상향식으로 발전됩니다. 어느 순간에는 리더(또는 리더 그룹)가 비난 없는 문화를 만들기 위한 패턴과 방법을 도입해야 합니다. 팀이 일상 업무에서 비난 없이 일을 한다면 좋죠! 하지만 이를 달성해도 이뤄지지 않는다고 생각하는 상급자들이 있다면 상황은 그대로입니다.

하지만 엔지니어로서 당신에게 역할이 있습니다. 결국, 문화는 회사 전체에서 공유되는 가치, 신념 및 행동의 모음이기 때문이죠.

<div class="content-ad"></div>

이러한 변경을 원활하게 이끄는 가장 좋은 방법은 원하는 행동을 모델링하고 리더들에게 옹호하는 것입니다.

본인이 속한 팀과 관련된 사고에 대한 책임을 짊어지며 모델링하세요. 손가락질을 하지 않고 누구가 잘못했는지보다는 무엇이 일어났는지에 중점을 두는 것으로 모델링하세요. 미래에 발생할 문제를 피하기 위한 일련의 다음 단계를 식별하고 즉시 참여할 의지를 나타내며 리드하세요. 후행 분석을 작성하는 경우 Google SRE의 관점을 수용해야 합니다.

동시에 이러한 변화를 당신의 매니저와 다른 리더들에게 옹호해야 합니다. Accelerate, DevOps 핸드북(아래 섹션에서 논의됨)의 연구 결과 및 효과적인 팀을 형성하는 심리적 안전성의 전반적인 이점을 인용하여 사례를 구축하세요. 단독으로 진행하는 대신 이러한 변화를 밀어내기 위해 유사한 생각을 가진 동료를 찾으세요.

그들을 완전히 설득하지 못할 수도 있습니다. 그것은 괜찮아요. 이러한 변화는 종종 시간이 걸리고 여러 차례의 노력이 필요합니다. 그저 가만히 앉아서 모든 것을 지켜보지 마세요.

<div class="content-ad"></div>

# 작동되나요?

하지만 이러한 모든 변경 사항이 아무런 영향을 미치지 않는다면 어떻게 될까요? 무죄 후퇴검구는 미래의 장애를 예방하는 데 도움이 될까요?

다행히도, 우리에게는 분명히 "예"라고 답변할 수 있는 많은 데이터가 있습니다.

Google의 연구, Gene Kim의 DevOps Handbook (Etsy와 HubSpot과 같은 회사를 포함)에서 모두 후퇴검구가 그들의 성공에 중요하다고 말하고 있습니다. 비난을 받지 않는 후퇴검구는 실수(또는 행복한 작은 사고)가 사람들의 잘못이 아닌 시스템의 잘못을 나타내는 생리적 안전 문화의 지지대입니다.

<div class="content-ad"></div>

나는 내 경력에서 과잉 책임을 지우지 않는 프로세스의 효과에 대해 직접 이야기할 수도 있어요. 사고 또는 장애가 발생했을 때 즉각적인 비난, 수치심, 심지어 관련 당사자들을 해고시키는 팀에서는 그 결과가 그냥 아무도 소프트웨어를 출시하고 싶어하지 않는다는 것이었어요. 실수를 범하겠다는 두려움 때문에 사람들은 기능을 출시하는 것을 피하기 위해 시간을 낭비했어요. 그래서 더 크고 더 먼 간격으로 계속해서 출시하게 되었고, 더 많은 실패한 배포와 사고를 초래했어요.

이러한 팀들은 자신들의 사고에 대한 원인을 적게 알리려고 했어요. 그런 다음 다음주에는 다른 팀이 동일한 실수를 저지르게 되었죠, 이전의 실수에서 배우지 못했기 때문이었어요.

다른 한편으로, 어떻게 발생했는지와 다음에는 어떻게 피할 수 있는지에 초점을 맞추며 배움으로 나아가는 것으로 팀은 솔루션과 실행에 주력하게 되었어요. 팀은 자신들의 실패에 책임지며(압박이나 손가락질과 무관한), 미래에 그것들을 피하기 위한 조치를 취하게 되었어요. 모든 사람은 서로의 성공과 실패로부터 배우며 팀 간 경계 내에서 고립된 통찰을 각자에게 남기지 않게 되었어요.

이 글로의 교훈, 여러분도 아시다시피, 우연이 발생할 수 있다는 것이에요. 우연을 일으키는 사람들을 제거하는 데 초점을 맞추는 대신, 우리는 사고에 대해 분노나 처벌로 기본으로 되돌아가는 문화를 만들어야 한다는 것이에요.

<div class="content-ad"></div>

물론 때로는 반복된 부주의나 완전한 무관심으로 인해 해고 결정을 내릴 필요가 있는 경우가 있습니다. 그러나 대부분의 경우에는 누군가가 그냥 잘못된 터미널에 로그인해서 다른 환경에 있다고 생각한 것 뿐입니다.

다음 사고를 예방하기 위해 사건에서 배우는 것에 집중합시다. 이미 일어난 실패에 대해 꾸짖는 것보다 올바른 일을 한 사람들을 보상하는 문화를 만드는 것이 중요합니다.

우리 팀은 그 결과로 성장할 것입니다. 그것이 바로 진정한 목표입니다.

즐거운 코딩 되세요!

<div class="content-ad"></div>

본문은 https://dangoslen.me 에서 원래 발행되었습니다.