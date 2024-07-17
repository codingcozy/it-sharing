---
title: " Dev Drive로 Windows에서 Node 모듈을 더 빠르게 사용하는 방법"
description: ""
coverImage: "/assets/img/2024-07-12-NodeModulesNowFasterOnWindowsWithDevDrive_0.png"
date: 2024-07-12 18:35
ogImage:
  url: /assets/img/2024-07-12-NodeModulesNowFasterOnWindowsWithDevDrive_0.png
tag: Tech
originalTitle: "🚀 Node Modules Now Faster On Windows With Dev Drive"
link: "https://medium.com/@tomaszs2/node-modules-now-faster-on-windows-with-dev-drive-a7eadfbd5662"
---

<img src="/assets/img/2024-07-12-NodeModulesNowFasterOnWindowsWithDevDrive_0.png" />

# **Windows에 있는 새로운 기능인 dev drive를 소개합니다! node_modules 및 다른 거대한 요소들을 더욱 빠르게 이용해보세요**

프로그래머들이 가장 많이 사용하는 전문 및 개인용 운영 체제인 Windows입니다.

하지만 최근에 이것들이 쌓이면서 약간의 단점이 있습니다. 그렇지만 한 해 전에 발표된 새로운 멋진 기능 하나가 있습니다. 그런데 아무도 주목하지 않았습니다.

<div class="content-ad"></div>

Windows의 문제는 폴더가 100k개의 1KB 파일을 가지고 있을 것을 기대하지 않는다는 것입니다. 솔직히 아무도 그렇게 예상하지 않았습니다. 그러나 웹 애플리케이션이 구축되는 방식이 그런 상황을 초래합니다.

npm을 사용하거나 호환되는 다른 패키지 관리자를 사용하면 프로젝트의 모든 종속성이 node_modules 폴더에 설치됩니다. 또한 이러한 종속성들의 종속성도 그렇습니다.

갑자기 1MB 웹 애플리케이션을 개발하려면 역사 박물관의 도서관을 컴퓨터에 저장해야 하는 것이 분명해집니다.

Windows는 많은 작은 파일을 다루는 데 극도로 느리다는 것으로 수 년 동안 유명합니다. FAT, NTFS, HDD 또는 SDD를 사용하든 상관없이 항상 시간이 소요됩니다.

<div class="content-ad"></div>

어떤 사람들은 파일 형식을 탓하고, 일부는 모든 이 파일들을 처리하는 데 시간이 걸리는 Windows Defender를 탓합니다.

이 문제에 대한 다양한 접근 방식이 있습니다. 예를 들어, pnpm은 npm에 비해 node_modules를 처리하는 속도가 훨씬 빠릅니다.

다른 옵션은 RAM에 가상 디스크를 생성하고 프로젝트를 저장하는 것입니다. 속도 향상이 놀라울 정도로 빠릅니다. 처음에 그 디스크로 파일을 복사하는 데 시간이 걸리고 전원이 꺼진 경우 미 커밋된 변경 사항을 잃을 수 있는 위험이 있습니다. 그러나 이 점은 노트북에서 해소할 수 있습니다.

또 다른 해결책은 WSL을 사용하는 것입니다. Windows Subsystem for Linux입니다.

<div class="content-ad"></div>

`<table>` 태그를 Markdown 형식으로 변경하세요.

<div class="content-ad"></div>

보통 가상 드라이브를 설정하고 프로젝트 파일을 그곳에 그냥 복사하면 됩니다.

공식적으로 성능이 20-40% 향상되며, 제3자 벤치마크 결과가 10% 향상을 보여주었습니다.

성능과 DirectX를 향상시키는 또 다른 방법이 있습니다.

항상 백업을 정기적으로 하는 것을 강력히 권장합니다. 그 기능을 사용하지 않더라도 말이죠.

<div class="content-ad"></div>

해당 기능은 멋지고 시도할 가치가 있어서, 원한다면 지금 읽기를 멈추고 시도해 보는 것이 적절할 수도 있어요.

공식 문서를 읽어본 바에 의하면 가상 드라이브를 다른 디스크로 복사하는 것은 권장하지 않는다고 합니다.

이렇게 하면 새로운 기능이 얼마나 실험적인지 궁금해집니다.

또 다른 궁금증은 왜 Microsoft가 기존의 NFTS 형식을 개선하거나 전체 디스크를 위한 새로운 형식을 도입하지 않는지에 대한 것인데요.

<div class="content-ad"></div>

제가 두 번이나 node_modules 작업으로 디스크를 망가뜨렸던 후 최종 질문은, 왜 우리가 여전히 그것을 사용하는 건가요?

100k개의 극히 작은 파일을 가지고 있다는 것은 성능 측면에서 가장 최악의 일입니다. 이 폴더를 복사하는 것은 어떤 시스템에서도 악몽이며, 삭제하는 것 또한 마찬가지입니다.

그 개수의 작은 파일에 대해 최적화하기 힘든 읽기 및 쓰기 작업에 대해서 언급할 것도 있습니다.

패키지 관리자에 이어서 이를 개선하기 위한 노력이 있지만, 오히려 점점 더 많은 오픈 소스 유지자들이 모든 것에 종속성을 사용하지 않기로 결정하고 있습니다. "0 dependencies(0 종속성)"라는 것이 점점 더 많은 라이브러리에서 발견될 수 있는 것입니다.

<div class="content-ad"></div>

JavaScript으로만 한정되지 않고 다른 프로그래밍 언어와 앱에도 영향을 미치는 문제입니다.

Microsoft가 적어도 문제를 해결할 필요가 있다는 것을 인식했다는 것은 좋은 일입니다. 시장에서 유일한 운영 체제가 아니라는 것이며, 리눅스도 매년 코앞에 있는 것입니다.

코딩을 좋아하시면 더 많은 코딩 기사를 구독해주세요!
