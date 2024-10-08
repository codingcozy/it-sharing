---
title: "Vertical Slice는 모듈화된 모놀리식 구조인가?"
description: ""
coverImage: "/assets/img/2024-07-21-TheCoreQuestionAreVerticalSlicesJustModularMonoliths_0.png"
date: 2024-07-21 11:29
ogImage: 
  url: /assets/img/2024-07-21-TheCoreQuestionAreVerticalSlicesJustModularMonoliths_0.png
tag: Tech
originalTitle: "The Core Question Are Vertical Slices Just Modular Monoliths"
link: "https://medium.com/@rico-fritzsche/the-core-question-are-vertical-slices-just-modular-monoliths-9b1ef2358c53"
isUpdated: true
updatedAt: 1723816748736
---



<img src="/assets/img/2024-07-21-TheCoreQuestionAreVerticalSlicesJustModularMonoliths_0.png" />

몇 일 전에 수직 슬라이스 아키텍처가 결국 모듈식 단일체 아키텍처가 아닌가라는 질문을 받았습니다. 이 질문은 매우 흥미롭고 흥미로워서 그것에 대해 생각하고 기사를 쓰기 충분히 했습니다. 이 탐구는 단지 학문적인 연습이 아닌 개인적인 여정과 소프트웨어 엔지니어링 분야에서의 실용적 경험의 반영입니다. 이 기사에서 나는 내 인사이트와 몇 년간의 실무 경험에서 배운 교훈을 바탕으로 이 흥미로운 질문을 해결해보려고 합니다.

실세계 응용 프로그램, 시행착오 및 계속적인 학습 및 재학습 과정이 여기서 공유하는 관점을 형성했습니다.

# 특징 슬라이스

<div class="content-ad"></div>

내 의견으로는 수직 슬라이스 아키텍처의 정의 핵심은 비즈니스 기능에 따라 코드를 그룹화하는 아이디어입니다. 이 아이디어는 추상화 수준에 따라 적용될 수 있습니다. 슬라이스는 모듈일 수도 있고, 동일한 집합 아래의 사용 사례 그룹일 수도 있으며, 심지어 사용 사례 그 자체일 수도 있습니다. 내 기사에서 자주 설명하고 보여준 대로, 나는 주로 '기능 슬라이스'라고 불리는 것을 사용하는 것에 초점을 맞췄습니다. 기능에 필요한 모든 것을 기본적으로 담고 있습니다.

내 의견으로는, 이것은 시스템과 상호작용을 설명하는 Jimmy Bogart의 기술 중에 시스템과의 상호작용이 쿼리 또는 명령 요청일 수 있다는 것이 가장 잘 맞는다고 생각합니다. 사용자의 관점에서 보면 항상 "뭔가를 주세요" 또는 "뭔가를 실행해주세요"로 이해할 수 있습니다. 이것이 Jimmy Bogart가 예제에서 보여준 방식이며, 나는 이를 가이드로 사용했습니다. 내 의견으로는 두 가지 포인트가 중요합니다:

- 각 수직 슬라이스는 요청을 가장 잘 수행하는 방법을 결정할 수 있어야 합니다.
- 슬라이스 간 결합을 최소화하고 슬라이스 내 결합을 최대화해야 합니다.

그래서 내가 보기에는 각 모듈이 루트(즉, 사용자)이고, 작은 슬라이스를 포함하여 CreateUser, GetUsers 등과 같은 원자적인 작업 수준까지 다운되는 구조입니다.

<div class="content-ad"></div>

코드 안에서 이를 조직하는 것은 꽤 쉬운 일이에요. 기본적으로 폴더 구조로 할 수 있어요. 최상위 폴더를 “Features”라고 부르고, 다음 레벨은 모듈을 나타낼 수 있어요. 예를 들어 Users이고, 그 아래에 실제 기능들이 있어요.

```js
Features/
├── Users/
│   ├── GetUsers/
│   ├── CreateUser/
│   ├── SetUserActive/
│   └── ...
├── Orders/
│   ├── PlaceOrder/
│   ├── CancelOrder/
│   └── ...
```

제 경험상 이 방식은 코드 조직에서 명확성을 제공하고 슬라이스 구현에서 자유를 주는 것 같아요.

그런데 이제 질문으로 넘어가볼게요. 수직 슬라이스 아키텍처는 모듈러 모놀리스와 같은 건가요? 아니면 차이가 있나요?

<div class="content-ad"></div>

# 몬올리스

먼저 몬올리스가 실제로 무엇을 의미하는지 명확하게 해 보겠습니다. 몬올리스란 단순히 전체 애플리케이션이 하나의 실행 파일에 패키징되어 있는 것을 의미합니다. 예를 들어 Java 애플리케이션의 .jar 파일, .NET core 웹 애플리케이션의 .dll 파일 또는 모든 부분을 캡슐화하는 단일 Docker 컨테이너가 될 수 있습니다.

