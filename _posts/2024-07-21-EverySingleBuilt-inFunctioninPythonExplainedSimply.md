---
title: "파이썬 내장 함수 모두 쉽게 설명하기"
description: ""
coverImage: "/assets/img/2024-07-21-EverySingleBuilt-inFunctioninPythonExplainedSimply_0.png"
date: 2024-07-21 11:30
ogImage: 
  url: /assets/img/2024-07-21-EverySingleBuilt-inFunctioninPythonExplainedSimply_0.png
tag: Tech
originalTitle: "Every Single Built-in Function in Python Explained Simply"
link: "https://medium.com/gitconnected/every-single-built-in-function-in-python-explained-simply-4b3b26c6911e"
---



![Image](/assets/img/2024-07-21-EverySingleBuilt-inFunctioninPythonExplainedSimply_0.png)

# abs()

음수는 양수가 됩니다. 양수는 그대로 유지됩니다.

```python
print(abs(-100))  # 100
print(abs(100))   # 100
```

<div class="content-ad"></div>

# aiter()과 anext()

aiter는 async iter의 약자이며, anext는 async next의 약자입니다.

- aiter()는 iter()의 비동기적 동등물입니다.
- anext()는 next()의 비동기적 동등물입니다.

aiter()는 객체의 \__aiter\__() 메서드를 호출하고, anext()는 객체의 \__anext\__() 메서드를 호출합니다.

<div class="content-ad"></div>

# all()

일부 시퀀스(예: 리스트, 튜플 등)를 입력으로 받습니다.

시퀀스의 모든 요소가 True이면 True를 반환합니다.

시퀀스의 적어도 하나의 요소가 False이면 False를 반환합니다.

<div class="content-ad"></div>

```js
print(all([True, True, True]))    # True
print(all([True, True, False]))   # False
```

# any()

어떤 시퀀스인지(리스트, 튜플 등)를 가지고 있습니다.

시퀀스 안에 최소한 하나의 요소가 True일 때 True를 반환합니다.

<div class="content-ad"></div>

시퀀스 내 모든 요소가 False인 경우 False를 반환합니다.

```js
print(any([True, False, False]))    # True
print(any([False, False, False]))   # False
```

# ascii()

- 객체를 사람이 읽기 쉬운 버전으로 변환

<div class="content-ad"></div>

2. 특수 문자를 유니코드로 변환

```js
x = ascii("Symbols: Ω α β")
print(x) # 'Symbols: \u03a9 \u03b1 \u03b2'
```

# 빠른 휴식

최근에 Python, 프로그래밍 및 대부분이 알지 못하는 임의의 Python 사실에 관한 Substack 뉴스레터를 시작했습니다. 함께해요!

<div class="content-ad"></div>

