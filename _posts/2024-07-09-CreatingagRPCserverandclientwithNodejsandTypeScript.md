---
title: "Nodejs 및 TypeScript로 gRPC 서버와 클라이언트 만드는 방법"
description: ""
coverImage: "/assets/img/2024-07-09-CreatingagRPCserverandclientwithNodejsandTypeScript_0.png"
date: 2024-07-09 17:40
ogImage:
  url: /assets/img/2024-07-09-CreatingagRPCserverandclientwithNodejsandTypeScript_0.png
tag: Tech
originalTitle: "Creating a gRPC server and client with Node.js and TypeScript"
link: "https://medium.com/nerd-for-tech/creating-a-grpc-server-and-client-with-node-js-and-typescript-bb804829fada"
---

gRPC는 원격 프로시저 호출(RPC) API를 구축하기 위한 고성능 오픈 소스 프레임워크입니다. Protocol Buffer IDL 파일을 이용하여 서비스를 정의하고 여러 언어로 서버 및 클라이언트 코드를 생성할 수 있습니다. 이를 통해 서로 다른 언어로 작성된 클라이언트에서 사용할 수 있는 서비스를 쉽게 생성할 수 있습니다. Node.js를 포함한 다양한 언어로 클라이언트를 작성할 수 있습니다.

![image](/assets/img/2024-07-09-CreatingagRPCserverandclientwithNodejsandTypeScript_0.png)

본 튜토리얼에서는 Node.js와 TypeScript를 사용하여 간단한 gRPC 서비스를 생성합니다. 이 서비스에는 다음 네 가지 메소드가 포함됩니다:

- createAddress: 사용자 주소(hex) 문자열을 반환합니다.
- transaction: toAddress, points 및 metadata를 입력으로 받아 트랜잭션 ID를 반환합니다.
- balance: 주소를 입력으로 받아 총 잔고 및 이용 가능한 잔고를 반환합니다.
- walletInfo: 주소를 입력으로 받아 총 잔액, 이용 가능한 잔액 및 트랜잭션 목록을 반환합니다.

<div class="content-ad"></div>

# 서비스 정의

gRPC 서비스를 생성하는 첫 번째 단계는 Protocol Buffer IDL 파일을 사용하여 서비스를 정의하는 것입니다. 아래는 우리 서비스를 위한 IDL 파일이 어떻게 보일 수 있는지 예시입니다:

```js
syntax = "proto3";

service Wallet {
    rpc createAddress (CreateAddressRequest) returns (CreateAddressResponse);
    rpc transaction (TransactionRequest) returns (TransactionResponse);
    rpc balance (BalanceRequest) returns (BalanceResponse);
    rpc walletInfo (WalletInfoRequest) returns (WalletInfoResponse);
}

message CreateAddressRequest { }

message CreateAddressResponse {
    string address = 1;
}

message TransactionRequest {
    string toAddress = 1;
    int32 points = 2;
    string metadata = 3;
}

message TransactionResponse {
    int64 transaction_id = 1;
}

message BalanceRequest {
    string address = 1;
}

message BalanceResponse {
    int64 total = 1;
    int64 available = 2;
}

message WalletInfoRequest {
    string address = 1;
}

message WalletInfoResponse {
    int64 total = 1;
    int64 available = 2;
    repeated Transaction transactions = 3;
}

message Transaction {
    string to_address = 1;
    int32 points = 2;
    string metadata = 3;
}
```

# 코드 생성

<div class="content-ad"></div>

IDL 파일을 정의한 후에는 protoc 컴파일러를 사용하여 Node.js와 TypeScript에서 서비스용 gRPC 코드를 생성할 수 있습니다. 또한 생성된 코드와 함께 사용하려면 @grpc/proto-loader와 grpc npm 패키지를 설치해야 합니다.

protoc 컴파일러를 실행하는 방법의 예시는 다음과 같습니다:

```js
protoc --js_out=import_style=commonjs,binary:./ --grpc_out=./ --plugin=protoc-gen-grpc=`which grpc_tools_node_protoc_plugin` wallet.proto
```

이렇게 하면 다음 파일이 생성됩니다:

<div class="content-ad"></div>

- wallet_grpc_pb.js 및 wallet_grpc_pb.d.ts: gRPC 클라이언트 및 서버 코드입니다.
- wallet_pb.js 및 wallet_pb.d.ts: 요청 및 응답 메시지 코드입니다.

# 서비스 구현

