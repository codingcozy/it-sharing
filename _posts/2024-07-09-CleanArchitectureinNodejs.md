---
title: "Nodejs에서 클린 아키텍처 구현하는 방법"
description: ""
coverImage: "/assets/img/2024-07-09-CleanArchitectureinNodejs_0.png"
date: 2024-07-09 16:50
ogImage:
  url: /assets/img/2024-07-09-CleanArchitectureinNodejs_0.png
tag: Tech
originalTitle: "Clean Architecture in Node.js"
link: "https://medium.com/@ben.dev.io/clean-architecture-in-node-js-39c3358d46f3"
---

![Clean Architecture in Node.js](/assets/img/2024-07-09-CleanArchitectureinNodejs_0.png)

# 소개

Clean Architecture는 모듈화되고 유지보수가 쉬운 코드를 작성하는 데 중점을 둔 소프트웨어 개발 개념입니다. 이 아키텍처를 사용하면 개발자가 이해하기 쉽고 테스트하고 확장하기 쉬운 코드베이스를 만들 수 있습니다. 이 아키텍처는 응용 프로그램의 핵심 비즈니스 로직을 인프라 세부 정보와 분리하는 데 중점을 두고 있습니다. 이 문서에서는 Node.js에서 Clean Architecture에 대해 논의하고 코드 샘플 및 해당 개념을 적용하는 방법을 탐구할 것입니다.

![Clean Architecture in Node.js](/assets/img/2024-07-09-CleanArchitectureinNodejs_1.png)

<div class="content-ad"></div>

# 문제 파악하고 해결하기

Node.js 애플리케이션을 개발할 때 코드 중복, 코드베이스의 조직화되지 않은 상태, 그리고 강하게 결합된 문제 등 다양한 문제에 직면할 수 있습니다. 다음 섹션에서는 데이터베이스에서 데이터를 가져와 JSON 응답으로 반환하는 Node.js 애플리케이션의 간단한 예제를 사용하여 이러한 문제를 해결합니다. Clean Architecture는 다음 원칙을 강조하여 이러한 문제에 대한 해결책을 제공합니다:

## 관심사의 분리 (SoC)

소프트웨어 엔지니어링의 기본 원리로, 시스템을 구별된 모듈이나 구성 요소로 나누고 각각이 특정하고 독립적인 책임을 부여하는 중요성을 강조합니다. Node.js에서는 SoC를 모듈 아키텍처를 사용하여 자주 구현하는데, 각 모듈이 단일 작업이나 기능에 대해 책임을 지는 방식입니다.

<div class="content-ad"></div>

![Clean Architecture in Node.js](/assets/img/2024-07-09-CleanArchitectureinNodejs_2.png)

여기에 우리가 어떻게 관심사 분리를 이 코드에 적용할 수 있는지 예제가 있어요:

```js
// index.js - 나쁜 코드!!!!

const express = require("express");
const app = express();
const database = require("./database");

app.get("/users", async (req, res) => {
  try {
    const users = await database.getUsers();
    res.json(users);
  } catch (error) {
    res.status(500).send("내부 서버 오류");
  }
});

app.listen(3000, () => {
  console.log("서버가 3000번 포트에서 듣고 있어요");
});
```

이 코드에서 라우팅, 데이터 검색 및 오류 처리와 같은 관심사를 하나의 파일에 섞어 놓았어요. 이렇게 하면 코드가 어려워지며 애플리케이션이 성장할수록 유지 보수와 확장이 어려워질 수 있어요.

<div class="content-ad"></div>

여기에 우리가이 코드에 관심을 분리하는 방법을 적용할 수 있습니다:

```js
// index.js

const express = require("express");
const app = express();
const usersRouter = require("./routes/users");

app.use("/users", usersRouter);

app.listen(3000, () => {
  console.log("서버가 포트 3000에서 청취 중입니다");
});
```

```js
// routes/users.js

const express = require("express");
const router = express.Router();
const usersController = require("../controllers/users");

router.get("/", usersController.getUsers);

module.exports = router;
```

