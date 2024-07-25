---
title: "깔끔한 코드를 작성하는 방법 주요 원칙과 실천법"
description: ""
coverImage: "/assets/img/2024-07-25-WritingCleanCode_0.png"
date: 2024-07-25 12:15
ogImage: 
  url: /assets/img/2024-07-25-WritingCleanCode_0.png
tag: Tech
originalTitle: "Writing Clean Code"
link: "https://medium.com/@kenslearningcurve/writing-clean-code-775c2dff07bd"
---


<img src="/assets/img/2024-07-25-WritingCleanCode_0.png" />

요즘 깨끗한 코드에 관한 많은 기사들을 보게 되는군요. 그것은 좋은 일이에요! 예전에 제가 교사였을 때 사람들이 깨끗한 코드에 대한 제 시각이 무엇인지 물어봤었어요. 왜 사람들이 '제 시각'에 대해 물었는지 이해가 안 가더라구요. 저는 로버트 C. 마틴, 말콰 '언클 밥'에 대해 읽고, 제가 한 일과 배운 것을 따르기로 했어요.

하지만 어떤 사람들은 책을 읽고 그것을 깨끗한 코드의 성경으로 따르곤 해요. 깨끗한 코드에 대한 최고의 아이디어를 말씀하기에 그 이상의 것이 있다고 믿고 있어요.

저의 깨끗한 코드의 시각은 경험과 다른 사람들로부터 배움에 기반하고 있어요. 제가 지키고 있다며 다른 분들에게 가르치려고 노력하고 있는 몇 가지 기본 '규칙'이 있어요. 이것이 유일한 방법은 아니며, 모두가 동의하지 않을 수도 있지만, 저는 많은 해 동안 다음의 팁을 사용해 왔어요.

<div class="content-ad"></div>

이 글은 C#을 기반으로 하고 있지만, Java, PHP 및 기타 OOP 언어에도 적용할 수 있습니다.

# 팀원들

깨끗한 코드는 10계명처럼 주어진 책이 아니에요. 좋은 개발자가 되기 위해 노력하면 발견하고 발전시킬 수 있어요. 네, 보통 팀에서 일하지 않는 좋은 개발자들이 있어요. 왜냐하면 팀으로서 모든 팀원이 코드를 어떻게 작성해야 하는지에 대한 몇 가지 기본 규칙을 정하기 때문이에요. 이 아이디어는 간단해요: 팀의 모든 사람은 동일한 방식으로 코드를 개발해 나가기 때문에 모두가 읽고 수정하고 계속할 수 있어요.

우리는 이러한 '규칙'을 코드 스타일이라고 부르고요. 대부분의 경우, 팀이 특정 개발 스타일에 대해 어떤 결정을 내렸는지를 설명하는 (디지털) 문서입니다.

<div class="content-ad"></div>

모든 것에 대해 가능합니다. 예를 들어 변수를 사용하든 말든, 한 줄짜리 if 문에 괄호를 사용하든 말든, 대문자나 밑줄로 변수를 선언하든 말든에 대해 이야기할 수 있습니다. 또한 메소드가 가져야 하는 매개변수의 최대 수 같은 것도 있습니다.

이렇게 함으로써 팀원들은 특정 코드를 작성하는 방법을 알게 됩니다. 안타깝게도, 제가 일한 3개 회사에서 이러한 코딩 스타일을 보았습니다... 21개 중에요. 이는 보통 당혹감, (해결되지 않은) 논의, 심지어 싸움까지 초래했습니다. 네, 정말 그랬던 적이 있죠.

그래서 코딩 스타일 문서를 어떻게 만들까요? 팀원들이 모여 현재 코드에 대해 논의하는 것으로 시작합니다. 어떤 멤버는 다른 멤버들의 선택에 대해 질문이나 불일치가 있을 수 있습니다. 누가 옳고 그르다고 경쟁하는 게 아니라, 코드를 읽고 이해할 수 있도록 팀을 함께 모이는 방법이죠. 팀의 두 멤버 간에 합의할 수 없을 때는 보통 리드 개발자가 최종 결정을 내립니다.

