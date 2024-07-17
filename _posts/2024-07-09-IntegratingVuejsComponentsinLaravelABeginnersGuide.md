---
title: "Laravel에서 Vuejs 컴포넌트 통합하기 초심자 가이드"
description: ""
coverImage: "/it-shar.ing/assets/no-image.jpg"
date: 2024-07-09 17:23
ogImage:
  url: /it-shar.ing/assets/no-image.jpg
tag: Tech
originalTitle: "Integrating Vue.js Components in Laravel: A Beginner’s Guide"
link: "https://medium.com/@f24aalam/integrating-vue-js-components-in-laravel-a-beginners-guide-cf8414a27d6b"
---

Laravel과 Vue.js는 웹 개발에서 일반적으로 사용되는 두 강력한 프레임워크입니다. Laravel은 포괄적인 백엔드 솔루션을 제공하는 반면, Vue.js는 유연하고 효율적인 방법으로 대화형 프론트엔드 구성 요소를 구축하는 데 도움을 줍니다. Laravel은 동적 인터페이스에 대한 대체 옵션으로 Inertia.js 및 Livewire와 같은 옵션을 제공하지만, 개발자들이 특히 기존 또는 초보자 프로젝트를 위해 Vue.js를 Laravel 애플리케이션에 직접 통합하는 것을 선호하는 상황도 있습니다.

이 자습서는 이러한 프로젝트로 시작하는 초심자를 대상으로 특별히 설계되었으며, Vue.js 구성 요소를 Laravel 애플리케이션에 원활하게 통합하는 방법을 배우고 싶어하는 분들을 대상으로 합니다. 이 자습서에서 안내된 단계에 따라 따라하면, Vue.js 구성 요소를 Laravel 프로젝트에 효과적으로 통합하는 데 유용한 기술을 습득할 수 있습니다. 심지어 기존에 Vue.js 없이 구축된 프로젝트에도 적용할 수 있습니다. Laravel 개발 기술을 향상시키는 방법을 발견하고 Vue.js의 강점을 활용하는 방법에 대해 알아봅시다!

필수 요구 사항: 시작하기 전에 다음 사항을 확인하세요:

- Laravel 프레임워크의 기본 지식
- Vue.js 기초에 대한 이해

<div class="content-ad"></div>

## 단계 1: Laravel 설정하기

먼저 로컬 머신에 Laravel이 설치되어 있는지 확인해주세요. 그렇지 않다면, 공식 Laravel 문서를 따라 설치하고 새 프로젝트를 설정해보세요. Laravel 프로젝트가 준비되면 터미널에서 프로젝트 디렉토리로 이동해주세요.

## 단계 2: Laravel Breeze 설치하기

Laravel Breeze는 Laravel 애플리케이션에서 인증을위한 최소한의 설정을 제공하는 가벼운 스캐폴딩 패키지입니다. Laravel Breeze를 설치하려면 터미널에서 다음 명령을 실행하세요:

<div class="content-ad"></div>

```js
composer require laravel/breeze --dev
```

설치가 완료되면 다음 명령어를 실행하여 인증 스캐폴딩을 생성하세요:

```js
php artisan breeze:install
```

## 단계 3: 데이터베이스 생성하기

<div class="content-ad"></div>

다음으로, Laravel 애플리케이션을 위한 새 데이터베이스를 생성해주세요. 데이터베이스 관리 도구인 phpMyAdmin을 사용하거나 데이터베이스 서버에서 적절한 SQL 명령을 실행할 수 있습니다.

## 단계 4: 마이그레이션 실행

이제, 인증 및 사용자 관리를 위한 필요한 테이블을 설정하기 위해 데이터베이스 마이그레이션을 실행해야 합니다. 터미널에서 다음 명령을 실행해주세요:

```js
php artisan migrate
```

<div class="content-ad"></div>

## 단계 5: 종속성 설치 및 에셋 빌드

필요한 프런트엔드 종속성을 설치하고 에셋을 빌드하려면 다음 명령을 실행하세요:

```js
npm install
npm run dev
```

## 단계 6: 라라벨 애플리케이션 서빙

<div class="content-ad"></div>

로컬에서 Laravel 애플리케이션을 실행하려면 다음 명령을 실행하세요:

```js
php artisan serve
```

이 명령을 실행하면 로컬 개발 서버가 시작되며, 제공된 URL을 사용하여 웹 브라우저에서 애플리케이션에 액세스할 수 있습니다.

