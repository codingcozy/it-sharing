---
title: "PyEnv와 Poetry 튜토리얼 데이터 사이언스를 위한 궁극의 세팅 방법"
description: ""
coverImage: "/assets/img/2024-07-19-PyEnvPoetryTutorialUltimateDataScienceSetup_0.png"
date: 2024-07-19 13:36
ogImage: 
  url: /assets/img/2024-07-19-PyEnvPoetryTutorialUltimateDataScienceSetup_0.png
tag: Tech
originalTitle: "PyEnv , Poetry Tutorial Ultimate Data Science Setup"
link: "https://medium.com/towards-data-science/pyenv-poetry-tutorial-ultimate-data-science-setup-af0de6d47355"
---


![이미지](/assets/img/2024-07-19-PyEnvPoetryTutorialUltimateDataScienceSetup_0.png)

저는 아나콘다를 큰 팬은 아니에요. 데이터 과학과 파이썬 생태계에 입문할 때는 훌륭한 도구이지만, 설치 과정이 너무 번거롭고 방해적이라고 생각해요.

그래서 저는 파이썬 관리에는 PyEnv를, 의존성 관리에는 Poetry를 사용하는 것을 선호해요. 이 조합은 가벼우면서도 비교적 작고 이해하기 쉬운 기술이라서 저에게 아주 잘 맞아요.

이 글에서는 프로젝트에 PyEnv와 Poetry를 사용하는 방법에 대한 간단한 튜토리얼을 제공하려고 해요!

<div class="content-ad"></div>

# PyEnv

## 이게 뭐죠?

그럼 PyEnv는 뭘까요? Python Environment의 줄임말로 여러분의 Python 버전을 관리하는 도구입니다. 창조자들이 GitHub 페이지에서 인용한 대로:

PyEnv를 사용하면 여러 디렉토리와 프로젝트에서 각기 다른 Python 버전을 가지고 지정할 수 있어 원활한 의존성 관리를 보장합니다.

<div class="content-ad"></div>

하나의 Python 버전을 설치하여 모든 것을 설치하는 것은 끔찍한 일이 될 수 있습니다. 이후에 패키지 버전 충돌을 초래할 수 있기 때문입니다.

## 설치

이 부분은 터미널/셸과 기본 명령어에 대해 알고 있다고 가정합니다. 더 자세히 알고 싶다면 이전 글을 확인해보세요.

나는 Homebrew를 통해 PyEnv를 설치하는 것을 추천합니다. 이것은 'MacOS를 위한 누락된 패키지 관리자'로 불리며, 맥에서 코딩하는 사람들에게 매우 유용합니다.

<div class="content-ad"></div>

홈브류를 설치하려면 아래 웹사이트에서 제공하는 명령어를 사용하면 됩니다:

