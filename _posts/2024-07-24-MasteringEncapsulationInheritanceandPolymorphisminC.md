---
title: "C언어 캡슐화, 상속, 다형성 정리"
description: ""
coverImage: "/assets/img/2024-07-24-MasteringEncapsulationInheritanceandPolymorphisminC_0.png"
date: 2024-07-24 12:10
ogImage: 
  url: /assets/img/2024-07-24-MasteringEncapsulationInheritanceandPolymorphisminC_0.png
tag: Tech
originalTitle: "Mastering Encapsulation, Inheritance, and Polymorphism in C"
link: "https://medium.com/@jinlow/mastering-encapsulation-inheritance-and-polymorphism-in-c-88843b0821b8"
isUpdated: true
updatedAt: 1723813155118
---



## 실용적인 예제와 모범 사례로 필수 객체 지향 프로그래밍(OOP) 개념 학습

친지로 추천받은 회원으로서 저를 지원해주시면 정말 기쁠 것입니다. 하지만, 이 플랫폼에서의 기여로 얻은 수익은 제가 원하는 업데이트 빈도를 유지하기에는 충분하지 않다는 것을 깨달았습니다. 이런 현실에 직면하여, 저는 어려운 결정을 내렸습니다 — 새로운 수입 창구를 모색하기로 했습니다. 그러나 이것은 끝이 아닌, 새롭고 흥미진진한 시작이기에요. 저는 곧 시작할 Substack 뉴스레터를 소개하게 되어 너무 흥분됩니다. 거기서는 투자 시스템을 깊이 파헤치고 IT 기술의 엄청난 잠재력을 활용하여 투자 전략에 시스템적 사고 방식을 채택할 예정입니다. 안심하세요, 영감이 찾아올 때마다 여전히 글을 올릴 것입니다.

객체 지향 프로그래밍(OOP)에서 캡슐화, 상속 및 다형성을 이해하는 것은 소프트웨어 개발자에게 필수적입니다. 이러한 개념은 코드를 재사용 가능하게 구성하고 복잡성을 관리하여 설계를 향상시킵니다. 캡슐화는 클래스 내의 데이터와 메서드를 보호합니다. 상속은 코드 재사용을 촉진하고 클래스 계층을 확립하며, 다형성은 객체가 유연하게 동작할 수 있도록 합니다. 이러한 원칙을 숙달하는 것은 확장 가능한 소프트웨어를 작성하거나 심화된 OOP 도전에 대처하는 데 중요합니다. OOP를 배우는 것은 이론을 실용적인 코드로 번역하고 객체 중심적인 사고 방식을 채택하는 것을 의미하며, 이는 맥락과 실습이 필요합니다.

- 캡슐화

<div class="content-ad"></div>

- 데이터(속성)와 그 데이터를 처리하는 메서드(함수)를 일반적으로 클래스 단위로 묶는 것을 의미합니다.
- 객체의 내부 상태는 외부 세계에서 숨겨지고, 특정 허가된 메서드만이 이 상태에 액세스하거나 수정할 수 있습니다.
- 클래스의 내부 구현 변경은 공개 메서드(인터페이스)가 변경되지 않는 한 프로그램의 다른 부분에 영향을 미치지 않습니다.

```js
public class BankAccount {
    // 개인 속성
    private double balance;
    private string accountNumber;

    // 생성자
    public BankAccount(string accountNumber, double initialBalance) {
        this.accountNumber = accountNumber;
        this.balance = initialBalance;
    }

    // 돈을 예금하는 공개 메서드
    public void Deposit(double amount) {
        if (amount > 0) {
            balance += amount;
        }
    }

    // 돈을 인출하는 공개 메서드
    public void Withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
        }
    }

    // 잔액을 가져오는 공개 메서드
    public double GetBalance() {
        return balance;
    }

    // 계좌 번호를 가져오는 공개 메서드
    public string GetAccountNumber() {
        return accountNumber;
    }
}
```

- balance와 accountNumber 속성은 개인입니다.
- Deposit, Withdraw, GetBalance, GetAccountNumber 메서드는 공개되어 있으며, 개인 속성에 제어된 액세스를 제공합니다.

명명 규칙

<div class="content-ad"></div>

- Public members — 클래스 외부에서 접근할 수 있습니다. 예: public void Deposit(double amount)
- Protected members — 클래스와 하위 클래스 내에서만 접근할 수 있도록 하는 멤버입니다. 예: protected void CalculateInterest()
- Private members — 클래스 내부에서만 접근할 수 있도록 하는 멤버입니다. 예: private string _accountNumber;

![Link text](/assets/img/2024-07-24-MasteringEncapsulationInheritanceandPolymorphisminC_0.png)

2. 상속

- 하위 클래스가 다른 클래스(상위 클래스)의 속성과 메서드를 상속받을 수 있습니다.
- 명확하고 논리적인 클래스 계층 구조를 만들 수 있습니다.
- 하위 클래스가 상위 클래스의 메서드를 재정의할 수 있어 다형성을 구현할 수 있습니다.
- 공통 속성과 메서드를 가진 기본 클래스를 만들 수 있습니다.
- : 구문을 사용하여 상위 클래스에서 상속받은 하위 클래스를 만들 수 있습니다.

<div class="content-ad"></div>

