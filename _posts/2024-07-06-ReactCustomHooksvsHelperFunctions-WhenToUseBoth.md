---
title: "React 커스텀 훅 vs 헬퍼 함수 - 언제 사용해야 할까"
description: ""
coverImage: "/assets/img/2024-07-06-ReactCustomHooksvsHelperFunctions-WhenToUseBoth_0.png"
date: 2024-07-06 00:46
ogImage:
  url: /assets/img/2024-07-06-ReactCustomHooksvsHelperFunctions-WhenToUseBoth_0.png
tag: Tech
originalTitle: "React Custom Hooks vs. Helper Functions - When To Use Both"
link: "https://dev.to/andrewbaisden/react-custom-hooks-vs-helper-functions-when-to-use-both-2587"
---

개발자로 일할 때 매일 다양한 기술과 사용 사례를 접하는 것은 꽤 흔한 일입니다. 두 가지 인기 있는 개념은 리액트 커스텀 훅(React Custom Hooks)과 도우미 함수(Helper functions)입니다. 도우미 함수 개념은 매우 오랫동안 존재했지만 리액트 커스텀 훅은 비교적 현대적입니다. 두 개념 모두 개발자들이 작성한 코드를 다양한 방식으로 추상화하고 재사용할 수 있게 해주지만 약간 다른 사용 사례를 갖고 있습니다.

오늘은 이 두 가지 사이의 유사점을 살펴보고 각각을 사용해야 하는 적절한 시기에 대해 결론을 도출해보겠습니다.

이제 리액트 커스텀 훅이 무엇인지 살펴보겠습니다.

## 리액트 커스텀 훅이란 무엇인가요?

<div class="content-ad"></div>

리액트 커스텀 훅은 자바스크립트 함수로, 리액트 코드베이스 전체에서 상태 로직을 재사용할 수 있는 기능을 제공합니다. 커스텀 훅을 사용할 때는 리액트 훅 API에 접근하여 상태와 사이드 이펙트를 관리할 수 있으며, 이는 리액트 함수형 컴포넌트 프로세스를 따라갑니다.

커스텤 훅의 독특한 특성 중 하나는 상태 관리를 활용할 수 있다는 것인데, 이는 useState나 useEffect와 같은 리액트 내장 훅에 접근할 수 있음을 의미합니다. 또 다른 독특한 식별자는 리액트 커스텀 훅을 위한 명명 규칙을 따라야 한다는 것인데, 훅의 시작 부분에 use라는 단어를 접두사로 붙여 이름을 지정해야 합니다. 예를 들어 useFetch와 같이 말이죠.

커스텀 훅을 사용하면 컴포넌트 상태에 접근하고 변경할 수 있으며, 라이프사이클 메서드를 포함하여 리액트 컴포넌트의 로직과 깊게 연결될 수 있습니다.

아래 코드 예제에서 리액트 커스텀 훅이 어떻게 보이는지 살펴볼 수 있습니다:

<div class="content-ad"></div>

```js
import { useState, useEffect } from 'react';

export function useFetch(url) {
  const [data, setData] = useState([]);
  const [error, setError] = useState(null);
  const [isLoading, setIsLoading] = useState(null);

  useEffect(() => {
    const fetchData = async () => {
      try {
        const json = await fetch(url).then((r) => r.json());
        setIsLoading(false);
        setData(json);
      } catch (error) {
        setError(error);
        setIsLoading(false);
      }
    };

    fetchData();

  return { data, error, isLoading };
}
```

이 사용자 정의 훅은 useFetch라고 불리며 API에서 데이터를 가져오는 재사용 가능한 로직을 가지고 있습니다. 사용 중인 컴포넌트에서 로딩 및 오류 상태를 관리할 수 있고 여러 컴포넌트로 가져올 수 있습니다.

리액트 커스텀 훅에 대한 기초적인 이해를 갖고 나면 이제 이를 도우미 함수와 비교하는 방법을 살펴보겠습니다.

## 도우미 함수란 무엇인가요?

