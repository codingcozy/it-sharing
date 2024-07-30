---
title: "React에서 Refresh Tokens 구현하는 방법"
description: ""
coverImage: "/assets/img/2024-07-30-Part23HowtoImplementRefreshTokensinReact_0.png"
date: 2024-07-30 16:54
ogImage: 
  url: /assets/img/2024-07-30-Part23HowtoImplementRefreshTokensinReact_0.png
tag: Tech
originalTitle: "Part 2 3 How to Implement Refresh Tokens in React"
link: "https://dev.to/zenstok/part-23-how-to-implement-refresh-tokens-in-react-84c"
---


안녕하세요!

저희 앱이 XSS 공격에 면역이라는 전제 하에, 액세스 및 리프레시 토큰을 로컬 스토리지에 저장할 예정입니다. 이를 위해 JSX에 임베드된 모든 값을 렌더링하기 전에 이스케이프하는 React를 사용할 것입니다. 이는 XSS 공격에 대비하는 데 큰 도움이 됩니다.

리프레시 토큰을 구현하는 우리의 세 번째 시리즈 중 두 번째 에피소드입니다. 이전 기사에서는 NestJS에서 리프레시 토큰을 구현하는 방법을 탐색했습니다. 이 튜토리얼의 데모를 쉽게 실행하려면 이전 내용도 꼭 확인해주세요.

액세스 및 리프레시 토큰을 로컬 스토리지에 저장하는 것은 편리하지만 보안 위험이 따릅니다. 견고한 XSS 공격 방지에도 불구하고, 여전히 타사 라이브러리를 통한 공격 취약점이 존재합니다. 다행히도 시리즈의 최종 회차에서는 HTTP-only 쿠키를 사용하여 리프레시 토큰을 안전하게 저장하는 방법을 안내할 예정이며, 이는 보안성을 높일 것입니다.

<div class="content-ad"></div>

### 구현 개요

애플리케이션을 직관적으로 유지하기 위해 기능 중요 부분만 구현했습니다. 여기에 우리가 달성하고자 하는 목표와 주의할 점 몇 가지가 있습니다:

#### 목표:

- 지속적인 인증: 페이지 새로고침 이후에도 사용자가 인증된 상태를 유지할 수 있도록 합니다.
- 자동 로그아웃: API 엔드포인트가 401 Unauthorized 응답을 반환할 때 사용자가 자동으로 로그아웃되어야 합니다.
- 데이터에 원활한 접근: 보호된 엔드포인트를 호출할 때 유효한 리프레시 토큰이 있는 경우 앱은 데이터를 검색해야 합니다. 첫 번째 요청이 401을 반환하면 앱은 토큰을 갱신하려고 시도해야 합니다. 토큰을 갱신한 후에도 계속해서 401로 실패한다면 사용자를 로그아웃해야 합니다.

<div class="content-ad"></div>

#### 잠재적인 주의사항:

주목할만한 문제는 리프레시 토큰 엔드포인트 호출이 이전 리프레시 토큰을 무효화한다는 것입니다. 이것은 /auth/refresh-tokens를 동시에 한 번 이상 호출하면 문제가 발생할 수 있으며, 토큰 처리에서 불일치나 실패로 이어질 수 있습니다. 이를 완화하기 위해 앱 전체에서 여러 번 보호된 요청이 이루어져도 리프레시 토큰이 동시에 여러 번 요청되지 않도록 보장해야 합니다.

만약 당신이 GitHub 리포지토리로 바로 이동하여 우리가 그것을 어떻게 했는지 확인하고 싶다면, 여기를 확인해 보세요.

### 시작하기

<div class="content-ad"></div>

인증 상태는 앱에서 전역 개념이므로 각 구성 요소가이 상태에 액세스 할 수 있도록 보장해야합니다. 이를 관리하는 가장 좋은 방법은 React Context를 사용하여 전역 컨텍스트 제공자를 정의하는 것입니다. 이를 자습서에서 나중에 살펴보겠습니다.

먼저, 액세스 및 리프레시 토큰의 로컬 스토리지 키 조작을 관리하는 클래스를 정의합니다.

