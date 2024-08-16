---
title: "자바에서 참조와 객체 캐싱하는 방법"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-07-26 12:00
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Reference and Object Equality in JavaInteger caching"
link: "https://medium.com/@vikas.taank_40391/reference-and-object-equality-in-java-integer-caching-e82e3491ce7e"
isUpdated: true
updatedAt: 1723813353128
---



자바 언어에서 얕은 비교 == 연산자는 객체의 참조(또는 주소)를 비교하며 값을 심층적으로 비교하지 않습니다. 그러나 객체에 저장된 값을 비교하려면 등호(==) 연산자 대신 equals 연산자를 사용해야 합니다.

## 하지만 여기 한 가지 주의할 점이 있습니다:

# 정수 캐싱

자바 런타임에 의해 -128에서 127 사이의 값에 대한 정수 객체는 캐싱됩니다. 이는 이 범위 내의 값을 갖는 Integer 객체를 생성할 때, 자바가 최적화하여 동일한 객체 인스턴스를 재사용한다는 의미입니다. 이 범위를 벗어난 값에 대해선 새로운 Integer 객체가 매번 생성됩니다.

<div class="content-ad"></div>

# 왜 이런 일이 발생하는가:

- 작은 정수가 캐시됩니다: -128부터 127 사이의 정수 값에 대해 Java 가상 머신(JVM)은 캐시를 사용합니다. 이 범위 내의 Integer 객체를 할당하면 동일한 객체를 참조합니다.