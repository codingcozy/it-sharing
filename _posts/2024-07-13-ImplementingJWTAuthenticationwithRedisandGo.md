---
title: "Redis와 Go를 사용한 JWT 인증 구현 방법"
description: ""
coverImage: "/assets/img/2024-07-13-ImplementingJWTAuthenticationwithRedisandGo_0.png"
date: 2024-07-13 18:46
ogImage:
  url: /assets/img/2024-07-13-ImplementingJWTAuthenticationwithRedisandGo_0.png
tag: Tech
originalTitle: "Implementing JWT Authentication with Redis and Go"
link: "https://medium.com/devops-dev/implementing-jwt-authentication-with-redis-and-go-a12b48629dc9"
---

![2024-07-13-ImplementingJWTAuthenticationwithRedisandGo_0.png](/assets/img/2024-07-13-ImplementingJWTAuthenticationwithRedisandGo_0.png)

Redis는 캐시 및 메시지 브로커로 자주 사용되지만 주 데이터베이스로도 사용할 수 있는 인메모리 데이터 구조 저장소입니다.

Redis는 속도, 확장성, TTL(수명), 세션 저장소로 인해 JWT 인증 토큰에 적합합니다.

제가 사용한 방법을 보여드리기 위해 자체 리포지토리를 사용할 것이며, 동영상 형식을 따르고 싶다면 아래의 YouTube 동영상을 확인해 주세요.

<div class="content-ad"></div>

# 소개

제가 사용하는 JWT 인증 시스템에서는 PostgreSQL, Mongo 또는 Firebase와 같은 주요 데이터베이스를 사용하는 것이 합리적입니다.

아래 명령을 실행해주세요.

```js
go get github.com/redis/go-redis/v9
```

<div class="content-ad"></div>

한 번 설치하고 나서 Redis를 Docker 이미지나 Upstash와 같은 PaaS 제공자에서 실행할지 결정해보세요👇

Docker를 사용하여 Redis를 실행할 수 있습니다.

간단히 말하자면, 내 프론트엔드는 React이지만 백엔드는 Gin Web Framework를 사용하는 Go로 되어 있으며, 주요 데이터베이스는 Firebase이며 Redis는 프론트엔드에서 로그인했을 때 userID와 AuthToken을 저장할 것입니다.

# 아키텍처 및 코드

<div class="content-ad"></div>

구조는 구현에 따라 달라집니다. 이것이 저의 접근 방식입니다. 조직화된 방식으로 만들기 위해서 노력했습니다.

![이미지](/assets/img/2024-07-13-ImplementingJWTAuthenticationwithRedisandGo_1.png)

## 어플리케이션 구성

LoadConfigurations: Firebase 및 Redis에 연결하는 초기 구성을 설정합니다.

<div class="content-ad"></div>

```go
func (a *Application) LoadConfigurations() error {
    ctx := context.Background()

    fireClient, err := GetFirestoreClient(ctx)
    if err != nil {
        return err
    }
    a.FireClient = fireClient

    fireAuth, err := GetAuthClient(ctx)
    if err != nil {
        return err
    }
    a.FireAuth = fireAuth

    // Redis env variable depending if PaaS server provided if not 6379 port used.
    // So basically Docker image.
    a.RedisPort = envy.Get("REDIS_SERVER", "localhost:6379")

    redisClient, err := RedisConnect(a.RedisPort)
    if err != nil {
        return err
    }

    a.RedisClient = redisClient

    a.ListenPort = envy.Get("PORT", "8080")

    return nil
}
```

RedisConnect Function: Connect to Redis, handling both Docker and PaaS setups

```go
func redisClientPort(port string, envExists bool) (*redis.Client, error) {
    if envExists {
        opt, err := redis.ParseURL(port)
        if err != nil {
            return nil, fmt.Errorf("failed to parse Redis URL: %w", err)
        }
        return redis.NewClient(opt), nil
    }

    return redis.NewClient(&redis.Options{
        Addr:     port,
        Password: "",
        DB:       0,
    }), nil
}

func RedisConnect(port string) (*redis.Client, error) {
    _, ok := os.LookupEnv(port)
    client, err := redisClientPort(port, ok)
    if err != nil {
        return nil, fmt.Errorf("failed to ping Redis server: %w", err)
    }

    ping, err := client.Ping(context.Background()).Result()
    if err != nil {
        return nil, fmt.Errorf("failed to ping Redis server: %w", err)
    }

    fmt.Println("Ping response from Redis:", ping)
    return client, nil
}
```

Start Function: Initialize the Gin router and set up routes and middleware.

```js

```

<div class="content-ad"></div>

```js
func Start(a *app.Application) error {
    router := gin.New()

    router.Use(cors.New(md.CORSMiddleware()))

    api.SetCache(router, a.RedisClient)

    api.SetRoutes(router, a.FireClient, a.FireAuth, a.RedisClient)

    err := router.Run(":" + a.ListenPort)
    if err != nil {
       return err
    }

    return nil
}
```

SetCache 함수: 캐시 설정 및 Firebase에서 처리하는 요청에 대한 엔드포인트를 정의합니다.

