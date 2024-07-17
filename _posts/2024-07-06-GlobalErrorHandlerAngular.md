---
title: "전역 오류 핸들러  Angular에서 사용하기"
description: ""
coverImage: "/assets/img/2024-07-06-GlobalErrorHandlerAngular_0.png"
date: 2024-07-06 02:01
ogImage:
  url: /assets/img/2024-07-06-GlobalErrorHandlerAngular_0.png
tag: Tech
originalTitle: "Global Error Handler — Angular"
link: "https://medium.com/@awaisshaikh94/global-error-handler-angular-ed1564c2f9fd"
---

Angular에서 글로벌 오류 처리에 대해 더 깊게 알아보겠습니다. 세부 정보와 고려 사항이 포함되어 있습니다.

/assets/img/2024-07-06-GlobalErrorHandlerAngular_0.png

# 상세한 오류 처리:

사용자 정의 GlobalErrorHandler의 handleError 메서드는 로깅 외에도 더 포괄적인 작업을 수행할 수 있습니다. 아래 예는 Router 서비스를 주입하여 오류 유형에 따라 사용자를 특정 오류 페이지로 이동시킵니다. 또한 네트워크 오류를 처리하고 쿼리 파라미터를 통해 보다 정보가 풍부한 메시지를 제공하는 방법을 보여줍니다.

<div class="content-ad"></div>

```js
import { ErrorHandler, Injectable, Injector } from '@angular/core';
import { Router } from '@angular/router';

@Injectable()
export class GlobalErrorHandler implements ErrorHandler {

  constructor(private injector: Injector) {}

  handleError(error: any) {
    const router = this.injector.get(Router);

    console.error('Error handled globally:', error.message || error);

    // Handle different error types (optional)
    if (error.status === 404) {
      router.navigate(['/not-found']);
    } else if (error.error instanceof ProgressEvent) {
      // Handle network errors
      console.error('Network error:', error);
    } else {
      // Generic error handling (fallback)
      router.navigate(['/error'], { queryParams: { error: error.message } });
    }
  }
}
```

# Provider Registration:

Remember to register your custom GlobalErrorHandler in your AppModule using the ErrorHandler token:

```js
@NgModule({
  // ...
  providers: [{ provide: ErrorHandler, useClass: GlobalErrorHandler }],
  // ...
})
export class AppModule {}
```

<div class="content-ad"></div>

# HTTP 인터셉터:

![GlobalErrorHandlerAngular_1](/assets/img/2024-07-06-GlobalErrorHandlerAngular_1.png)

고급 오류 처리:

HttpErrorInterceptor의 intercept 메서드를 사용하면 심층적인 오류 분석 및 사용자 정의 응답이 가능합니다. 아래 인터셉터는 특정 HTTP 상태 코드를 기반으로 한 오류 처리를 보여주어 사용자에게 더 맥락에 맞는 메시지를 제공합니다. 또한 성공적인 응답을 처리하는 선택적 논리도 보여줍니다.

<div class="content-ad"></div>

```js
import { HttpEvent, HttpHandler, HttpInterceptor, HttpRequest, HttpResponse } from '@angular/common/http';
import { Injectable } from '@angular/core';
import { catchError, map, Observable, throwError } from 'rxjs';

@Injectable()
export class HttpErrorInterceptor implements HttpInterceptor {
  intercept(request: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
    return next.handle(request).pipe(
      catchError(error => {
        if (error instanceof HttpErrorResponse) {
          let errorMessage = '예기치 못한 오류가 발생했습니다.';
          switch (error.status) {
            case 400:
              if (error.error) {
                errorMessage = '잘못된 요청: ' + error.error.message;
              }
              break;
            case 401:
              errorMessage = '인가되지 않은 액세스입니다.';
              break;
            case 404:
              errorMessage = '찾을 수 없습니다.';
              break;
          }
          // 상태 코드에 기반한 사용자 친화적인 메시지로 오류를 처리합니다.
          return throwError(() => new Error(errorMessage));
        } else {
          // 다른 유형의 오류 처리 (예: 네트워크 오류)
          console.error('네트워크 오류:', error);
          return throwError(() => new Error('네트워크 오류가 발생했습니다.'));
        }
      }),
      map<HttpEvent<any>, any>(event => {
        if (event instanceof HttpResponse) {
          // 필요한 경우 성공적인 응답을 처리합니다 (선택 사항)
        }
        return event;
      })
    );
  }
}
```

매개체 등록 (멀티와 함께):

HTTP_INTERCEPTORS 토큰을 사용하여 AppModule에서 HttpErrorInterceptor를 등록하고, 여러 개의 인터셉터를 허용하려면 multi: true로 설정하세요:

```js
@NgModule({
  // ...
  providers: [{ provide: HTTP_INTERCEPTORS, useClass: HttpErrorInterceptor, multi: true }],
  // ...
})
export class AppModule {}
```

<div class="content-ad"></div>

Angular에서 전역 오류 처리는 튼튼하고 사용자 친화적인 애플리케이션을 구축하기 위한 중요한 실천 방법입니다. Global Error Handler와 HTTP Interceptor를 결합하여 애플리케이션 전반에 걸쳐 오류를 효과적으로 잡고 관리할 수 있습니다.

그럼 이만. 감사합니다.