<div class="content-ad"></div>

도움 함수는 기본적으로 독립형 함수로, 다양한 계산 또는 작업을 수행하는 데 사용되는 함수입니다. 이러한 유형의 함수는 React 컴포넌트나 상태 관리 시스템의 일부가 아니기 때문에 애플리케이션의 어디에서나 사용할 수 있습니다. 또 다른 중요한 차이점은 다양한 프로그래밍 언어에서 사용할 수 있으며 어떤 생태계에도 결합되어 있지 않다는 것입니다. 이는 어디서나 사용할 수 있는 개념입니다.

반면에 React 사용자 정의 함수와는 달리, 도움 함수는 주어진 입력과 관련된 계산 및 작업을 수행합니다. 이러한 함수는 부작용이나 컴포넌트 상태와 상호 작용할 수 없습니다. 또한 사용과 같은 미리 정의된 명명 규칙이 필요하지 않으며, 해당 작업을 위해 지정된 이름에 따라 명명해야 합니다.

아래 예시의 이 도움 함수를 살펴보세요:

```js
import dayjs from "dayjs";

function formatDate(date) {
  return dayjs(date).format("MM/DD/YYYY");
}

export default formatDate;
```

<div class="content-ad"></div>

위 코드 스니펫에서는 자바스크립트 날짜 라이브러리 Day.js를 사용하여 날짜를 구문 분석하고 형식을 지정하며, 이를 통해 더 강력한 방법을 얻을 수 있습니다.

React Custom Hooks 및 도우미 함수를 업데이트하고 이해한 후에는 이 두 가지를 간단한 애플리케이션에 어떻게 통합할 수 있는지 살펴보는 좋은 시기입니다. 다음 섹션에서는 그들을 모두 사용하는 앱을 구축할 것입니다.

## 사용자 지정 훅과 도우미 함수를 사용하는 앱 구축

구축할 앱은 단순한 포켓몬 포켓몬 도감 애플리케이션으로, 다음 그림에서 확인할 수 있습니다.

<div class="content-ad"></div>

<img src="/assets/img/2024-07-06-ReactCustomHooksvsHelperFunctions-WhenToUseBoth_0.png" />

포켓몬 API에서 포켓몬 데이터와 정보를 가져와서 해당 데이터를 사용하여 애플리케이션을 구축한 다음 Tailwind CSS를 사용하여 스타일을 입혔어요. 설명이 끝나면 앱을 만들기 시작할 때가 왔습니다.

여기 GitHub에서 온라인으로 코드베이스를 찾을 수 있습니다: https://github.com/andrewbaisden/pokemon-pokedex-app

할 일은 먼저 프로젝트 구조를 설정하는 것이에요. 프로젝트에 대한 위치를 찾아서, 데스크톱과 같은 위치에서 다음 명령을 실행하여 Next.js 프로젝트를 만들어보세요.

<div class="content-ad"></div>

```js
npx create-next-app pokemon-pokedex-app
cd pokemon-pokedex-app
```

이제 Next.js 프로젝트가 생성되었고 pokemon-pokedex-app 폴더 내에 있어야 합니다. 이제 이 응용 프로그램에 필요한 JavaScript 패키지를 설치해야 합니다. 시간 및 날짜 계산을 위해 dayjs를 설치하고 포켓몬에 대한 고유한 ID를 생성하기 위해 uuid를 설치해야 합니다.

다음 명령을 사용하여 두 패키지를 모두 설치하세요:

```js
npm i dayjs uuid
```

<div class="content-ad"></div>

우리의 패키지가 설치되었으니, 이제는 프로젝트를 위한 모든 파일과 폴더를 생성할 차례입니다.

다음 명령어를 실행하여 프로젝트 구조를 만들어봅시다:

```js
cd src/app
mkdir components hooks utils
mkdir components/Header components/Pokemon components/PokemonDetails
touch components/Header/Header.js components/Pokemon/Pokemon.js components/PokemonDetails/PokemonDetails.js
touch hooks/usePokemon.js
touch utils/dateUtils.js utils/fetchUtils.js
cd ../..
```