```js
// controllers/users.js

const database = require("../database");

async function getUsers(req, res) {
  try {
    const users = await database.getUsers();
    res.json(users);
  } catch (error) {
    res.status(500).send("내부 서버 오류");
  }
}

module.exports = {
  getUsers,
};
```

<div class="content-ad"></div>

이 리팩토링 된 코드에서는 라우팅, 컨트롤러 및 데이터 검색과 같은 역할을 세 개의 별도 파일로 분리했습니다. index.js 파일은 Express 앱을 생성하고 라우트를 등록하는 역할을 맡고, routes/users.js 파일은 /users 라우트를 처리하고 요청을 usersController에 위임합니다. 또한 controllers/users.js 파일은 데이터베이스에서 데이터를 검색하고 응답을 클라이언트에 반환하는 역할을 합니다.

이러한 역할을 분리함으로써 코드의 모듈성과 유지보수성을 향상시킬 수 있습니다. 예를 들어, 더 많은 라우트를 추가하고 싶다면 routes 디렉토리에 새 파일을 만들고 index.js 파일에서 등록하면 기존 코드를 수정하지 않아도 됩니다. 마찬가지로, 데이터를 데이터베이스에서 검색하는 방식을 변경하고 싶다면 route나 controller 코드를 수정하지 않고 database.js 파일을 수정할 수 있습니다.

## 의존성 주입 (DI)

Clean Architecture에서는 의존성이 주입되며, 이는 상위 수준 모듈이 하위 수준 모듈에 의존해야 함을 의미합니다. 이를 통해 비즈니스 로직이 인프라 구성 세부 정보와 결합되지 않도록 보장합니다.

<div class="content-ad"></div>

아래는 해당 코드에 의존성 주입이 어떻게 적용될 수 있는지의 예시입니다:

```js
// index.js - 나쁜 코드 !!!

const express = require("express");
const app = express();

const usersController = require("./controllers/users");
app.get("/users", async (req, res) => {
  try {
    const users = await usersController.getUsers();
    res.json(users);
  } catch (error) {
    res.status(500).send("내부 서버 오류");
  }
});

app.listen(3000, () => {
  console.log("서버가 3000번 포트에서 수신 대기 중입니다");
});
```

```js
// controllers/users.js

const database = require("../database");
async function getUsers() {
  try {
    const users = await database.getUsers();
    return users;
  } catch (error) {
    throw new Error("사용자를 가져오는 중 에러가 발생했습니다");
  }
}

module.exports = {
  getUsers,
};
```

<div class="content-ad"></div>

이 코드에서 index.js 파일은 usersController 모듈에 직접 의존하고, usersController 모듈은 다시 database 모듈에 직접 의존합니다. 이는 의존성 주입 원칙을 위반하는 것입니다. 왜냐하면 고수준 모듈은 저수준 모듈에 의존해서는 안 되기 때문입니다.

이 코드에 의존성 주입을 적용하는 방법은 다음과 같습니다:

```js
// index.js

const express = require("express");
const app = express();
const usersController = require("./controllers/users");
const database = require("./database");

app.get("/users", async (req, res) => {
  try {
    const users = await usersController.getUsers(database);
    res.json(users);
  } catch (error) {
    res.status(500).send("Internal Server Error");
  }
});

app.listen(3000, () => {
  console.log("Server is listening on port 3000");
});
```

```js
// controllers/users.js

async function getUsers(database) {
  try {
    const users = await database.getUsers();
    return users;
  } catch (error) {
    throw new Error("Error retrieving users");
  }
}

module.exports = {
  getUsers,
};
```

<div class="content-ad"></div>

이 리팩터링된 코드에서는 Dependency Injection을 적용하여 usersController 모듈의 getUsers 함수에 데이터베이스 모듈을 매개변수로 전달했습니다. 이렇게 함으로써 고수준 index.js 모듈이 저수준 데이터베이스 모듈에 직접 의존하지 않고 두 모듈 모두 getUsers 함수의 추상화에 의존하게 됩니다.

