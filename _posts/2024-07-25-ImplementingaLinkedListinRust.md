---
title: "Rust로 연결 리스트 구현하는 방법"
description: ""
coverImage: "/assets/img/2024-07-25-ImplementingaLinkedListinRust_0.png"
date: 2024-07-25 12:08
ogImage: 
  url: /assets/img/2024-07-25-ImplementingaLinkedListinRust_0.png
tag: Tech
originalTitle: "Implementing a Linked List in Rust"
link: "https://medium.com/gitconnected/implementing-a-linked-list-in-rust-70626c8c95d4"
---



![2024-07-25-ImplementingaLinkedListinRust_0](/assets/img/2024-07-25-ImplementingaLinkedListinRust_0.png)

저는 스스로 공부한 프로그래머이며 지난 몇 달간 Rust를 배우고 있습니다. Rust를 사용할 만한 특별한 이유는 없습니다. 주로 웹 개발을 하고 있고, 일반적으로 해결해야 하는 문제들은 완전히 다른 언어로 전환할 정도의 성능 향상을 요구하지 않습니다!

대신, 프로그래밍이 실제로 어떻게 작동하는지, JavaScript나 Python과 같은 상위 수준 언어가 우리를 위해 무엇을 하는지에 대한 내 이해를 높이기 위해 Rust를 사용하고 있습니다.

그 목표를 갖고 데이터 구조에 대한 이해도를 높여보는 것이 어떨까요?


<div class="content-ad"></div>

# 왜 연결 리스트를 사용해야 하나요?

연결 리스트는 컴퓨터 과학에서 기본적인 데이터 구조 중 하나입니다.

하지만 웹 개발자라면, 프론트엔드 애플리케이션 코드에서 특정 리스트 구현이 필요한 경우는 드뭅니다. 언어에 내장되어 있고 언더라이잉 JavaScript 엔진이 사용하는 저수준 언어인 보통 C++에서 최적화된 배열을 사용하는 것이 더 빠릅니다.

하지만 Rust에서는 올바른 데이터 구조를 선택하는 것이 정말 중요합니다. 낮은 수준의 메모리 관리와 같은 도구에 액세스할 수 있기 때문에 올바른 데이터 구조를 선택하면 성능이나 메모리 측면에서 우위를 점할 수 있습니다.

<div class="content-ad"></div>

만약 Rust나 데이터 구조에 대해 배우고 있다면 (또는 둘 다에 대해), 당신은 올바른 곳에 왔습니다!

# 익숙한 영역에서 시작하기

더 높은 수준의 언어를 알고 있다면 배열에 익숙할 것입니다. 이들은 각 항목이 인덱스로 지정되는 리스트 유형입니다. 보통 0부터 시작하는 인덱스가 지정됩니다. 아래와 같이요.

![Implementing a Linked List in Rust](/assets/img/2024-07-25-ImplementingaLinkedListinRust_1.png)

<div class="content-ad"></div>

인덱스를 사용하는 것은 우리에게 강력한 이점을 제공합니다. 이는 배열의 크기와 관계 없이 요소에 액세스하는 데 고정된 시간이 걸리기 때문입니다. 빅 O 표기법에서 이를 O(1)이라고 합니다.

이 효율성은 배열이 메모리를 사용하는 방식인 "메모리 레이아웃" 때문에 있습니다. 배열 내 각 항목은 "인접한" 메모리 블록에 저장됩니다. 각 항목의 크기와 배열의 시작 주소를 알기 때문에 어떤 인덱스의 위치도 직접 계산할 수 있습니다.

그래서 배열에서 요소에 빨리 액세스하는 것이 매우 빠릅니다. 그러나 다른 연산은 느립니다.

# 배열의 일부 제한점

<div class="content-ad"></div>

새로운 요소를 삽입하는 것은 어떻습니까? 배열 끝에 요소를 푸시하면 해당 작업도 상수 시간 내에 발생합니다.

하지만 배열의 시작에 새 항목을 삽입해야 하는 경우 다른 항목의 인덱스를 증가시켜야 합니다. 0은 1이 되고, 1은 2가 되는 식으로 진행됩니다. 이는 작업이 선형적이라는 것을 의미합니다.

