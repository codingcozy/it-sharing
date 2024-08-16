---
title: "프론트엔드 개발자라면 알아야 할 5가지 파이썬 꿀팁"
description: ""
coverImage: "/assets/img/2024-07-21-5PythonBooleanThingsIRegretNotKnowingEarlier_0.png"
date: 2024-07-21 11:32
ogImage: 
  url: /assets/img/2024-07-21-5PythonBooleanThingsIRegretNotKnowingEarlier_0.png
tag: Tech
originalTitle: "5 Python Boolean Things I Regret Not Knowing Earlier"
link: "https://medium.com/@zlliu/5-python-boolean-things-i-regret-not-knowing-earlier-a4caf4b00d1d"
isUpdated: true
updatedAt: 1723812500769
---



<img src="/assets/img/2024-07-21-5PythonBooleanThingsIRegretNotKnowingEarlier_0.png" />

이렇게요! 이전에 알았더라면 좋았을 5가지 Python boolean quirks를 여기 나열했어요. 참고 - 나는 Python 코드베이스에서 이를 계속 볼 수 있어요!

# 1) Truthy와 Falsy 값

참 값으로 평가되는 값은 직접 조건으로 사용할 때 True로 평가돼요. Truthy한 값의 예시:

<div class="content-ad"></div>

- 0 및 0.0
- 빈 문자열
- 빈 리스트/셋/사전/튜플/불변셋
- None

```js
if 100:
    print('this prints')

if 'apple':
    print('this prints')

if [1, 2, 3]:
    print('this prints')

if {'apple':1, 'orange':2}:
    print('this prints')
```

<div class="content-ad"></div>

```js
if 0:
  print('이 부분은 출력되지 않습니다')

if '':
  print('이 부분은 출력되지 않습니다')

if []:
  print('이 부분은 출력되지 않습니다')

if {}:
  print('이 부분은 출력되지 않습니다')

if None:
  print('이 부분은 출력되지 않습니다')
```

# 2) bool()을 사용하여 참/거짓 값 확인하기

bool(obj)를 사용하면 obj가 참 값일 경우 True를 반환하고, 거짓 값일 경우 False를 반환합니다.

```js
print(bool(10))          # True
print(bool('apple'))     # True
print(bool([1, 2, 3]))   # True
print(bool({1:2, 3:4}))  # True
print(bool(object()))    # True

print(bool(0))           # False
print(bool(''))          # False
print(bool([]))          # False
print(bool({}))          # False
print(bool(None))        # False
```

<div class="content-ad"></div>

따라서 bool() 유형을 사용하여 값이 참인지 거짓인지를 확인할 수 있습니다. 확실하게 하려면요.

### 3) any() 및 all()

all(sequence)는 다음을 반환합니다:

- 시퀀스 내의 모든 것이 True로 평가되면 True
- 시퀀스 내의 최소한 하나의 요소가 False로 평가되면 False

<div class="content-ad"></div>

```js
print(all([True, True, True]))    # True
print(all([True, True, False]))   # False
```

any(sequence) 함수는 다음을 반환합니다:

- sequence 내부에 있는 적어도 하나의 항목이 True로 평가되면 True를 반환합니다.
- sequence 내부에 있는 모든 항목이 False로 평가되면 False를 반환합니다.

```js
print(any([True, True, False]))   # True
print(any([False, False, False])) # False
```

<div class="content-ad"></div>

# 4) 'x 또는 100'의 의미

코드에서 가끔 이렇게 표현된 것을 볼 수 있습니다:

```js
// x가 정의되어 있다고 가정합니다

a = x 또는 100
```

'x 또는 100'이 의미하는 바:

<div class="content-ad"></div>

- 만약 x가 None이면, x or 100은 기본값 100을 취합니다
- 만약 x가 None이 아니면, x or 100은 x의 원래 값으로 설정됩니다

```js
def get_fruit() -> str | None:
    # 과일이나 None을 반환할 수 있음

fruit = get_fruit()  # 이는 어떤 과일이든 가능하거나 None일 수 있음

fruit = fruit or 'apple'
# 만약 fruit이 None이 아니라면, 변경사항 없음
# 만약 fruit이 None이라면, fruit = 'apple'

print(fruit)  
```

fruit = fruit or `apple`라는 줄은 fruit가 반드시 문자열이 되도록 합니다.

- 만약 get_fruit()이 문자열을 반환하면, 아무 변경사항 없음
- 만약 get_fruit()이 None을 반환하면, fruit은 'apple'로 자동 재할당됩니다

<div class="content-ad"></div>

# 5) 'and' 및 'or'의 단락 평가 특성

```js
if X and Y and Z:
    # 작업 수행
```

위 조건에서 Python은 결과를 알게 되면 즉시 평가를 중단합니다.

이를 시각화하기 위해 몇 가지 함수를 만들어 봅시다.

<div class="content-ad"></div>

```js
def func1():
    print('func1')
    return True

def func2():
    print('func2')
    return False

def func3():
    print('func3')
    return True

if func1() and func2() and func3():
    print('not printed')

# func1
# func2
```

여기서 주목해야할 점은 func1과 func2만 실행됩니다 (출력된 값을 확인하세요)

- func1은 True를 반환하기 때문에 `func1`이 출력됩니다.
- func2는 False를 반환하기 때문에 `func2`가 출력됩니다.
- func1() and func2() and func3()는 어떤 경우에도 False가 됩니다.
- Python은 func3()를 평가할 필요가 없기 때문에 호출되지 않습니다.

or 키워드에 대해서도 마찬가지로 작동합니다.

<div class="content-ad"></div>

```python
def func1():
    print('func1')
    return True

def func2():
    print('func2')
    return False

def func3():
    print('func3')
    return True

if func1() or func2() or func3():
    print('this is printed')

# func1
# this is printed
```

- func1()은 True로 평가됩니다.
- func1() 또는 func2() 또는 func3()는 무엇이든 True가 됩니다.
- Python은 func2()와 func3()를 평가할 필요가 없으므로 이들을 평가하지 않습니다.

# 결론

이 내용이 명확하고 이해하기 쉬웠기를 바랍니다


<div class="content-ad"></div>

# 저를 창작자로서 지지하고 싶다면

- https://zlliu.substack.com/ 에서 제 서브스택 뉴스레터를 구독해주세요 — 저는 매주 파이썬 관련 이메일을 보냅니다
- https://payhip.com/b/vywcf 에서 제 책 101 Things I Never Knew About Python을 구매해주세요
- 이 이야기를 50번 클랩해주세요
- 댓글을 남겨서 당신의 생각을 나누어주세요
- 이 이야기 중 가장 마음에 드는 부분을 강조해주세요

감사합니다! 이러한 작은 행동들이 큰 도움이 되고, 정말 감사드립니다!

YouTube: https://www.youtube.com/@zlliu246

<div class="content-ad"></div>

LinkedIn: [https://www.linkedin.com/in/zlliu/](https://www.linkedin.com/in/zlliu/)