---
title: "React 컴포넌트를 체계적으로 조직하는 최고의 방법은"
description: ""
coverImage: "/assets/img/2024-07-07-WhatstheBestWaytoOrganiseYourReactComponents_0.png"
date: 2024-07-07 12:25
ogImage:
  url: /assets/img/2024-07-07-WhatstheBestWaytoOrganiseYourReactComponents_0.png
tag: Tech
originalTitle: "What’s the Best Way to Organise Your React Components?"
link: "https://medium.com/@seeitsmanish/whats-the-best-way-to-organise-your-react-components-63d9d72fea58"
---

![React Components Organization](/assets/img/2024-07-07-WhatstheBestWaytoOrganiseYourReactComponents_0.png)

React 컴포넌트를 더 깔끔하고 깨끗하게 구조화하는 방법을 배워보세요…

## 주의사항:

React 컴포넌트를 구성하는 가장 좋은 방법은 이것뿐이 아닙니다. 이것은 내가 하는 방법일 뿐이지만, 나에게는 잘 작동합니다. 여러분의 경험은 달라질 수 있고, 그것도 괜찮습니다. 이것은 내 5개월간의 인턴 경험 중에 배운 최고의 접근 방법입니다.

<div class="content-ad"></div>

## 소개

그래서, React 프로젝트에 작업하고 있고 컴포넌트 폴더가 십대 소년의 침실처럼 보이기 시작했다면 어떻게 해야 할까요? 어디에 뭘 넣었는지 기억도 안 나는데 여기저기 뭔가가 흩어져 있습니다. 익숙한 상황인가요? 그럼 해결해보겠습니다.

## 간단한 검색 상자를 만들어 이해해 봅시다

![이미지](/assets/img/2024-07-07-WhatstheBestWaytoOrganiseYourReactComponents_1.png)

<div class="content-ad"></div>

위의 방법 중 하나는 다음과 같이 모든 코드를 한 파일에 모아서 쓰는 것입니다.:-

```js
import { Search } from "lucide-react";
import React, { useEffect, useState } from "react";
import useDebouncedValue from "../../hooks/useDebouncedValue";

type SearchBoxProps = {};

type listItem = {
    value: string;
    label: {
        name?: string;
        email?: string;
    }
}

const SearchBox: React.FC<SearchBoxProps> = () => {
    const [list, setList] = useState<listItem[]>([]);
    const [query, updateQuery] = useState('');
    const debouncedQuery = useDebouncedValue(query);

    const convertApiDataToSearchItem = (apiData: any[]): listItem[] => {
        return apiData.map(item => ({
            value: item.id,
            label: {
                name: item.name,
                email: item.email
            }
        }));
    };

    // Other helper & Utility functions

    useEffect(() => {
        if (debouncedQuery.trim() !== '') {
            fetch(`https://freetestapi.com/api/v1/users?search=${debouncedQuery}`)
                .then((res) => res.json())
                .then((res) => setList(convertApiDataToSearchItem(res)))
                .catch(error => console.error(error));
        } else {
            setList([]);
        }
    }, [debouncedQuery]);

    return (
        <div className="w-[400px]">
            <div
                className={`flex gap-3 items-center w-full bg-white border
                            outline-none px-3 rounded-md border-black
                            ${list.length > 0 && 'rounded-b-none border-b-0'}
                            `}
            >
                <Search className="size-5" />
                <input
                    className="h-10 bg-transparent w-full outline-none"
                    type="search"
                    value={query}
                    onChange={(e) => updateQuery(e.target.value)}
                    placeholder="Search..."
                />
            </div>

            <div className={`w-full ${list.length > 0 && 'border border-black'} border-t-0`}>
                {
                    list.length > 0 && (
                        list.map((listItem, index) => (
                            <div key={index} className="cursor-pointer border-b p-2 bg-white hover:bg-slate-300">
                                <p className="text-sm font-bold text-nowrap">{listItem.label.name}</p>
                                <p className="text-sm italic text-nowrap">{listItem.label.email}</p>
                            </div>
                        ))
                    )
                }
            </div>
        </div>
    );
};

export default SearchBox;
```

자, 이제 우리는 코드를 가장 깔끔하게 작성하는 방법에 대해 이야기해 보겠습니다. 내 방식은 다음과 같습니다:

➔ 관심사 분리 - 컴포넌트는 자신이 가장 잘 아는 일에 충실해야 합니다.
➔ 단일 책임 - 왜 복잡하게 만들까요?
➔ 간단한 테스트 - 테스트는 로켓 과학일 필요가 없습니다.

<div class="content-ad"></div>

위의 원칙을 따르면 구성 요소가 정리되어 삶을 조직하는 데 도움이 될 수도 있어요.

다음은 제 구성 요소 구조입니다 -`

