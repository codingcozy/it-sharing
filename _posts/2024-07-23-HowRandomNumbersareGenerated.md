---
title: "난수 생성 방법은 어떻게 될까"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-07-23 21:48
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "How Random Numbers are Generated"
link: "https://dev.to/sh20raj/how-random-numbers-are-generated-40e0"
---


# 난수 생성 이해하기

난수는 시뮬레이션, 암호화, 통계 샘플링 등 다양한 분야에서 중요한 역할을 합니다. 이 글은 무작위 숫자가 어떻게 생성되는지에 대해 다루며, JavaScript와 Python이라는 인기 있는 프로그래밍 언어를 중점적으로 살펴봅니다.

## 의사난수 생성기 (PRNGs)

대부분의 프로그래밍 언어는 의사난수 생성기(PRNGs)를 사용하여 무작위 숫자를 생성합니다. PRNGs는 수학적 알고리즘을 사용하여 무작위로 보이는 숫자 시퀀스를 생성합니다. 이 숫자들은 초기값인 시드에 의해 결정되기 때문에 실제로 무작위가 아닙니다. 하지만 많은 응용 프로그램에는 충분합니다.

<div class="content-ad"></div>

### PRNGs의 특성

- 결정론적: 동일한 시드가 주어지면 PRNG는 항상 동일한 숫자 시퀀스를 생성합니다.
- 주기성: PRNG는 일정 기간이 지난 후 숫자 시퀀스를 반복합니다.
- 속도: PRNG는 일반적으로 빠르고 효율적입니다.

## JavaScript: Math.random()

JavaScript의 Math.random() 함수는 임의의 숫자를 생성하는 데 자주 사용됩니다. Math.random()에서 사용하는 정확한 알고리즘은 다른 JavaScript 엔진 간에 다를 수 있지만, 널리 사용되는 알고리즘은 Mersenne Twister입니다.

<div class="content-ad"></div>

### Math.random()이 어떻게 작동하는지

Mersenne Twister는 긴 주기와 고품질의 난수로 유명합니다. 다음은 JavaScript에서 Mersenne Twister 알고리즘을 구현하는 간소화된 예제입니다:

```js
class MersenneTwister {
  constructor(seed) {
    if (seed === undefined) {
      seed = new Date().getTime();
    }
    this.mt = new Array(624);
    this.index = 0;
    this.mt[0] = seed;
    for (let i = 1; i < 624; i++) {
      this.mt[i] = (0x6c078965 * (this.mt[i - 1] ^ (this.mt[i - 1] >> 30)) + i) >>> 0;
    }
  }

  generate() {
    if (this.index === 0) {
      this.twist();
    }
    let y = this.mt[this.index];
    y = y ^ (y >> 11);
    y = y ^ ((y << 7) & 0x9d2c5680);
    y = y ^ ((y << 15) & 0xefc60000);
    y = y ^ (y >> 18);
    this.index = (this.index + 1) % 624;
    return y / 0xffffffff;
  }

  twist() {
    for (let i = 0; i < 624; i++) {
      const y = (this.mt[i] & 0x80000000) + (this.mt[(i + 1) % 624] & 0x7fffffff);
      this.mt[i] = this.mt[(i + 397) % 624] ^ (y >> 1);
      if (y % 2 !== 0) {
        this.mt[i] = this.mt[i] ^ 0x9908b0df;
      }
    }
  }
}

// 사용 예제:
const mt = new MersenneTwister(12345); // 시드 값
const randomNumber = mt.generate(); // 랜덤 숫자 가져오기
console.log(randomNumber);
```

이 코드는 난수를 생성하는 데 사용되는 Mersenne Twister 알고리즘의 간소화된 버전을 보여줍니다.

<div class="content-ad"></div>

### Math.random()을 사용하여

JavaScript에서는 Math.random()을 사용하여 0(포함)과 1(미포함) 사이의 임의의 숫자를 생성할 수 있어요:

```js
const randomNumber = Math.random();
console.log(randomNumber);
```

## Python: random 모듈

<div class="content-ad"></div>

파이썬은 무작위 숫자를 생성하는 다양한 함수를 포함하는 random 모듈을 제공합니다. 파이썬의 random 모듈이 사용하는 기본 PRNG 알고리즘은 Mersenne Twister입니다.

### Python의 random 모듈 사용 방법

다음은 Python에서 무작위 숫자를 생성하는 예시입니다:

```python
import random

# 0.0부터 1.0까지의 무작위 부동 소수점 숫자 생성
random_float = random.random()
print(random_float)

# 1부터 100까지의 무작위 정수 생성
random_int = random.randint(1, 100)
print(random_int)

# 평균이 0이고 표준편차가 1인 정규분포에서 무작위 숫자 생성
random_normal = random.gauss(0, 1)
print(random_normal)
```

<div class="content-ad"></div>

### 랜덤 숫자 생성기 시드하는 방법

재현성을 보장하기 위해 Python에서 랜덤 숫자 생성기를 시드할 수 있습니다:

```python
import random

# 랜덤 숫자 생성기 시드
random.seed(12345)

# 랜덤 숫자 생성
print(random.random())
print(random.randint(1, 100))
```

동일한 시드 값을 사용하면 프로그램을 실행할 때마다 동일한 순서의 랜덤 숫자가 생성됩니다.

<div class="content-ad"></div>

## 결론

랜덤 숫자 생성은 다양한 응용 분야에서 중요한 개념입니다. JavaScript의 Math.random() 및 Python의 random 모듈에서 생성된 숫자는 완전히 무작위가 아니지만, 대부분의 실제 용도에는 충분히 무작위성을 제공합니다. 이러한 생성기가 어떻게 작동하는지 이해하고 효과적으로 사용하는 방법을 파악하는 것은 개발자와 연구자에게 모두 중요합니다.

본 문서는 JavaScript와 Python에서 랜덤 숫자가 생성되는 기본 개요와 Math.random() 및 Python의 random 모듈을 활용하는 실용적인 예제를 제공합니다.