제 Substack: [https://zlliu.substack.com](https://zlliu.substack.com)

저는 최근에 '파이썬에 대해 몰랐던 101가지'라는 책을 썼어요. 이 책에는 일찍 배울 수 있었던 파이썬 관련 내용들이 담겨 있어요.

여기서 확인해보세요: [https://payhip.com/b/vywcf](https://payhip.com/b/vywcf)

# bin()

<div class="content-ad"></div>

숫자를 이진수로 변환합니다.

```js
print(bin(2))    # 0b10
print(bin(4))    # 0b100
print(bin(8))    # 0b1000
```

# bool()

객체를 True 또는 False 중 하나로 변환합니다.

<div class="content-ad"></div>

- falsy 객체 (예: 0 또는 비어있는 리스트/튜플)는 False가 됩니다.
- truthy 객체 (예: 0이 아닌 수 또는 비어 있지 않은 시퀀스)는 True가 됩니다.

```js
print(bool(0))    # False
print(bool([]))   # False
print(bool('')    # False

print(bool(100))  # True
print(bool([1]))  # True
print(bool('a'))  # True
```

# breakpoint()

Python Debugger (PDB) 콘솔을 사용한 디버깅에 사용됩니다.

<div class="content-ad"></div>

```js
a = 4
b = 5
breakpoint()
c = 5
total = a + b + c
```

만약 이 코드를 실행한다면 다음 출력이 나올 것입니다:

```js
> /Users/lzl/Documents/repos/test/test/b.py(4)<module>()
-> c = 5
(Pdb)
```

breakpoint() 함수는 c = 5 직전에 파이썬 디버거를 멈추게 하며, 이 시점에서 print()를 사용하여 기존 변수들을 검사할 수 있습니다.

<div class="content-ad"></div>

```js
> /Users/lzl/Documents/repos/test/test/b.py(4)<module>()
-> c = 5
(Pdb) print(a, b)
4 5
(Pdb)
```

# bytearray()

가변 bytearrays로 객체를 변환합니다.

```js
print(bytearray(0))    # bytearray(b'')
print(bytearray(1))    # bytearray(b'\x00')
print(bytearray(3))    # bytearray(b'\x00\x00\x00')
```

<div class="content-ad"></div>

# bytes()

객체를 변경할 수 없는 바이트로 변환합니다. 이는 바이트 어레이의 변경할 수 없는 버전입니다.

```js
print(bytes(0))    # b''
print(bytes(1))    # b'\x00'
print(bytes(3))    # b'\x00\x00\x00'
```

# callable()

<div class="content-ad"></div>

해당 객체가 호출 가능하면 True를 반환하고, 그렇지 않으면 False를 반환합니다. 함수인 경우나 __call__ 메서드가 구현된 경우에 객체는 호출 가능합니다.

```js
a = 5

def b():
    print('hi')

print(callable(a))    # False
print(callable(b))    # True
```

# chr()

chr은 character의 약자입니다. 각 문자는 특정 유니코드를 가지고 있습니다.

<div class="content-ad"></div>

chr(유니코드)은 유니코드 번호를 입력하면 해당 문자를 반환합니다.

```js
print(chr(65))    # A
print(chr(66))    # B

print(chr(97))    # a
print(chr(98))    # b

print(chr(990))   # Ϟ
print(chr(991))   # ϟ
```

# classmethod()

클래스에서 사용되는 데코레이터로, 메서드가 클래스 메서드여야 함을 나타냅니다.

<div class="content-ad"></div>

```python
class Dog:
    breeds = ['german shepherd', 'mongrel', 'retriver']

    @classmethod
    def get_breeds(cls):
        return cls.breeds

print(Dog.get_breeds())

# ['german shepherd', 'mongrel', 'retriver']
```

참고 — 클래스 메서드는 클래스 속성에만 액세스할 수 있습니다. 인스턴스 속성에는 액세스할 수 없습니다.

# compile()

파이썬 코드를 나타내는 문자열을 가져와서 나중에 실행할 수 있게 컴파일합니다.


<div class="content-ad"></div>

```python
# 문자열에서 코드를 컴파일합니다
code = 'a = 4\nb = 5\nprint(a + b)'
code = compile(code, 'test.py', 'exec')

print(code)
# <code object <module> at 0x102a22880, file "test.py", line 1>
```

```python
# 실제 코드를 실행합니다
exec(code)

# 9
```

# complex()

복소수를 생성할 때 사용합니다 (네, 수학에서 배웠던 그것)


<div class="content-ad"></div>

```js
x = complex(4, 5)

print(x)  # (3+5j)
```

# 저작권()

저작권 고지를 출력합니다.

```js
저작권()
'''
Copyright (c) 2001-2023 Python Software Foundation.
All Rights Reserved.

Copyright (c) 2000 BeOpen.com.
All Rights Reserved.

Copyright (c) 1995-2001 Corporation for National Research Initiatives.
All Rights Reserved.

Copyright (c) 1991-1995 Stichting Mathematisch Centrum, Amsterdam.
All Rights Reserved.
'''
```

<div class="content-ad"></div>

# credits()

파이썬에 대한 크레딧을 출력합니다.

```js
credits()

'''
    CWI, CNRI, BeOpen.com, Zope Corporation, 그리고 수많은 사람들에게
    파이썬 개발을 지원해줘서 감사합니다. 더 많은 정보는 www.python.org에서 확인하세요.
'''
```

# delattr()

<div class="content-ad"></div>

오브젝트에서 속성을 삭제합니다.

```js
class Dog:
    def __init__(self, name, age):
        self.name = name
        self.age = age

dog = Dog('rocky', 4)

print(dog.__dict__) # {'name': 'rocky', 'age': 4}
```

```js
delattr(dog, 'age')   # dog 오브젝트에서 'age' 속성 삭제

print(dog.__dict__)   # {'name': 'rocky'}

# ^ age 속성이 삭제되었습니다!
```

# dict()

<div class="content-ad"></div>

딕셔너리를 생성합니다.

```python
d = dict(a=4, b=5, c=6)

print(d)

# {'a': 4, 'b': 5, 'c': 6}
```

## dir()

객체의 모든 속성/메서드를 list[str] 형식으로 반환합니다.

<div class="content-ad"></div>

```js
n = 1

print(dir(n))

'''
[
  '__abs__', '__add__', '__and__', '__bool__', '__ceil__', 
  '__class__', '__delattr__', '__dir__', '__divmod__', 
  '__doc__', '__eq__', '__float__', '__floor__', 
  '__floordiv__', '__format__', '__ge__', '__getattribute__', 
  '__getnewargs__', '__getstate__', '__gt__', '__hash__', 
  '__index__', '__init__', '__init_subclass__', '__int__', 
  '__invert__', '__le__', '__lshift__', '__lt__', '__mod__', 
  '__mul__', '__ne__', '__neg__', '__new__', '__or__', 
  '__pos__', '__pow__', '__radd__', '__rand__', '__rdivmod__', 
  '__reduce__', '__reduce_ex__', '__repr__', '__rfloordiv__', 
  '__rlshift__', '__rmod__', '__rmul__', '__ror__', '__round__', 
  '__rpow__', '__rrshift__', '__rshift__', '__rsub__', 
  '__rtruediv__', '__rxor__', '__setattr__', '__sizeof__', 
  '__str__', '__sub__', '__subclasshook__', '__truediv__', 
  '__trunc__', '__xor__', 'as_integer_ratio', 'bit_count', 
  'bit_length', 'conjugate', 'denominator', 'from_bytes', 
  'imag', 'is_integer', 'numerator', 'real', 'to_bytes'
]
'''
```

객체를 검사하고 어떤 속성/메서드를 사용해야 하는지 알아내는 데 유용할 수 있습니다.

# divmod()

동시에 몫과 나머지를 계산합니다.

<div class="content-ad"></div>


quotient, remainder = divmod(37, 10)

print(quotient)    # 3
print(remainder)   # 7


^ 37을 10으로 나누면 몫은 3이고 나머지는 7입니다.

# enumerate()

일련의 값/반복 가능한 요소의 인덱스와 값을 모두 생성합니다.


<div class="content-ad"></div>

```python
fruits = ['apple', 'orange', 'pear']

for index, fruit in enumerate(fruits):
    print(f'{index=} {fruit=}')

'''
index=0 fruit='apple'
index=1 fruit='orange'
index=2 fruit='pear'
'''
```

# eval()

문자열을 입력으로 받아들이고, 해당 문자열을 파이썬 코드로 평가하여 결과 값을 반환합니다.

```python
print(eval('1 + 2 + 3 + 4 + 5')) # 15
```

<div class="content-ad"></div>

```js
x = 5

print(eval('100 + x'))  # 105
```

# exec()

문자열을 입력으로 받아들이고 해당 문자열을 Python 코드로 실행한 후 None을 반환합니다.

```js
x = 5

exec('x = 100')

print(x) # 100
```

<div class="content-ad"></div>

# exit()

파이썬 프로그램을 완전히 종료합니다.

```js
x = 4
print(x + 1)

exit()  # 이 곳에서 파이썬 프로그램이 종료됩니다. 이후로는 아무 일도 발생하지 않습니다.

print(x + 2)
```

# filter()

<div class="content-ad"></div>

`filter(condition, iterable)`은 조건과 반복 가능한 객체를 받아옵니다.

그리고 조건을 충족하는 요소만을 출력합니다.

다음은 [1,2,3,4,5,6,7]에서 홀수만 남기는 예제입니다.

```js
mylist = [1, 2, 3, 4, 5, 6, 7]

def condition(n: int) -> bool:
    """
    n이 홀수일 경우 True를 반환하고, 그렇지 않으면 False를 반환합니다.
    """
    return n % 2 == 1

for i in filter(condition, mylist):
    print(i, end=' ')
```

<div class="content-ad"></div>

## float()

기본 float 데이터 유형입니다. 객체를 float로 변환합니다.

```js
n = float('3.14')

print(n) # 3.14
```

## format()

<div class="content-ad"></div>

일부 객체를 특정 형식으로 변환하려면 객체의 __format__ 메서드에 따라 특정 형식으로 지정한다.

```python
n = 17

x = format(n, 'b')

print(x)  # 10001
```

^ 메모 — 이것은 정수에 대해서만 특정 방식으로 작동합니다. 다른 데이터 유형은 format()에 대해 다른 동작을 합니다.

# frozenset()

<div class="content-ad"></div>

프로즌셋을 생성합니다. 프로즌셋은 변경할 수 없는 집합입니다.

```js
fs1 = frozenset([1, 2, 3])

print(fs1) # frozenset({1, 2, 3})
```

프로즌셋은 변경할 수 없기 때문에 1) 프로즌셋을 사전 키로 사용하거나 2) 다른 집합에 프로즌셋을 추가할 수 있습니다.