이 명령어를 통해 우리는:

<div class="content-ad"></div>

- 헤더, 포켓몬 및 포켓몬 상세 컴포넌트를 위한 components 폴더를 생성해주세요.
- 포켓몬 API에서 데이터를 가져오는 usePokemon 훅을 위한 hooks 폴더를 생성해주세요.
- fetch 및 날짜 유틸리티 함수를 위한 utils 폴더를 생성해주세요.

다음 단계에서는 코드를 파일에 추가하여 프로젝트를 완료할 예정이니, 코드 편집기에서 프로젝트를 열어주세요.

먼저 최상위 폴더에 있는 next.config.mjs 파일을 수정해주세요.

해당 파일의 모든 코드를 아래의 새 코드로 교체해주세요:

<div class="content-ad"></div>

```js
/** @type {import('next').NextConfig} */
const nextConfig = {
  images: {
    remotePatterns: [
      {
        protocol: "https",
        hostname: "raw.githubusercontent.com",
      },
    ],
  },
};

export default nextConfig;
```

이 파일에서 하는 일은 GitHub의 이미지 패턴을 추가해서 어플리케이션에서 포켓몬 이미지에 오류 없이 접근할 수 있도록 하는 것뿐입니다. 그 경로를 정의해야만 승인됩니다.

이제 layout.js 파일을 수정해 보겠습니다. 아래의 코드로 모든 코드를 교체해주세요:

```js
import { Yanone_Kaffeesatz } from "next/font/google";
import "./globals.css";

const yanone = Yanone_Kaffeesatz({ subsets: ["latin"] });

export const metadata = {
  title: "Create Next App",
  description: "Generated by create next app",
};

export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body className={yanone.className}>{children}</body>
    </html>
  );
}
```

<div class="content-ad"></div>

이 파일에서 주요 변경 사항은 우리 애플리케이션에 기본 Inter 글꼴을 대체하는 Yanone_Kaffeesatz Google 글꼴을 사용하는 것입니다.

다음으로 globals.css 파일이 리스트에서 다음 작업입니다. 코드 정리를 해야 합니다.

이전과 같이 다음 스니펫으로 코드를 대체하세요:

```js
@tailwind base;
@tailwind components;
@tailwind utilities;

body {
  font-size: 20px;
}
```

<div class="content-ad"></div>

우리는 코드를 정리했고 우리 애플리케이션의 기본 글꼴 크기를 20px로 설정했습니다.

이로써 초기 구성 파일을 다 처리했으니 이제 컴포넌트, 훅, 유틸 및 메인 페이지용 코드를 추가하면 애플리케이션이 준비됩니다.

맨 위부터 시작해서 Header.js 컴포넌트를 Header/Header.js 안에 만들어봅시다.

다음 코드를 파일에 추가하세요:

<div class="content-ad"></div>

```js
import { useState, useEffect } from "react";
import { getLiveDateTime } from "../../utils/dateUtils";

export default function Header() {
  const [dateTime, setDateTime] = useState(getLiveDateTime());

  useEffect(() => {
    const interval = setInterval(() => {
      setDateTime(getLiveDateTime());
    }, 1000);

    return () => clearInterval(interval);
  }, []);
  return (
    <>
      <header className="flex row justify-between items-center bg-slate-900 text-white p-4 rounded-lg">
        <div>
          <h1 className="text-5xl uppercase">포켓몬</h1>
        </div>
        <div>
          <p>날짜: {dateTime.date}</p>
          <p>시간: {dateTime.time}</p>
        </div>
      </header>
    </>
  );
}
```

이 컴포넌트는 주요 기능으로 애플리케이션의 제목 "포켓몬"을 표시하며 실시간 날짜와 시간도 표시합니다. 이를 위해 날짜 및 시간을 계산하기 위해 dayjs JavaScript 라이브러리를 사용하는 utils/dateUtils.js 도우미 함수를 가져옵니다.