따라서, 삽입 작업은 O(1)의 최상의 경우 시나리오를 가지고 있지만, O(N)의 최악의 경우 시나리오도 있습니다. 평균적으로, 항목을 삽입하는 데 걸리는 시간이 배열의 길이와 비례하므로 우리는 이를 O(N)이라고 부릅니다.

시작 부분에 빠르게 항목을 삽입해야 하는 경우, 다른 데이터 구조가 필요할 것입니다...

<div class="content-ad"></div>

# 링크드 리스트 소개 (꼬리 포인터 포함)

꼬리 포인터가 있는 링크드 리스트에서는 상수 시간에 요소를 삽입할 수 있습니다. 이는 리스트의 시작 부분이나 꼬리 부분에 삽입할 수 있기 때문입니다. 그러나 요소에 접근하는 것은 선형 시간이 걸립니다.

링크드 리스트의 각 요소 또는 노드에는 다음 내용이 포함되어 있습니다:

- 값
- 시퀀스에서 다음 요소를 참조하는 포인터

<div class="content-ad"></div>

예를 들어:

![LinkedList](/assets/img/2024-07-25-ImplementingaLinkedListinRust_2.png)

위의 예시에서, 첫 번째 노드는 값 13과 다음 노드를 가리키는 화살표로 표시됩니다. 두 번째 노드는 값 45와 다음 노드를 가리키는 화살표가 있고, 그 다음으로 이어집니다.

시퀀스의 마지막 요소는 보통 러스트에서 None으로 나타내는 아무것도 가리키는 참조를 가지고 있습니다.

<div class="content-ad"></div>

시퀀스에서 첫 번째와 마지막 요소를 기록하는 것도 도움이 됩니다. 이러한 요소를 각각 head(머리)와 tail(꼬리)라고 부릅니다:

![LinkedList](/assets/img/2024-07-25-ImplementingaLinkedListinRust_3.png)

(꼬리를 기록하면 리스트의 끝에 항목을 상수 시간에 삽입할 수도 있습니다.)

효율적인 삽입은 대기열 또는 스택을 구현하는 경우와 같은 맥락에서 유용할 수 있습니다. 여기서 요소가 목록의 시작이나 끝에서 자주 추가 및 제거됩니다.

<div class="content-ad"></div>

# 우리 구현을 위한 주목할만한 Rust 기능들

구현에 들어가기 전에, 퍼포먼스 좋은 링크드 리스트를 만들 수 있도록 도와줄 몇 가지 Rust 기능들을 살펴보겠습니다.

여기서는 메모리 관리와 관련된 개념에 초점을 맞추어 설명하겠습니다. 이러한 내용은 고수준의 가비지 수집 언어에서 오신 분들에게는 조금 덜 익숙할 수도 있습니다.

# Box

<div class="content-ad"></div>

Rust에서 Box 타입은 스택이 아닌 힙에 메모리를 할당하는 데 사용됩니다. 컴파일 시간에 크기를 알 수 없는 데이터 구조에 중요하며, 링크드 리스트가 동적 크기이므로 노드를 저장하기 위해 Box를 사용해야 합니다.

# Unsafe

Rust는 컴파일 시간에 메모리 안전성을 보장합니다. 그러나 가능한 한 효율적인 코드를 작성하려고 노력할 때 제한적일 수 있습니다.

예를 들어 값을 복제하는 대신에 필요 없는 복사본을 만들지 않고 포인터를 수동으로 이동하고 싶을 수 있습니다. 이것이 더 효율적이지만, Rust는 더 이상 코드가 안전하다고 보장할 수 없습니다: 필요한 경우에 이러한 제한을 우회하기 위해 unsafe 키워드를 사용합니다.

<div class="content-ad"></div>

이를 통해 강력한 저수준 기능인 Box::from_raw와 같은 기능을 (조심스럽게!) 사용할 수 있게 됩니다.

unsafe가 절대 필요하지 않다는 점을 알아두어야 합니다. 이는 trade-off이며 이 글 끝에 대안적 접근 방법의 예시가 있습니다.

# 역참조

대여(borrowing)는 Rust에서 근본적인 개념입니다. 이는 값 자체가 아닌 값에 대한 참조를 전달할 수 있도록 해주는데, 이는 더 효율적이고 때로는 더 간단하게 관리할 수 있게 해줍니다.