```js
// 부모 클래스
public class Account
{
    public decimal Balance { get; protected set; }

    public void Deposit(decimal amount)
    {
        Balance += amount;
        Console.WriteLine($"입금액: {amount}, 새 잔액: {Balance}");
    }

    public virtual void Withdraw(decimal amount)
    {
        Balance -= amount;
        Console.WriteLine($"인출액: {amount}, 새 잔액: {Balance}");
    }
}

// 서브클래스
public class SavingsAccount : Account
{
    public override void Withdraw(decimal amount)
    {
        if (amount <= Balance)
        {
            base.Withdraw(amount);  // 부모 클래스 메서드 호출
        }
        else
        {
            Console.WriteLine("잔고가 부족합니다");
        }
    }
}

// 사용 예
class Program
{
    static void Main()
    {
        SavingsAccount myAccount = new SavingsAccount();
        myAccount.Deposit(1000);  // Account 클래스에서 상속
        myAccount.Withdraw(500);  // SavingsAccount 클래스의 오버라이드된 메서드
        myAccount.Withdraw(600);  // 잔고가 부족한 경우 SavingsAccount 클래스의 오버라이드된 메서드
    }
}
```

- 부모 클래스(Account) — 돈을 입금하고 인출하는 속성과 메서드를 포함하고 있습니다. Withdraw 메서드는 오버라이드할 수 있도록 가상 메서드로 선언되어 있습니다.
- 서브클래스(SavingsAccount) — Account 클래스를 상속받아서 잔고를 확인한 후에 인출을 진행하기 위해 Withdraw 메서드를 오버라이드합니다.

3. 다형성

- 메서드, 객체 또는 클래스가 여러 형태를 취할 수 있도록 합니다.
- 다양한 기본 데이터 유형에 대해 동일한 인터페이스를 제시합니다.
- 서로 다른 유형의 객체를 공통 슈퍼클래스의 객체로 취급할 수 있게 합니다.
- 다형성의 주요 유형은 컴파일 시(메서드 오버로딩)와 런타임(메서드 오버라이딩)이 있습니다.
- 메서드 오버로딩 — 같은 클래스 내에서 이름은 같지만 매개변수가 다른 여러 메서드를 작성하는 것이 가능합니다. 다른 매개변수 목록을 가진 메서드를 정의합니다. 서로 다른 유형의 입력을 처리하는 여러 버전의 메서드가 필요한 경우 사용합니다.
- 메서드 오버라이딩 — 서브클래스가 이미 정의된 메서드에 특정 구현을 제공할 수 있게 합니다. 상위 클래스 메서드에 virtual 키워드를 사용하고 파생 클래스 메서드에 override 키워드를 사용합니다. 기본 클래스에서 정의된 메서드의 동작을 변경하거나 확장해야 할 때 사용합니다.

<div class="content-ad"></div>

메서드 오버로딩 — 컴파일 시 다형성

- 호출할 메서드를 결정하는 것은 컴파일 시간에 이루어집니다. 컴파일러는 메서드 시그니처(이름 및 매개변수 목록)을 기반으로 적절한 메서드를 호출하기로 결정합니다.

```csharp
public class BankAccount
{
    public void Transaction(int amount)
    {
        Console.WriteLine($"Performing transaction of {amount} dollars.");
    }

    public void Transaction(string type, int amount)
    {
        Console.WriteLine($"Performing {type} transaction of {amount} dollars.");
    }

    public void Transaction(int amount, string currency)
    {
        Console.WriteLine($"Performing transaction of {amount} {currency}.");
    }
}

// 사용법
BankAccount account = new BankAccount();
account.Transaction(100);              // 출력: Performing transaction of 100 dollars.
account.Transaction("deposit", 200);   // 출력: Performing deposit transaction of 200 dollars.
account.Transaction(300, "euros");     // 출력: Performing transaction of 300 euros.
```

메서드 오버라이딩(런타임 다형성)

<div class="content-ad"></div>

- 어떤 메소드를 호출할지 결정하는 것은 실행 시간에 이루어집니다. 호출할 메소드는 참조 타입이 아닌 실제 객체 타입에 기반하여 결정됩니다.

```js
public class SavingsAccount : BankAccount
{
    public override void Transaction(int amount)
    {
        Console.WriteLine($"Savings Account에 {amount}달러를 입금하는 중.");
    }
}

// 사용 예시
BankAccount savingsAccount = new SavingsAccount();
savingsAccount.Transaction(300); // 출력: Savings Account에 300달러를 입금하는 중.
```

참고

만약 제 게시물 중 어떤 것이 도움이 되었다면, 제 작업을 지원하기 위해 커피 한 잔 사주시거나 후원해주시는 것도 고려해주시면 감사하겠습니다. 😊 이를 위해서는

<div class="content-ad"></div>

페이지트론

코파이 닷컴

바이미어커피

마지막으로, 아직 미디엄 멤버가 아니고 그렇게 되기를 계획 중이라면 아래 링크를 사용해 가입해 주시겠어요? 여러분은 추가 비용 없이 멤버십 비용의 일부가 저에게 돌아가게 됩니다.

<div class="content-ad"></div>

첫 번째 제휴 프로그램입니다. 시스템 지식을 더 향상시키고 싶다면 링크를 클릭하여 강의를 구매할 수 있습니다. 솔직히 말해서, 여러분이 강의 수강료의 20%를 내지만 추가 비용은 없습니다. 저희 강의에는 무제한 액세스가 제공됩니다. 만료 기간이 없으며 미래 업데이트에 무료로 액세스할 수 있습니다.