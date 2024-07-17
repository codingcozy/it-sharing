---
title: "React 구성 패턴 총정리 효과적으로 컴포넌트 재사용하는 방법"
description: ""
coverImage: "/assets/img/2024-07-09-ReactCompositionalPatterns_0.png"
date: 2024-07-09 13:28
ogImage:
  url: /assets/img/2024-07-09-ReactCompositionalPatterns_0.png
tag: Tech
originalTitle: "React Compositional Patterns"
link: "https://medium.com/@matthewmacfarquhar/react-compositional-patterns-39398c183386"
---

![React 18 Compositional Patterns](/assets/img/2024-07-09-ReactCompositionalPatterns_0.png)

# 소개

최근 React 18에 대한 책과 관련된 최상의 실천 방법에 대해 공부하고 있습니다. 이 책을 통해 가장 흥미로운 내용을 추출하여 React에 관한 이 시리즈 기사에 담아낼 것입니다. 이 첫 번째 기사에서는 React에서 사용되는 몇 가지 구성 패턴을 소개하여 코드베이스를 더 강력하고 재사용 가능하며 테스트 가능하게 만드는 방법에 대해 자세히 설명하겠습니다.

# 컨테이너 패턴

<div class="content-ad"></div>

![React Compositional Patterns](/assets/img/2024-07-09-ReactCompositionalPatterns_1.png)

이 첫 번째 패턴은 컨테이너 패턴입니다. 이 패턴의 목표는 관심사를 분리하고 React 애플리케이션 내의 컴포넌트의 유지보수 및 재사용성을 향상시키는 것입니다.

이 패턴은 로직 및 상태 관리를 처리하는 "컨테이너" 컴포넌트와 props에 기반하여 UI를 렌더링하는 "프레젠테이션" 컴포넌트를 생성하는 것을 포함합니다.

이로 인해 컴포넌트의 재사용이 쉬워지며, 컴포넌트가 단순화되고, 테스트가 쉬워지며, 상태 관리와 UI 간의 관심사 분리가 정교해집니다.

<div class="content-ad"></div>

아래는 우리가 Geolocation 프레젠테이션 컴포넌트와 상태를 관리하고 프레젠테이션 부분에 전달하는 GeolocationContainer를 가지고 있는 패턴의 예시입니다.

```js
type GeoLocationProps = {
    latitude: number,
    longitude: number
}
export const Geolocation: FC<GeoLocationProps> = ({latitude, longitude}) => {
    return (
        <div>
            <h1>Geolocation:</h1>
            <div>Latitude: {latitude}</div>
            <div>Longitude: {longitude}</div>
        </div>
    )
}

export const GeolocationContainer = () => {
    const [latitude, setLatitude] = useState<number | null>(null)
    const [longitude, setLongitude] = useState<number | null>(null)

    const handleSuccess = ({coords: {latitude, longitude}: {coords: {latitude: number, longitude: number}) => {
        setLatitude(latitude)
        setLongitude(longitude)
    }

    useEffect(() => {
        if (navigator.geolocation) {
            navigator.geolocation.getCurrentPosition(handleSuccess)
        }
    }, [navigator])

    return <Geolocation latitude={latitude ?? 0} longitude={longitude ?? 0}/>
}
```

이러한 방식으로 프레젠테이션 컴포넌트는 완전히 별도의 UX 디자이너/프론트엔드 개발자에 의해 소유될 수 있으며 논리적인 컨테이너 컴포넌트는 다른 개발자가 처리할 수 있습니다.

이 패턴은 복잡한 대규모 프로젝트에 적합하지만, 각각의 단일 컴포넌트에 대해 두 개의 컴포넌트를 생성하는 것은 작은 프로젝트에 대한 상당한 오버헤드입니다.

<div class="content-ad"></div>

# 고차 컴포넌트

![고차 컴포넌트 이미지](/assets/img/2024-07-09-ReactCompositionalPatterns_2.png)

고차 컴포넌트(Higher Order Components, HOCs)는 컴포넌트 로직을 재사용할 수 있게 해주며 코드 모듈화를 촉진하고 컴포넌트 조합을 강화합니다. 이를 통해 우리는 일반적인 고차 컴포넌트 속성을 모듈화하고 기본 컴포넌트를 여러 개의 고차 컴포넌트를 통해 전달하여 구성 가능한 컴포넌트를 만들 수 있습니다.

아래 예시에서는 innerWidth 프롭을 가져오는 어떤 컴포넌트에도 쉽게 HOC에서 제공되는 창 innerWidth를 가져올 수 있도록 합니다.

<div class="content-ad"></div>

withInnerWidth은 컴포넌트를 입력으로 받고 컴포넌트를 반환하기 때문에 하이어오더 컴포넌트입니다. 일반 컴포넌트는 몇 가지 프롭을 입력으로 받아 일부 DOM 노드를 반환할 수 있습니다.