## 단계 7: Vue.js 및 Vue Loader 설치하기

<div class="content-ad"></div>

Vue.js 컴포넌트 작업 시 개발 경험을 향상시키기 위해 Vue Loader를 설치하겠습니다. Vue Loader는 웹팩 로더로, 단일 파일 Vue 컴포넌트 작성을 가능하게 합니다. HTML, CSS, JavaScript을 하나의 파일에 결합하여 컴포넌트 구조를 간소화합니다.

Vue Loader를 설치하려면 다음 명령어를 실행하세요:

```js
npm install vue@next vue-loader@next
```

## 단계 8: Vite용 Vue.js 플러그인 설치

<div class="content-ad"></div>

Vite은 차세대 프론트엔드 빌드 도구이며, Laravel은 Laravel 애플리케이션과 통합하기 위한 플러그인을 제공합니다. 다음 명령을 실행하여 Vite용 Vue.js 플러그인을 설치하세요:

```js
npm i @vitejs/plugin-vue
```

## 단계 9: Vite 구성 업데이트

Vite를 구성하려면 vite.config.js 파일을 열고 다음 코드로 업데이트하세요:

<div class="content-ad"></div>

```js
import { defineConfig } from "vite";
import laravel from "laravel-vite-plugin";
import vue from "@vitejs/plugin-vue";

export default defineConfig({
  plugins: [
    vue({
      template: {
        transformAssetUrls: {
          base: null,
          includeAbsolute: false,
        },
      },
    }),
    laravel({
      input: ["resources/css/app.css", "resources/js/app.js"],
      refresh: true,
    }),
  ],
  resolve: {
    alias: {
      "@": "/resources/js",
      vue: "vue/dist/vue.esm-bundler.js",
    },
  },
});
```

## 단계 10: app.js 파일 업데이트

app.js 파일을 열어 다음과 같이 업데이트하십시오:

```js
import "./bootstrap";
import { createApp } from "vue";

const app = createApp({});

app.mount("#app");
```

<div class="content-ad"></div>

## 단계 11: 블레이드 템플릿 수정

Vue.js 컴포넌트가 Laravel의 블레이드 템플릿 엔진과 함께 작동하려면 블레이드 파일을 수정해야 합니다. app.blade.php 및 guest.blade.php 파일을 열어 부모 요소에 id="app"을 추가해주세요.

```js
<!DOCTYPE html>
<html lang="{ str_replace('_', '-', app()->getLocale()) }">
    <head>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <meta name="csrf-token" content="{ csrf_token() }">

        <title>{ config('app.name', 'Laravel') }</title>

        <!-- Fonts -->
        <link rel="stylesheet" href="https://fonts.bunny.net/css2?family=Nunito:wght@400;600;700&display=swap">

        <!-- Scripts -->
        @vite(['resources/css/app.css', 'resources/js/app.js'])
    </head>
    <body class="font-sans antialiased">
        <div class="min-h-screen bg-gray-100" id="app">

....
....
....
```

```js
<!DOCTYPE html>
<html lang="{ str_replace('_', '-', app()->getLocale()) }">
    <head>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <meta name="csrf-token" content="{ csrf_token() }">

        <title>{ config('app.name', 'Laravel') }</title>

        <!-- Fonts -->
        <link rel="stylesheet" href="https://fonts.bunny.net/css2?family=Nunito:wght@400;600;700&display=swap">

        <!-- Scripts -->
        @vite(['resources/css/app.css', 'resources/js/app.js'])
    </head>
    <body class="font-sans text-gray-900 antialiased">
        <div class="min-h-screen flex flex-col sm:justify-center items-center pt-6 sm:pt-0 bg-gray-100" id="app">
....
....
....
```

<div class="content-ad"></div>

## 단계 12: Vue.js 컴포넌트 생성하기

resources/js 디렉토리 안에 "components"라는 새 폴더를 생성하세요. "components" 폴더 안에 ComponentA.vue와 같은 Vue.js 컴포넌트 파일을 만들고 컴포넌트의 템플릿과 로직을 정의하세요. 예를 들어:

```js
<template>
  <h1>작동 중</h1>
</template>
```

## 단계 13: Vue.js 컴포넌트 등록하기

<div class="content-ad"></div>

앱.js 파일을 열고 생성된 Vue.js 컴포넌트를 가져옵니다. app.component 메서드를 사용하여 컴포넌트를 등록하세요. 예를 들면:

```js
import "./bootstrap";

import { createApp } from "vue";
import ComponentA from "./components/ComponentA.vue";

const app = createApp({});

app.component("ComponentA", ComponentA);

app.mount("#app");
```

## 단계 14: Blade 템플릿에서 Vue.js 컴포넌트 사용

Vue.js 컴포넌트가 등록되었으므로 이를 Blade 템플릿에서 사용할 수 있습니다. Blade 파일을 열고 `component-a`/`component-a` 구문을 사용하여 컴포넌트를 추가하세요. 컴포넌트를 `div id="app"`/`/div` 요소 안에 포함시키는 것을 잊지 마세요.

<div class="content-ad"></div>

## 또 다른 컴포넌트 및 Laravel에서 데이터 가져오기

데이터베이스에서 데이터를 가져오는 것을 보여주기 위해 components 디렉토리에 ComponentB.vue라는 또 다른 컴포넌트를 만들어 보겠습니다. 이 컴포넌트 안에 원하는 코드를 작성할 수 있습니다. 첫 번째 컴포넌트(ComponentA)를 등록한 방식과 동일하게 app.js 파일에 등록하세요.

```js
import "./bootstrap";

import { createApp } from "vue";
import ComponentA from "./components/ComponentA.vue";
import componentB from "./components/ComponentB.vue";

const app = createApp({});

app.component("ComponentA", ComponentA).component("ComponentB", ComponentB);

app.mount("#app");
```

## 블레이드 파일에서 Vue 컴포넌트로 데이터 전송하기

<div class="content-ad"></div>

라라벨 블레이드 파일에서 Vue 구성 요소로 데이터를 전송하려면 User 객체를 ComponentA.vue 컴포넌트로 전달해야 합니다. ComponentA.vue 파일을 열고 다음 변경 사항을 적용하세요:

```js
<script setup>
const props = defineProps({
    user: Object
});
</script>

<template>
    <h1>Hello, { user.name }님</h1>
</template>
```

이제 `component-a` 태그를 사용하여 ComponentA로 데이터를 전송할 수 있습니다. 다음과 같이 dashboard.blade.php 파일을 업데이트하세요:

```js
<x-app-layout>
    <x-slot name="header">
        <h2 class="font-semibold text-xl text-gray-800 leading-tight">
            { __('대시보드') }
        </h2>
    </x-slot>

    <div class="py-12">
        <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
            <div class="bg-white overflow-hidden shadow-sm sm:rounded-lg">
                <div class="p-6 text-gray-900">
                    <component-a :user="{ Auth::user() }"/>
                </div>
            </div>
        </div>
    </div>
</x-app-layout>
```

<div class="content-ad"></div>

페이지를 다시로드하면 ComponentA에 로그인한 사용자의 이름이 표시됩니다.

## 데이터베이스에서 데이터 가져오기

Vue 컴포넌트에서 데이터베이스에서 데이터를 가져오려면 Axios 패키지를 사용합니다. 다음 명령을 실행하여 Axios를 설치하세요:

```js
npm install axios
```

<div class="content-ad"></div>

Vue.js 컴포넌트를 Blade 템플릿 내에서 통합하고 세션 정보 및 기타 웹 특정 기능에 액세스해야하기 때문에 web.php 파일에 라우트를 생성할 것입니다.

Laravel에서 web.php 파일은 세션 관리, 쿠키 처리, CSRF(Cross-Site Request Forgery) 보호 및 기타 웹 특정 기능을 처리하는 것에 특히 사용됩니다.

web.php에서 라우트를 정의함으로써 Vue 컴포넌트가 백엔드와 상호 작용하거나 HTTP 요청을 할 때 세션 데이터와 기타 웹 관련 기능에 액세스할 수 있도록 보장합니다.

우리의 예제에서는 서버로부터 데이터를 가져오는 것을 보여주기 위해 간단한 라우트를 생성할 것입니다. web.php 파일에 다음 라우트를 추가해주세요:

<div class="content-ad"></div>

```php
Route::get('/demo-request', function() {
    return response()->json(Auth::user());
});
```

이 루트는 /demo-request URL로 수신된 GET 요청에 응답합니다. 이 경우 인증된 사용자의 데이터를 JSON 응답으로 반환합니다.

web.php 대신 api.php를 사용함으로써, 루트가 세션 정보, 쿠키 및 Vue 구성 요소에서 필요한 기타 웹 관련 기능에 액세스 할 수 있도록 합니다.