의존성 주입을 적용함으로써 코드의 모듈성, 유연성, 그리고 테스트 용이성을 높일 수 있습니다. 예를 들어, 다른 데이터베이스 구현체로 전환하고 싶을 때는 getUsers 함수를 구현하는 새 모듈을 생성하여 usersController 모듈에서 getUsers 함수의 매개변수로 전달하면서 기존 코드를 수정하지 않을 수 있습니다. 마찬가지로 단위 테스트 목적으로 데이터베이스 모듈을 쉽게 모의(mock)할 수도 있습니다.

## 단일 책임 원칙(SRP)

각 모듈 또는 함수는 단일 책임을 가져야 합니다. 이는 이해하기 쉽고 유지보수하기 쉬운 코드베이스를 만드는 데 도움이 됩니다.

<div class="content-ad"></div>

<img src="/assets/img/2024-07-09-CleanArchitectureinNodejs_4.png" />

Node.js에서 SRP 원칙을 보여주는 예제가 있습니다.

```js
// user.js - 나쁜 코드 !!!

class User {
  constructor(name, email, password) {
    this.name = name;
    this.email = email;
    this.password = password;
  }

  saveToDatabase() {
    await database.saveUser(user)
    .then(() => {
      res.status(200).send()
    })
    .catch(err => {
      res.status(400).send('이메일 오류')
    });
  }

  sendWelcomeEmail() {
    await mailService.send(user, '환영합니다')
    .then(() => {
      res.status(200).send()
    })
    .catch(err => {
        res.status(400).send('이메일 오류')
    });
  }
}
```

이 예제에서 User 클래스는 두 가지 책임을 가지고 있습니다:

<div class="content-ad"></div>

- 사용자 데이터를 데이터베이스에 저장
- 사용자에게 환영 이메일을 보냄

이것은 SRP 원칙을 위반합니다. 사용자 클래스가 변경해야 할 이유가 하나 이상 있는 경우입니다. 예를 들어, 우리가 사용자 데이터를 데이터베이스에 저장하는 방식을 변경하기로 결정하면, 환영 이메일을 보내는 것과는 관련이 없어도 사용자 클래스를 수정해야 합니다.

SRP 원칙을 따르기 위해 이러한 책임을 별도의 클래스로 분리할 수 있습니다:

```js
class User {
  constructor(name, email, password) {
    this.name = name;
    this.email = email;
    this.password = password;
  }
}

class UserRepository {
  async saveToDatabase(user) {
    await database.saveUser(user)
    .then(() => {
      res.status(200).send()
    })
    .catch(err => {
      res.status(400).send('이메일 오류')
    });
  }
}

class EmailService {
  async sendWelcomeEmail(user) {
      await mailService.send(user, '환영합니다')
      .then(() => {
        res.status(200).send()
      })
      .catch(err => {
        res.status(400).send('이메일 오류')
      });
    }
  }
}
```

<div class="content-ad"></div>

지금은 하나의 책임을 지는 세 가지 클래스가 있습니다:

- 사용자 클래스: 사용자를 표현하는 데 책임이 있습니다.
- UserRepository 클래스: 사용자 데이터를 데이터베이스에 저장하는 데 책임이 있습니다.
- EmailService 클래스: 사용자에게 환영 이메일을 보내는 데 책임이 있습니다.

책임을 분리함으로써 코드를 더 모듈화하고 유지보수하기 쉽게 만들었습니다. 데이터베이스에 사용자 데이터를 저장하는 방식을 변경해야 하는 경우 UserRepository 클래스만 수정하면 됩니다. User 클래스와 EmailService 클래스는 변경되지 않습니다.

# 개념 및 세부 설계

<div class="content-ad"></div>

Node.js에서의 Clean Architecture는 Robert C. Martin (별명 Uncle Bob)이 소개한 Clean Architecture 원칙을 기반으로 합니다. 이는 관심사의 분리, 의존성 해제 및 모듈화를 강조합니다. 목표는 이해하기 쉽고 테스트하며 유지보수하기 쉬운 코드베이스를 만드는 것입니다.

Clean Architecture의 주요 아이디어는 애플리케이션을 서로 다른 계층으로 나누고 각 계층에 특정한 책임을 부여하는 것입니다. 계층은 서로 잘 정의된 인터페이스를 통해 통신합니다. 이를 통해 애플리케이션의 쉬운 수정 및 테스트가 가능하며 코드베이스의 다른 부분에 영향을 주지 않습니다.

