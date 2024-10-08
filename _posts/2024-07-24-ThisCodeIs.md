---
title: "이 코드는 , 어떻게 고치나요"
description: ""
coverImage: "/assets/img/2024-07-24-ThisCodeIs_0.png"
date: 2024-07-24 12:12
ogImage: 
  url: /assets/img/2024-07-24-ThisCodeIs_0.png
tag: Tech
originalTitle: "This Code Is ,"
link: "https://medium.com/@clivethompson/this-code-is-136fc1015559"
isUpdated: true
updatedAt: 1723816785138
---



![ThisCodeIs_0.png](/assets/img/2024-07-24-ThisCodeIs_0.png)

코딩은 여러 가지 면을 가지고 있어요: 머리 아픈, 도전적인, 재밌는, 강력한, 때로는 수행자들에게 신들의 복복을 주기도 해요.

하지만 주된 부분은 무엇일까요? 컴퓨터 프로그래밍은 깊고, 고통스럽게 짜증나는 일이에요. (이에 대해 이전에 블로그를 썼죠.) 컴퓨터는 잔인하고 용서하지 않는 주인. 제대로 했다고 생각해도 중요한 작은 실수를 저지르면 — 단 하나의 구두점 수준에서도 — 컴퓨터는 협조하지 않고 서 있을 거에요. 손톱을 갈면서 차분히, 혹은 델파이 오라클의 선고처럼 이해하기 어려운 오류 메시지를 제공할 거에요. 프로그래밍할 때 전체적인 멍청이 같이 느껴지고 큰소리로 욕을 하는 것은 아주 쉬워요.

코드 안에서 욕하기도 쉬워요. 연구결과, 프로그래머들은 코드 안에 많은 욕설을 쓴다는 사실이 밝혀졌어요.

<div class="content-ad"></div>

더 흥미로운 사실은...

...일부 욕설이 실제로 유용할 수도 있다는 거예요.

한번 자세히 살펴보겠습니다!

프로그래머들이 코드에 욕설을 자주 사용하는 빈도를 어떻게 알게 됐는지 기억해요. 이는 2010년대 초반, 개발자 Andrew Vos가 GitHub의 "커밋 메시지"를 살펴보기로 결정했을 때였어요.

<div class="content-ad"></div>

커밋 메시지의 작동 방식은 다음과 같습니다: 프로그래머들이 작업을 완료하고 GitHub에 업로드할 때, 그들은 일반적으로 수행한 작업에 대한 간단한 텍스트 설명을 추가합니다.

Vos는 100만 건의 커밋 메시지를 분석하여 종종 욕설이 포함되어있음을 발견했습니다. 때로는 개발자가 자신의 무능함에 짜증을 내며 자신을 욕하곤 했고, 다른 경우에는 다른 사람들의 엉망한 작업에 짜증을 내며 고치기를 강요받았습니다. 그리고 때로는 그들이 웃기려고 했을 수도 있습니다.

그들은 다음과 같은 내용을 썼습니다...

(그의 GitHub 계정에서 더 많은 예시를 볼 수 있습니다.)

<div class="content-ad"></div>

Vos는 또한 욕설이 가장 많이 포함된 프로그래밍 언어가 C++이라는 것을 확인했습니다. 그 다음으로 루비와 자바스크립트가 뒤를 이었습니다. (그것이 나에게 부분적으로 놀라운 것이었다: C++는 유명하게 어수선하기로 유명하기 때문에 그런 이유는 이해가 갑니다. 그리고 자바스크립트는 악몽이 될 수 있으며, 그것은 내가 흐느낀 개인적인 경험으로 알고 있습니다. 그러나 루비는 꽤 재미있는 언어로 여겨지므로, 누가 알 수 있을까요?)

![image](/assets/img/2024-07-24-ThisCodeIs_1.png)

그러나 더 흥미롭게도, 코드에서 욕설을 사용하는 것이 코드의 품질과 상관관계가 있는지도 모릅니다.

올해 초에 Karlsruhe 공과대학교의 학부생인 Jan Strehmel은 온라인에 흥미로운 논문을 게시했습니다. 그는 실제로 코드 내부에 욕설이 포함된 GitHub의 3,800개의 C 코드를 분석했습니다: 어떤 변수가 욕설을 사용할 수도 있었고(예: "itsBullshit"와 같은), 또는 코드 주석에 사용되었을 수도 있습니다(예: "// omg this is. fucked"). 그런 다음에 그는 어떤 욕설 단어가 들어있지 않은 7,600개의 C 코드를 수집했습니다.