```js
fs1 = frozenset([1])
fs2 = frozenset([1, 2])

d = {fs1:100, fs2: 200}   # 문제없음
```

<div class="content-ad"></div>

```python
# getattr()

# 객체의 속성에 접근합니다.

class Dog:
    def __init__(self, name):
        self.name = name

dog = Dog('rocky')

print(dog.name)              # rocky
```

```python
print(getattr(dog, 'name'))  # rocky
```

<div class="content-ad"></div>

`dog` 객체의 `name` 속성에 접근하는 것은 `getattr(dog, 'name')`와 `dog.name`이 동일합니다.

`getattr()`은 객체의 속성에 동적으로 접근하고 싶을 때 사용합니다. 속성 이름을 문자열 값으로 전달할 수 있습니다.

## globals()

모든 전역 변수를 포함하는 사전을 반환합니다.

<div class="content-ad"></div>

```js
a = 4
b = 5

print(globals())

# { # bunch of stuff , 'a': 4, 'b': 5}
```

globals() 함수를 사용하여 변수도 동적으로 설정할 수 있어요

```js
globals()['a'] = 100

print(a) # 100
```

hasattr() 함수


<div class="content-ad"></div>

객체가 속성을 가지고 있는 경우 True를 반환하고, 그렇지 않으면 False를 반환합니다.