# 나의 따르는 규칙들

<div class="content-ad"></div>

팀 외에도 깔끔한 코드를 작성하는 방법에 대한 결정을 내리는 것이 중요합니다. 혼자 프로젝트를 진행 중이라면 마구뽀로 진행해도 되지만, 누군가가 도와주거나 온라인에 코드를 공유할 경우를 생각해보세요.

따라서, 여러분이 자신만의 방식과 깔끔한 코드에 대한 아이디어를 결정하는 데 도움이 되도록 저가 사용하는 팁을 공유하려고 합니다.

## var을 사용할지 말지

대부분의 개발자들이 이 논의를 알 것으로 생각합니다: var를 사용해야 할까요, 명시적 선언을 해야 할까요? 제가 일하는 회사에서 var와 명시적 선언을 모두 사용하도록 요청받아서 두 가지를 사용해왔습니다. 그러나 프로젝트를 혼자 진행할 때는 var를 사용했습니다. 네, 과거형입니다.

<div class="content-ad"></div>

```js
var someString = "Hello world";
var client = new HttpClient();
```

Var is an alias of what's on the right side of the =. So in the first line, var is a string. In the second line, var is an HTTPClient. A big advantage is that you don't have to type the type twice. So… Var is for lazy people? Nah, it's just easier. It's not about being lazy or not. It's a way of declaring a variable.

But it does make the code a little bit harder to read. If you use explicit declaration on the previous code it would look like this:

<div class="content-ad"></div>

```js
string someString = "Hello world";
HttpClient client = new HttpClient();
```

위 예제에서 변수의 유형을 빠르게 볼 수 있습니다. 이 변수 선언 방식은 자습서나 가르침 예제 코드에서 강제됩니다. 또 다른 장점은 이제 오른쪽에 있는 유형을 제거하고 'new'만 입력할 수 있다는 점입니다. 

예를 들어:

```js
string someString = "Hello world";
HttpClient client = new HttpClient();
```

문자열에 대해서는 이 방법이 작동하지 않습니다. 초기화가 없기 때문입니다.

<div class="content-ad"></div>

var를 많이 사용했지만, 이제는 명시적인 방법을 훨씬 더 사용하고 있어요. 왜냐하면 새로운 타입의 초기화 방법이 효율적으로 (더 간단한 코드로) 바뀌어서 읽기 쉬워졌거든. 일부 경우에는 아직 var를 사용하지만, 그것은 단위 테스트에 제한되어 있어요.

누군가가 var를 사용하는 것이 부족하다고 말했어요. 왜냐하면 어플리케이션이 var를 실제 타입으로 변환해야 한다는데요. 이게 사실은 아니에요; 컴파일 시점에 var는 실제 타입으로 대체되어, 어플리케이션 실행 시 var가 부족하지 않아요.

# 중괄호

중괄호의 사용은 C# 및 다른 언어에서 잘 알려져 있어요. 이것은 코드 뭉치를 정의하는데 사용되며, 메소드, 클래스, if문 등으로 구성된 블록을 정의합니다. 일부 경우에는 중괄호를 생략할 수도 있어요.

<div class="content-ad"></div>

우리는 이 코드 조각을 모두 알고 있어요:

```js
if(val == "somestring")
{
    Console.WriteLine("Correct!");
}
```

하나의 라인만 있는 간단한 if 문입니다. 그런데 중괄호를 제거할 수도 있어요. 그 이유는 중괄호가 적어서 코드가 조금 더 깔끔하게 보이며 코드에 더 집중할 수 있기 때문이에요.

```js
if(val == "somestring")
    Console.WriteLine("Correct!");
```

<div class="content-ad"></div>

