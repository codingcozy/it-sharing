---
title: "ExpressoTS Render 엔진 개념 및 내용 정리"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-07-23 21:47
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "ExpressoTS Render Engine"
link: "https://dev.to/expressots/expressots-render-engine-2lpi"
isUpdated: true
updatedAt: 1723812859983
---



## 렌더 엔진

ExpressoTS는 Express.js의 웹 서버 기능을 향상시켜 보기를 렌더링하는 과정을 간소화하여 제공합니다. ExpressoTS는 EJS, PUG 및 Handlebars (HBS)를 포함한 다양한 렌더 엔진을 기본으로 지원하여 개발자들이 추가 구성이 필요하지 않고 각 엔진별로 기본 설정이 제공되므로 쉽게 뷰를 렌더링할 수 있습니다.

## 지원하는 렌더 엔진

ExpressoTS는 다음과 같은 렌더 엔진을 지원합니다.

<div class="content-ad"></div>

- EJS
- PUG
- HBS (핸들바)

### 핸들바 구성 옵션

```js
export type HandlebarsOptions = {
    viewEngine?: string; // 사용할 뷰 엔진
    viewsDir?: string; // 뷰 폴더의 경로
    partialsDir?: string; // 파셜 폴더의 경로
};
```

#### 기본 핸들바 구성 옵션

<div class="content-ad"></div>

```js
{
    viewEngine: "hbs",
    viewsDir: <root>/views,
    partialsDir: <root>/views/partials,
}
```

### Default Folder structure

Default folder structure for Handlebars:

```js
src
views
|--partials
|   |--partial.hbs
|--index.hbs
```

<div class="content-ad"></div>

다른 엔진들은 모두 동일한 구조를 따릅니다. Handlebars에만 특정한 부분 폴더(partials folder)가 있는 점을 제외하고는요.

### Handlebars 파일 예시

```js
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>HBS 예시</title>
    </head>
    <body>
        <h1>Handlebars에서 안녕하세요</h1>
        <p>파셜 렌더링: {> partial}</p>
    </body>
</html>
```

## 다른 엔진 구성 옵션

<div class="content-ad"></div>

### Pug 구성 옵션

```js
export type PugOptions = {
    viewEngine?: string;
    viewsDir?: string;
};
```

#### 기본 Pug 구성 옵션

```js
{
    viewEngine: "pug",
    viewsDir: <root>/views,
}
```

<div class="content-ad"></div>

### EJS 구성 옵션

```js
export type EjsOptions = {
    viewsDir?: string;
    viewEngine?: string;
    serverOptions?: EjsOptionsServer;
};
```

#### 기본 EJS 구성 옵션

```js
{
    viewEngine: "ejs",
    viewsDir: <root>/views,
    serverOptions: {
        cache: true,
        compileDebug: false,
        debug: false,
        delimiter: "%",
        strict: false,
    },
}
```

<div class="content-ad"></div>

## 사용 방법

ExpressoTS를 설정하여 응용 프로그램에서 HBS(핸들바)와 같은 렌더 엔진을 사용하는 방법에 대해 알려드리겠습니다:

app.provider 구성 공급자에서 렌더 엔진을 설정할 수 있습니다:

```js
export class App extends AppExpress {
    private middleware: IMiddleware;
    private provider: ProviderManager;

    constructor() {
        super();
        this.middleware = container.get<IMiddleware>(Middleware);
        this.provider = container.get(ProviderManager);
    }

    protected configureServices(): void {
        // 렌더 엔진을 HBS로 설정합니다.
        this.setEngine(Engine.HBS);

        this.middleware.setErrorHandler();
    }

    protected async postServerInitialization(): Promise<void> {
        if (this.isDevelopment()) {
            this.provider.get(Env).checkAll();
        }
    }

    protected serverShutdown(): void {}
}
```

<div class="content-ad"></div>

만약 렌더 엔진에 사용자 정의 옵션을 전달하려면, 원하는 옵션을 포함한 객체를 setEngine 메서드에 전달하면 됩니다. 예를 들어, 보기 디렉토리를 사용자 정의 경로로 설정하려면 다음과 같이 할 수 있습니다:

```js
this.setEngine<HBS>(Engine.HBS, { viewsDir: <<custom-path>> });
```

### ExpressoTS에서 보기 렌더링하는 방법

ExpressoTS에서 보기를 렌더링하려면 Response 객체에서 제공하는 render 메서드를 사용할 수 있습니다. 아래는 ExpressoTS에서 보기를 렌더링하는 예시입니다:

<div class="content-ad"></div>

```js
@Get("/")
root(@response() res: Response) {
    res.render("index", { date: new Date(), name: "Random information" });
}
```

위 예제에서는 Response 객체의 render 메서드가 호출되어 렌더링할 뷰의 이름과 뷰로 전달할 데이터가 포함된 객체가 전달됩니다. 뷰 엔진은 제공된 데이터로 뷰를 렌더링하고 렌더링된 HTML을 클라이언트에게 반환합니다.

### Render 데코레이터

@Render 데코레이터는 컨트롤러 메서드에 사용하여 지정된 뷰 엔진을 사용해 뷰를 렌더링하는 데에 사용할 수 있습니다. 다음은 ExpressoTS에서 @Render 데코레이터를 사용하여 뷰를 렌더링하는 방법의 예시입니다:

<div class="content-ad"></div>

#### 데코레이터에서 뷰 및 기본 데이터 전달 렌더링

```js
@Get("/")
@Render("index", { date: new Date(), name: "랜덤 정보" })
root() {}
```

#### 데코레이터에서 뷰만 전달 렌더링

```js
@Get("/")
@Render("index")
root() {
    return { date: new Date(), name: "랜덤 정보" };
}
```

<div class="content-ad"></div>

행복한 코딩하세요! 🐎