```js
SearchBox/
│
├── index.ts
├── models.ts
├── utils.ts
│
├── hooks/
│   └── useDebouncedValue.tsx
│
├── ListItem.tsx
├── List.tsx
├── SearchInput.tsx
└── SearchBox.tsx
```

하나씩 알아보도록 할게요:-`

<div class="content-ad"></div>

1. **index.ts**:
   내부 모듈에서 내보낸 것을 조직화하여 구현 세부 정보를 추상화하고 응용 프로그램의 외부 부분에 인터페이스를 제공합니다.

```typescript
export { listItem } from "./models"; // 타입
export { default as SearchInput } from "./SearchInput";
export { default as List } from "./List";
export { default as ListItem } from "./ListItem";
```

2. **models.ts**:
   이 파일은 listItem과 convertApiResponseToListData와 같은 TypeScript 타입 및 변환 함수를 내보내는 VIP 섹션입니다. 이러한 내보내기는 타입 안전성을 유지하고 데이터가 앱 전체에서 잘 작동하도록 보장합니다.

```typescript
export type listItem = {
  value: string;
  label: {
    name?: string;
    email?: string;
  };
};

export const convertApiResponseToListData = (apiData: any[]): listItem[] => {
  return apiData.map((item) => ({
    value: item.id,
    label: {
      name: item.name,
      email: item.email,
    },
  }));
};
```

<div class="content-ad"></div>

3. hooks/ useDebouncedValue.ts:
   컴포넌트 내의 훅 폴더인가요? 전역적으로 사용되는 훅 폴더로 알려져 있기는 하지만, 컴포넌트 내에서만 사용되는 훅을 위한 지역적인 훅 폴더를 가지는 것은 꽤 괜찮습니다.

이러한 설정은 UI 파일을 깨끗하게 유지하고 렌더링을 주요 작업으로 집중할 수 있도록 돕습니다. 일반적으로 반응형 로직은 UI 파일에서 너무 복잡해지면 훅 내부로 이동하는 것이 좋습니다.

예를 들어, useDebouncedValue가 여기에 저장되어 있지만, 여러 컴포넌트에서 필요한 경우 전역 훅 폴더에 있어도 괜찮습니다.

```js
import { useEffect, useState } from "react";

const useDebouncedValue = (value: string, delay: number = 300) => {
  const [debouncedValue, setDebouncedValue] = useState(value);

  useEffect(() => {
    const handler = setTimeout(() => {
      setDebouncedValue(value);
    }, delay);

    return () => {
      clearTimeout(handler);
    };
  }, [value, delay]);

  return debouncedValue;
};

export default useDebouncedValue;
```

<div class="content-ad"></div>

4. utils.ts:
   utils.ts 파일은 현재 특정 유틸리티 함수가 즉시 필요하지 않아 비어 있습니다. 필요한 경우, 날짜 형식화나 문자열 조작과 같은 유틸리티 함수들이 여기에 배치될 수 있습니다. 필요에 따라 폴더로 구성하여도 좋습니다. 원하는 분리 정도와 유틸리티 함수의 수에 따라 결정하시면 됩니다.

저의 이전 프로젝트 중 하나에서 유틸 함수의 예시를 보여드리겠습니다:-

```js
/**
 * 주어진 날짜 문자열이 과거 또는 현재 날짜를 나타내는지 확인합니다.
 * @param {string} dateString - "YYYY-MM-DD" 형식의 날짜 문자열.
 * @returns {boolean} 주어진 날짜가 과거 또는 현재 날짜인 경우 true를 반환하고, 그 외에는 false를 반환합니다.
 * @example
 * // '2024-06-21'은 과거 날짜이기 때문에 true를 반환합니다.
 * isPastOrPresentDate('2024-06-21');
 */
export const isPastOrPresentDate = (dateString: string): boolean => {
  try {
    const date = new Date(dateString);
    if (isNaN(date.getTime())) {
      throw new Error("제공된 날짜 문자열이 올바르지 않습니다.");
    }
    const currentDate = new Date();
    return date <= currentDate;
  } catch (error) {
    return false;
  }
};
```

5. ListItem.tsx, List.tsx 그리고 SearchInput.tsx:
   하나의 UI를 ListItem.tsx, List.tsx, 그리고 SearchInput.tsx로 나누었습니다. 각 파일은 이제 불필요한 종속성 없이 특정 책임에만 집중합니다. 이 접근 방식은 재사용성을 향상시키고 UI 파일을 깔끔하고 명료하게 유지하여 목적과 기능의 명확성을 보장합니다.

<div class="content-ad"></div>

```js
// ListItem.tsx
import React from "react";

