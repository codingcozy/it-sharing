---
title: "짧은 인터뷰 Qn 동적 타입 VS 정적 타입 언어 최신 비교"
description: ""
coverImage: "/assets/img/2024-07-25-ShortInterviewQnDynamically-typedVSStatically-typedLanguages_0.png"
date: 2024-07-25 12:09
ogImage: 
  url: /assets/img/2024-07-25-ShortInterviewQnDynamically-typedVSStatically-typedLanguages_0.png
tag: Tech
originalTitle: "Short Interview Qn Dynamically-typed VS Statically-typed Languages"
link: "https://medium.com/gitconnected/short-interview-qn-dynamically-typed-vs-statically-typed-languages-688257c66134"
---


<img src="/assets/img/2024-07-25-ShortInterviewQnDynamically-typedVSStatically-typedLanguages_0.png" />

만약 소프트웨어 엔지니어 포지션 면접을 보러 가는 경우, 인터뷰어가 이 질문을 할 수도 있어요.

출처: 과거에 이 질문을 받아본 적 있어요

# 동적으로 타입이 지정된 프로그래밍 언어

<div class="content-ad"></div>

예제: Python

이러한 언어에서는 인터프리터가 실행 시간에 변수에 유형을 할당합니다.

또한 이러한 언어에서는 변수의 유형을 정의할 필요가 없습니다.

```js
# Python에서 변수 정의하기
a = 5
b = 'apple'
c = [1, 2, 3]
```

<div class="content-ad"></div>

^ 언제든지 a를 정수여야 한다고 정의하지 않고 그냥 5를 할당할 수 있다는 점에 유의하세요.

```js
a = 5
a = 'apple'
a = {'apple': 5}
```

^ 일반적으로 임의로 다른 데이터 유형을 a에 할당할 수 있고, 이는 합법적입니다. (합법적이라고 해서 좋은 방법이 될 필요는 없다는 의미로요)

# 정적으로 형식화된 프로그래밍 언어

<div class="content-ad"></div>

예시: 자바

이러한 언어에서는 컴파일 시에 타입 체크가 발생합니다.

즉, 모든 변수에 데이터 유형을 정의해야 합니다.

```js
// 자바 코드

int a = 5;
String b = "apple";
int[] b = {1, 2, 3, 4, 5};
```

<div class="content-ad"></div>

여기서 Java에서는 a가 정수임을 int a를 사용하여 선언해야 코드를 컴파일할 수 있습니다.

그리고 int a를 사용하여 a를 정수로 선언한 후에는 a에는 정수 값만 할당할 수 있습니다.

```js
int a = 5;
a = "apple"    // 오류
```

# 장단점

<div class="content-ad"></div>

속도 - 정적으로 유형이 지정된 언어가 빠릅니다. 동적으로 유형이 지정된 언어는 런타임에서 유형을 할당하는 데 추가 시간이 필요할 수 있습니다.

유형 안전성 - 정적으로 유형이 지정된 언어는 일반적으로 보다 구조화되어 있으며 유형 오류를 더 쉽게 잡아냅니다. 특히 프로젝트가 커지면 더욱 그렇습니다.

개발 속도 - 동적으로 유형이 지정된 언어가 여기서 이깁니다. 정적으로 유형이 지정된 언어와 비교하여 빠른 프로토타입을 만드는 것이 더 빠릅니다.

사용 편의성 - 동적으로 유형이 지정된 언어는 일반적으로 배우기 쉽고 정적으로 유형이 지정된 언어보다 학습 곡선이 더 부드럽습니다.

<div class="content-ad"></div>

# 동적 타입이지만 Python이 좋은 이유

Python에는 타입 힌트가 있습니다:

```js
x: int = 5
```

변수가 특정 타입이어야 한다는 힌트를 줍니다. 큰 프로젝트를 관리하기 쉽게 만들어줍니다.

<div class="content-ad"></div>

파이썬에는 실제로 데이터 유형을 강제하는 Pydantic이 있습니다.

```python
from pydantic import BaseModel

class Person(BaseModel):
    name: str
    age: int
    gender: str

person = Person(
    name='bob', age=5, gender='male'
)  # 문제 없음

person = Person(
    name='bob', age='hi', gender='male'
)
'''
age
  입력값은 유효한 정수여야 합니다. 문자열을 정수로 구문 분석할 수 없음
    [type=int_parsing, input_value='hi', input_type=str]
    더 많은 정보는 
      https://errors.pydantic.dev/2.7/v/int_parsing 를 참조하세요
'''
```

^ 여기서 나이(age)를 문자열로 변경하려고 하면 Pydantic에 잡힙니다.

# 그럼 Java를 사용하지 않는 이유는?

<div class="content-ad"></div>

자바 쓰는 거 진짜 싫어해서 말야. 어려운 내용은 아니야, 그냥 내가 불편하다고 느끼는 거야.

자바에서 느끼는 부족함:

- 우아한 리스트/사전/집합 내포 (스트림이 있긴 한데 짜증나)
- 튜플 언패킹
- *args와 **kwargs, 그리고 *와 **를 이용한 다른 장난감들
- 빠른 프로토타이핑 능력 (자바로 빠르게 프로토타이핑하는 건 거절해)

하지만 자바 개발자를 깎아내리는 건 아니야 (친한 친구 중에 자바를 좋아하는 사람들도 있거든). 그냥 내 취향이 아닐 뿐이야.

<div class="content-ad"></div>

# 결론

이것이 명확하고 이해하기 쉬웠으면 좋겠어요.

# 만약 제가 크리에이터로서 저를 지원하고 싶다면

- https://zlliu.substack.com/ 에서 제 Substack 뉴스레터 가입하기 - 저는 주간 이메일을 통해 Python에 관련된 내용을 보내드려요
- https://payhip.com/b/vywcf 에서 제 책인 101 Things I Never Knew About Python 구매하기
- 이 글에 50번 박수 치기
- 당신의 생각을 나에게 알려주는 댓글 남기기
- 이야기에서 가장 마음에 드는 부분을 강조하기

<div class="content-ad"></div>

감사합니다! 이런 작은 조치들이 큰 도움이 되고 정말 감사합니다!

YouTube: [https://www.youtube.com/@zlliu246](https://www.youtube.com/@zlliu246)

LinkedIn: [https://www.linkedin.com/in/zlliu/](https://www.linkedin.com/in/zlliu/)