다음으로 작업할 파일은 Pokemon 폴더의 Pokemon.js 파일입니다.

여기에 파일 코드가 있습니다:

<div class="content-ad"></div>

```js
import { useState, useEffect } from "react";
import usePokemon from "../../hooks/usePokemon";
import { fetchPokemon } from "../../utils/fetchUtils";
import PokemonDetails from "../PokemonDetails/PokemonDetails";

export default function Pokemon() {
  const { data, isLoading, error } = usePokemon("https://pokeapi.co/api/v2/pokemon");

  const [pokemonDetails, setPokemonDetails] = useState([]);

  useEffect(() => {
    const fetchPokemonDetails = async () => {
      if (data && data.results) {
        const details = await Promise.all(
          data.results.map(async (pokemon) => {
            const pokemonData = await fetchPokemon(pokemon.url);
            return pokemonData;
          })
        );
        setPokemonDetails(details);
      }
    };

    fetchPokemonDetails();
  }, [data]);

  if (isLoading) {
    return <div>Loading...</div>;
  }

  if (error) {
    return <div>Error: {error.message}</div>;
  }

  return (
    <>
      <div className="flex row flex-wrap gap-4 justify-evenly">
        <PokemonDetails pokemonDetails={pokemonDetails} />
      </div>
    </>
  );
}
```

우리의 주요 Pokémon 컴포넌트 파일입니다. Pokémon API에서 데이터를 가져 오는 usePokemon.js 훅을 사용합니다. 이것은 데이터를 가져 오기위한 유틸리티 fetchUtils.js 파일과 함께 작동합니다. 데이터를 가져 오는 데 오류 처리가 설정되어 있으며 상태 데이터는 사용자 인터페이스를 렌더링하는 PokemonDetails.js 컴포넌트로 전달됩니다.

맞아, 이제 PokemonDetails 폴더의 PokemonDetails.js 파일에 코드를 추가해야 합니다.

<div class="content-ad"></div>

```js
import Image from "next/image";
import { v4 as uuidv4 } from "uuid";

export default function PokemonDetails({ pokemonDetails }) {
  return (
    <>
      {pokemonDetails.map((pokemon) => (
        <div
          key={pokemon.id}
          className={
            pokemon.types[0].type.name === "fire"
              ? "bg-orange-400"
              : pokemon.types[0].type.name === "water"
              ? "bg-blue-400"
              : pokemon.types[0].type.name === "grass"
              ? "bg-green-400"
              : pokemon.types[0].type.name === "bug"
              ? "bg-green-700"
              : pokemon.types[0].type.name === "normal"
              ? "bg-slate-400"
              : ""
          }
        >
          <div className="text-white p-4">
            <div className="capitalize">
              <h1 className="text-4xl">{pokemon.name}</h1>
            </div>
            <div className="flex row gap-2 mt-4 mb-4">
              <div className="bg-indigo-500 shadow-lg shadow-indigo-500/50 p-2 rounded-lg text-sm">
                Height: {pokemon.height}
              </div>
              <div className="bg-indigo-500 shadow-lg shadow-indigo-500/50 p-2 rounded-lg text-sm">
                Weight: {pokemon.weight}
              </div>
            </div>
            <div className="bg-white text-black rounded-lg p-4">
              {pokemon.stats.map((stat) => (
                <div key={uuidv4()}>
                  <div className="capitalize flex row items-center gap-2">
                    <table>
                      <tr>
                        <td width={110}>{stat.stat.name}</td>
                        <td width={40}>{stat.base_stat}</td>
                        <td width={40}>
                          <div
                            style={{
                              width: `${stat.base_stat}px`,
                              height: "0.5rem",
                              backgroundColor: `${
                                stat.base_stat <= 29 ? "red" : stat.base_stat <= 60 ? "yellow" : "green"
                              }`,
                            }}
                          ></div>
                        </td>
                      </tr>
                    </table>
                  </div>
                </div>
              ))}
            </div>
            <div>
              <Image
                priority
                alt={pokemon.name}
                height={300}
                width={300}
                src={pokemon.sprites.other.home.front_default}
              />
            </div>
          </div>
        </div>
      ))}
    </>
  );
}
```

