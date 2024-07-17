---
title: "React 디자인 패턴 Render Prop 사용 방법"
description: ""
coverImage: "/assets/img/2024-07-09-ReactdesignpatternTherenderprop_0.png"
date: 2024-07-09 13:00
ogImage:
  url: /assets/img/2024-07-09-ReactdesignpatternTherenderprop_0.png
tag: Tech
originalTitle: "React design pattern: The render prop"
link: "https://medium.com/javascript-in-plain-english/react-design-pattern-the-render-prop-208f37f9ec2e"
---

![React Design Pattern - The Render Prop](/assets/img/2024-07-09-ReactdesignpatternTherenderprop_0.png)

## 문제

어플리케이션에 상품 목록과 사용자 목록 두 가지 타입의 목록이 있다고 상상해보세요.

![React Design Pattern - The Render Prop](/assets/img/2024-07-09-ReactdesignpatternTherenderprop_1.png)

<div class="content-ad"></div>

- 일반적으로 목록 항목, 로딩 표시기 및 오류 발생 시 대체 UI에 대한 3개의 상태 변수를 갖게 됩니다.
- useEffect 훅 내부에서 API 호출을 통해 항목 목록을 가져옵니다.
- 이후 로딩 상태, 오류 상태 또는 실제 항목 목록을 조건부로 렌더링합니다.

```js
import React, { useState, useEffect } from "react";

const Products = () => {
  const [products, setProducts] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchProducts = async () => {
      setLoading(true);
      try {
        const response = await fetch("https://dummyjson.com/products?limit=5");
        const products = await response.json();
        setProducts(products);
      } catch (err) {
        setError(err);
      } finally {
        setLoading(false);
      }
    };

    fetchProducts();
  }, []);

  if (loading) return <p>Loading...</p>;
  if (error) return <p>Error: {error.message}</p>;
  return (
    <div>
      <h2>Products</h2>
      {products?.products?.map((product) => (
        <div key={product.id}>
          <img src={product.thumbnail} alt="" />
          <div>
            <h2>{product.title}</h2>
            <p>{product.description}</p>
          </div>
        </div>
      ))}
    </div>
  );
};

export default Products;
```

위와 같은 형태가 됩니다. API URL을 제품 대신 사용자로 변경하고 일부 변수 이름을 조정하면 유사한 `Users /` 컴포넌트를 얻을 수 있습니다.

이것이 항목 목록을 렌더링하는 전형적인 방법이지만, 더 나은, 더 모듈화된 방법이 있습니다. React에서 "렌더 프롭" 패턴이라는 디자인 패턴을 사용할 수 있습니다.

<div class="content-ad"></div>

## 해결책

이 패턴은 컴포넌트의 데이터와 JSX를 분리함으로써 코드 반복을 피할 수 있는 간단한 기술입니다. 리스트 예제로 돌아가 봅시다. 우리는 사용자 목록과 제품 목록 두 가지 목록 컴포넌트가 있었습니다. 각 컴포넌트는 API 호출을 수행하고 결과를 상태 변수에 저장했습니다. 이 데이터에 기초하여 일부 JSX를 조건적으로 렌더링했습니다.

이제 이 두 컴포넌트의 데이터 가져오기 및 저장 측면은 본질적으로 동일합니다. 따라서 이를 두 번 반복하는 대신 공통 컴포넌트 내에 모두 이동할 수 있습니다.

```js
// 이전
<Products />
<Users />

// 이후
<DataList />
<DataList />
```

<div class="content-ad"></div>

API URL만 변경되는 것이기 때문에 이 컴포넌트에서 사용할 API URL은 prop에서 얻을 수 있어요.

```js
<DataList url='https://dummyjson.com/products?limit=5' /> // 제품
<DataList url='https://dummyjson.com/users?limit=5' /> // 사용자
```

DataList 컴포넌트 내에서 몇 개의 변수명도 변경해야 해요. 예를 들어, [products, setProducts]또는 [users, setUsers] 대신에 공통적인 [data, setData]를 사용해볼 수 있어요. 이렇게 수정된 DataList 컴포넌트는 이제 다음과 같습니다.