```js
class Dog:
    def __init__(self, name):
        self.name = name

dog = Dog('rocky')

print(hasattr(dog, 'name'))      # True
print(hasattr(dog, 'nickname'))  # False
```

# hash()

불변 객체에 대한 해시값을 반환합니다. hash()는 변경 가능한 객체에 작동하지 않으며, 객체가 변경 가능한지 확인하는 데 사용할 수 있습니다.

<div class="content-ad"></div>

```js
print(hash(5))        # 5
print(hash(100))      # 100
print(hash('apple'))  # 6125743690088875441
```

```js
print(hash([]))       # ERROR
print(hash({}))       # ERROR
```

# help()

pass in하신 객체에 대한 문서를 프린트합니다.

<div class="content-ad"></div>

```js
print에 대한 도움말

내장함수 print에 대한 도움말:

print(*args, sep=' ', end='\n', file=None, flush=False)
    값을 스트림 또는 기본적으로 sys.stdout에 출력합니다.

    sep
      값 사이에 삽입되는 문자열, 기본값은 공백입니다.
    end
      마지막 값 뒤에 추가되는 문자열, 기본값은 새 줄입니다.
      ...
```

# hex()

숫자를 16진수로 변환합니다.

```js
print(hex(8))    # 0x8
print(hex(9))    # 0x9

print(hex(10))   # 0xa
print(hex(11))   # 0xb

print(hex(15))   # 0xf
print(hex(16))   # 0x10

print(hex(17))   # 0x11
print(hex(18))   # 0x12
```

<div class="content-ad"></div>

익숙하지 않은 분들을 위해 16진법은 16진수로 계산하는 것을 의미합니다. 기본적으로 10개 대신 16개의 손가락이 있는 것처럼 수를 세는 방식이죠.

# id()

파이썬의 모든 객체는 고유한 id를 갖습니다. id(object)는 해당 id를 반환합니다.

```python
a = 'apple'
b = 'orange'

print(id(a))    # 4365506656
print(id(b))    # 4365501328
```

<div class="content-ad"></div>

이 ID의 16진수 형식은 객체를 출력할 때 기본 __str__() 메서드에서 반환되는 것과 동일합니다.

