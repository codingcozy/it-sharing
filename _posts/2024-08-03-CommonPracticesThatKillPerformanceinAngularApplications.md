---
title: "Angular 애플리케이션 성능을 망치는 습관 10가지"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-03 20:48
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Common Practices That Kill Performance in Angular Applications"
link: "https://medium.com/@erick.98zanetti.98/common-practices-that-kill-performance-in-angular-applications-e0a25a5870cd"
isUpdated: true
updatedAt: 1723817095301
---



고성능 Angular 앱을 개발하려면 흔히 범하는 함정을 피해야 합니다. 성능에 해를 끼치는 주요 사례와 그 해결 방법을 살펴보겠습니다.

![이미지](https://miro.medium.com/v2/resize:fit:1360/1*e5Fle3T638N5u18AhwSGgQ.gif)

Angular로 고성능 앱을 개발하기 위해서는 종종 간과되는 세부사항에 주의를 기울여야 합니다. Angular 애플리케이션의 성능을 저하시킬 수 있는 일반적인 사례들과 잘못된 코드의 예시 및 그 수정 방법을 살펴보겠습니다.

# 1. 과도한 변경 감지

<div class="content-ad"></div>

## 잘못된 코드:

성능에 영향을 미치는 자주 발생하는 변경 감지

```js
@Component({
  selector: 'app-my-component',
  templateUrl: './my-component.component.html',
})
export class MyComponent {
  @Input() data: any;
}
```

## 수정:

<div class="content-ad"></div>

가능한 경우 컴포넌트에서 OnPush 전략을 사용하세요. 이렇게 하면 컴포넌트의 입력이 변경될 때만 변경 감지가 발생합니다.

```js
@Component({
  selector: 'app-my-component',
  templateUrl: './my-component.component.html',
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class MyComponent {
  @Input() data: any;
}
```

# 2. 비효율적인 DOM 조작

## 나쁜 코드:

<div class="content-ad"></div>

직접 DOM 조작은 비효율적이고 관리하기 어렵습니다.

```js
ngAfterViewInit() {
  document.getElementById('elementId').style.color = 'blue';
}
```

## 수정:

Angular의 Renderer2를 사용하여 DOM을 더 효율적으로 조작하세요.

<div class="content-ad"></div>

```ts
import { Renderer2 } from '@angular/core';

constructor(private renderer: Renderer2) {}

ngAfterViewInit() {
  const element = this.renderer.selectRootElement('#elementId');
  this.renderer.setStyle(element, 'color', 'blue');
}
```

## 3. 파이프의 남발

### 나쁜 코드:

번거로운 작업을 수행하는 파이프는 자주 다시 실행되어 성능에 영향을 줄 수 있습니다.

<div class="content-ad"></div>

```js
@Pipe({
  name: 'heavyPipe',
  pure: false
})
export class HeavyPipe implements PipeTransform {
  transform(value: any): any {
    // Heavy transformation logic
  }
}
```

## 수정:

가벼운 작업에는 순수 파이프(pure: true)를 사용하거나 무거운 작업에는 파이프를 피하십시오.

```js
@Pipe({
  name: 'heavyPipe',
  pure: true
})
export class HeavyPipe implements PipeTransform {
  transform(value: any): any {
    // 가벼운 변환 작업
  }
}
```

<div class="content-ad"></div>

# 4. 초기 로드 과도

## 잘못된 코드:

초기화 시 모든 모듈 및 컴포넌트를 로드하면 로딩 시간이 지연될 수 있습니다.

```js
@NgModule({
  declarations: [AppComponent, FeatureComponent],
  imports: [BrowserModule, FeatureModule],
  bootstrap: [AppComponent]
})
export class AppModule {}
```

<div class="content-ad"></div>

## 수정 사항:

즉시 필요하지 않은 모듈에 대해 lazy loading을 사용하세요.

```js
const routes: Routes = [
  { path: 'feature', loadChildren: () => import('./feature/feature.module').then(m => m.FeatureModule) }
];
```

# 5. 관리되지 않는 구독

<div class="content-ad"></div>

## 잘못된 코드:

구독을 취소하지 않으면 메모리 누수가 발생할 수 있습니다.

```js
ngOnInit() {
  this.myService.myObservable.subscribe(value => {
    // value를 처리합니다
  });
}
```

## 수정:

<div class="content-ad"></div>

테이블 태그를 마크다운 형식으로 변경해 주세요.


Use the async pipe or manage subscriptions with takeUntil or Subscription.

```js
import { Subject } from 'rxjs';
import { takeUntil } from 'rxjs/operators';

@Component({
  selector: 'app-my-component',
  templateUrl: './my-component.component.html',
  styleUrls: ['./my-component.component.css']
})
export class MyComponent implements OnDestroy {
  private destroy$ = new Subject<void>();

  constructor(private myService: MyService) {
    this.myService.getData()
      .pipe(takeUntil(this.destroy$))
      .subscribe(data => {
        // Handle the data
      });
  }

  ngOnDestroy() {
    this.destroy$.next();
    this.destroy$.complete();
  }
}
```

For Angular 16+ use takeUntilDestroyed.

```js
import { Component, OnDestroy } from '@angular/core';
import { takeUntilDestroyed } from '@angular/core/rxjs-interop';

@Component({
  selector: 'app-my-component',
  templateUrl: './my-component.component.html',
  styleUrls: ['./my-component.component.css']
})
export class MyComponent implements OnInit, OnDestroy {

  constructor(private myService: MyService) {
  }

  ngOnInit(): void {
    this.myService.getData()
      .pipe(takeUntilDestroyed(this))
      .subscribe(data => {
        // Handle the data
      });
  }

  ngOnDestroy() {
  }
}
```

<div class="content-ad"></div>

# 6. 공급자에서 과도한 종속성

## 잘못된 코드:

루트 모듈에서 너무 많은 공급자를 등록하면 번들 크기가 커집니다.

```js
@NgModule({
  providers: [MyService]
})
export class AppModule {}
```

<div class="content-ad"></div>

## 수정:

필요한 곳에만 공급자를 등록하세요.

```js
@NgModule({
  providers: [MyService] // 필요한 곳에만 등록하세요
})
export class FeatureModule {}
```

# 7. AOT 컴파일 부족

<div class="content-ad"></div>

## 잘못된 코드:

JIT(Just-in-Time) 컴파일을 사용하면 효율이 떨어질 수 있습니다.

```js
// AOT에 대한 특정 구성 없음
```

## 수정:

<div class="content-ad"></div>

```json
{
  "projects": {
    "my-app": {
      "architect": {
        "build": {
          "options": {
            "aot": true
          }
        }
      }
    }
  }
}
```

# 8. 번들 최적화 부족

## 나쁜 코드:

<div class="content-ad"></div>

미 최적화된 번들은 로딩 시간을 늘립니다.

```js
// 기본 Angular CLI 구성
```

## 수정:

웹팩과 Angular CLI 빌드 구성과 같은 도구를 사용하여 번들을 최적화하세요.

<div class="content-ad"></div>

```js
{
  "projects": {
    "my-app": {
      "architect": {
        "build": {
          "configurations": {
            "production": {
              "optimization": true,
              "sourceMap": false,
              "extractCss": true,
              "namedChunks": false,
              "aot": true,
              "vendorChunk": false,
              "buildOptimizer": true
            }
          }
        }
      }
    }
  }
}
```

# 결론

위의 권고 사항과 수정 사항을 따르면 Angular 애플리케이션의 성능을 크게 향상시킬 수 있으며 더 효율적이고 반응이 빠른 애플리케이션으로 만들 수 있습니다.

LinkedIn 팔로우하기: https://www.linkedin.com/in/erickzanetti