```js
const ACCESS_TOKEN_KEY = "rabbit.byte.club.access.token";
const REFRESH_TOKEN_KEY = "rabbit.byte.club.refresh.token";

class AuthClientStore {
  static getAccessToken() {
    return localStorage.getItem(ACCESS_TOKEN_KEY);
  }

  static setAccessToken(token: string) {
    localStorage.setItem(ACCESS_TOKEN_KEY, token);
  }

  static removeAccessToken(): void {
    localStorage.removeItem(ACCESS_TOKEN_KEY);
  }

  static getRefreshToken() {
    return localStorage.getItem(REFRESH_TOKEN_KEY);
  }

  static setRefreshToken(token: string) {
    localStorage.setItem(REFRESH_TOKEN_KEY, token);
  }

  static removeRefreshToken(): void {
    localStorage.removeItem(REFRESH_TOKEN_KEY);
  }
}

export default AuthClientStore;
```

다음으로, 앱 서버에 요청을 보내는 방법을 추상화하는 useApi 훅을 정의합니다. 예제를 통해 작성 방법을 더 자세히 살펴보세요.

<div class="content-ad"></div>

```js
import AuthClientStore from "../../auth/client-store/auth-client-store.ts";
import { ApiMethod } from "../types.ts";

const apiUrl = import.meta.env.VITE_API_BASE_URL as string;

const sendRequest = (
  method: ApiMethod,
  path: string,
  // eslint-disable-next-line
  body?: any,
  authToken?: string | null,
  init?: RequestInit,
) => {
  return fetch(apiUrl + path, {
    method,
    ...(body && { body: JSON.stringify(body) }),
    ...init,
    headers: {
      "Content-Type": "application/json",
      ...(authToken && { Authorization: `Bearer ${authToken}` }),
      ...init?.headers,
    },
  }).then((response) => {
    if (response.status >= 400) {
      throw response;
    }
    return response.json();
  });
};

const sendProtectedRequest = (
  method: ApiMethod,
  path: string,
  // eslint-disable-next-line
  body?: any,
  useRefreshToken = false,
  init?: RequestInit,
) => {
  const authToken = useRefreshToken
    ? AuthClientStore.getRefreshToken()
    : AuthClientStore.getAccessToken();
  if (!authToken) {
    throw new Error("No auth token found");
  }

  return sendRequest(method, path, body, authToken, init);
};

export const useApi = () => {
  return { sendRequest, sendProtectedRequest };
};
```

sendProtectedRequest 메소드는 useRefreshToken 매개변수를 옵션으로 받습니다. 이 매개변수는 토큰을 새로 고침해야 하는 루트에서만 사용됩니다. 새로 고침 토큰이 필요한 이유는 베어러 인증을 위해서입니다. 다른 경우에는 기본 동작으로 액세스 토큰을 사용합니다.

이제 마법이 일어나는 useAuthApi 훅을 정의해야 합니다.

먼저, sendRequest와 sendProtectedRequest 메소드를 호출해야 하는 useApi 훅을 사용할 것입니다:

<div class="content-ad"></div>

이제 로그인 함수를 정의해 봅시다. 성공적으로 인증을하면 액세스 및 리프레시 토큰을 로컬 스토리지에 설정합니다.

```js
export const useAuthApi = () => {
  const { sendRequest, sendProtectedRequest } = useApi();

  const login = async (email: string, password: string) => {
    const response = await sendRequest(ApiMethod.POST, routes.auth.login, {
      email,
      password,
    });

    AuthClientStore.setAccessToken(response.access_token);
    AuthClientStore.setRefreshToken(response.refresh_token);

    return response;
  };
}
```

그 다음, 로그아웃 함수는 간단합니다. 로컬 스토리지에서 액세스 및 리프레시 토큰을 삭제하기만 하면 됩니다.

<div class="content-ad"></div>

```js
export const useAuthApi = () => {
  const { sendRequest, sendProtectedRequest } = useApi();

  const login = async (email: string, password: string) => {
    const response = await sendRequest(ApiMethod.POST, routes.auth.login, {
      email,
      password,
    });

    AuthClientStore.setAccessToken(response.access_token);
    AuthClientStore.setRefreshToken(response.refresh_token);

    return response;
  };

  const logout = () => {
    AuthClientStore.removeAccessToken();
    AuthClientStore.removeRefreshToken();
  };
}
```

필수적으로 정의해야 하는 메소드는 로그인 함수와 유사한 간단한 로직을 가진 refreshTokens입니다:

