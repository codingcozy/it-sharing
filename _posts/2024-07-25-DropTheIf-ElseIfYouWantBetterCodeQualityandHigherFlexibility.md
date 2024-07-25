---
title: "더 나은 코드 품질과 유연성을 원한다면 If-Else를 버려야 하는 이유"
description: ""
coverImage: "/assets/img/2024-07-25-DropTheIf-ElseIfYouWantBetterCodeQualityandHigherFlexibility_0.png"
date: 2024-07-25 12:05
ogImage: 
  url: /assets/img/2024-07-25-DropTheIf-ElseIfYouWantBetterCodeQualityandHigherFlexibility_0.png
tag: Tech
originalTitle: "Drop The If-Else If You Want Better Code Quality and Higher Flexibility"
link: "https://medium.com/the-better-software-initiative/drop-the-if-else-if-you-want-better-code-quality-and-higher-flexibility-8a466fa49771"
---



![image](https://miro.medium.com/v2/resize:fit:1400/1*jhwjrhMvRJATpQM5rwZ8og.gif)

풀이 가능한, 유지보수 가능한 소프트웨어 작성의 중요성을 거의 모든 사람이 인정한다면, 개발자들이 왜 if-else 문과 같은 전통적인 분기를 계속 사용하는지 궁금해질 때가 자연스러울 것입니다.

처음에는 if-else가 모든 개발자의 프로그래밍 101 교육의 일부입니다. 모든 사람이 배웁니다. 처음에는 쉽게 구현할 수 있으며 (처음에는) 잘 읽힙니다 (추가 조건이 추가될 때까지).

그러나 지속적인 경험을 통해 초록색 필드 프로젝트를 만들고 다른 사람의 코드를 유지 보수한 후 전통적인 분기가 일으키는 문제와 그것이 속도에 미치는 심각한 영향을 직접 경험했습니다.


<div class="content-ad"></div>

if-else 및 switch case를 작성하는 것은 시스템이 수명 동안 어떻게 변할지 분석하고 예측하는 노력을 피하는 것입니다.

코드로 들어가기 전에, 실행 경로를 하드코딩하는 몇 가지 이유를 나열해 볼게요.

- 각 if, else if, else 및 switch case는 새로운 조건문을 기존 분기에 추가하는 영향을 효과적으로 이해하기 어렵게 만드는 불필요한 순환 복잡성을 추가합니다.
- if, else if, else, switch case는 종종 막대한 다형성이 아니며 바탕이 튼튼한 구조라 할 수 없습니다.
- 전통적인 분기 기술을 사용하여 새로운 기능을 구현하면 코드가 지나치게 부풀려집니다.
- 분기 결합 의존성을 만들 가능성이 높습니다. 즉, 예를 들어 특정 분기에서만 사용되는 생성자 의존성 또는 메서드 인수가 있을 수 있습니다.
- 특정 분기에서 사용되지 않는 분기 의존성에 대해 생성자 인수로 null을 전달하는 어색한 테스트를 작성해야 합니다.
- 계속해서 새로운 If를 덧붙이는 것은 기존 코드를 수정하므로 SOLID의 개방/폐쇄 원칙을 위반합니다.
- 단일 클래스에 더 많은 기능을 추가하면 협업 확장성이 줄어들어 충돌 발생 가능성이 높아집니다.
- 새로운 기능을 추가하려면 기존 코드를 수정해야 하며, 이에 대응하여 동일한 메서드에 추가 단위 테스트를 추가하고 전체 회귀 테스트를 수행해야 합니다.
- 버그 수정은 버그와 관련이 없는 분기 코드를 수정해야 하기 때문에 위험합니다.

솔직히 말해서 전통적인 분기가 대체로 좋지 않은 아이디어임을 긴 시간 동안 설명할 수 있어요. 이것은 제 마음에 떠오르는 첫 번째 몇 가지 문제에 불과합니다.

<div class="content-ad"></div>

`else if`와 `switch` 케이스를 사용하는 유일한 이유는 주니어 개발자들이 이해하기 쉽도록 코드를 작성하기 위한 것이지만, 이는 사실 좋은 아이디어가 아니며, 결국 사용하지 않게 될 코드 최적화에 대한 문제도 발생할 수 있습니다.

문제를 들어 설명한 코드 몇 가지를 살펴보도록 하겠습니다.

# 문제 소개

전통적인 분기 처리를 비판하고 해당 방법이 왜 나쁜 습관인지 몇 마디로 말하는 것은 쉽습니다. 하지만 이를 더 명확하게 이해하기 위해 몇 가지 예시를 살펴보는 것은 어떨까요?

<div class="content-ad"></div>

간단하면서도 설명적인 것을 유지하며, 웹 응용 프로그램에서 주어진 객체 유형에 대해 다양한 출력 형식을 생성할 수 있는 서비스를 개발해야 한다고 상상해봅시다. 사용자는 표시할 형식을 선택할 수 있습니다.

이 예시에서는 시작으로 Employee 클래스를 가져가 JSON 또는 XML로 출력해야 합니다.

우리가 여기서 구축하는 것은 중요하지 않다는 것을 염두에 두세요. 중요한 것은 개념, 기술, 및 접근 방식입니다.

![이미지](https://miro.medium.com/v2/resize:fit:1400/1*1dzKpL8bAHpmQI8hetTNBg.gif)

<div class="content-ad"></div>

쉽죠.

Employee 클래스, OutputFormat를 선택하는 enum 및 EmployeeFormatter가 필요합니다. 아래 코드를 한 번 읽어보세요.

```js
public class Employee
{
    public Employee() : this(Guid.NewGuid()) { }
    public Employee(Guid id) => Id = id;
    
    public Guid Id { get; }
    public string Name { get; init; }
    public DateOnly HiringDate { get; init; }
}

public enum OutputFormat
{
    Json,
    Xml,
}

public class EmployeeFormatter
{
    // 프런트 엔드에서 사용 가능한 형식을 얻는 데 사용됩니다
    public IReadOnlyList<string> GetAvailableFormats()
    {
        return Enum.GetNames(typeof(OutputFormat));
    }

    public string Format(Employee employee, OutputFormat format)
    {        
        if (format == OutputFormat.Json)
        {
            JsonSerializerOptions jsonOptions = new()
            {
                PropertyNamingPolicy = JsonNamingPolicy.CamelCase,
                WriteIndented = true
            };
            return JsonSerializer.Serialize(employee, jsonOptions);
        }
        else if (format == OutputFormat.Xml)
        {
            XmlWriterSettings xmlSettings = new()
            {
                Indent = true,
                NewLineChars = Environment.NewLine,
                NewLineHandling = NewLineHandling.Replace,
                OmitXmlDeclaration = false
            };
            EmployeeXml employeeXml = EmployeeXml.FromEmployee(employee);
            var xmlSerializer = new XmlSerializer(employeeXml.GetType());
            
            using var writer = new StringWriter();
            using var xmlWriter = XmlWriter.Create(writer, xmlSettings);
            xmlSerializer.Serialize(xmlWriter, employeeXml);

            return writer.ToString();
        }
        else
        {
            return "";
        }
    }

    /// <summary>
    /// 이 유형은 format == OutputFormat.Xml 분기에서만 사용됩니다.
    /// </summary>
    [XmlType(typeName: "Employee")]
    public record EmployeeXml
    {
        [XmlElement]
        public Guid Id { get; set; }
        
        [XmlElement]
        public string Name { get; set; }
        
        [XmlElement]
        public DateTime HiringDate { get; set; }
        
        public static EmployeeXml FromEmployee(Employee employee)
        {
            return new EmployeeXml
            {
                Id = employee.Id, 
                Name = employee.Name,
                HiringDate = new DateTime(employee.HiringDate, TimeOnly.MinValue)
            };
        }
    }
}
```

따라서 프런트 엔드에서는 사용 가능한 형식을 표시할 때 GetAvailableFormats 메서드를 사용하고, Format(employee, outputFormat)는 형식화된 결과를 생성합니다.

<div class="content-ad"></div>

분기 구현에서 형식 메서드를 제거하면 다음과 같이 남게 됩니다:

```js
public string Format(Employee employee, OutputFormat outputFormat)
{   
    if (outputFormat == OutputFormat.Json)
    {
    }
    else if (outputFormat == OutputFormat.Xml)
    {
    }
    else
    {
    }
}
```

이 시점에서 쉽게 이해할 수 있고 문제가 많이 발생하지 않습니다. 이제 또 다른 형식을 추가해보세요. 예를 들어 PDF 형식을 추가한다면 어떤 일이 일어나는지 살펴보세요.

```js
public enum OutputFormat
{
    Json,
    Xml,
    Pdf // ← 새로운 형식
}

public string Format(Employee employee, OutputFormat outputFormat)
{   
    if (outputFormat == OutputFormat.Json)
    {
      // json 구현
    }
    else if (outputFormat == OutputFormat.Xml)
    {
      // xml 구현
    }
    else if (outputFormat == OutputFormat.Pdf) // ← 새로운 형식
    {
      // pdf 구현
    }
    else
    {
    }
}
```

<div class="content-ad"></div>

여러 곳에서 코드를 수정해야 했고, 사이클로매틱 복잡성이 증가했으며 완전히 다른 출력을 기대하는 동일한 메서드에 대해 더 많은 단위 테스트를 추가해야 했습니다.

무엇보다도, 이는 극히 순진한 구현 방식입니다. 다른 형식을 추가하거나 형식을 갑자기 켜고 끌 때 어떤 일이 발생하는지 상상해보십시오.

## 클래스 추출 리팩터링을 통해 복잡성을 줄이려는 시도

약간의 경험을 쌓은 주니어 개발자는 클래스 추출 및 의존성 주입 리팩터링과 같은 명백한 리팩터링 기술을 적용하기만 하면 반환하고 있는 것 같습니다.

<div class="content-ad"></div>

자, 이제 빠르게 시도해보고 코드 품질에 어떤 영향을 미치는지 확인해보겠어요. 전체 코드를 작성하지는 않겠지만 아마도 의도는 전달될 거에요.

```js
public interface IEmployeeFormatter {
  Format(Employee employee);
}

// 구현은 간결함을 위해 생략했습니다.
public class EmployeeJsonFormatter : IEmployeeFormatter {}
public class EmployeeXmlFormatter : IEmployeeFormatter {}
public class EmployeePdfFormatter : IEmployeeFormatter{}

public class EmployeeFormatter {
  public EmployeeFormatter(
            EmployeeJsonFormatter json,
            EmployeeXmlFormatter xml,
            EmployeePdfFormatter pdf) {
    // 필드 설정
  }
}

public string Format(Employee employee, OutputFormat outputFormat)
{   
    if (outputFormat == OutputFormat.Json)
    {
      return json.Format(employee);
    }
    else if (outputFormat == OutputFormat.Xml)
    {
      return xml.Format(employee)
    }
    else if (outputFormat == OutputFormat.Xml)
    {
      return pdf.Format(employee)
    }
    else
    {
      // 다른 작업
    }
}
```

실제 형식 지정 로직을 별도의 클래스로 이동하는 것 외에는 거의 변경 사항이 없었어요. 이것은 EmployeeFormatter를 "깔끔하게" 유지하고 가독성을 높이는 데 도움이 됩니다.

위의 코드 예제에서 아주 중요한 특징을 발견하셨나요? 각 종속성은 분기간 독립적으로 단 한 번만 사용됩니다. 우리는 효과적으로 분기에 결합된 종속성으로 리팩터링했어요.

<div class="content-ad"></div>

또한, 저희 테스트는 null 종속성으로 인해 문제를 겪고 있습니다:

```js
[Fact]
public void FormatEmployeeAsPdf()
{
    // Arrange
    var formatter = new PdfFormatter();
    var employee = new Employee
    {
        Name = "Faxe Kondi",
        HiringDate = new DateOnly(2023, 5, 20)
    };
    
    var sut = new EmployeeFormatter(
                    null, // ← 
                    null, // ←
                   formatter // 유일하게 중요한 종속성
              );
    
    // Act
    string result = sut.Format(employee, OutputFormat.Pdf);
    
    // Assert
    // 일부 어서션
}
```

EmployeeFormatter 생성자에 두 개의 null을 전달하고 있음을 주목해 주세요. 이것은 분명히 코드 냄새가 나고 매우 손상되기 쉬운 테스트를 만듭니다.

## 협업적 확장성 역시 문제가 됩니다.

<div class="content-ad"></div>

많은 개발자들이 이를 충분히 만족스럽게 여기고 리팩토링이 성공했다고 말할 것입니다.

하지만 상상해보세요. 어느 한 명의 개발자가 새로운 형식을 추가해야 하고, 다른 개발자가 응용 프로그램을 재시작하지 않고 형식을 켜고 끌 수 있도록 작업해야 하는 시나리오를 생각해보세요.

두 개발자는 기존의 if-else 문에 새로운 생성자 매개변수와 조건부 요구 사항을 추가하기로 결정했습니다. EmployeeFormatter를 새로 생성하는 모든 테스트를 수정해야 하며 충돌이 발생할 수 있습니다.

# 유연한 조건부 실행 다루기.

<div class="content-ad"></div>

좋아요, 각 branch를 별도의 클래스로 추출하는 것은 올바른 방향으로 나아간 것이었습니다. 이제 유연성과 확장성을 허용하기 위해 if, else-if, else를 제거하기만 하면 됩니다.

먼저 enum OutputFormat을 제거하고 각 구체적인 "포매터"가 어떤 형식을 출력할 수 있는지 알려주도록 할 것입니다.

아래 변경 사항을 읽어보세요.

```js
public interface IEmployeeFormatter
{
    string Format(Employee employee);
    string FormattingType { get; } // ← 새로운 부분
}

public class EmployeeFormatterManager(ILogger<EmployeeFormatterManager> logger)
{
    private readonly Dictionary<string, IEmployeeFormatter> formatters = [];
    
    public IReadOnlyList<string> GetAvailableFormats() 
        => formatters.Select(kv => kv.Key).ToList();

    public string Format(Employee employee, string format)
    {
        bool hasFormatter = formatters.ContainsKey(format);
        return hasFormatter
            ? formatters[format].Format(employee)
            : throw new UnknownFormatException(format, GetAvailableFormats());
    }

    public EmployeeFormatterManager Add(IEmployeeFormatter formatter)
    {
        logger.LogEmployeeFormatterAdded(formatter.GetType(), formatter.FormattingType);
        
        formatters.Add(formatter.FormattingType, formatter);
        return this;
    }

    public EmployeeFormatterManager Add(IEnumerable<IEmployeeFormatter> employeeFormatters)
    {
        foreach (IEmployeeFormatter employeeFormatter in employeeFormatters) Add(employeeFormatter);
        return this;
    }
}

public class UnknownFormatException(string formatType, IEnumerable<string> availableFormats)
    : Exception($"Unknown type {formatType}. Available formats are: {string.Join(",", availableFormats)}");
```

<div class="content-ad"></div>

먼저, "formatters"는 해당하는 형식과 함께 사전에 저장됩니다.

둘째, GetAvailableFormats는 여전히 "사용 가능한 형식은 무엇인가?” 묻는 데 사용됩니다. 그러나 이번에는 열거형을 검토하는 대신 실제로 사용 가능한 형식을 살펴봅니다. 이는 값을 쉽게 추가할 수 있고 실제로 실행 경로가 제공되지 않더라도 열거형에 값 추가가 가능하다는 더 견고한 접근 방식입니다.

셋째, 직원을 문자열 출력으로 형식화할 때 "formatter"가 사용된 것을 설계 시에 알 수 없으며 사실 직원 형식 관리자(EmployeeFormatManager)도 상관하지 않아야 합니다.

# 기존 코드를 수정하지 않고 새로운 기능을 추가합니다.

<div class="content-ad"></div>

이러한 변경 사항은 코드를 수정하지 않고 EmployeeFormatManager에 원하는 만큼의 형식을 추가할 수 있기 때문에 유연성과 확장성을 제공합니다.

새 기능(포맷)을 추가하는 것이 매우 쉽기 때문에 새 클래스를 생성하여 EmployeeFormatManager에 추가하기만 하면 됩니다.

```js
public class EmployeeCsvFormatter : IEmployeeFormatter {
  public Format(Employee employee) {
    // 구현 세부사항
  }
}

// 코드 다른 곳에
var csv = new EmployeeCsvFormatter();
manager.Add(csv)
manager.Format(someEmployee, "csv");
```

잘 구조화된 코드의 장점 중 하나는 테스트를 수행하기가 얼마나 즐거운지입니다. EmployeeFormatterManager가 구체적인 구현에 의존하지 않기 때문에 그러한 인스턴스를 생성할 필요가 없어서 null 생성자 인수가 없게 됩니다.

<div class="content-ad"></div>

```js
// EmployeeFormatterManager 테스트
[사실]
public void 추가된형식지정자포함여부테스트()
{
    // 정렬
    var 첫번째 = Substitute.For<IEmployeeFormatter>();
    첫번째.FormattingType.Returns("first");
    
    var 두번째 = Substitute.For<IEmployeeFormatter>();
    두번째.FormattingType.Returns("second");
    
    EmployeeFormatterManager sut = new EmployeeFormatterManager(NullLogger<EmployeeFormatterManager>.Instance)
        .Add(첫번째)
        .Add(두번째);

    // 실행
    IReadOnlyList<string> 결과 = sut.GetAvailableFormats();
    
    // 확인
    결과.Should().Contain(["first", "second"]);
}
```

마찬가지로, 각 "형식지정자"는 다른 클래스의 실행 경로(if, else-if, else)의 구현 세부사항이 아니기 때문에 자체로 테스트할 수 있습니다.

```js
// EmployeeJsonFormatter 테스트
[사실]
public void 직원형식지정()
{
    var employee = new Employee(Guid.Parse("f9e827ab-29aa-4a50-92a1-3688c6a4b9fe"))
    {
        Name = "Faxe Kondi",
        HiringDate = new DateOnly(2023, 5, 1)
    };

    var sut = new EmployeeJsonFormatter();
    
    // 실행
    string 결과 = sut.Format(employee);
    
    // 확인
    // language=json <- JetBrains IDE에서 하이라이팅을 허용합니다.
    const string 예상된Json = """ 
                                {
                                  "id": "f9e827ab-29aa-4a50-92a1-3688c6a4b9fe",
                                  "name": "Faxe Kondi",
                                  "hiringDate": "2023-05-01"
                                }
                                """;
    결과.Should().BeEquivalentTo(예상된Json);
}
```

협력적 확장성에 대한 언급으로, 다른 개발자들은 IEmployeeFormatter 인터페이스를 구현하면 다른 프로젝트에 속한 형식 구현을 가져올 수 있습니다.

<div class="content-ad"></div>

# 런타임에서 기능을 토글하기

이제 비즈니스에서 새로운 요구 사항이 생겼고 애플리케이션을 다시 컴파일하거나 배포하지 않고 런타임에서 특정 형식을 활성화하거나 비활성화 할 수 있는 작업을 맡았습니다.

![이미지](https://miro.medium.com/v2/resize:fit:1400/1*jhwjrhMvRJATpQM5rwZ8og.gif)

기존의 브랜치 기반 접근 방식은 기존 클래스를 수정하고 해당하는 모든 테스트를 업데이트해야 합니다.

<div class="content-ad"></div>

다음과 같이 구현할 수 있습니다:

```js
public class EmployeeFormatter(IOptionsMonitor<AvaiableFormatOptions> options)
{
    public IReadOnlyList<string> GetAvailableFormats()
    {
        return Enum.GetNames(typeof(OutputFormat))
            .Where(of => options.CurrentValue.EmployeeFormats.Contains(of))
            .ToList();
    }

  public string Format(Employee employee, OutputFormat outputFormat)
  {
      bool unknownFormat = !GetAvailableFormats().Contains(outputFormat.ToString());
      if (unknownFormat) return "";
      
      if (outputFormat == OutputFormat.Json)
      {
         // json
      }
      else if (outputFormat == OutputFormat.Xml)
      {
        // xml
      } else if (outputFormat == OutputFormat.Pdf)
      {
        // pdf
      }
      else
      {
          return "";
      }
  }
}
```

나쁘지 않네요. 코드가 어려워지는 속도를 쉽게 알 수 있습니다. 또한, 구현되지 않은 형식을 허용하여 버그를 초래할 수 있기도 합니다.

하드코딩된 실행 경로 대신 구체적인 포매터를 추가하는 것에 의존하는 업데이트된 버전에 대한 코드 변경 사항은 없습니다. 모든 것이 동일하게 유지됩니다.

<div class="content-ad"></div>

## 의존성 주입 프레임워크를 사용하여 EmployeeFormatManager 구성하기.

그래서 IoC 컨테이너를 사용하여 이 유연성을 어떻게 활성화하는지에 대해 되돌아와야 한다고 약속했었죠. 이 방식은 C# .NET에만 해당되는 것은 아니며 다른 언어에서도 손쉽게 재현할 수 있습니다.

닷넷에서는 이를 Microsoft.Extensions.DependencyInjection 라이브러리를 사용하여 구현할 수 있으며, 이미 aspnetcore 프레임워크와 함께 제공됩니다.

```js
// appsettings.json
{
  "EmployeeFormatterTypes": {
      "EmployeeFormats": [
        "Json",
        "Xml",
        "Pdf"
      ]
    }
 } 

// Program.cs (aspnetcore 애플리케이션)
builder.Services
    .Configure<EmployeeFormatterTypes>(
        builder.Configuration.GetSection(nameof(EmployeeFormatterTypes))
    );

builder.Services
    .AddSingleton<IEmployeeFormatter, EmployeeXmlFormatter>()
    .AddSingleton<IEmployeeFormatter, EmployeeJsonFormatter>()
    .AddSingleton<IEmployeeFormatter, EmployeePdfFormatter>()
    .AddScoped<EmployeeFormatterManager>(provider =>
    {
        var logger = provider.GetRequiredService<ILogger<EmployeeFormatterManager>>();
        var options = provider.GetRequiredService<IOptionsMonitor<EmployeeFormatterTypes>>();

        IEnumerable<IEmployeeFormatter> formatters = provider.GetServices<IEmployeeFormatter>()
            .Where(ef => options.CurrentValue.EmployeeFormats.Contains(ef.FormattingType));

        return new EmployeeFormatterManager(logger).Add(formatters);
    });
```  

<div class="content-ad"></div>

이렇게하면 EmployeeFormatManager가 요청될 때마다 appsettings.json에 사용 가능한 형식을 확인하고 구체적인 포매터를 찾아 관리자에 추가합니다.

다른 기능을 추가하는 문제를 새로운 실행 경로를 하드 코딩하는 문제에서 구성 문제로 효과적으로 변경했습니다.

# 계속 연락하세요!

YouTube 채널(@Nicklas Millard)을 확인하고 Medium 회원이 되거나 새 기사를 게시할 때 알림을받으세요.

<div class="content-ad"></div>

LinkedIn에서 연락해 주세요.

# 궁금한 분들을 위한 자료

- Github 저장소

![이미지](/assets/img/2024-07-25-DropTheIf-ElseIfYouWantBetterCodeQualityandHigherFlexibility_0.png)