이 파일의 대부분 코드는 포켓몬 어플리케이션의 인터페이스를 만드는 데 사용됩니다. 스타일링은 Tailwind CSS를 사용하여 완료되었습니다.

프로젝트가 완료되기 전에 해야 할 작업이 몇 개 더 남았습니다. 다음으로 작업할 파일은 hooks 폴더 내의 usePokemon.js 파일입니다.

이 코드를 추가해야 할 것이므로 지금 추가하세요:

<div class="content-ad"></div>

```js
import { useState, useEffect } from "react";
import { fetchPokemon } from "../utils/fetchUtils";

const usePokemon = (initialUrl) => {
  const [data, setData] = useState(null);
  const [isLoading, setIsLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchData = async () => {
      try {
        const pokemonData = await fetchPokemon(initialUrl);
        setData(pokemonData);
      } catch (error) {
        setError(error);
      } finally {
        setIsLoading(false);
      }
    };
    fetchData();
  }, [initialUrl]);
  return { data, isLoading, error };
};

export default usePokemon;
```

이 커스텀 훅은 API에서 데이터를 가져오는 데 사용됩니다. 우리의 경우에는 포켓몬 API를 위한 것입니다.

이제 utils 폴더의 dateUtils.js 파일을 다음 코드로 완성하겠습니다:

```js
import dayjs from "dayjs";

export const getLiveDateTime = () => {
  const now = dayjs();
  return {
    date: now.format("MMMM D, YYYY"),
    time: now.format("h:mm:ss A"),
  };
};
```

<div class="content-ad"></div>

이 유틸리티 파일을 사용하면 dayjs JavaScript 라이브러리를 사용하여 해당 파일을 가져온 모든 파일에서 날짜와 시간을 계산할 수 있습니다.

그럼, 두 번째 유틸리티 파일인 fetchUtils.js에서 다음 코드를 추가해보세요:

```js
export const fetchPokemon = async (url) => {
  try {
    const response = await fetch(url);
    const data = await response.json();
    console.log(data);
    return data;
  } catch (error) {
    console.error("Error fetching Pokemon:", error);
    throw error;
  }
};
```

이 유틸리티 파일은 usePokemon.js 훅과 함께 작동하여 API에서 데이터를 가져옵니다.

<div class="content-ad"></div>

마침내 루트 폴더 내의 메인 페이지.js 파일에 코드를 교체하고 추가하여 프로젝트를 완료해 봅시다.

다음은 해당 파일에 필요한 코드입니다:

```js
"use client";
import Header from "./components/Header/Header";
import Pokemon from "./components/Pokemon/Pokemon";

export default function PokemonList() {
  return (
    <div className="p-5">
      <Header />
      <h1 className="text-4xl mt-4 mb-4">포켓몬 도감</h1>
      <Pokemon />
    </div>
  );
}
```

우리의 page.js 파일은 모든 컴포넌트의 주요 진입점이며, 이 코드를 통해 이제 프로젝트가 완료되었습니다.

<div class="content-ad"></div>

프로젝트를 실행하려면 다음과 같이 일반적인 Next.js 실행 스크립트를 사용하십시오. 그러면 브라우저에서 포켓몬 포켓덱스 애플리케이션을 볼 수 있습니다:

```js
npm run dev
```

## 결론

오늘은 코드를 구성하고 정리하며 관리할 수 있는 방법을 알아보기 위해 도우미 함수와 React Custom의 차이를 알아야 하는 중요성을 배웠습니다. React에서 상태 논리를 재사용할 때는 도우미 함수보다는 사용자 정의 후크를 권장합니다. 도우미 함수는 상태가 없는 일반 작업에 가장 적합합니다. 언제 어느 것을 사용해야 하는지에 대한 최선의 판단을 내림으로써 코드베이스의 모듈화와 재사용성을 향상시킬 수 있습니다.