```js
const refreshTokens = async () => {
  const response = await sendProtectedRequest(
    ApiMethod.POST,
    routes.auth.refreshTokens,
    undefined,
    AuthClientStore.getRefreshToken(),
  );

  AuthClientStore.setAccessToken(response.access_token);
  AuthClientStore.setRefreshToken(response.refresh_token);
};
```

새로고침 토큰 엔드포인트는 새로고침 토큰을 인증 방법으로 필요로 하기 때문에, 다른 요청에 사용되는 기본 액세스 토큰 대신 새로고침 토큰을 베어러로 사용하도록 sendProtectedRequest에 새로고침 토큰을 매개변수로 전달하는 것에 유의해야 합니다.


<div class="content-ad"></div>

리프레시 토큰 메소드에 대해 필요한 모든 것인가요? 앞으로, 각각의 액세스 토큰을 새로 고칠 때마다 이전의 리프레시 토큰이 무효화될 것을 염두해두세요. 즉, 앱의 두 부분이 동시에 액세스 토큰을 새로 고치는 경우, 첫 번째 요청으로 인해 사용되는 리프레시 토큰이 무효화되어 두 번째 요청이 실패할 수 있습니다. 따라서, 이 메소드가 한 번만 호출되도록 보장하는 것이 매우 중요합니다. 이 로직을 효율적으로 관리하기 위해 이 메소드를 약간 꾸밀 필요가 있습니다.

앱의 여러 부분이 리프레시 토큰 메소드를 동시에 호출할 수 있는 경우를 처리하기 위해, 이러한 호출을 디바운스하여 하나의 요청만 이루어지도록 해야 합니다. 또한, 각 호출자가 동일한 액세스와 리프레시 토큰 쌍을 받도록 보장해야 합니다. 이렇게 달성할 수 있는 방법은 다음과 같습니다:

먼저, 디바운스 로직을 관리하기 위해 후크 외부에 몇 가지 변수를 정의합니다:

```js
/*
 * 이 변수들은 refreshTokens 함수를 디바운스하는 데 사용됩니다
 */
let debouncedPromise: Promise<unknown> | null = null;
let debouncedResolve: (...args: unknown[]) => void;
let debouncedReject: (...args: unknown[]) => void;
let timeout: number;
```

<div class="content-ad"></div>

지금은 디바운싱을 포함한 `refreshTokens` 메서드를 업데이트했습니다:

```js
const refreshTokens = async () => {
  clearTimeout(timeout);
  if (!debouncedPromise) {
    debouncedPromise = new Promise((resolve, reject) => {
      debouncedResolve = resolve;
      debouncedReject = reject;
    });
  }

  timeout = setTimeout(() => {
    const executeLogic = async () => {
      const response = await sendProtectedRequest(
        ApiMethod.POST,
        routes.auth.refreshTokens,
        undefined,
        AuthClientStore.getRefreshToken(),
      );

      AuthClientStore.setAccessToken(response.access_token);
      AuthClientStore.setRefreshToken(response.refresh_token);
    };

    executeLogic().then(debouncedResolve).catch(debouncedReject);

    debouncedPromise = null;
  }, 200);

  return debouncedPromise;
};
```

이 작업은 다음과 같이 작동합니다:

- 새 호출자가 200ms 윈도우 내에서 이 메서드를 호출할 때마다 타임아웃을 지우하여 호출을 효과적으로 디바운싱합니다.
- `debouncedPromise`는 모든 호출자가 동일한 약속을 받도록 보장하며, 토큰 갱신 로직이 완료되면 약속이 해결되거나 거부됩니다.
- 처리가 완료된 후 `debouncedPromise`가 재설정되어 나중에 새 호출을 처리합니다.

<div class="content-ad"></div>

다음으로, 보호된 API 경로를 위한 게이트키퍼 역할을 하는 메서드를 정의합니다. 이 메서드는 요청을 시도하고, 401 (권한 없음) 오류가 발생하면 액세스 토큰을 새로 고치고 요청을 다시 시도합니다:

```js
const sendAuthGuardedRequest = async (
  userIsNotAuthenticatedCallback: () => void,
  method: ApiMethod,
  path: string,
  body?: any,
  init?: RequestInit,
) => {
  try {
    return await sendProtectedRequest(method, path, body, undefined, init);
  } catch (e) {
    if (e?.status === 401) {
      try {
        await refreshTokens();
      } catch (e) {
        userIsNotAuthenticatedCallback();
        throw e;
      }
      return await sendProtectedRequest(method, path, body, undefined, init);
    }

    throw e;
  }
};
```

userIsNotAuthenticatedCallback 매개변수를 통해 인증 컨텍스트 제공자가 전역 인증 상태를 업데이트할 수 있습니다. 앱의 모든 구성 요소가 이를 청취할 수 있습니다.

마지막으로, 사용자가 인증되었는지 확인하는 메서드를 /auth/me 엔드포인트를 호출하여 정의합니다. 앱 시작시에 실행되어야 합니다:

<div class="content-ad"></div>

```js
const me = (userIsNotAuthenticatedCallback: () => void) => {
  return sendAuthGuardedRequest(
    userIsNotAuthenticatedCallback,
    ApiMethod.GET,
    routes.auth.me,
  ) as Promise<User>;
};
```

우리의 후크는 이제 완료되었습니다. 여기에 전체 버전이 있습니다:

```js
/*
 * These variables are used to debounce the refreshTokens function
 */
let debouncedPromise: Promise<unknown> | null;
let debouncedResolve: (...args: unknown[]) => void;
let debouncedReject: (...args: unknown[]) => void;
let timeout: number;

export const useAuthApi = () => {
  const { sendRequest, sendProtectedRequest } = useApi();

  const login = async (email: string, password: string) => {
    const response = await sendRequest(ApiMethod.POST, routes.auth.login, {
      email,
      password,
    });

    AuthClientStore.setAccessToken(response.access_token);
    AuthClientStore.setRefreshToken(response.refresh_token);

    return response;
  };

  const logout = () => {
    AuthClientStore.removeAccessToken();
    AuthClientStore.removeRefreshToken();
  };
  const refreshTokens = async () => {
    clearTimeout(timeout);
    if (!debouncedPromise) {
      debouncedPromise = new Promise((resolve, reject) => {
        debouncedResolve = resolve;
        debouncedReject = reject;
      });
    }

    timeout = setTimeout(() => {
      const executeLogic = async () => {
        const response = await sendProtectedRequest(
          ApiMethod.POST,
          routes.auth.refreshTokens,
          undefined,
          AuthClientStore.getRefreshToken(),
        );

        AuthClientStore.setAccessToken(response.access_token);
        AuthClientStore.setRefreshToken(response.refresh_token);
      };

      executeLogic().then(debouncedResolve).catch(debouncedReject);

      debouncedPromise = null;
    }, 200);

    return debouncedPromise;
  };

  const sendAuthGuardedRequest = async (
    userIsNotAuthenticatedCallback: () => void,
    method: ApiMethod,
    path: string,
    // eslint-disable-next-line
    body?: any,
    init?: RequestInit,
  ) => {
    try {
      return await sendProtectedRequest(method, path, body, undefined, init);
    } catch (e) {
      if (e?.status === 401) {
        try {
          await refreshTokens();
        } catch (e) {
          userIsNotAuthenticatedCallback();
          throw e;
        }
        return await sendProtectedRequest(method, path, body, undefined, init);
      }

      throw e;
    }
  };

  const me = (userIsNotAuthenticatedCallback: () => void) => {
    return sendAuthGuardedRequest(
      userIsNotAuthenticatedCallback,
      ApiMethod.GET,
      routes.auth.me,
    ) as Promise<User>;
  };

  return { login, logout, me, sendAuthGuardedRequest };
};
```

이제 인증 제공자 구성 요소를 살펴보겠습니다. 전역 인증 상태를 처리합니다.


<div class="content-ad"></div>

이 구현에서는 인증 API 훅 메서드를 래핑하고 API 응답에 따라 isAuthenticated 상태를 관리합니다. 이 구성요소의 마지막 메서드는 매우 중요합니다: 다른 모든 훅이나 컴포넌트에서 API로 보호된 요청을 수행해야 할 때 사용됩니다. userIsNotAuthenticated 콜백을 통합하여 토큰 만료로 인한 엔드포인트 호출 오류가 발생할 때 인증 상태가 업데이트되도록 보장합니다. 이 접근 방식을 통해 isAuthenticated 상태가 false로 설정되어 앱 전체의 모든 컴포넌트가 적절하게 동작을 조정하도록 유도합니다.