아마 예제 코드만으로는 괄호를 제거한 이유를 명확히 보여주지 못할 수도 있습니다. 그래서 이제 괄호가 있는 더 큰 예제를 준비했습니다:

```js
string greeting = "Hello world";

if(greeting.GetType() == typeof(String))
{
    // 이것은 문자열입니다
    Console.WriteLine(greeting);
}
else
{
    Console.WriteLine("이것은 문자열이 아닙니다.");
}

if(greeting.Length > 5)
{
    Console.WriteLine("인사말의 길이가 5자를 초과합니다.");
}
else
{
    Console.WriteLine(greeting);
}
```

20줄로 이루어진 간단한 코드 조각입니다. 괄호를 제거하여 라인 수를 11줄로 줄일 수 있습니다:

```js
string greeting = "Hello world";

if(greeting.GetType() == typeof(String))
    Console.WriteLine(greeting);
else
    Console.WriteLine("이것은 문자열이 아닙니다.");

if(greeting.Length > 5)
    Console.WriteLine("인사말의 길이가 5자를 초과합니다.");
else
    Console.WriteLine(greeting);
```

<div class="content-ad"></div>

위의 내용은 if문 본문에 다른 줄을 추가할 때 나타나는 단점입니다. 한 줄만 있는 경우 대괄호를 제거할 수 있지만, 더 많은 줄을 추가하면 다시 대괄호를 추가해야 합니다.

```js
string greeting = "Hello world";

if(greeting.GetType() == typeof(String))
    Console.WriteLine(greeting);
else
    Console.WriteLine("This is not a string.");
Console.WriteLine("Please use a string"); // 이 줄은 항상 출력됩니다.

if(greeting.Length > 5)
    Console.WriteLine("The length of the greeting exceeds 5 characters.");
else
    Console.WriteLine(greeting);
```

새 줄이 왼쪽으로 이동되어 있는 모습을 확인할 수 있습니다. 이 줄의 WriteLine은 항상 실행되며, greeting이 문자열인 경우에도 마찬가지입니다.

저는 대괄호를 별로 선호하지 않습니다. 코드를 약간 흐릿하게 만들기 때문인데요. 가능한 경우 대괄호의 수를 최소화하려고 노력합니다. 이로 인해 종종 코드를 더 개선할 수 있는 다른 옵션들을 알아차릴 수도 있습니다. 위의 코드에서처럼 줄 수를 더욱 줄일 수도 있습니다.

<div class="content-ad"></div>

```js
// 문자열 변수 선언
string greeting = "Hello world";

// 조건문으로 문자열 길이에 따라 출력
if (greeting.GetType() == typeof(string) && greeting.Length <= 5)
    Console.WriteLine(greeting);
else if (greeting.Length > 5)
    Console.WriteLine("인사말의 길이가 5자를 초과합니다.");
else
    Console.WriteLine("이것은 문자열이 아닙니다.");
```

# The Green Code: Comments

이미 주석에 대한 블로그를 올렸기 때문에 여기에 간단한 요약이 있습니다.

주석을 사용하지 않는 것이 좋습니다.

<div class="content-ad"></div>

…

감사합니다.

코멘트는 코드를 흐리게 만들어 코드를 읽기 어렵게 할 수 있습니다. 코멘트를 사용해야 하는 경우는 다음과 같습니다:

- 혼자 프로젝트를 작업할 때.
- 인터페이스에 요약을 작성할 때.
- 정말 어려운 계산을 설명할 때.

<div class="content-ad"></div>

다음과 같이 테이블 태그를 Markdown 형식으로 변경하세요:


| Header One | Header Two |
|------------|------------|
| Row 1, Col 1 | Row 1, Col 2 |
| Row 2, Col 1 | Row 2, Col 2 |


<div class="content-ad"></div>

- PascalCasing
- camelCasing