![](/assets/img/2024-07-09-CleanArchitectureinNodejs_5.png)

Node.js는 웹 애플리케이션을 개발하기 위한 인기 있는 런타임 환경입니다. Clean Architecture를 구현하는 데 사용할 수 있는 다양한 라이브러리와 프레임워크의 생태계가 있습니다. Node.js에서의 Clean Architecture의 주요 개념은 다음과 같습니다:

<div class="content-ad"></div>

## 인프라스트럭처 레이어

인프라스트럭처 레이어는 데이터베이스, API 또는 파일 시스템과 같은 외부 의존성을 처리하는 역할을 합니다. 도메인 레이어와 독립적이어야 하며 쉬운 테스트와 수정을 위해 분리되어야 합니다. 인프라스트럭처 레이어는 도메인 레이어에서 정의된 인터페이스를 구현해야 합니다.

Node.js에서는 인프라스트럭처 레이어를 패키지 또는 모듈을 사용하여 구현할 수 있습니다. 예를 들어, 인기 있는 Knex.js 패키지를 사용하여 데이터베이스 쿼리를 처리할 수 있습니다. 인프라스트럭처 레이어는 외부 의존성을 쉽게 대체할 수 있도록 설계되어야 합니다.

다음은 데이터베이스 어댑터를 구현하는 인프라스트럭처 모듈의 예시입니다:

<div class="content-ad"></div>

```js
const knex = require("knex");

class UserDatabase {
  constructor(config) {
    this.db = knex(config);
  }

  async getById(id) {
    const data = await this.db("users").where({ id }).first();
    return data ? User.create(data) : null;
  }

  async save(user) {
    const data = User.toData(user);
    const { id } = user;

    if (id) {
      await this.db("users").where({ id }).update(data);
    } else {
      const [newId] = await this.db("users").insert(data);
      user.id = newId;
    }
  }
}
```

이 모듈은 Knex.js 라이브러리를 사용하여 사용자를 ID로 가져오고 사용자를 데이터베이스에 저장하는 메서드를 제공합니다.

## 프레젠테이션 레이어

프레젠테이션 레이어는 응용 프로그램의 출력을 사용자에게 표시하고 사용자 입력을 처리하는 역할을 담당합니다. 응용 프로그램 레이어 및 인프라스트럭처 레이어와 독립적이어야 합니다. 프레젠테이션 레이어는 Express.js 또는 Hapi.js와 같은 웹 프레임워크를 사용하여 구현할 수 있습니다.

<div class="content-ad"></div>

Node.js에서 표현 계층은 웹 프레임워크나 모듈을 사용하여 구현할 수 있습니다. 웹 프레임워크는 표현 계층을 구현하고 모든 로직을 함께 호스팅하는 강력하고 유연한 방법을 제공합니다.

다음은 Express.js 웹 프레임워크를 사용하여 REST API를 구현하는 표현 모듈의 예시입니다:

```js
const express = require("express");
const bodyParser = require("body-parser");
const UserService = require("./services/user-service");
const UserDatabase = require("./infra/user-database");

const app = express();

app.use(bodyParser.json());

const userDatabase = new UserDatabase(config);
const userService = new UserService(userDatabase);

app.get("/users/:id", async (req, res) => {
  const { id } = req.params;
  try {
    const user = await userService.getUserById(id);
    res.json(user);
  } catch (error) {
    res.status(404).json({ error: error.message });
  }
});

app.put("/users/:id", async (req, res) => {
  const { id } = req.params;
  const { userData } = req.body;
  try {
    let user = await userService.getUserById(id);
    user = User.create({ ...user, ...userData });
    await userService.saveUser(user);
    res.json(user);
  } catch (error) {
    res.status(404).json({ error: error.message });
  }
});
```

이 모듈은 Express.js 애플리케이션을 생성하고 사용자 서비스 및 데이터베이스를 설정하며, 사용자 데이터를 가져오고 업데이트하는 REST API 엔드포인트를 제공합니다.