그런데 다시 질문으로 돌아가보죠: 위의 구조를 살펴보고 몬올리스의 정의를 적용하면 우리의 접근 방식이 몬올리스라는 것을 알 수 있습니다. 코드의 기능들이 서로 독립적으로 존재하더라도, 최선의 경우에는 직접적인 의존성이 없는 경우에도 빌드 중에 실행 파일로 패키징되어 배포됩니다. 새로운 모듈이나 기능을 추가하려면 전체 애플리케이션을 롤아웃해야 합니다. 10년 전에 마이크로서비스 사용을 옹호할 때에는 항상 몬올리스에 대항하여 주장되었습니다. 그러나 실제로 이것이 단점인가요? 결코 그렇지 않습니다! 예를 들어, GitHub Actions를 사용해 Azure Web App에서 하는 자동화 배포는 문제없이 잘 작동합니다. 따라서 단점이라고 보지 않습니다.

팀들은 모듈을 독립적으로 작업할 수도 있지만, 모듈이나 기능 슬라이스 간에 의존성이 없도록 확인하는 한에는 문제없이 작동합니다. 모듈이나 기능 슬라이스 간에 어떻게 통신해야 하는지에 대한 질문은 실제로 매우 간단합니다. 애플리케이션 내에서 다른 모듈이나 기능 슬라이스 간에 효율적으로 관리하는 통신은 이벤트나 알림을 사용함으로써 가능합니다. 이 접근 방식은 모듈을 독립적으로 유지하는 아이디어와 완벽하게 일치합니다.

<div class="content-ad"></div>

MediatR를 사용하는 맥락에서, 이 통신은 알림을 게시함으로써 용이하게 이뤄집니다. 슬라이스나 모듈 내에서 특정 이벤트가 발생하면, MediatR을 사용하여 알림을 발행할 수 있습니다. 이 알림은 브로드캐스트 메시지처럼 작용하여 애플리케이션의 다른 부분에 무슨 일이 일어났는지 신호를 보냅니다.

이 관점에서 수직 슬라이스 구조는 모듈식 단일체입니다. 저는 심지어 유지보수성과 이해하기 쉬운 구조를 보장하는 모듈식 단일체라고 말할 수 있습니다.

# 단일체 모듈화

수직 기능 슬라이스는 모듈식 단일체를 구축하기 위한 좋은 선택지이지만, 유일한 방법은 아닙니다. 모듈을 제대로 분리하는 데 주의를 기울인다면 전통적인 계층화 된 아키텍처를 사용하여 유사한 수준의 모듈화를 달성할 수 있습니다. 이 접근 방식은 .NET의 기능과 잘 호환됩니다.

<div class="content-ad"></div>

.NET 환경에서 각 모듈을 별도의 프로젝트로 다루는 것은 매우 효과적일 수 있습니다. 이렇게 하면 모듈별 코드를 자체 범위 내에 캡슐화할 수 있습니다. 웹 애플리케이션을 빌드하거나 배포하는 등의 실제 측면에서 이러한 모듈은 별도의 DLL로 나타납니다. 이들은 개발 중에 격리되어 있을 뿐만 아니라 최종 빌드에서 종속성으로서 독립성을 유지합니다. 이 방법은 모듈성을 애플리케이션의 구조에 포함시킵니다.

각 모듈을 유계 컨텍스트와 결합하고 Onion Architecture를 도입하는 것은 한 단계 더 발전시키는 방법입니다. Onion Architecture는 단순히 계층 구조에 관한 것이 아니라, 비즈니스 로직의 견고한 핵심을 외부 관심사(사용자 인터페이스 및 데이터 액세스와 같은)를 다루는 계층으로 둘러싼 것에 관한 것입니다. 이 설계의 장점은 외부 계층의 변화에 적응하여 핵심 비즈니스 규칙이 방해받지 않도록 하는 능력입니다. 이는 튼튼한 기초와 적응 가능한 벽을 갖춘 요새를 건설하는 것과 같아서, 애플리케이션의 핵심이 외부 세계의 혼돈에 영향을 받지 않도록 보장합니다.

에릭 에반스의 "도메인 주도 설계"라는 책에서 제시한 통찰은 몇 년 동안 수많은 프로젝트에서 내가 직접 구현한 접근 방식과 밀접한 관련이 있습니다. 효과적인 모듈을 설계하는 핵심은 각 모듈 내에서 높은 내부 일관성을 유지하면서 최소한의 결합을 달성하는 것입니다. 이 원칙은 이론적인 것뿐만 아니라 견고하고 유지보수 가능한 소프트웨어의 구조화에 지혜로운 안내가 됩니다.

<div class="content-ad"></div>

.NET 프레임워크를 사용할 때 이러한 설계 장점 중 하나는 프로젝트 또는 모듈간의 순환 참조를 피하는 것이 눈에 띕니다. 이것은 기술적인 특징뿐만 아니라 전략적인 장점이기도 합니다. 이는 모듈 간의 관계가 얽히지 않고 독립적이며 복잡한 의존성을 피해 개발과 유지 보수를 어렵게 만드는 것을 방지합니다.

