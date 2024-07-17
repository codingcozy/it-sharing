---
title: "JavaScript에서 parseInt00000005가 5를 반환하는 이유"
description: ""
coverImage: "/assets/img/2024-07-09-WhyparseInt00000005returns5inJavaScript_0.png"
date: 2024-07-09 16:26
ogImage:
  url: /assets/img/2024-07-09-WhyparseInt00000005returns5inJavaScript_0.png
tag: Tech
originalTitle: "Why parseInt(0.0000005) returns 5 in JavaScript"
link: "https://medium.com/coding-beauty/why-parseint-0-0000005-returns-5-in-javascript-406d7114a14b"
---

우리 눈을 의심할 수밖에 없어요:

![Image](/assets/img/2024-07-09-WhyparseInt00000005returns5inJavaScript_0.png)

우리가 parseInt에 대해 모든 것을 알고 있다고 생각했을 때, 자바스크립트만이 하는 어리석은 일들에 대해 생각해봤을 때?

재밌는 점은 십진점 뒤에 4와 다섯 개의 영을 넣으면 0을 반환한다는 겁니다.

<div class="content-ad"></div>

영저택.PNG" />

하지만 하나의 0을 더하면 엉뚱하게 작동하죠?

Number와의 차이는 더욱 두드러집니다.

✅ parseInt 대 Number -- four and five zeroes:

<div class="content-ad"></div>

![Image](/assets/img/2024-07-09-WhyparseInt00000005returns5inJavaScript_2.png)

But:

❌ parseInt vs Number -- six zeroes:

![Image](/assets/img/2024-07-09-WhyparseInt00000005returns5inJavaScript_3.png)

<div class="content-ad"></div>

하지만 결국 해결책을 찾아 냈어요.

# parseInt: 부족한 부분들

parseInt 함수가 어떻게 작동하는지 이해하면:

두 개의 인수를 사용합니다: 문자열과 기본 또는 진수입니다.

<div class="content-ad"></div>

![image](/assets/img/2024-07-09-WhyparseInt00000005returns5inJavaScript_4.png)

Look what it does for 6 zeroes when it’s a string:

![image](/assets/img/2024-07-09-WhyparseInt00000005returns5inJavaScript_5.png)

But for Number:

<div class="content-ad"></div>

<img src="/assets/img/2024-07-09-WhyparseInt00000005returns5inJavaScript_6.png" />

# 하지만 숫자인 경우 어떻게 될까요?

예를 들어 0.0000005와 같은 숫자가 있을 때는 어떻게 될까요?

# 단계 1: 숫자를 문자열로 변환

<div class="content-ad"></div>

숫자가 문자열로 변환됩니다!

그래서 parseInt가 0.0000005에 무엇을 하는지 살펴보세요:

![image](/assets/img/2024-07-09-WhyparseInt00000005returns5inJavaScript_7.png)

## 단계 2: 실제 반올림하기

<div class="content-ad"></div>

그럼요!

parseInt(0.0000005) → parseInt(`5e-7`)

그리고 이제 `5e-7`를 parseInt로 변환했을 때 어떤 일이 벌어지는지 확인해보세요:

![링크 텍스트](/assets/img/2024-07-09-WhyparseInt00000005returns5inJavaScript_8.png)

<div class="content-ad"></div>

왜요?

parseInt가 작동하는 방식 때문에요: 그것은 문자열의 시작 부분만을 정수 값으로 해석해요.

e를 인식하지 못해서 e 이후의 모든 것을 무시해요.

이것이 왜 동일한 이유인 이유겠죠:

<div class="content-ad"></div>

![image](/assets/img/2024-07-09-WhyparseInt00000005returns5inJavaScript_9.png)

- parseInt(999999999999999999999) → parseInt(1e+21) → 1
- parseInt(0.0000007) → parseInt(7) → 7

So Number is your best bet if you wanna avoid surprises when converting a string to a number

![image](/assets/img/2024-07-09-WhyparseInt00000005returns5inJavaScript_10.png)

<div class="content-ad"></div>

만일 정수 부분만 필요하다면, Math.floor() 함수를 사용할 수 있어요:

![이미지](/assets/img/2024-07-09-WhyparseInt00000005returns5inJavaScript_11.png)

# 자바스크립트가 하는 매우 이상한 일들

이미 이상한 것들을 다 아는 줄 알았는데,
자바스크립트가 하는 매우 이상한 일들로부터 고통스럽지 않고 소중한 시간을 아껴주세요. '자바스크립트가 하는 매우 이상한 일들'은 자바스크립트의 섬세한 함정과 잘 알려지지 않은 부분들을 소개하는 매혹적인 안내서입니다.

<div class="content-ad"></div>

오늘은 여기서 무료 사본을 받아가세요.

![Image](/assets/img/2024-07-09-WhyparseInt00000005returns5inJavaScript_12.png)

원본 게시물은 codingbeautydev.com에서 확인할 수 있습니다.