```python
class Dog:
    pass

dog = Dog()

print(dog)           # <__main__.Dog object at 0x1048265a0>

print(id(dog))       # 4370621856
print(hex(id(dog)))  # 0x1048265a0
```

# input()

파이썬 프로그램에 입력을 입력할 수 있게 합니다.

<div class="content-ad"></div>

```python
이름 = input('이름을 입력하세요 >>> ')

print('당신의 이름은', 이름)

# 이름을 입력하세요 >>> 톰
# 당신의 이름은 톰
```

# int()

기본 정수 데이터 유형. 개체를 정수로 변환합니다.

```python
n = int('100')

print(n)      # 100
print(n + 1)  # 101
```

<div class="content-ad"></div>

# isinstance()

isinstance(o, type)는 o가 type의 인스턴스인지 확인합니다.

```js
a = 'apple'

print(isinstance(a, str))  # True

print(isinstance(a, int))  # False
```

이것은 상위 클래스에 대해서도 동작합니다:

<div class="content-ad"></div>

```python
class Animal:
    ...

class Dog(Animal):
    ...

dog = Dog()

print(isinstance(dog, Dog))      # True

print(isinstance(dog, Animal))   # True
```

# issubclass()

`issubclass(class1, class2)` 함수는 class1이 class2의 하위 클래스(또는 자식 클래스)인지 확인합니다.

```python
class Animal:
    ...

class Dog(Animal):
    ...

print(issubclass(Dog, Animal))
```

<div class="content-ad"></div>

# iter()과 next()

iter(mylist)는 반복자(iterator) 객체를 반환합니다.

next(반복자_객체)는 반복자 객체에서 다음 요소를 반환합니다.

```js
# 보통 우리가 반복하는 방법

fruits = ['apple', 'orange', 'pear']

for fruit in fruits:
    print(fruit)

# apple orange pear
```

<div class="content-ad"></div>

```python
# iter() 및 next()를 사용하여 반복하는 방법

fruits = ['apple', 'orange', 'pear']

iterator = iter(fruits)

a = next(iterator)
print(a)  # apple

b = next(iterator)
print(b)  # orange

c = next(iterator)
print(c)  # pear
```

iter() 및 next()는 우리에게 반복 작업에서 더 많은 통제를 제공해주어 유용합니다.

# len()

len(object)은 객체의 길이를 찾습니다.


<div class="content-ad"></div>

```js
mystring = 'applepie'

print(len(mystring)) # 8
```

```js
mylist = [1, 2, 3, 4, 5]

print(len(mylist))  # 5
```

```js
mydict = {'apple':1, 'orange':2}

print(len(mydict))  # 2
```

# license()

<div class="content-ad"></div>

이것은 파이썬 라이센스를 출력합니다:

```js
license()

'''
A. 소프트웨어의 역사
===========================

파이썬은 1990년대 초에 네덜란드의 Stichting Mathematisch Centrum (CWI, https://www.cwi.nl 참조)에서
Guido van Rossum에 의해 만들어졌습니다. ABC 라는 언어의 후속 제품으로 개발되었습니다. Guido는 파이썬의
주요 저자이지만 다른 사람들로부터 많은 기여를 받았습니다.

등등 등등
'''
```

# list()

리스트 데이터 유형입니다. 다른 iterable 데이터 유형도 리스트로 변환합니다.

<div class="content-ad"></div>

```python
s = 'apple'

ls = list(s)

print(ls)  # ['a', 'p', 'p', 'l', 'e']
```

# locals()

로컬 변수를 포함하는 사전을 반환합니다.

```python
a = 1
b = 2

def test():
    c = 3
    d = 4
    print(locals())

test()

# {'c': 3, 'd': 4}
```

<div class="content-ad"></div>

^ 변수 a와 b는 전역 변수이므로 locals()에 나타나지 않습니다.

# map()

map(func, iterable)은 iterable의 모든 요소에 func을 적용합니다.

```js
def add10(n: int) -> int:
    return n + 10

numbers = [1, 2, 3, 4, 5]

new = map(add10, numbers)
new = list(new)

print(new)    # [11, 12, 13, 14, 15]
```

<div class="content-ad"></div>