<div class="content-ad"></div>

Rust를 배우기 시작할 때, 우리는 & 및 &mut 연산자를 사용하여 값을 빌릴 수 있다는 것을 배웁니다.

하지만 선택적인 값을 다룰 때, as_deref 및 as_deref_mut 함수를 사용하면 Option 타입 안의 값을 빌릴 수 있게 됩니다.

구체적으로 이 함수들은 Option`&T`를 Option`&U`로 변경하는데, 여기서 U는 T에 대한 참조입니다.

# 연결 리스트 구현하기

<div class="content-ad"></div>

기본 개념과 Rust 기능을 검토했으니, 이제 연결 리스트를 구현해봅시다.

# 노드 정의

우선 값과 다음 노드를 가리키는 참조를 포함하는 LinkedListNode 구조체를 정의해봅시다.

```rust
#[derive(Debug)]
struct LinkedListNode<T> {
    value: T,
    next: Option<Box<LinkedListNode<T>>>,
}
```

<div class="content-ad"></div>

이 코드에서는 Box가 사용됩니다. 이는 컴파일 시점에 next의 크기를 알 수 없기 때문입니다.

우리는 제네릭 타입 T를 사용하여 노드가 어떤 종류의 값이든 저장할 수 있도록 할 것입니다. 또한 우리가 생성하는 모든 구조체에 Debug 트레이트를 추가할 것이며, 이를 통해 dbg! 매크로를 사용하여 구조체를 출력할 수 있게 됩니다.

다음으로, 주어진 값으로 새 노드를 생성하는 새 함수를 구현해보겠습니다.

```js
impl<T> LinkedListNode<T> {
    fn new(value: T, next: Option<Box<LinkedListNode<T>>>) -> LinkedListNode<T> {
        LinkedListNode { value, next }
    }
}
```

<div class="content-ad"></div>

# 리스트 정의하기

이제는 리스트의 머리와 꼬리를 포함하는 LinkedList 구조체를 정의하겠습니다.

```js
#[derive(Debug)]
struct LinkedList<T> {
    head: Option<Box<LinkedListNode<T>>>,
    tail: Option<*mut LinkedListNode<T>>,
}
```

tail 필드에 대해서는, 꼬리를 수정해야 할 때 대여 문제를 피하기 위해 원시 포인터 `*mut LinkedListNode<T>`를 사용할 수 있습니다.

<div class="content-ad"></div>

LinkedListNode과 마찬가지로, 원소가 없는 새로운 목록을 생성하는 새 함수를 구현할 것입니다.

```rust
impl<T: PartialEq> LinkedList<T> {
    fn new() -> LinkedList<T> {
        LinkedList {
            head: None,
            tail: None,
        }
    }
}
```

이제 목록과 상호 작용할 수 있는 방법을 구현하는 시간입니다!

연결 리스트는 원소를 시작 부분에 삽입하는 데 훌륭하기 때문에, 그런 목적을 달성하기 위해 prepend 메서드를 구현하는 것부터 시작하겠습니다. 구현하는 모든 메서드는 LinkedList 구조체에 대한 impl 블록의 일부여야 합니다.

<div class="content-ad"></div>

# Prepend

먼저, 주어진 값과 현재 헤드를 참조하는 새 노드를 생성할 것입니다. 기존 헤드 값을 이동시키고 None을 남겨둘 수 있는 take 메서드를 사용할 수 있습니다.

```js
fn prepend(&mut self, value: T) {
    let new_node = Box::new(LinkedListNode::new(value, self.head.take()));

    // 새 노드를 헤드로 이동
}
```

다음으로, 새 노드를 헤드로 이동시키겠습니다.

<div class="content-ad"></div>

```rust
fn prepend(&mut self, value: T) {
    let new_node = Box::new(LinkedListNode::new(value, self.head.take()));

    self.head = Some(new_node);
}
```

지금까지는 매우 간단하죠!

그러나 만약 테일이 None이라면 해당 값을 업데이트해야 합니다. 우리는 new_node를 불필요하게 복제하는 것을 막기 위해 테일을 raw 포인터로 저장할 수 있습니다. 여기서 unsafe 키워드가 필요한 이유입니다.