이제 생성된 코드를 사용하여 서비스를 구현할 수 있습니다.

```js
async function walletInfo(call: ServerUnaryCall<string>, callback: sendUnaryData<WalletInfoResponse>) {
  const address = call.request;
  try {
    const result = {
      total: 100,
      available: 80,
    };
    const transactions = [
      {
        toAddress: "0x1234567890",
        value: 10,
        timestamp: Date.now(),
        metadata: {},
      },
      {
        toAddress: "0x0987654321",
        value: 20,
        timestamp: Date.now(),
        metadata: {},
      },
    ];
    callback(null, {
      total: result.total,
      available: result.available,
      transactions: transactions,
    });
  } catch (err) {
    callback(err, null);
  }
}
```

<div class="content-ad"></div>

```js
import { BalanceResponse, BalanceRequest } from "./wallet_pb";

function balance(call: ServerUnaryCall<BalanceRequest>, callback: sendUnaryData<BalanceResponse>) {
  // 필요한 비즈니스 로직 수행
  const total = 100;
  const available = 50;
  const balanceResponse = new BalanceResponse();
  balanceResponse.setTotal(total);
  balanceResponse.setAvailable(available);
  callback(null, balanceResponse);
}
```

```js
import { TransactionResponse, TransactionRequest } from "./wallet_pb";

function transaction(call: ServerUnaryCall<TransactionRequest>, callback: sendUnaryData<TransactionResponse>) {
  // 필요한 비즈니스 로직 수행
  const transactionId = Date.now();
  const transactionResponse = new TransactionResponse();
  transactionResponse.setTransactionId(transactionId);
  callback(null, transactionResponse);
}
```

```js
import { CreateAddressResponse, CreateAddressRequest } from "./wallet_pb";

function createAddress(call: ServerUnaryCall<CreateAddressRequest>, callback: sendUnaryData<CreateAddressResponse>) {
  // 필요한 비즈니스 로직 수행
  const address = "0x1234567890abcdef";
  const createAddressResponse = new CreateAddressResponse();
  createAddressResponse.setAddress(address);
  callback(null, createAddressResponse);
}
```

마지막으로, gRPC 서버를 설정하고 시작하는 방법의 예시입니다:

<div class="content-ad"></div>

```js
import * as grpc from "grpc";
import { WalletService } from "./wallet_grpc_pb";

const server = new grpc.Server();
server.addService(WalletService.service, {
  createAddress,
  transaction,
  balance,
  walletInfo,
});
server.bind("0.0.0.0:50051", grpc.ServerCredentials.createInsecure());
server.start();
```

로컬 머신의 포트 50051에서 gRPC 서버를 시작할 것입니다. 이 서버에는 이전에 정의한 네 가지 서비스 핸들러가 포함되어 있으며 보안되지 않은(평문) 연결을 사용할 것입니다.

# 클라이언트 생성

이제 gRPC 서버가 준비되었으니, 이에 연결할 클라이언트를 만들 수 있습니다.

<div class="content-ad"></div>

다음은 클라이언트를 작성하고 walletInfo 서비스를 호출하는 방법의 예시입니다.

```js
import { WalletServiceClient } from "./wallet_grpc_pb";
import { WalletInfoRequest, WalletInfoResponse } from "./wallet_pb";

const client = new WalletServiceClient("localhost:50051", grpc.credentials.createInsecure());
const request = new WalletInfoRequest();
request.setAddress("0x1234567890abcdef");
client.walletInfo(request, (error, response) => {
  if (error) {
    console.error(error);
  } else {
    console.log(response.getTotal());
    console.log(response.getAvailable());
    console.log(response.getTransactionsList());
  }
});
```

이 예제는 이전에 설정한 서버에 연결하고 지정된 주소로 walletInfo 서비스를 호출하는 gRPC 클라이언트를 생성합니다. 클라이언트는 보안되지 않은 (평문) 연결을 사용할 것입니다.

# Persistence

<div class="content-ad"></div>

거래를 저장하려면 데이터베이스가 필요합니다. 추가 전용 거래 저장소로서의 일반적인 선택은 블록체인과 같은 분산 원장 기술 (DLT) 데이터베이스입니다. 그러나 이 예제에서는 간단하게 유지하기 위해 Postgres와 같은 관계형 데이터베이스를 사용할 것입니다.

Postgres를 설치하고 거래를 저장할 새 데이터베이스와 테이블을 생성해야 합니다. 테이블은 다음과 같은 열을 가져야 합니다:

- from: 송신자 주소를 나타내는 문자열
- to: 수신자 주소를 나타내는 문자열
- value: 전송된 포인트 수를 나타내는 정수
- timestamp: 거래 시간을 나타내는 타임스탬프
- metadata: 거래에 대한 추가 정보를 나타내는 JSON 객체

Node.js 애플리케이션에서 데이터베이스와 상호 작용하기 위해 TypeORM과 같은 ORM (객체 관계 매핑)을 사용할 수 있습니다. TypeORM은 TypeScript를 위한 인기 있는 ORM입니다. 객체 지향적인 방식으로 데이터베이스와 상호 작용할 수 있는 방법을 제공합니다.

<div class="content-ad"></div>

다음은 TypeORM을 사용하여 트랜잭션을 생성하고 데이터베이스에 저장하는 방법을 예시로 설명해 드리겠습니다:

```js
import { Entity, PrimaryGeneratedColumn, Column } from "typeorm";

@Entity()
class Transaction {
  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  from: string;

  @Column()
  to: string;

  @Column()
  value: number;

  @Column("timestamp")
  timestamp: Date;

  @Column("json")
  metadata: any;
}

async function createTransaction(from: string, to: string, value: number, metadata: any) {
  const transaction = new Transaction();
  transaction.from = from;
  transaction.to = to;
  transaction.value = value;
  transaction.timestamp = new Date();
  transaction.metadata = metadata;

  await getManager().save(transaction);
}
```

업데이트된 walletInfo 메소드:

```js
async function walletInfo(call: ServerUnaryCall<string>, callback: sendUnaryData<WalletInfoResponse>) {
  const address = call.request;
  try {
    const result = await db.query(
      `SELECT SUM(value) as total, SUM(CASE WHEN to_address = '${address}' THEN value ELSE 0 END) as available FROM transactions WHERE from_address = '${address}'`
    );
    const transactions = await db.query(
      `SELECT to_address, value, timestamp, metadata FROM transactions WHERE from_address = '${address}'`
    );
    callback(null, {
      total: result[0].total,
      available: result[0].available,
      transactions: transactions,
    });
  } catch (err) {
    callback(err, null);
  }
}
```

<div class="content-ad"></div>

위의 코드는 데이터베이스와 상호 작용하기 위해 ORM 라이브러리를 사용하며, 쿼리는 일반 SQL로 작성되었다는 점을 강조해야 합니다. 사용하는 ORM 라이브러리에 따라 쿼리를 다르게 작성해야 할 수 있습니다.

또한 SQL 인젝션을 피하기 위해 준비된 문장을 사용하고, 데이터베이스에서 여러 작업을 수행하는 경우 트랜잭션을 사용해야 합니다.

# 마지막으로

이 튜토리얼은 Node.js와 TypeScript를 사용하여 gRPC 서버와 클라이언트를 생성하는 방법을 보여줍니다. 우리는 네 가지 메서드(createAddress, transaction, balance, walletInfo)를 가진 간단한 서비스를 정의했으며, 서비스 핸들러를 위한 모의 구현과 트랜잭션 저장을 위한 간단한 MySQL 데이터베이스도 구현했습니다.

<div class="content-ad"></div>

이 예시는 비교적 간단하지만, 데이터베이스와 통합한 gRPC 서비스를 구축하는 기본 사항을 보여줍니다. 프로덕션 시스템에서는 보안, 확장성 및 기타 요소를 고려해야 하지만, 이 예시는 좋은 시작점으로 삼을 수 있습니다.

gRPC 서비스를 구축하는 것은 많은 동시 요청을 처리할 수 있는 고성능, 저지연 시스템을 구축하는 좋은 방법일 수 있습니다. 데이터 직렬화를 위한 프로토콜 버퍼는 서비스를 높은 효율성과 쉬운 진화 가능성을 갖게 만듭니다.

또한 gRPC에는 플로우 제어, 양방향 스트리밍, 공식적으로 다양한 언어와 플랫폼을 지원하는 등 많은 훌륭한 기능들이 있어 다중 언어 시스템을 쉽게 구축할 수 있습니다.

gRPC 서비스를 구축에 관심이 있다면 공식 문서를 확인하고 다양한 온라인 자료를 탐색하는 것을 추천합니다. 이 튜토리얼이 도움이 되었기를 바라며, 당신만의 gRPC 서비스 구축을 즐기시기를 바랍니다.