```js
export const withInnerWidth = (Component: ComponentType<any>) => (props: any) => {
  const [innerWidth, setInnerWidth] = useState(window.innerWidth);
  const handleResize = () => {
    setInnerWidth(window.innerWidth);
  };

  useEffect(() => {
    window.addEventListener("resize", handleResize);
    return () => {
      window.removeEventListener("resize", handleResize);
    };
  }, []);

  return <Component {...props} innerWidth={innerWidth} />;
};

const MyComponent: FC<{ innerWidth: number }> = ({ innerWidth }) => {
  console.log("window.innerWidth", innerWidth);
  return <div>{innerWidth}</div>;
};

export const MyComponentWithInnerWidth = withInnerWidth(MyComponent);
```

우리는 이러한 하이어오더 컴포넌트를 원하는 만큼 연쇄적으로 연결하고, 이 기능을 자식에게 전달할 수 있습니다. 자식은 트리 상위의 하이어오더 컴포넌트에서 제공하는 기능을 프롭으로 기대할 수 있습니다.

HOC의 단점은 깊게 중첩될 수 있어 개발자가 코드베이스를 학습하는 데 혼란스러울 수 있고, 어떤 HOC가 원인이 되는 버그를 해결하기 위해 디버깅하는 데 어려움을 겪을 수 있다는 점입니다.

<div class="content-ad"></div>

# 자식으로서의 기능

React 구성에서 자식으로서의 기능 패턴은 함수를 프롭으로 전달하여 컴포넌트 간에 로직을 공유하는 기술입니다. 이 패턴을 사용하면 특정 동작이나 상태를 캡슐화한 재사용 가능한 컴포넌트를 만들 수 있으며, 컴포넌트의 사용자가 렌더링을 제어할 수 있습니다.

이것은 우리가 가지고 있는 컨테이너 아이디어와 얼마나 비슷한지를 말합니다. 다만 외부 부모가 일반적으로 보다 일반적으로 구성됩니다.

예를 들어, URL에서 가져온 데이터를 처리하는 부모 컴포넌트가 있고, 해당 데이터를 자식 함수로 전달할 것입니다. 이 패턴을 사용하면 URL에서 데이터를 필요로 하는 모든 UI 컴포넌트를 Fetch 컴포넌트 중 하나로 래핑할 수 있으므로, 모든 컴포넌트에서 데이터를 가져오는 논리를 반복하지 않아도 됩니다.

<div class="content-ad"></div>

```js
인터페이스 FetchProps {
    url: 문자열
    children: (props: { data: any; loading: boolean; error: any }) => ReactNode
}

const Fetch: FC<FetchProps> = ({ url, children }) => {
    const [data, setData] = useState(null)
    const [loading, setLoading] = useState(true)
    const [error, setError] = useState(null)

    useEffect(() => {
        const fetchData = async () => {
            try {
                // const response = await fetch(url)
                // const result = await response.json()
                const result: any = [{name: "bill"}, {name: "bob"}]
                setData(result)
            } catch (error: any) {
                setError(error)
            } finally {
                setLoading(false)
            }
        }

        fetchData()
    }, [url])

    return children({ data, loading, error })
}

인터페이스 DataDisplayProps {
    data: any
    loading: boolean
    error: any
}
const DataDisplay: FC<DataDisplayProps> = ({data, loading, error}) => {
    if (loading) return <p>Loading...</p>
    if (error) return <p>Error: {error.message}</p>

    return (
        <div>
            <h1>불러온 데이터:</h1>
            <pre>{JSON.stringify(data, null, 2)}</pre>
        </div>
    )
}

const FunctionAsAChild: FC = () => {
    const url = "http://api.example.com/data"

    return (
        <Fetch url={url}>
            {({data, loading, error}) => (
                <DataDisplay data={data} error={error} loading={loading}/>
            )}
        </Fetch>
    )
}
```

만약 여러 컴포넌트에서 사용될 수 있는 일반 기능이 있다면, 해당 로직을 캡슐화하는 부모 프롭을 생성하고 함수를 자식으로 사용하는 패턴을 사용하여 중복되는 코드 양을 줄일 수 있습니다.

# 결론

이 문서에서는 세 가지 주요 React 합성 패턴을 살펴보았습니다. 컴포넌트를 단순화하고 각 컴포넌트가 다루어야 하는 관심사를 분리하는 데 도움이 되는 패턴입니다. 이러한 합성 패턴의 많은 부분이 컴포넌트의 일부 기능을 격리하고 결과 데이터를 프롭을 통해 자식에게 전달하는 데 관련되어 있어 모든 데이터 처리 및 렌더링을 단일 컴포넌트에 넣는 대안을 깔끔하게 해 줍니다.