실제로 응용 프로그램의 각 모듈이 자체 포괄적이고 강력하며 독립적이며 인프라 및 공유 모듈만이 공통 의존성인 설계가 되도록하여 각 모듈이 독립성을 유지하는 것이 중요합니다. 이 독립성이 팀이 서로 발을 밟지 않고 서로 다른 모듈에 병렬로 작업할 수 있도록 해줍니다.

모듈화, 의존성 회피, 관심사를 격리하는 것은 이 설계의 핵심입니다. 우리가 본 대로 수직적인 기능 슬라이스를 사용해 모듈화된 단일 돌출체를 형성하는 한 가지 방법을 알아봤지만, 이것이 절대적인 것은 아닙니다. 대안적인 방법은 물리적으로 구분된 모듈로 단일 돌출체를 구성하는 것인데, 각 모듈은 서로 다른 아키텍처 스타일을 포괄할 수 있습니다.

# 기능 슬라이스의 유연성

<div class="content-ad"></div>

위에서 나온 스크립트를 참조하여 수직 슬라이싱의 가능성에 대해 고려해 봅시다. 이것은 거대한 구조에 얽매이지 않고 슬라이스를 만드는 것을 의미합니다. 각 슬라이스를 독립된 람다 함수로 생각해 보세요. 이들은 모두 공통의 데이터 모델을 공유합니다. 

이 접근법은 단순히 조직에 관한 것뿐만이 아니라 확장성에 대한 전략적 선택입니다. 

수직 슬라이스 접근법을 사용하여 응용 프로그램을 개발한다고 상상해 봅시다. 프로젝트가 진전됨에 따라 특정 슬라이스가 컴퓨팅 성능을 향상시키고 더 많은 자원이 필요할 때 전체 응용 프로그램을 확장해야 할까요? 전혀 그렇지 않습니다. 이 접근법의 실제 장점입니다. 우리는 그 고성능 슬라이스를 독립적인 Azure 함수나 AWS 람다 함수로 전환하고 필요에 따라 독립적으로 확장할 수 있습니다.

여기서 중요한 점은 슬라이스 사이의 낮은 응집력입니다. 각 슬라이스가 자체 포함된 단위이기 때문에 슬라이스를 추출하고 확장하는 것이 큰 문제가 되지 않습니다. 이는 기계의 업그레이드가 필요한 부분을 식별하고 전체 기계를 다시 만들지 않고 그 부분만 교체하는 것과 비슷합니다.

이 방법론은 매우 유연합니다. 우리를 처음부터 분산 접근법을 선택하도록 강요하지 않습니다. 대신 응용 프로그램의 변화하는 요구 사항에 기반하여 아키텍처를 조정하고 적응할 수 있도록 합니다. 이런 방식으로 우리는 필요에 따라 성장하고 변화할 수 있는 적응형 생태계를 구축하고 있습니다.

<div class="content-ad"></div>

# 결론

요약하면, 수직 슬라이스 아키텍처와 모듈형 단일체 아키텍처는 종종 단일 배포 가능한 단위로 구현되지만, 애플리케이션의 내부 구성 요소를 구조화하는 방식에서 큰 차이가 있습니다. 수직 슬라이스 아키텍처는 비즈니스 기능을 기반으로 코드를 구성하는 데 중점을 두며, 각 기능 슬라이스가 특정 기능 또는 비즈니스 요구 사항에 필요한 모든 것을 포함하는 독립적인 단위입니다. 이 접근 방식은 애플리케이션 내에서 독립성과 모듈성을 향상시키며, 각 슬라이스가 별도로 개발, 테스트 및 유지보수될 수 있도록 합니다.

모듈형 단일체 아키텍처는 일반적으로 단일 배포 가능한 단위이지만, 전체 애플리케이션 구조의 모듈식 조직화에 중점을 두고 있습니다. 이는 애플리케이션을 구별된 모듈로 나누어 각각이 독립적인 도메인 로직과 책임을 갖도록 하는 것을 포함합니다. 이러한 모듈은 동일한 배포 가능한 단위의 일부이지만 가능한 독립적으로 설계되어 유지보수 및 확장성을 용이하게 합니다.

따라서 핵심적인 차이점은 단일 배포 가능한 단위 내에서 모듈성과 구조화에 대한 접근 방식입니다. 수직 슬라이스 아키텍처는 특징 중심적이고 교차 절단 형태의 모듈성 접근 방식을 제공하는 반면, 모듈형 단일체 아키텍처는 보다 격리된 모듈 기반 접근 방식에 중점을 두고 있습니다. 두 가지 방식 모두 모놀리틱 애플리케이션 내에서 유지보수성과 확장성을 달성하기 위해 다른 구조적 패러다임을 통해 노력합니다.

<div class="content-ad"></div>

건배!