<div class="content-ad"></div>

## 응용 계층

응용 계층은 도메인 계층과 인프라 계층 간의 상호 작용을 조정하는 역할을 합니다. 응용 계층에는 응용 프로그램의 사용 사례가 포함되어 있으며, 사용자와 시스템 간의 상호 작용을 나타냅니다. 응용 계층은 도메인 계층과 인프라 계층과 분리되어야 합니다.

Node.js에서는 클래스 또는 모듈을 사용하여 응용 계층을 구현할 수 있습니다. 클래스는 관심사의 명확한 분리와 캡슐화를 제공합니다. 모듈은 응용 계층을 구현하는 간단한 접근 방식을 제공합니다.

다음은 사용자 서비스를 나타내는 응용 프로그램 클래스의 예시입니다:

<div class="content-ad"></div>

```js
class UserService {
  constructor(userDatabase) {
    this.userDatabase = userDatabase;
  }

  async getUserById(id) {
    const user = await this.userDatabase.getById(id);
    if (!user) {
      throw new Error(`ID가 ${id}인 사용자를 찾을 수 없습니다.`);
    }
    return user;
  }

  async saveUser(user) {
    await this.userDatabase.save(user);
  }
}
```

이 클래스는 UserDatabase 인프라 모듈을 사용하여 ID로 사용자를 가져오고 사용자를 저장하는 메서드를 제공합니다.

## 도메인 (Enterprise) 레이어

도메인 레이어는 애플리케이션의 핵심이며 비즈니스 로직과 규칙을 포함합니다. 외부 라이브러리나 프레임워크와 독립적이어야 하며 테스트하거나 수정하기 쉬워야 합니다. 도메인 레이어는 애플리케이션의 가장 중요한 부분이며 변경 사항을 신중히 처리해야 합니다.

<div class="content-ad"></div>

Node.js에서 도메인 계층은 클래스 또는 모듈을 사용하여 구현할 수 있습니다. 클래스는 관심사의 명확한 분리, 캡슐화 및 재사용성을 제공합니다. 반면에 모듈은 도메인 계층을 구현하는 더 간단한 방법을 제공합니다.

다음은 사용자 엔티티를 나타내는 도메인 클래스의 예시입니다:

```js
class User {
  constructor(id, name, email) {
    this.id = id;
    this.name = name;
    this.email = email;
  }

  changeName(name) {
    this.name = name;
  }

  changeEmail(email) {
    this.email = email;
  }

  static create(data) {
    const { id, name, email } = data;
    return new User(id, name, email);
  }

  static toData(user) {
    const { id, name, email } = user;
    return { id, name, email };
  }
}
```

이 클래스는 사용자 데이터를 캡슐화하고 사용자의 이름과 이메일을 변경하는 메서드를 제공합니다.

<div class="content-ad"></div>

# 써드 파티 패키지 사용하기

써드 파티 패키지를 사용하는 것은 코드를 깔끔하게 관리하는 훌륭한 방법입니다. 여러 가지 패키지를 활용하여 코드베이스를 구성하고 모듈화하는 데 도움을 줄 수 있습니다. 예를 들어, Express.js와 같은 패키지를 사용하여 라우팅 및 미들웨어 처리를 할 수 있습니다. 또한 Knex.js와 같은 패키지를 사용하여 데이터베이스 쿼리를 처리할 수도 있습니다. 이러한 패키지를 사용하면 코드 중복을 줄이고 코드베이스를 더 체계적으로 관리할 수 있습니다.

다음은 써드 파티 패키지를 활용하는 몇 가지 예시입니다:

## Lodash

<div class="content-ad"></div>

로다시는 배열, 객체 및 기타 데이터 구조를 다루는 다양한 함수를 제공하는 유틸리티 라이브러리입니다. Lodash를 사용하면 필터링, 정렬, 데이터 변환과 같은 작업을 위해 반복적인 코드를 작성하는 것을 피할 수 있습니다.

![이미지](/assets/img/2024-07-09-CleanArchitectureinNodejs_6.png)