이제 ComponentA.vue 파일을 업데이트하여 Axios를 사용하여 데이터를 가져오겠습니다:

<div class="content-ad"></div>

```js
<script setup>
import axios from 'axios';
import { ref } from 'vue';

const user = ref();

async function sendRequest() {
    const response = await axios.get('/demo-request')

    if (response.status === 200) {
        user.value = response.data;
    }
}

</script>

<template>
    <h1>Hello, { user ? user.name : 'World' }</h1>
    <button class="inline-flex items-center px-4 py-2 bg-gray-800 border border-transparent rounded-md font-semibold text-xs text-white uppercase tracking-widest hover:bg-gray-700 focus:bg-gray-700 active:bg-gray-900 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-offset-2 transition ease-in-out duration-150" @click="sendRequest">Send Request</button>
</template>
```

이 업데이트된 컴포넌트에서는 버튼을 추가했습니다. 버튼을 클릭하면 sendRequest 함수가 트리거됩니다. 이 함수는 이전에 만든 /demo-request 경로로 GET 요청을 수행하기 위해 Axios를 사용합니다. 그런 다음 응답 데이터가 user ref에 할당됩니다.

이제 컴포넌트는 기본적으로 "Hello, World"를 표시합니다. 버튼을 클릭하면 데이터가 데이터베이스에서 가져와지면 텍스트가 "Hello, User Name"으로 변경됩니다.

또한 Vue 컴포넌트 내에서 POST 요청을 처리하고 다른 API 작업을 수행하기 위해 Axios를 사용할 수도 있습니다.

<div class="content-ad"></div>

## POST 요청 및 CSRF 토큰 전송

POST 요청을 처리하고 CSRF 토큰을 포함시키려면 다음 단계를 따르세요:

- web.php 파일에서 POST 요청을 처리하기 위한 새 경로를 만드세요:

```js
Route::post('/demo-post', function (Request $request) {
    // 필요한 경우 요청 데이터 유효성 검사

    // 데이터 처리 수행

    // 응답 반환
    return response()->json(['message' => '포스트 요청 성공']);
});
```

<div class="content-ad"></div>

2. Blade 템플릿에서 레이아웃 파일의 `head` 섹션에 CSRF 토큰을 추가해주세요:

```js
<head>
    <!-- 다른 메타 태그 및 스타일 시트 -->
    <meta name="csrf-token" content="{ csrf_token() }">
</head>
```

라라벨(Laravel)의 web.php 파일을 다룰 때 내장된 CSRF(Cross-Site Request Forgery) 보호를 사용한다면, Blade 템플릿의 `head` 태그에 CSRF 토큰을 포함하는 것이 중요합니다. 이는 응용 프로그램과 서버 간의 안전한 통신을 보장하기 위해 필요합니다. CSRF 토큰 없이는 요청이 실패할 수 있습니다.

3. Vue.js 컴포넌트에서 Axios 패키지를 사용하여 POST 요청을 처리하세요:

<div class="content-ad"></div>

```js
import axios from "axios";

async function submitData() {
  const csrfToken = document.querySelector('meta[name="csrf-token"]').getAttribute("content");

  try {
    const response = await axios.post(
      "/demo-post",
      { data },
      {
        headers: {
          "X-CSRF-TOKEN": csrfToken,
        },
      }
    );

    // 응답 처리
  } catch (error) {
    // 오류 처리
  }
}
```

루트, 폼 필드 및 데이터 처리 코드를 귀하의 특정 애플리케이션 요구 사항에 맞게 조정해주세요.

바로 여기까지입니다! 이제 Laravel 애플리케이션 내 Vue.js 구성 요소에서 CSRF 토큰을 포함하여 POST 요청을 보내는 방법을 배웠습니다.

Laravel과 Vue.js를 사용할 때, 추가 패키지를 활용하여 개발 경험을 향상시킬 수 있습니다. 그 중 하나인 robsontenorio/vue-api-query와 같은 패키지는 spatie/laravel-query-builder와 호환성을 제공합니다. 이 패키지를 통합하면 Vue.js 애플리케이션에서 API 쿼리 빌딩을 간소화할 수 있습니다. 이러한 도구를 탐구하고 Laravel 생태계를 최신 상태로 유지하면 개발 작업 흐름을 최적화하고 새로운 가능성을 활용할 수 있습니다. 즐거운 코딩하세요!