```js
// api/controller.go
func SetCache(router *gin.Engine, client *redis.Client) {
    router.POST("/set-cache", func(c *gin.Context) {
       setUserCache(c, client)
    })

    router.GET("/check-expiration", func(c *gin.Context) {
       checkTokenExpiration(c, client)
    })

}

func SetRoutes(router *gin.Engine, client *firestore.Client, auth *auth.Client, redisClient *redis.Client) {
    router.OPTIONS("/*any", func(c *gin.Context) {
       c.Status(http.StatusOK)
    })

    // Gin에서 Use는 필수를 의미합니다
    router.Use(func(c *gin.Context) {
       authToken := getUserCache(c, redisClient)
       md.AuthJWT(auth, authToken)(c)
    })

    router.GET("/", func(c *gin.Context) {
       showRecepies(c, client)
    })

    router.POST("/", func(c *gin.Context) {
       addRecepie(c, client)
    })
```

SetRouters에서는 데이터베이스를 변경하기 위한 주요 요청을 처리합니다. AuthJWT는 Firebase에서 클라이언트 측에서 데이터베이스에 대한 요청을 인증하기 위해 설정된 것입니다.

<div class="content-ad"></div>

AuthToken은 우리가 이야기할 것이며, 사용자가 상호 작용을 허용하거나 거부하기 위해 AuthJWT에 전달되는 인증 토큰입니다.

다음 단계는 세션 관리를 위해 메인 GET, SET 작업을 설정하는 것입니다. TTL을 포함합니다.

```js
// models/cache.go
type UserCache struct {
    UserID    string `redis:"UserID"`
    AuthToken string `redis:"AuthToken"`
}
```

위 구조체는 React에서 들어오는 데이터를 구문 분석하기 위해 유일하게 중요합니다. ☝️

<div class="content-ad"></div>

## 캐시 작업 처리

사용자 캐시 가져오기: Redis에서 사용자 캐시를 검색합니다.

```js
// api/cache.go
func getUserCache(ctx *gin.Context, client *redis.Client) string {
    userID := ctx.Query("userID")
    authToken, err := models.GetUserCacheToken(ctx, client, userID)
    if err != nil {
       log.Printf("캐시 토큰을 불러오는 중 문제가 발생했습니다 %v", err)
       return ""
    }

    return authToken
}

// models/cache.go
func GetUserCacheToken(ctx *gin.Context, client *redis.Client, userID string) (string, error) {
 key := fmt.Sprintf("user:%s", userID)
 cache, err := client.HGetAll(ctx, key).Result()
 if err != nil {
  return "", fmt.Errorf("캐시 가져오기 실패: %v", err)
 }

 authToken, ok := cache["AuthToken"]
 if !ok {
  return "", fmt.Errorf("캐시에서 AuthToken을 찾을 수 없습니다")
 }

 return authToken, nil
}
```

사용자 캐시 설정: TTL이 있는 사용자 캐시를 Redis에 설정합니다.

<div class="content-ad"></div>

```js
// api/cache.go
func setUserCache(ctx *gin.Context, client *redis.Client) {
    var userCache models.UserCache

    err := models.UnmarshallRequestBodyToAPIData(ctx.Request.Body, &userCache)
    if err != nil {
       ctx.JSON(http.StatusBadRequest, gin.H{
          "error": "데이터를 구문 분석할 수 없습니다",
       })
       return
    }

    key := fmt.Sprintf("user:%s", userCache.UserID)
    _, notExists := client.HGetAll(ctx, key).Result()

    if notExists == nil {
       userCache.SetCachedToken(ctx, client, key)
       return
    }

}

// models/cache.go

func (c *UserCache) SetCachedToken(ctx *gin.Context, client *redis.Client, key string) {
 fields := map[string]interface{}{
  "UserID":    c.UserID,
  "AuthToken": c.AuthToken,
 }
 err := client.HSet(ctx, key, fields).Err()
 if err != nil {
  log.Printf("캐시 토큰 설정 중 문제 발생 %v", err)
 }

 client.Expire(ctx, key, 7*24*time.Hour)

}
```

![이미지](/assets/img/2024-07-13-ImplementingJWTAuthenticationwithRedisandGo_2.png)

## 토큰 만료 확인

토큰 만료 확인: 토큰이 만료되었는지 확인합니다.

<div class="content-ad"></div>

```js
// api/cache.go
func checkTokenExpiration(ctx *gin.Context, client *redis.Client) {
    userID := ctx.Query("userID")
    key := fmt.Sprintf("user:%s", userID)

    ttl, err := client.TTL(ctx, key).Result()
    if err != nil {
       ctx.JSON(http.StatusInternalServerError, gin.H{
          "error": "Failed to get TTL",
       })
       return
    }

    expired := ttl <= 0

    ctx.JSON(http.StatusOK, expired)
}
```

## 결론

이 설정은 Go 어플리케이션에서 Redis를 이용한 JWT 인증을 효율적으로 관리하고 세션을 관리하며 토큰을 유효성 검증하는 강력한 구조를 제공합니다. 질문이 있으시면 자유롭게 물어보세요(GPT에게 물으면 됩니다). 아래에서 제 저장소를 찾을 수 있습니다.

# 관련 이야기들