여기에서는 Microsoft의 코드 규칙을 따릅니다. 파스칼 케이싱 또는 카멜 케이싱을 언제 사용해야 하는지에 대한 그들의 견해를 따릅니다. 이 주제에 관한 전체 문서가 있습니다. 하지만 꽤 간단합니다:

- 공개 타입은 PascalCasing
이에는 클래스, 인터페이스, 메서드, 속성, 및 열거형을 포함합니다.
- 비공개 타입은 camelCasing
이에는 메서드, 변수, 그리고 비공개 속성을 포함합니다.

예시:

<div class="content-ad"></div>

```js
namespace ExampleCasing // namespace = PascalCasing
{
    // interface = PascalCasing. Mind the capital I and E.
    public interface IExampleClass 
    {
        string Name { get; set; }
    }

    public class ExampleClass // class = PascalCasing
    {
        public string Name { get; set; } // public property = PascalCasing
        private DateTime currentDateTime; // private class variable = camelCasing
        public ExampleClass() // class = PascalCasing
        {
            string exampleVariable = "Hello world"; // private variable = camelCasing
        }

        private void DoSomething() // private method = camelCasing
        {
            ...
        }
    }

    public enum Gender // enum = PascalCasing
    {
        Male,
        Female,
        Other
    }
}
```

"지금 '이해했어!'라고 생각하는 독자들을 위해... 네, 주석을 사용하지 말라는 챕터 뒤에 주석을 사용했어요. 하지만 이 주석은 교육 목적으로만 사용하고 실제 코드에선 사용하지 않아요.

# 네이밍 규칙

카멜 케이싱과 파스칼 케이싱은 모두 네이밍 규칙의 일부입니다. 하지만 이 두 가지 뿐만 아니라 더 많은 네이밍 규칙이 있어요."

<div class="content-ad"></div>

저는 몇 가지 규칙을 따라요:

- 이름은 짧아야 돼요.
위와 같이 긴 이름은 피해주세요.
- 이름이 설명 가능해야 해요.
private void Execute() … 무얼 실행할까요?
private void ExecuteSQLStatement()… 알겠어요!
- 문자만 사용하세요.
숫자, 밑줄 또는 기타 문자는 사용하지 마세요. 일부는 허용되지 않아요. 단위 테스트 메서드는 예외에요.
private void Execute_Sql_Statement().
- 변수에 접두사를 사용하지 마세요
string sGreeting = “Hello world”;
string intRating = “1”;
var lAmount = 1029265527892;

# 가독성 유지하기

여러 단어로 이루어진 변수, 클래스 또는 메서드 이름은 각 단어의 첫 글자를 대문자로 써주세요. 예를 들면:

<div class="content-ad"></div>

```js
private string helloWorld { get; set; } // 옳음; W 대문자로
private string executeStatement() // 틀림. statement의 's'는 대문자 S여야 함.
{
    ...
}

public class SomeNewClass // 옳음
{

}
```

우리가 이렇게 하는 이유는 이름을 더 읽기 쉽게 만들기 위함입니다. 예를 들어, 두 번째 메서드는 더 읽기 어렵습니다. 단어를 찾아봐야 하기 때문이죠. 하지만 프로퍼티와 클래스는 더 쉽게 읽을 수 있습니다.

# 밑줄

Microsoft 코딩 규칙에 따르면, 클래서의 private 프로퍼티가 있을 때는 밑줄을 접두사로 추가해야 합니다. 이는 나는 사용하지 않는 Microsoft '규칙' 중 하나입니다. 카멜 케이싱으로 작성된 변수는 private임을 보여주기 때문입니다.

<div class="content-ad"></div>

```java
public class ExampleClass
{
    private DateTime currentDateTime;
    private string _greetingText;
}
```

# 메소드 본문 크기