메모 — map() 함수 자체가 제너레이터를 반환하므로 위 예제에서 숫자를 표시하려면 리스트로 변환해야 합니다.

# max()

시퀀스 내에서 가장 큰 값을 찾습니다.

```js
numbers = [1, 5, 3, 9, 2]

n = max(numbers)

print(n) # 9
```

<div class="content-ad"></div>

# min()

시퀀스 내부의 최솟값을 찾습니다.

```js
numbers = [1, 5, 3, 9, 2]

n = min(numbers)

print(n) # 1
```

# memoryview

<div class="content-ad"></div>

메모리 뷰는 객체의 버퍼 프로토콜을 안전하게 노출하는 방법으로, 객체의 내부 버퍼에 액세스할 수 있게 해줍니다.

memoryview() 함수는 메모리 뷰를 반환하며, 데이터에 접근할 수 있게 해줍니다. 데이터를 복사/복제할 필요가 없습니다.

```python
x = b'apple'

mv = memoryview(x)

print(mv)  # <memory at 0x10085e2c0>
```

```python
print(mv[0])  # 97
print(mv[1])  # 112
print(mv[2])  # 112
print(mv[3])  # 108
print(mv[4])  # 101
```

<div class="content-ad"></div>

요 — 아마도 알아두면 좋은 사실이야. 실제 작업에서는 사용해본 적이 없어.

# object()

object()는 아무 작업도 수행할 수 없는 빈 객체를 반환해.

이 객체는 모든 다른 클래스의 기본 클래스로, 즉 사용자 지정 객체는 모두 object를 상속받아야 해.

<div class="content-ad"></div>

```python
class Dog:
    pass

dog = Dog()

print(dog.__bases__)

# (<class 'object'>,)
```

^ 모든 객체는 기본 객체로부터 자동으로 상속됩니다

# oct()

oct(n) 함수는 숫자를 입력받아 해당 숫자의 8진수 값을 반환합니다.

<div class="content-ad"></div>

```js
print(oct(6))   # 0o6
print(oct(7))   # 0o7 
print(oct(8))   # 0o10
print(oct(9))   # 0o11
print(oct(10))  # 0o12
```

8진수에서는 8진법으로 숫자를 세어야 합니다. 즉, 0부터 7까지의 숫자만 사용됩니다.

10은 7 다음에, 20은 17 다음에, 30은 27 다음에 옵니다.

# open()

<div class="content-ad"></div>

파일을 읽기/쓰기용으로 열 때 사용합니다.

```python
with open('fruits.txt') as f:
    # fruits.txt 안에 있는 텍스트를 읽어옵니다.
    text = f.read()

print(text)
```

## ord()

ord(chr)는 문자 chr의 순서를 반환합니다.

<div class="content-ad"></div>

```js
print(ord('A'))  # 65
print(ord('B'))  # 66

print(ord('a'))  # 97
print(ord('b'))  # 98
print(ord('c'))  # 99
```

# pow()

`pow(a, b)` 함수는 2개의 숫자를 받아서 a를 b 승으로 반환합니다.

기본적으로 `a ** b`와 같습니다.

<div class="content-ad"></div>

```js
print(pow(2, 2))  # 4
print(pow(2, 3))  # 8
print(pow(2, 4))  # 16
```

## print()

standard output에 내용을 출력합니다 (일반적으로 터미널)

## property()

<div class="content-ad"></div>

클래스에 사용되는 데코레이터로 속성을 프로퍼티로 변환하는 것

```js
class Dog:
    def __init__(self, name):
        self.__name = name

    @property
    def name(self):
        return self.__name

    @name.setter
    def name(self, new_name):
        self.__name = new_name

dog = Dog('name')
```

dog.name을 직접 호출하여 이름을 얻을 수 있습니다

```js
print(dog.name)    # rocky
```

<div class="content-ad"></div>

위에서 setter 메서드를 정의했기 때문에 dog.name을 직접 변경할 수도 있어요

```js
dog.name = 'fifi'

print(dog.name)    # fifi
```

# quit()

exit()과 같이 Python 프로그램을 완전히 중단시킵니다.

<div class="content-ad"></div>

# range()

일련의 숫자를 생성할 수 있게 해줍니다.

```js
for i in range(5):
    print(i)

# 0 1 2 3 4
```

```js
for i in range(2, 6):
    print(i)

# 2 3 4 5
```