type ListItemProps = {
    listItem: listItem;
};

const ListItem: React.FC<ListItemProps> = ({ listItem }) => {
    return (
        <div className="curs border-b p-2 bg-white hover:bg-slate-300">
            <p className="text-sm font-bold text-nowrap">{listItem.label.name}</p>
            <p className="text-sm italic text-nowrap">{listItem.label.email}</p>
        </div>
    );
};

export default ListItem;

// List.tsx
import React from "react";
import ListItem from "./ListItem";

type ListProps = {
    items: listItem[];
};

const List: React.FC<ListProps> = ({ items }) => {
    return (
        <div className={`w-full border border-black border-t-0`}>
            {items.map((listItem, index) => (
                <ListItem key={index} listItem={listItem} />
            ))}
        </div>
    );
};

export default List;

// SearchInput.tsx
import { Search } from "lucide-react";
import React, { useState } from "react";
import useDebouncedValue from "./hooks/useDebouncedValue";

type SearchInputProps = {
    onSearchChange: (query: string) => void;
    query: string;
};

const SearchInput: React.FC<SearchInputProps> = ({ onSearchChange, query }) => {
    return (
        <div
            className={`flex gap-3 items-center w-full bg-white border outline-none px-3 rounded-md border-black`}
        >
            <Search className="size-5" />
            <input
                className="h-10 bg-transparent w-full outline-none"
                type="search"
                value={query}
                onChange={(e) => onSearchChange(e.target.value)}
                placeholder="Search..."
            />
        </div>
    );
};

export default SearchInput;
```

5. SearchBox.tsx:
   검색 상자(SearchBox)는 SearchInput, List 및 ListItem을 통합하여 사용하는 주요 컴포넌트입니다.

보통 다음을 따릅니다:
➔ UI에 세 가지 이상의 조건이 있는 경우, 새로운 컴포넌트로 분리합니다.
➔ useEffect가 두 개 이상인 경우, 상황에 따라 별도의 컴포넌트 또는 훅으로 분리합니다.

```js
import React, { useEffect, useState, useCallback } from "react";
import { SearchInput, List } from "./";
import { listItem, convertApiResponseToListData } from "./models";
import useDebouncedValue from "./hooks/useDebouncedValue";

const SearchBox: React.FC = () => {
    const [list, setList] = useState<listItem[]>([]);
    const [query, setQuery] = useState('');
    const debouncedQuery = useDebouncedValue(query);

    const fetchData = useCallback(async (searchQuery: string) => {
        if (searchQuery.trim() !== '') {
            try {
                const response = await fetch(`https://freetestapi.com/api/v1/users?search=${searchQuery}`);
                const data = await response.json();
                setList(convertApiResponseToListData(data));
            } catch (error) {
                console.error(error);
            }
        } else {
            setList([]);
        }
    }, []);

    useEffect(() => {
        fetchData(debouncedQuery);
    }, [debouncedQuery, fetchData]);

    const handleSearchChange = useCallback((newQuery: string) => {
        setQuery(newQuery);
    }, []);

    return (
        <div className="w-[400px]">
            <SearchInput query={query} onSearchChange={handleSearchChange} />
            {list.length > 0 && <List items={list} />}
        </div>
    );
};

export default SearchBox;
```

<div class="content-ad"></div>

6. 각 컴포넌트 별로 스타일 폴더 또는 전용 스타일 파일:
   저는 Tailwind CSS를 사용했기 때문에 별도의 스타일 파일을 만들지 않았습니다.
   그러나 원하시면 전용 파일이나 모든 스타일 파일을 포함하는 단일 폴더를 가질 수 있습니다.

지금까지 이것으로 마치겠습니다. 몇 가지 통찰을 얻으셨기를 바랍니다. 더 나은 방법으로 작업하는 것이 더 있을 수 있다고 생각할 수 있습니다, 그것도 괜찮아요. 저는 대학에서 6개월 동안 React를 배웠고 5개월 동안 인턴을 하면서 배우고 있습니다. 여전히 배우는 여정 중이며 더 나은 기술을 탐험하고 있습니다.

개선 제안이 있다면 자유롭게 공유해 주세요. 새로운 방법을 배우는 것을 열정적으로 기다리고 있습니다.

읽어 주셔서 감사합니다!
