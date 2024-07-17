---
title: "폼 제출 후 로그인 문제가 발생하는 5가지 원인과 해결 방법"
description: ""
coverImage: "/it-shar.ing/assets/no-image.jpg"
date: 2024-07-07 12:19
ogImage:
  url: /it-shar.ing/assets/no-image.jpg
tag: Tech
originalTitle: "why login not working after form submission ?"
link: "https://dev.to/samir_soupape/why-login-not-working-after-form-submission--4lli"
---

login.ts 파일 내용입니다:

```typescript
async onSubmit(): Promise<void> {
    this.email = true;
    this.pwd = true;
    this.mailform = true;
    this.isLoginInProgress.set(true);

    this._authService
      .login(this.form.controls.email.value.trim(), this.form.controls.password.value.trim())
      .pipe(
        catchError((error: HttpErrorResponse) => {
          this.handleAuthError(error);
          return of(error);
        }),
        tap(response => this._handleLogin(response)),
        finalize(() => this.isLoginInProgress.set(false))
      )
      .subscribe({
        error: (error) => {
          console.error('Login error:', error);
        }
      });
}

private _handleLogin(response: any): void {
    if (!response?.user) return;
    this.login = true;
    const accessToken = response.user.accessToken;
    const user_uid = response.user.uid;
    localStorage.setItem(`accessToken`, accessToken);
    localStorage.setItem(`user_uid`, user_uid);
    window.location.reload();
    this._router.navigateByUrl(`/dashboard`);
}
```

<div class="content-ad"></div>

```typescript
// handleAuthError(err: HttpErrorResponse): void
if (!err.error.code) return;

this.authError.set(true);
this.form.valueChanges
  .pipe(
    take(1),
    tap(() => this.authError.set(true))
  )
  .subscribe();
```

<div class="content-ad"></div>

login.html:

`form [formGroup]="form" (ngSubmit)="onSubmit()" class="form-detail"`
`/form`

authservice.ts:
`login(email: string, password: string): Observable`

```javascript
console.log(`Logging in user:`, email);
const promise = signInWithEmailAndPassword(this.firebaseAuth, email, password)
  .then((response) => {
    console.log(`Login successful:`, response);
    return response;
  })
  .catch((error) => {
    console.error(`Login error:`, error);
    throw error;
  });

return from(promise);
```