<div class="content-ad"></div>


# repr()

객체를 표현하는 문자열을 반환합니다. 주로 개발 및 디버깅에 사용됩니다.

```js
from datetime import datetime

dt = datetime(2024, 3, 7)

print(str(dt))   # 2024-03-07 00:00:00

print(repr(dt))  # datetime.datetime(2024, 3, 7, 0, 0)
```


<div class="content-ad"></div>

# reversed()

뒤로 반복하는 reverseiterator를 반환합니다.

```js
ls = [1, 2, 3, 4, 5]

ls = list(reversed(ls))

print(ls)   # [5, 4, 3, 2, 1]
```

# round()

<div class="content-ad"></div>

소수점에서 가장 가까운 수로 반올림합니다.

```js
pi = 3.14159265

print(round(pi))      # 3
print(round(pi, 2))   # 3.14
print(round(pi, 5))   # 3.14159
```

이것은 또한 가장 가까운 10, 100, 1000 등으로 수를 반올림하는 데 사용할 수 있습니다.

```js
num = 987654321

print(round(num))      # 987654321
print(round(num, -1))  # 987654320
print(round(num, -2))  # 987654300
print(round(num, -3))  # 987654000
```

<div class="content-ad"></div>

# set()

내장된 집합 데이터 타입. 집합:

- 중복 값을 포함할 수 없음
- 원소가 존재하는지 확인하는 데 O(1) 시간이 걸림

```js
s = set()

s.add(1)
s.add(2)
s.add(3)

print(s)  # {1, 2, 3}
```

<div class="content-ad"></div>

# setattr()

어떤 객체의 속성에 값을 설정할 수 있게 해줍니다.

```js
# 일반적인 Dog 클래스와 객체

class Dog:
    pass

dog = Dog()
```

```js
# 속성을 보통 설정하는 방법

dog.name = 'rocky'

print(dog.name)  # rocky
```

<div class="content-ad"></div>

```python
# setattr() 함수를 사용하여 속성 설정하기

setattr(개, '이름', '피피')

print(개.이름)  # 피피
```

setattr() 함수는 객체 내부에서 특정 속성 이름에 동적으로 값을 설정하고 싶을 때 유용합니다.

# slice()

문자열 슬라이싱을 기억하시나요? (이 방법은 리스트에 대해서도 동일하게 작동합니다)

<div class="content-ad"></div>

```js
s = 'abcdefg'

print(s[0:3])  # abc
print(s[3:])   # defg
```

- s[0:3]에서의 0:3은 사실 s[slice(0, 3)]과 동일합니다.
- s[3:]에서의 3:은 사실 s[slice(3, None)]과 동일합니다.

```js
s = 'abcdefg'

print(s[slice(None, 3)])  # abc
print(s[slice(3, None)])  # defg
```

^ 이제 이 지식을 가지고 동적 슬라이싱을 수행할 수 있습니다.

<div class="content-ad"></div>

# sorted()

시퀀스(예: 리스트)의 정렬된 복사본을 반환합니다.

```js
ls = [1, 5, 3, 4, 2]

new = sorted(ls)

print(new)
# [1, 2, 3, 4, 5]
```

참고 - 원래 리스트 ls는 변경되지 않고 여전히 [1,5,2,4,3]입니다.

<div class="content-ad"></div>

# staticmethod()

클래스에서 정적 메서드를 만들기 위해 사용되는 데코레이터입니다.

```js
class MyClass

    @staticmethod
    def doStuff(x, y):
        return x + y

val = MyClass.doStuff(1, 4)

print(val) # 5
```

정적 메서드는 인스턴스 속성이나 클래스 속성에 접근할 수 없습니다. 보통 우리는 이름 공간을 위해 정적 메서드를 사용합니다.

<div class="content-ad"></div>

# str()

내장 문자열 유형입니다. 또한 다른 객체를 문자열로 변환합니다.

```js
a = 5
b = 3.14

print(str(a))    # 5
print(str(b))    # 3.14

new = str(a) + str(b)

print(new)       # 53.14
```

# sum()

<div class="content-ad"></div>

sum(sequence) 함수는 sequence 내부의 요소를 합하여 총 합계를 반환합니다.

```js
numbers = [1, 2, 3, 4, 5]

total = sum(numbers)

print(total)    # 15
```