다음으로, 인증 상태에 쉽게 액세스할 수 있도록 useAuthContext 훅을 정의하겠습니다:

```js
export const useAuthContext = () => {
  const ctx = useContext(AuthContext);

  if (!ctx) {
    throw new Error("useAuthContext must be within AuthProvider");
  }

  return ctx;
};
```

<div class="content-ad"></div>

응용 프로그램을 모든 구성 요소에 인증 상태를 제공하기 위해 AuthProvider로 래핑되어 있는지 확인해주세요.

```js
ReactDOM.createRoot(document.getElementById("root")!).render(
  <React.StrictMode>
    <AuthProvider>
      <App />
    </AuthProvider>
  </React.StrictMode>,
);
```

다음으로 사용자 작업과 관련된 API 메서드를 처리하는 useUserApi 훅을 정의합니다.

```js
export const useUserApi = () => {
  const { sendAuthGuardedRequest } = useAuthContext();

  const findAllUsers = async (
    limit: number,
    offset: number,
  ): Promise<FindAllUsersResponse> => {
    const queryString = buildQueryParams([
      { key: "limit", value: limit.toString() },
      { key: "offset", value: offset.toString() },
    ]);

    return sendAuthGuardedRequest(
      ApiMethod.GET,
      routes.user.findAll + queryString,
    );
  };

  const findOneUser = async (id: number): Promise<User> => {
    return sendAuthGuardedRequest(ApiMethod.GET, routes.user.findOne(id));
  };

  return { findAllUsers, findOneUser };
};
```

<div class="content-ad"></div>

지금은 인증 컨텍스트에서 sendAuthGuardedRequest 메소드를 사용하고 있어요.

이제 우리의 App 컴포넌트를 살펴보겠습니다.

```js
function App() {
  const { isAuthenticated, login, logout, me } = useAuthContext();
  const [appIsLoading, setAppIsLoading] = useState(true);

  const { findAllUsers } = useUserApi();

  useEffect(() => {
    me()
      .catch(() => {})
      .finally(() => setAppIsLoading(false));
  }, []);

  if (appIsLoading) {
    return <div>Loading...</div>;
  }

  if (!isAuthenticated) {
    return (
      <form
        style={{ display: "flex", flexDirection: "column", gap: 16 }}
        onSubmit={(e) => {
          e.preventDefault();
          login(e.target[0].value, e.target[1].value);
        }}
      >
        <div>Authentication</div>
        <input placeholder="Email" />
        <input placeholder="Password" type="password" />
        <button type="submit">Login</button>
      </form>
    );
  }

  return (
    <>
      <div>
        <a href="https://rabbitbyte.club" target="_blank">
          <img
            src={rabitByteClubLogo}
            className="logo"
            alt="logo-rabbit-byte"
          />
        </a>
      </div>
      <h1>Rabbit Byte Club</h1>
      <div style={{ display: "flex", gap: 16 }}>
        <button
          onClick={() => {
            for (let i = 0; i < 5; i++) {
              findAllUsers(10, 0);
            }
          }}
        >
          Simulate 5 concurrent requests
        </button>
        <button onClick={() => logout()}>Logout</button>
      </div>
    </>
  );
}

export default App;
```

우리는 /auth/me 엔드포인트가 완료되면 성공 여부에 관계없이 appLoading 상태를 false로 설정합니다. 사용자가 인증되지 않은 경우 로그인 폼을 표시합니다.

<div class="content-ad"></div>

사용자가 인증되었으면 데모에서 로직을 쉽게 테스트할 수 있도록 5개의 동시 요청을 시뮬레이션하는 버튼이 제공됩니다.

### 데모

전제 조건:

기능을 빠르고 쉽게 테스트하려면 백엔드 애플리케이션에서 JWT 액세스 토큰 만료를 10초로 설정하세요.

<div class="content-ad"></div>

다음은 마크다운(Markdown) 포맷을 사용하여 표를 표시한 코드입니다.

```js
JwtModule.registerAsync({
  inject: [ConfigService],
  useFactory: (configService: ConfigService<EnvironmentVariables>) => ({
    secret: configService.get('jwtSecret'),
    signOptions: { expiresIn: '10s' },
  }),
}),
```