unsafe 블록에서 new_node를 Box::into_raw를 사용하여 raw 포인터로 변환합니다. 이후, 테일에 이 포인터를 설정할 수 있고, 마지막으로 Box::from_raw를 사용하여 raw 포인터를 Box로 다시 변환하고 이를 새 헤드로 설정할 수 있습니다.

<div class="content-ad"></div>

```rust
impl<T: PartialEq> LinkedList<T> {
  // other methods

  fn prepend(&mut self, value: T) {
      let new_node = Box::new(LinkedListNode::new(value, self.head.take()));

      unsafe {
          let raw_node: *mut _ = Box::into_raw(new_node);

          if self.tail.is_none() {
              self.tail = Some(raw_node);
          }

          self.head = Some(Box::from_raw(raw_node));
      }
  }
}
```

요소를 앞쪽에 추가하는 것은 리스트의 크기와 관계 없이 일정 수의 작업만 필요함을 알 수 있어요!

이번엔 리스트의 끝에 요소를 추가하는 방법은 어떨까요?

# 추가하기


<div class="content-ad"></div>

상수 시간 내 요소를 추가할 수도 있습니다. 이때는 꼬리 포인터를 업데이트하여 추가할 것입니다. 이는 prepend 메소드와 유사하지만, 현재 꼬리 노드의 다음 필드를 업데이트해야 합니다.

또한, 리스트가 비어 있는 경우와 head가 None인 경우도 고려해야 합니다.

```rust
fn append(&mut self, value: T) {
    let new_node = Box::new(LinkedListNode::new(value, None));

    unsafe {
        let raw_node: *mut _ = Box::into_raw(new_node);

        if self.head.is_none() {
            self.head = Some(Box::from_raw(raw_node));
        } else if let Some(tail) = self.tail {
            (*tail).next = Some(Box::from_raw(raw_node));
        }

        self.tail = Some(raw_node);
    }
}
```

두 번째 if 문은 조금 복잡합니다. self.tail에 값이 있다면, tail 포인터를 역참조하여 next가 새 노드를 가리키도록 해야 합니다.

<div class="content-ad"></div>

마침내, 새 노드의 raw 포인터 값으로 꼬리 포인터를 업데이트합니다.

# 삽입

다음으로, 우리는 선형 작업을 살펴보겠습니다. 특정 인덱스에 요소를 삽입하려면 원하는 인덱스에 도달할 때까지 목록을 횡단해야 합니다.

이것은 배열과 달리 연결 리스트의 n번째 요소에 직접 액세스할 수 없기 때문입니다. 머리에서 시작하여 다음 참조를 따라 원하는 인덱스에 도달할 때까지 진행해야 합니다.

<div class="content-ad"></div>

여기 insert 메서드를 구현하는 한 가지 접근 방법이 있습니다:

```rust
fn insert(&mut self, value: T, index: usize) {
    if index == 0 {
        self.prepend(value);
        return;
    }

    let mut count = 1;
    let mut current_node = self.head.as_deref_mut();

    while let Some(node) = current_node {
        if count == index {
            let new_node = Box::new(LinkedListNode::new(value, node.next.take()));
            node.next = Some(new_node);
            return;
        }
        current_node = node.next.as_deref_mut();
        count += 1;
    }

    self.append(value);
}
```

이 메서드에서는 먼저 인덱스가 0인지 확인합니다. 0이라면 요소를 prepend 합니다. 그렇지 않으면 리스트를 순회하여 원하는 인덱스에 도달합니다.

루프에서는 Box::new를 사용하여 주어진 값을 가지고 새로운 노드를 힙에 만듭니다. take 메서드는 현재 노드의 다음 값을 가져가고 그 자리에 None을 남깁니다. 그런 다음 현재 노드의 다음 필드를 새로운 노드를 가리키도록 업데이트할 수 있습니다.

<div class="content-ad"></div>

마침내 리스트의 끝에 도달했을 때 원하는 인덱스를 찾지 못하면 요소를 추가할 수 있습니다.

# 찾기

연결 리스트에서 요소를 찾는 것은 선형 작업입니다. 우리가 찾고 있는 요소를 찾을 때까지 리스트를 통과해야 합니다.

