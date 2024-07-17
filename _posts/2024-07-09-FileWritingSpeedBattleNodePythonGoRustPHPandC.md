---
title: "파일 쓰기 속도 배틀 Nodejs, 파이썬, Go, Rust, PHP와 C 비교"
description: ""
coverImage: "/assets/img/2024-07-09-FileWritingSpeedBattleNodePythonGoRustPHPandC_0.png"
date: 2024-07-09 17:33
ogImage:
  url: /assets/img/2024-07-09-FileWritingSpeedBattleNodePythonGoRustPHPandC_0.png
tag: Tech
originalTitle: "File Writing Speed Battle: Node, Python, Go, Rust, PHP and C"
link: "https://medium.com/towardsdev/file-writing-speed-battle-node-python-go-rust-php-and-c-8a1c35ad870e"
---

<img src="/assets/img/2024-07-09-FileWritingSpeedBattleNodePythonGoRustPHPandC_0.png" />

저는 다양한 프로그래밍 언어가 내 신뢰하는 로컬 디스크에 엄청난 90,000줄의 코드를 얼마나 빨리 쓸 수 있는지 확인하기 위해 엉뚱한 모험에 돌입하기로 결정했습니다. 이 실험은 화려한 장비가 설치된 최상급 랩에서 진행된 것이 아니라, 제 겸손한 ARM 64 M2 칩에서 진행되었습니다. 여러분, 이 실험은 정말 스릴 넘치죠!

그런데 유의해 주세요: 이것은 전형적으로 꼼꼼하게 조절된 연구가 아닙니다. 이는 오히려 알 수 없는 세계로의 대담한 원정으로 생각해 보십시오. 약간의 혼돈을 더한 것이죠. 그래도 실험복, 안전 고글이 없어도 여러분은 아주 멋진 통찰력을 얻게 됩니다. 그러니 잠그র 묶은 후락하세요. 이야기가 흥미롭게 될 것입니다!

# 소스 코드

<div class="content-ad"></div>

# 실험 결과

- 파일 크기는 약 1.7MB입니다.

- 노드JS가 이렇게도 느릴 수 있다니! 상상도 못 했네요.

- 파일에 90000줄을 작성하는 가장 빠른 방법은 버퍼링된 단일 스레드를 사용하는 것입니다. 이는 여러 스레드를 만들고 관리하는 것이 오버헤드를 추가할 수 있으며, 특히 파일로 쓰는 것과 같은 I/O 바운드 작업에 대해서입니다. 버퍼링된 쓰기는 기존 작성자에 대한 호출 수를 최소화하며, 작은 쓰기가 많이 포함된 작업에 대해 성능을 크게 향상시킬 수 있습니다.

<div class="content-ad"></div>

# Node 파일을 실행하는 방법

```js
➜  SpeedyWrite git:(main) ✗ node main.js
writeStream: 718.873ms
writeFile: 736.844ms
```

# Python 파일을 실행하는 방법

```js
➜  SpeedyWrite git:(main) ✗ python main.py
파이썬 버전에 소요된 시간: 12.826204299926758 밀리초
```

<div class="content-ad"></div>

파이썬의 내장 파일 입출력 함수는 매우 최적화되어 있어요. 'w' 모드를 사용하여 파일을 쓸 때 open 함수를 사용하면 Python은 버퍼링된 이진 파일 객체를 생성하는데, 이는 버퍼를 사용하여 작업을 일괄적으로 기록하여 시스템 호출 수를 줄이고 성능을 향상시킵니다.

# Go 파일 실행 방법

```js
➜  writeFile git:(master) ✗ go build -o file
➜  writeFile git:(master) ✗ ./file
처음 버전으로 실행 중...
첫 번째 버전에 걸린 시간: 2.455568375초
향상된 버전으로 실행 중...
향상된 버전에 걸린 시간: 7.690667ms
```

Go 1.17부터 기본 크기가 4096바이트인 bufio.NewWriter를 사용하거나 크기를 지정할 수 있는 bufio.NewWriterSize를 사용할 수 있어요.

<div class="content-ad"></div>

# 러스트 파일 실행하는 방법

```bash
➜  SpeedyWrite git:(main) ✗ cargo run main.rs
   speedy_write v0.1.0을 컴파일 중
    0.18초 내에 빌드 완료 [최적화되지 않음 + 디버그 정보 포함]
     `target/debug/speedy_write main.rs` 실행 중
함수 내에서 경과된 시간은: 6.700958밀리초
```

# PHP 파일 실행하는 방법

```bash
➜  SpeedyWrite git:(main) ✗ php main.php
PHP 버전에 소요된 시간: 116.61 밀리초
```

<div class="content-ad"></div>

# C 파일 실행 방법

```js
➜  cFile git:(main) ✗ gcc main.c -o cFile
➜  cFile git:(main) ✗ ./cFile
소요 시간: 18.044000 밀리초
```

프로그램의 성능은 코드의 효율성, 언어의 기능, 컴파일러의 성능 및 실행되는 시스템의 특성과 같은 여러 요소에 의해 영향을 받을 수 있습니다.

이 경우 C 코드가 파일 I/O 작업을 처리하는 방식으로 인해 Rust 및 Go 코드보다 느릴 수 있습니다. C 표준 라이브러리의 파일 I/O 함수인 fputs와 같은 것들은 일반적으로 Rust와 Go에서 사용되는 파일 I/O 라이브러리보다 효율성이 떨어질 수 있습니다.

<div class="content-ad"></div>

Rust과 Go는 성능을 염두에 두고 설계된 더 현대적인 표준 라이브러리를 가지고 있어요. 버퍼링 및 다른 기술을 사용하여 시스템 호출의 횟수를 최소화하여 파일 I/O 작업의 속도를 크게 향상시킬 수 있어요.

게다가 Rust와 Go 컴파일러는 C를 사용하는 gcc 컴파일러보다 더 발전된 최적화 능력을 가지고 있어요. 코드에 다양한 최적화를 자동으로 적용하여 더 빠르게 실행될 수 있도록 할 수 있어요.

# 왜 NodeJS가 Python을 따라잡지 못하는 이유

## 비동기적인 부담:

<div class="content-ad"></div>

Node.js는 본질적으로 비동기적이며 이벤트 주도 아키텍처를 사용합니다. 이는 많은 작업을 동시에 처리하는 데 효과적이지만, 특히 I/O 바운드 작업에 대한 콜백 및 이벤트 루프 설정 및 해제로 인해 약간의 오버헤드가 발생할 수 있습니다.

## V8 엔진:

Node.js는 V8 JavaScript 엔진에서 실행되며, 높은 최적화 수준을 갖추고 있지만, 특히 I/O 바운드보다는 CPU 바운드 작업에 대해서는 Python의 CPython 인터프리터보다 빠르지 않을 수 있습니다.

## 버퍼링:

<div class="content-ad"></div>

파이썬의 파일 I/O 작업은 기본적으로 버퍼링되어 있습니다. 이는 많은 소규모 쓰기 작업을 한꺼번에 모아 하나의 큰 작업으로 처리하여 시스템 호출의 수를 줄이고 성능을 향상시킵니다. 반면 Node.js의 fs.createWriteStream도 버퍼링을 사용하지만, 구체적으로 구현된 방식에 따라 파이썬의 구현보다 느릴 수도 있습니다.