JWT 리프레시 토큰 만료 시간을 30초로 설정하십시오:

```js
    const newRefreshToken = this.jwtService.sign(
      { sub: authUserId },
      {
        secret: this.configService.get('jwtRefreshSecret'),
        expiresIn: '30s',
      },
    );
```

더불어, 리프레시 토큰 엔드포인트의 쓰로틀링을 일시적으로 비활성화 하고 싶을 수도 있습니다:

<div class="content-ad"></div>

```js
// @Throttle({
//   short: { limit: 1, ttl: 1000 },
//   long: { limit: 2, ttl: 60000 },
// })
@ApiBearerAuth()
@Public()
@UseGuards(JwtRefreshAuthGuard)
@Post('refresh-tokens')
refreshTokens(@Request() req: ExpressRequest) {
  if (!req.user) {
    throw new InternalServerErrorException();
  }
  return this.authRefreshTokenService.generateTokenPair(
    (req.user as any).attributes,
    req.headers.authorization?.split(' ')[1],
    (req.user as any).refreshTokenExpiresAt,
  );
}
```

데모 실행 방법:

깃허브 프로젝트 클론:

```js
   git clone https://github.com/zenstok/react-auth-refresh-token-example
```

<div class="content-ad"></div>

의존성 설치:

```js
npm install
```

앱 실행:

```js
npm run dev
```

<div class="content-ad"></div>

백엔드가 실행 중인지 확인하세요:

백엔드 디렉토리로 이동한 후 다음을 실행하세요:

```js
   yarn dc up
```

필요한 경우 데이터베이스에 사용자 정보를 작성하세요.

<div class="content-ad"></div>

백엔드 디렉토리에서 새 터미널을 열고 다음을 실행해주세요:

```js
yarn dc-db-init
```

로그인:

로그인 화면에서 다음 자격 증명을 입력해주세요:

<div class="content-ad"></div>

- 이메일: admin@admin.com
- 비밀번호: 1234

![이미지](/assets/img/2024-07-30-Part23HowtoImplementRefreshTokensinReact_0.png)

테스트 기능:

로그인 후, "동시 5개 요청 시뮬레이션" 및 "로그아웃" 버튼이 표시됩니다.

<div class="content-ad"></div>


![Refresh Tokens Implementation Step 1](/assets/img/2024-07-30-Part23HowtoImplementRefreshTokensinReact_1.png)

- 브라우저의 Network 탭을 엽니다.
- 동시에 5개의 요청을 시뮬레이션하는 Simulate 버튼을 클릭합니다.

새로고침 토큰 로직의 효과를 확인할 수 있습니다.

![Refresh Tokens Implementation Step 2](/assets/img/2024-07-30-Part23HowtoImplementRefreshTokensinReact_2.png)


<div class="content-ad"></div>

앱은 리프레시 토큰 엔드포인트로의 단일 호출을 기다린 후 요청을 다시 실행할 것입니다. 성공!

목표의 확인:

- 지속적인 인증: 페이지를 새로고침하고 인증된 상태를 유지하는지 확인하세요. ✅
- 자동 로그아웃: 로그인한 후 30초 이상 기다리세요. "동시 5개 요청 시뮬레이트"를 누른 후 로그아웃된 것을 확인하세요. ✅

![이미지](/assets/img/2024-07-30-Part23HowtoImplementRefreshTokensinReact_3.png)

<div class="content-ad"></div>

- 데이터에 무결점한 액세스: 보호된 엔드포인트를 호출할 때, 앱은 유효한 리프레시 토큰이 존재하는 경우 데이터를 반환해야 합니다. ✅

### 결론

React에서 리프레시 토큰을 구현하는 과정에 도움이 되었기를 바랍니다. 이 시리즈의 최종 회차에서는 백엔드와 프론트엔드 논리를 HTTP-only 쿠키를 사용해 리프레시 토큰에 대한 전환을 진행할 것입니다.

Node.js 생태계에 관심 있는 주제를 더 다루길 원하시면, 댓글 섹션에 여러분의 제안을 자유롭게 남겨주세요. rabbitbyte.club 사이트를 통해 뉴스레터를 구독하여 업데이트를 받지 않도록 잊지 마세요!