# super()

클래스 내부에서 사용할 때, super() 함수는 이 객체의 부모 클래스를 나타내는 객체를 반환합니다. (부모 클래스 자체가 아님)

<div class="content-ad"></div>

```python
class Animal:
    pass

class Dog(Animal):
    def __init__(self):
        print(super())

Dog()  # <super: <class 'Dog'>, <Dog object>
```

^ `super()`을 사용하면 부모 클래스의 메소드에 액세스할 수 있어요.

```python
class Animal:
    def speak(self):
        print('I am Animal')

class Dog(Animal):
    def __init__(self):
        super().speak()
        self.speak()

    def speak(self):
        print('woof')

Dog()

# I am Animal
# woof
```

- Dog 클래스에서 `super().speak()`는 Animal의 speak 메소드에 접근합니다.
- Dog 클래스에서 `self.speak()`는 Dog의 speak 메소드에 접근합니다.


<div class="content-ad"></div>

# 튜플()

내장 튜플 데이터 유형입니다. 또한 객체를 튜플로 변환합니다.

익숙하지 않은 분들을 위해, 튜플은 생성 후에 수정할 수 없는 리스트입니다. 장점: 튜플은 사전 키로 사용될 수 있으며 집합에 추가할 수 있습니다.

```js
ls = [1, 2, 3]

t = tuple(ls)

print(t)    # (1, 2, 3)
```

<div class="content-ad"></div>

# type()

type(object)은 객체의 타입을 반환합니다.

```js
print(type(1))        # <class 'int'>

print(type(3.14))     # <class 'float'>

print(type('apple'))  # <class 'str'>

print(type([]))       # <class 'list'>

print(type({}))       # <class 'dict'>
```

사용자 정의 클래스에도 동작합니다

<div class="content-ad"></div>

```python
class Dog:
    pass

dog = Dog()

print(type(dog))    # <class '__main__.Dog'>
```

## vars()

vars(obj)는 obj의 __dict__ 속성을 반환합니다.

익숙하지 않은 분들을 위해, obj.__dict__는 그 속성이 변경 가능한 딕셔너리를 담고 있는 딕셔너리입니다.


<div class="content-ad"></div>

```python
class Dog:
    def __init__(self, name, age, gender):
        self.name = name
        self.age = age
        self.gender = gender

dog = Dog('rocky', 5, 'male')
```

```python
# printing dog.__dict__

print(dog.__dict__)

# {'name': 'rocky', 'age': 5, 'gender': 'male'}
```

```python
# using vars()

print(vars(dog))

# {'name': 'rocky', 'age': 5, 'gender': 'male'}
```

이 방법은 객체를 검사하고 싶을 때 유용합니다.

<div class="content-ad"></div>

# zip()

동시에 두 개 이상의 시퀀스를 반복할 수 있게 해줍니다.

```js
fruits = ['apple', 'orange', 'pear']
prices = [4, 5, 6]

for fruit, prices in zip(fruits, prices):
    print(fruit, price)

# apple 4
# orange 5
# pear 6
```

zip()에는 원하는 개수의 인수를 전달할 수 있습니다.

<div class="content-ad"></div>

```js
a = [1, 2, 3]
b = [2, 4, 6]
c = [3, 6, 9]
d = [4, 8, 12]

for i,j,k,l in zip(a, b, c, d):
    print(i, j, k, l)

# 1 2 3 4
# 2 4 6 8
# 3 6 9 12
```

# 결론

이 내용이 명확하고 이해하기 쉬웠으면 좋겠어요

# 제작자로서 저를 지원하고 싶다면

<div class="content-ad"></div>

- 제 Substack 뉴스레터에 가입해주세요: https://zlliu.substack.com/ — 매주 Python 관련 이메일을 보내드립니다.
- 제 책을 구매하세요: 101 Things I Never Knew About Python, https://payhip.com/b/vywcf
- 해당 이야기에 대해 50번 박수를 치세요.
- 귀하의 생각을 말씀해주세요.
- 이야기 중 가장 마음에 드는 부분을 강조해주세요.

감사합니다! 이 작은 조치들이 큰 도움이 되고, 정말 감사드립니다!

YouTube: https://www.youtube.com/@zlliu246

LinkedIn: https://www.linkedin.com/in/zlliu/