어떤 사람들은 메소드가 5줄 이상이 되어서는 안 된다고 말합니다. 이중 중괄호까지 포함된다면 5줄 안에 들어야 한다는 이야기인가요. 하지만 저는 이것이 거의 불가능하다고 생각합니다. 네, 수백 개의 작은 메소드를 작성할 수 있겠지만, 이것이 코드를 깔끔하게 만들어주는 데 도움이 되는지 의문입니다.

저는 제 메소드를 10줄에서 20줄 사이로 유지하려고 노력하며, 단일 책임 원칙을 명심합니다: 메소드나 클래스는 단일 책임을 가져야 합니다. 때로는 20줄을 넘어가기도 하지만, 보통 그 이유가 있는 경우일 때입니다. 이에 대해 너무 걱정하지 마세요.


<div class="content-ad"></div>

팁: 하나의 메소드에서 코드를 작성하기 시작하세요. 모든 것이 정상적으로 작동하고 단위 테스트가 통과하면, 해당 메소드를 더 작은 메소드로 나누는 작업을 해보세요. 그러면 최종적으로는 약 10 ~ 20줄 정도로 이루어진 작은 메소드들이 만들어질 것입니다.

# 메소드의 매개변수 개수

메소드는 매개변수를 가질 수 있습니다. 보통 매개변수는 최대 5개까지 사용하곤 합니다. 그 이상으로 매개변수가 늘어난다면, 보통 해당 매개변수를 보유하는 단일 객체를 생성해 하나의 매개변수 변수로 사용합니다.

어떤 사람들은 3개가 최대라고 생각하지만, 이는 가능합니다. 단일 책임 원칙을 염두에 두세요. 공통점이 없는 속성을 단일 객체에 함께 묶어놓으면 이 원칙을 위배할 수 있습니다.

<div class="content-ad"></div>

# 문자 수 세기

일반적으로 코드 행은 화면을 초과해서는 안 된다고 권장됩니다. 이 점에 대해 정말 고민 중이에요.

이 말은 에디터에서 좌우로 스크롤하지 않고 모든 코드를 볼 수 있어야 한다는 것을 의미합니다. 이는 화면의 해상도가 다양하기 때문에 거의 불가능합니다. 제 노트북 해상도는 1920×1080이에요. 제 데스크톱 PC는 각각 해상도가 3840 x 2160인 4K 모니터 두 개를 가지고 있어요. 하지만 낮은 해상도, 예를 들면 800×600인 경우, 제 화면에서 괜찮아 보이는 코드가 낮은 해상도에서는 스크롤바를 만들어 낼 수 있어요.

또 다른 문제는 Visual Studio나 일반적으로 다른 편집기들이 있어요. 아래 스크린샷을 보세요, 이게 제 Visual Studio에요.

<div class="content-ad"></div>


![이미지](/assets/img/2024-07-25-WritingCleanCode_1.png)

필요한 것만 있으면 되지. 에디터랑 솔루션 탐색기. 다른 창들은 SQL 브라우저, GIT, 그리고 에러 목록 같은 게 항상 필요한 건 아니라서 숨기거나 최소화해.

아니야, 봐봐. 뭐든 다 추적하고 싶어하는 사람의 화면이야:

![이미지](/assets/img/2024-07-25-WritingCleanCode_2.png)


<div class="content-ad"></div>

라인의 길이는 누가 결정하나요? 한 줄당 허용되는 글자 수는 몇 개인가요? 제 조언: 에디터를 깨끗하게 유지하세요.

# 결론

이것들은 제가 따라가려고 노력하는 깔끔한 코드와 코딩 관례의 기본적인 규칙입니다. 몇 가지는 쉽게 따를 수 있지만, 몇 가지는 그렇지 않습니다. 모든 것은 환경, 업무, 프로젝트에 따라 다릅니다.

다시 한 번 강조하지만, 이것들은 제가 업무 중에 배운 것들이며 제가 발견한 것들입니다. 동의하지 않아도 됩니다. 질문이나 추가 아이디어가 있다면 댓글로 알려주세요.