```js
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

홈브류가 설치되었는지 확인하려면 brew help를 실행하세요 (brew 명령어 목록과 사용 방법이 표시됩니다).

그런 다음, PyEnv를 설치하세요.

<div class="content-ad"></div>

```js
brew install pyenv
```

PyEnv가 설치되었는지 확인하려면 pyenv --version을 실행해주세요.

대부분의 현대 Mac은 기본 명령 줄 해석기로 Zsh를 사용하므로, 컴퓨터가 .zshrc 파일을 사용한다고 가정합니다.

그런 다음 PyEnv를 경로에 추가하려면 터미널에서 다음 세 명령을 먼저 실행해야 합니다.

<div class="content-ad"></div>

```js
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
echo '[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc
echo 'eval "$(pyenv init -)"' >> ~/.zshrc
```

위 코드들을 살펴보겠습니다:

```js
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
```

- ~/.zshrc 파일에 export PYENV_ROOT="$HOME/.pyenv" 를 추가합니다.
- PYENV_ROOT는 pyenv가 설치된 디렉토리를 가리키는 환경 변수로, 일반적으로 홈 디렉토리 하위 .pyenv 디렉토리에 설치됩니다.

<div class="content-ad"></div>

```js
echo '[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc
```

- ~/.zshrc 파일에 `[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"`를 추가합니다.
- 이 명령은 $PYENV_ROOT/bin 디렉토리가 있는지 확인하여 ([[ -d $PYENV_ROOT/bin ]]), 있다면 $PYENV_ROOT/bin을 PATH에 추가합니다.
- 이렇게 함으로써 pyenv가 셸의 검색 경로에 포함되어 pyenv 명령을 사용할 수 있게 됩니다.

```js
echo 'eval "$(pyenv init -)"' >> ~/.zshrc
```

- ~/.zshrc 파일에 `eval "$(pyenv init -)"`을 추가합니다.
- `eval "$(pyenv init -)"`은 새 셸을 시작할 때 pyenv를 초기화합니다. 이 명령은 환경 변수 및 함수를 설정하여 pyenv가 Python 버전을 올바르게 관리할 수 있도록 합니다.


<div class="content-ad"></div>

새 터미널 창을 열거나 셸을 재시작하여 이 변경 사항이 적용되도록 해보세요. 아래는 .zshrc 파일 내의 예시 행입니다. 여기서 다른 내용은 무시하고 마지막 세 줄만 보세요.


$ source ~/.zshrc
$ echo 'export PATH="$HOME/.pyenv/bin:$PATH"' >> ~/.zshrc
$ echo 'eval "$(pyenv init --path)"' >> ~/.zshrc


또는 `where pyenv`를 실행하여 PyEnv 이진 파일이 올바른 위치에 있는지 확인할 수도 있어요.

## 사용법

<div class="content-ad"></div>

파이썬 버전을 설치하려면 pyenv install 명령어를 사용해야 합니다.

```js
pyenv install 3.11.4
```

모든 파이썬 버전을 확인하려면 pyenv install -l을 실행하면 됩니다.

설치된 파이썬 버전을 확인하려면 pyenv versions를 사용합니다. 아래는 제가 사용 중인 모습입니다.

<div class="content-ad"></div>


<img src="/assets/img/2024-07-19-PyEnvPoetryTutorialUltimateDataScienceSetup_2.png" />

현재 디렉토리(홈)에 있다는 것을 알 수 있습니다. 시스템 Python이 현재 설정되어 있습니다. 다음 명령어를 사용하여 Python 버전을 변경할 수 있습니다:

- pyenv shell `version` — 현재 쉘 세션에만 선택
- pyenv local `version` — 특정 디렉토리에 대한 Python 버전
- pyenv global `version` — 사용자에 대한 전역 Python 버전

이제 pyenv global 3.11.4를 실행한 다음 pyenv versions를 실행하면 


<div class="content-ad"></div>


![이미지](/assets/img/2024-07-19-PyEnvPoetryTutorialUltimateDataScienceSetup_3.png)

지금 제 Python 버전이 3.11.4로 업데이트되었어요.

만약 이 디렉토리만을 위해 Python 버전을 변경하고 싶다면, 다음과 같이 `pyenv local 버전` 명령어를 사용할 수 있어요:

![이미지](/assets/img/2024-07-19-PyEnvPoetryTutorialUltimateDataScienceSetup_4.png)


<div class="content-ad"></div>

효과적으로 PyEnv를 사용하는 데 필요한 모든 명령어를 배웠어요!

## 작동 방식

PyEnv가 실제로 어떻게 작동하는지 궁금할 수 있어요. 깃허브 페이지에서 저자들이 훌륭한 요약을 제공했어요:

PATH는 python이나 pip과 같은 명령어를 실행할 때 검색되는 모든 디렉토리를 포함한 변수예요. 예를 들어, echo $PATH 명령어를 호출한 후 내 PATH는 다음과 같이 보일 거예요.

<div class="content-ad"></div>


/Users/egorhowell/.pyenv/shims
/opt/homebrew/bin:/opt/homebrew/sbin
/Users/egorhowell/.local/bin
/usr/local/bin
/System/Cryptexes/App/usr/bin
/usr/bin:/bin:/usr/sbin
/sbin
/var/run/com.apple.security.cryptexd/codex.system/bootstrap/usr/local/bin
/var/run/com.apple.security.cryptexd/codex.system/bootstrap/usr/bin
/var/run/com.apple.security.cryptexd/codex.system/bootstrap/usr/appleinternal/bin
/Applications/iTerm.app/Contents/Resources/utilities


제 PATH에서 첫 번째 줄을 주목해주세요. 이것은 Shim으로, Python 명령어를 PyEnv로 리디렉션합니다.

# Poetry

## 이것은 무엇인가요?


<div class="content-ad"></div>

이제 우리는 Python 버전을 설치했으며 패키지 및 의존성 관리자를 가져와볼 시간입니다.

이를 위해 Poetry를 사용할 것입니다. 공식 웹사이트에서 Poetry를 다음과 같이 설명하고 있습니다:

Poetry는 다른 패키지 관리자와 비교했을 때 많은 이점을 가지고 있으며, 이 Reddit 스레드에서 좋은 개요를 제공합니다.

저는 Poetry를 선호하는데, 사용하기 쉽고 신뢰할 수 있으며 가벼워서입니다. 처음에 말했듯이 conda는 무겁지만 poetry는 매우 편리합니다.

<div class="content-ad"></div>

## 설치하기

제 Medium-Articles git 레포지토리에 Poetry를 설치할 예정이에요. 먼저, 로컬 Python 버전을 pyenv local 3.11.8로 설정할 거예요.

![이미지](/assets/img/2024-07-19-PyEnvPoetryTutorialUltimateDataScienceSetup_5.png)

Poetry를 설치하려면 아래 명령어를 실행하세요.

<div class="content-ad"></div>

```shell
curl -sSL https://install.python-poetry.org | python -
```

여기서 Python은 PyEnv를 사용하여 설정한 Python 버전입니다.

Poetry가 설치되었는지 확인하려면 `poetry --version`을 실행하면 됩니다. 이상 없을 거에요!

## 사용법

<div class="content-ad"></div>

시작하려면, poetry new project 명령어를 실행할 수 있지만, 이미 기존 프로젝트가 있는 경우 poetry init를 호출해주세요.

모든 질문에 답한 후에는 현재 프로젝트 디렉토리에 pyproject.toml 파일이 생성됩니다. 이 파일은 열어보면 다음과 같아야 합니다.


[tool.poetry]
name = "medium-articles"
version = "0.1.0"
description = "내 미디엄 블로그 코드를 위한 Poetry 환경"
authors = ["Egor Howell <egorhowellbusiness@gmail.com>"]
readme = "README.md"

[tool.poetry.dependencies]
python = "^3.11"

[build-system]
requires = ["poetry-core"]
build-backend = "poetry.core.masonry.api"


pyproject.toml은 Poetry 자체가 아니라 Python과 Pep에 속하는 것이라는 점을 유의해주세요.

<div class="content-ad"></div>

패키지와 의존성을 추가하려면 단순히 tool.poetry.dependencies 섹션에 그것들을 지정하면 됩니다. 예를 들어:

```js
[tool.poetry]
name = "medium-articles"
version = "0.1.0"
description = "나의 미디엄 블로그 코드를 위한 Poetry 환경"
authors = ["Egor Howell <egorhowellbusiness@gmail.com>"]
readme = "README.md"

[tool.poetry.dependencies]
python = "^3.11"
numpy = "^1.21.4"


[build-system]
requires = ["poetry-core"]
build-backend = "poetry.core.masonry.api"
```

poetry add 명령어를 사용하여도 추가할 수 있습니다:

```js
poetry add pandas
```

<div class="content-ad"></div>

표를 마크다운 형식으로 변경하면 pyproject.toml 파일에 나타납니다.

```js
[tool.poetry]
name = "medium-articles"
version = "0.1.0"
description = "Poetry environmment for my medium blogs code"
authors = ["Egor Howell <egorhowellbusiness@gmail.com>"]
readme = "README.md"

[tool.poetry.dependencies]
python = "^3.11"
numpy = "^1.21.4"
pandas = "^2.2.2"

[build-system]
requires = ["poetry-core"]
build-backend = "poetry.core.masonry.api"
```

모든 종속성을 추가한 후 poetry install을 실행하여 환경에 모든 패키지와 종속성을 설치할 수 있습니다.

poetry env info 명령을 실행하여 올바르게 설치되었는지 확인할 수 있습니다.

<div class="content-ad"></div>


가상환경
Python:         3.11.8
구현체:          CPython
경로:           /Users/egorhowell/Library/Caches/pypoetry/virtualenvs/medium-articles-lMbxnF7V-py3.11
실행 파일:       /Users/egorhowell/Library/Caches/pypoetry/virtualenvs/medium-articles-lMbxnF7V-py3.11/bin/python
유효 여부:       True

기본
플랫폼:     darwin
운영 체제:  posix
Python:     3.11.8
경로:       /Users/egorhowell/.pyenv/versions/3.11.8
실행 파일: /Users/egorhowell/.pyenv/versions/3.11.8/bin/python3.11


프로젝트 루트 디렉토리에 poetry.lock 파일이 생성됩니다. 이 파일에는 모든 패키지와 의존성이 포함되어 있습니다. 이 파일은 나중에 재현 가능한 환경을 활성화하는 데 사용되며 버전 관리에 중요합니다.

코드를 실행하려면 poetry run python your_script.py를 사용할 수 있습니다.

대안으로 poetry shell을 사용할 수도 있습니다. 이는 poetry 환경으로 이동하고 각 명령을 실행할 때 매번 poetry run을 지정할 필요 없이 사용할 수 있습니다.

<div class="content-ad"></div>

제가 편리하게 작업할 수 있도록 PyCharm에서 기본 환경으로 설정해 놓았어요. 이 작업 방법에 대한 안내를 원하신다면 [여기](링크)를 참조해주세요.

## 기타 특징

Poetry가 훌륭한 이유는 다음과 같은 많은 유용한 기능들이 있어요:

- 의존성을 그룹으로 관리할 수 있어요. 예를 들어, 프로젝트를 테스트하거나 모델을 빌드하는 데 필요한 의존성만 따로 관리할 수 있어요.
- poetry update를 사용하여 모든 의존성을 최신 버전으로 업데이트할 수 있어요.
- 프로젝트와 의존성을 PyPi 휠과 패키지로 게시할 수 있어요.

<div class="content-ad"></div>

여기서 모든 Poetry 명령어와 함께 완전한 목록을 찾을 수 있어요.

## 요약

Poetry와 PyEnv는 프로젝트를 관리하는 데 뛰어난 두 가지 도구이며, 꼭 시도해보기를 추천해요. 사용하기 간단하고 가벼워서 의존성 오류를 계속 다루는 대신 모델 구축에 집중할 수 있어요.

## 참고문헌

<div class="content-ad"></div>

- 깊이있는 PyEnv 튜토리얼.
- PyEnv GitHub 문서.
- Poetry 튜토리얼 문서.
- Poetry GitHub 문서.
- Poetry에 대한 또 다른 훌륭한 튜토리얼.

# 또 다른 것!

저는 매주 실무 데이터 과학자로서 팁과 조언을 공유하는 무료 뉴스레터 'Dishing the Data'를 운영하고 있습니다. 또한 구독하면 무료 데이터 과학 이력서를 받을 수 있습니다!

# 저와 연락하기!

<div class="content-ad"></div>

- LinkedIn, X (Twitter), 또는 Instagram.
- 기술적인 데이터 과학 및 머신 러닝 개념을 배울 수 있는 내 YouTube 채널!