예를 들어, 나이로 사용자 배열을 필터링하는 다음 코드를 살펴보겠습니다:

```js
const users = [
  { name: "Alice", age: 25 },
  { name: "Bob", age: 30 },
  { name: "Charlie", age: 35 },
];

const filteredUsers = users.filter((user) => user.age >= 30);
```

<div class="content-ad"></div>

로다시 사용하여 다음 코드를 작성할 수 있습니다:

```js
const _ = require("lodash");

const users = [
  { name: "Alice", age: 25 },
  { name: "Bob", age: 30 },
  { name: "Charlie", age: 35 },
];

const filteredUsers = _.filter(users, (user) => user.age >= 30);
```

이 코드는 더 간결하고 읽기 쉽습니다. Lodash의 filter 함수를 사용하면 코드가 무엇을 하는지 명확해집니다.

## Moment.js

<div class="content-ad"></div>

Moment.js는 날짜와 시간을 파싱, 조작 및 포맷팅하기 위한 라이브러리입니다. Moment.js를 사용하면 복잡한 날짜 조작 코드를 작성하지 않고 오류 가능성을 줄일 수 있습니다.

![image](/assets/img/2024-07-09-CleanArchitectureinNodejs_7.png)

예를 들어, 날짜를 문자열로 포맷하는 다음 코드를 살펴보세요:

```js
const date = new Date();
const formattedDate = `${date.getFullYear()}-${date.getMonth() + 1}-${date.getDate()}`;
```

<div class="content-ad"></div>

Moment.js를 사용하면 다음과 같이 코드를 작성할 수 있습니다:

```js
const moment = require("moment");
const date = new Date();
const formattedDate = moment(date).format("YYYY-MM-DD");
```

이 코드는 더 읽기 쉽고 유지 관리하기 쉽습니다. Moment.js의 format 함수를 사용하면 코드가 무엇을 하는지 명확해집니다.

## Winston

<div class="content-ad"></div>

Winston은 다양한 로깅 기능을 제공하는 로깅 라이브러리입니다. 이는 다양한 로그 수준, 로그 파일 로테이션 및 사용자 정의 포맷팅을 포함합니다. Winston을 사용하면 사용자 정의 로깅 코드를 작성하지 않고도 로그를 일관적이고 쉽게 읽을 수 있게 할 수 있습니다.

![이미지](/assets/img/2024-07-09-CleanArchitectureinNodejs_8.png)

예를 들어, 다음과 같이 메시지를 로깅하는 코드를 고려해보세요:

```js
console.log(`[${new Date().toISOString()}] INFO: User logged in`);
```

<div class="content-ad"></div>

위스톤(Winston)을 사용하면 다음과 같이 코드를 작성할 수 있습니다.

```js
const winston = require("winston");

const logger = winston.createLogger({
  level: "info",
  format: winston.format.combine(
    winston.format.timestamp(),
    winston.format.printf((info) => `[${info.timestamp}] ${info.level}: ${info.message}`)
  ),
  transports: [new winston.transports.Console(), new winston.transports.File({ filename: "app.log" })],
});
logger.info("사용자가 로그인했습니다.");
```

이 코드는 더 구성 가능하고 읽기 쉽습니다. 또한 Winston을 사용하면 코드가 하는 일이 명확해집니다.

# 결론

<div class="content-ad"></div>

깨끗한 아키텍처는 Node.js 프로젝트에서 모듈화되고 유지보수 가능한 코드를 작성하는 데 도움이 되는 강력한 개념입니다. Clean Architecture의 원칙을 따르면 이해하기 쉽고 테스트하고 확장하기 쉬운 코드베이스를 만들 수 있습니다. 또한 다양한 타사 패키지와 다른 폴더 구조를 사용하여 코드의 가독성을 높일 수도 있습니다. Node.js 프로젝트에서 Clean Architecture를 구현하면 코드 중복, 구성되지 않은 코드베이스, 밀접한 결합 등의 일반적인 문제를 피할 수 있습니다.

![깨끗한 아키텍처](/assets/img/2024-07-09-CleanArchitectureinNodejs_9.png)

# ben.dev.io에서 방문해주세요.