```rust
fn find(&self, value: T) -> Option<&LinkedListNode<T>> {
    let mut current = self.head.as_deref();

    while let Some(node) = current {
        if node.value == value {
            return Some(node);
        }
        current = node.next.as_deref();
    }
    None
}
```

<div class="content-ad"></div>

# 삭제

마지막으로, 목록에서 요소를 삭제하는 메소드를 구현해 봅시다. 이것은 이전 메소드들보다 조금 복잡한 선형 작업입니다.

```js
fn delete(&mut self, value: T) {
    // 목록이 비어있는 경우 처리
    if self.head.is_none() {
        return;
    }

    // 헤드 노드를 삭제해야 하는 경우 처리
    if let Some(ref mut head) = self.head {
        if head.value == value {
            self.head = head.next.take();
            if self.head.is_none() {
                self.tail = None;
            }
            return;
        }
    }

    let mut current = self.head.as_deref_mut();

    // 헤드가 아닌 노드를 삭제해야 하는 경우 처리
    while let Some(ref mut node) = current {
        if let Some(ref mut next) = node.next {
            if next.value == value {
                node.next = next.next.take();
                if node.next.is_none() {
                    self.tail = Some(node);
                }
                return;
            }
        }
        current = node.next.as_deref_mut();
    }
}
```

헤드 노드를 삭제하는 것은 간단합니다: 다음 값을 헤드로 이동시키면 됩니다. 헤드를 삭제한 후 목록이 비어있는 경우 꼬리를 업데이트해 주어야 합니다.

<div class="content-ad"></div>

그 외 노드의 경우에는 삭제하려는 노드를 찾을 때까지 목록을 순회해야 합니다. 그러면 이전 노드의 다음 필드를 삭제할 노드 다음의 노드를 가리키도록 업데이트할 수 있습니다.

# 왜 'unsafe'가 필요한가요?

'unsafe'의 사용은 성능을 바꾸는 것이며, 이런 경우에는 메모리 할당을 직접해야 합니다.

저는 이런 trade-off를 하는 접근 방식을 보여주고 싶었습니다. 제가 생각하기에 잘 구현된 데이터 구조는 가능한 한 성능이 뛰어나야 하지만, 표준 라이브러리의 메서드를 사용하여 'unsafe'를 피할 수도 있습니다!

<div class="content-ad"></div>

여기 한 가지 방법이 있습니다. tail의 종류를 full 노드로 업데이트할 수 있습니다:

```js
#[derive(Debug, Clone)]
struct LinkedList<T> {
    head: Option<Box<LinkedListNode<T>>>,
    tail: Option<Box<LinkedListNode<T>>>,
}
```

LinkedListNode 및 LinkedList 구조체에 대해 Clone 트레이트를 구현해야합니다.

그런 다음, prepend메서드를 예로 들어 업데이트하면 다음과 같이 보일 수 있습니다:

<div class="content-ad"></div>


```js
fn prepend(&mut self, value: T) {
    let new_node = Box::new(LinkedListNode::new(value, self.head.take()));

    if self.tail.is_none() {
        self.tail = Some(new_node.clone());
    }

    self.head = Some(new_node);
}
```

클론 작업에는 잠재적인 오버헤드가 있으며, 크거나 복잡한 유형의 경우에는 중요할 수 있습니다. 그러나 이와 같은 방식으로 접근한다면, 우리는 더는 Rust의 불안전한 부분들에 대해 걱정할 필요가 없게 됩니다!

이게 다에요! 이 글이 러스트에서 연결 리스트의 기본을 이해하고 이를 어떻게 구현할 수 있는지에 대해 이해하는 데 도움이 되었으면 좋겠습니다.

물론, 이것은 상당히 간단한 구현입니다. 이를 개선할 수 있는 여러 가지 방법과 더 유용하게 만들기 위해 추가할 수 있는 많은 방법이 있습니다. 더 알아보고 싶다면, 이중 연결 리스트를 살펴보는 것을 권장합니다. 이중 연결 리스트를 사용하면 리스트를 양방향으로 탐색할 수 있습니다. 이중 연결 리스트의 각 노드는 이전 노드에 대한 참조와 다음 노드에 대한 참조를 포함합니다. 즐거운 코딩 되세요!