<div class="content-ad"></div>

마침내, 그는 SoftWipe라는 소프트웨어를 사용하여 그 코드 조각의 품질을 분석했습니다. (명확한 구조와 같이 더 높은 품질의 코드를 시사하는 요소를 찾습니다.)

결과는? 욕설이 포함된 코드가 욕설이 없는 코드보다 약간 더 높은 점수를 받았습니다. 정확히 말하면, 10점 만점 중 욕설이 포함된 코드는 평균 5.87점을 받았고, 욕설이 없는 코드는 평균 5.41점을 받았습니다.

왜 욕설이 포함된 코드가 조금 더 높은 품질을 보일까요? Ars Technica에서 Saima Sidik이 언급했듯이, 욕은 사람들이 고통을 덜고 신체적 성능을 높일 수 있도록 도와줄 수 있습니다. 그래서 아마도 욕이 코딩의 좌절과 존재적 고통을 관리하는 데 도움이 되어 성능을 향상시킨다고 볼 수 있을까요?

Sidik은 심리학자이자 <What the F: What Swearing Reveals About Our Language, Our Brains, and Ourselves>의 저자인 Benjamin Bergen과 이 문제에 대해 이야기했습니다...

<div class="content-ad"></div>

그래서 우리는 코드에 욕을 적극적으로 포함시켜야 할까요?

히히. 나는 아마 그렇지 않을 것 같아. 우선, Strehmel의 연구는 상관 관계가 있지만 인과 관계는 없습니다. 좋은 일을 하는 프로그래머들이 코드에 욕설을 포함할 가능성이 높을 수도 있습니다. 자신의 욕설을 늘리는 것이 소프트웨어를 더 나아지게 만들지는 않을 수도 있습니다.

더 중요한 것은 f-bombs를 날리는 것이 다른 개발자들에게 상당히 공격적인 행동으로 비춰질 수 있다는 점입니다. 코드는 (내가 그렇게 믿는대로) 주로 다른 사람들을 위해 작성되었으며(즉, 몇 년 후에 코드를 유지보수하게 될 불운한 사람들) 그리고 컴퓨터에게 물리적으로 두 번째로 작성되었음을 염두에 두고 코드를 명확하고 아마 재치 있게 작성하는 것이 타인을 비웃는 것보다 가능한 적게 다른 사람들을 비활성화시킬 가능성이 있는 방식으로 작성하는 것이 좋습니다.

나는 개인적으로 코드에 욕설이 들어간 것을 웃긴 것으로 생각해요. 하지만 다른 사람들도 그렇게 생각하지 않을 수 있습니다 - 아마도 당신의 매니저도요. 아헴.

<div class="content-ad"></div>

말했다시피, 코드는 인간의 표현입니다! 따라서 아마도 인간 표현의 모든 측면을 영원히 포함할 것입니다, 욕설도요.

또한, 어떤 #$@! 컴퓨터도 그것을 바꿀 수 없을 거에요.

(이 글을 즐겼다면? 그럼: 그 "박수" 버튼을 찾아서 눌러보세요. 한 독자 당 최대 50번까지 클릭할 수 있습니다!)

제가 Medium에 주간 두 번씩 글을 올리고 있습니다. 제 이메일로 모든 포스트를 받아보려면 여기를 팔로우해주세요. Medium 회원이 아닌 경우 여기에서 가입하고 월 이용료의 절반은 제 Medium 글쓰기를 지원하는 데 직접 쓰이며, 사이트의 다른 모든 콘텐츠에도 액세스할 수 있습니다.

<div class="content-ad"></div>

저는 뉴욕 타임스 매거진의 기고자이자 Wired 및 Smithsonian 잡지의 칼럼니스트이며 Mother Jones에 정기적으로 기고하고 있습니다. 또한 Coders: The Making of a New Tribe and the Remaking of the World 및 Smarter Than You Think: How Technology is Changing our Minds for the Better의 저자이기도 합니다. 트위터와 인스타그램에서는 @pomeranian99로, Mastodon에서는 @clive@saturation.social로 활동하고 있어요.