```js
import React, { useEffect, useState } from "react";

const DataList = ({ url }) => {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchData = async () => {
      setLoading(true);
      try {
        const response = await fetch(url);
        const data = await response.json();
        setData(data);
      } catch (err) {
        setError(err);
      } finally {
        setLoading(false);
      }
    };

    fetchData();
  }, [url]);
};

export default DataList;
```

<div class="content-ad"></div>

이제 해야 할 마지막 작업은 DataList 컴포넌트 내에서 일부 JSX를 반환하는 것입니다. 이 렌더링 로직은 컴포넌트 외부로 추출되어 렌더 prop으로 전달될 것입니다.

그래서 이 DataList 컴포넌트를 사용하는 App 컴포넌트 안에서 'render' prop을 추가해야 합니다. 반드시 'render'로 이름을 짓지 않아도 되지만, 여기서는 기본 규칙을 따를 것입니다. 이 prop은 DataComponent에서 데이터를 가져오는 함수입니다. 데이터 컴포넌트 내에서는 필요한 데이터를 전달하여 JSX를 동적으로 렌더링할 수 있는 prop 또는 함수를 호출할 것입니다.

```js
<DataList
  url="https://dummyjson.com/products?limit=5"
  render = {({ loading, error, data }) => {
    if (loading) return <p>Loading...</p>
    if (error) return <p>Error: {error.message}</p>
    return (
      <div>
        <h2>제품</h2>
        {data?.products?.map(product => {
          return (
            <div>
              <img src={product.thumbnail} alt="" />
              <div>
                  <h2>{product.title}</h2>
                  <p>{product.description}</p>
              </div>
            </div>
          )
        })}
      </div>
    )
  }
 />


<DataList
  url="https://dummyjson.com/users?limit=5"
  render = {({ loading, error, data }) => {
    if (loading) return <p>Loading...</p>
    if (error) return <p>Error: {error.message}</p>
    return (
      <div>
        <h2>사용자</h2>
        {data?.users?.map(user => {
          return (
            <div>
              <img src={product.thumbnail} alt="" />
              <div>
                  <h2>{user.firstName}</h2>
                  <p>{user.email}</p>
              </div>
            </div>
          )
        })}
      </div>
    )
  }
 />

```

DataList 컴포넌트 내에서는 단순히 데이터, 오류 및 로딩 상태를 전달하는 방식으로 이 렌더 함수를 호출하고 그 결과를 반환하면 됩니다.

<div class="content-ad"></div>

```js
import React, { useEffect, useState } from 'react'

const DataList = ({ url, render }) => {
    ...
    return render({ data, loading, error })
}

export default DataList
```

그래서 이 패턴을 요약하자면, 우리가 한 일은

- DataList 컴포넌트 내부에서 데이터 가져오기 부분을 모듈식으로 만들었습니다.
- URL prop에 기반하여 구별된 데이터를 가져왔습니다.
- DataList의 끝에서 상태(data, error, loading)를 매개변수로하여 render prop을 호출했습니다.
- DataList를 사용하는 컴포넌트 내에서 반환해야 할 JSX를 전달했습니다.

## 결론

<div class="content-ad"></div>

요는 기본적으로 렌더 프롭 디자인 패턴이에요. 컴포넌트 간에 코드를 공유하고 상태 관련 로직과 렌더링 로직을 분리하는 방법이죠. 다른 의문이나 제안이 있으면 댓글에 작성하시거나 내 소셜 미디어 계정 중 하나로 연락 주세요. 화이팅!

YouTube LinkedIn Twitter GitHub

# 간단히 설명하자면 🚀

In Plain English 커뮤니티의 일원이 되어주셔서 감사합니다! 떠나시기 전에:

<div class="content-ad"></div>

- 작가를 박수로 응원하고 팔로우하기 ️👏️️
- 팔로우하기: X | LinkedIn | YouTube | Discord | 뉴스레터
- 다른 플랫폼 방문하기: CoFeed | Differ
- 더 많은 콘텐츠: PlainEnglish.io
