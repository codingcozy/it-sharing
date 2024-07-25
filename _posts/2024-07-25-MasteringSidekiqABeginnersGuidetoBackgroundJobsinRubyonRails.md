---
title: " Sidekiq 완벽 가이드 루비 온 레일즈에서 배경 작업 시작하기"
description: ""
coverImage: "/assets/img/2024-07-25-MasteringSidekiqABeginnersGuidetoBackgroundJobsinRubyonRails_0.png"
date: 2024-07-25 12:02
ogImage: 
  url: /assets/img/2024-07-25-MasteringSidekiqABeginnersGuidetoBackgroundJobsinRubyonRails_0.png
tag: Tech
originalTitle: " Mastering Sidekiq A Beginners Guide to Background Jobs in Ruby on Rails"
link: "https://medium.com/@patrykrogedu/mastering-sidekiq-a-beginners-guide-to-background-jobs-in-ruby-on-rails-b8be07ff1ebf"
---


![링크](/assets/img/2024-07-25-MasteringSidekiqABeginnersGuidetoBackgroundJobsinRubyonRails_0.png)

사이드킥의 환상적인 세계에 오신 것을 환영합니다! 여기에 계신다면 루비 온 레일즈 애플리케이션에서 백그라운드 작업을 실행하고 싶지만 미치지 않고 싶으신 것 같군요.

# Sidekiq 젬 설치하기

우선 첫 번째로 해야 할 일은 Sidekiq 젬을 설치해야 합니다. 이를 위해 Gemfile에 한 줄을 추가하면 됩니다. 하지만 잠깐! 그 코드를 거기에 추가하기 전에, 나중에 우연히 발견할 수 있는 누군가를 위해 Sidekiq이란 무엇인지 설명하는 주석을 추가하자구요.

<div class="content-ad"></div>

```js
# Sidekiq을 사용하여 백그라운드 작업을 실행합니다
gem "sidekiq"
```

그 멋진 젬을 추가한 후에는 다음을 실행하세요:

```js
bundle install
```

알고 계실 거예요 — 많은 출력이 나오지만, 뭔가 잘못되지 않는 이상 무시하셔도 됩니다 (만약 문제가 발생한다면, 행운을 빕니다).


<div class="content-ad"></div>

# 구성 옵션 설정하기

자, 이제 구성 옵션 몇 가지를 자세히 알아보겠습니다. 다음은 설정해야 할 네 가지 주요 영역입니다:

- Redis 연결: Sidekiq가 필요한 것처럼 나는 아침 커피를 필요로 합니다.
- 동시성: 서버 프로세스 당 실행할 스레드 수를 결정하세요.
- 시간 초과: 작업을 강제로 종료하기 전에 대기해야 하는 시간을 설정하세요.
- 대기열: 나중에 쉽게 참고할 수 있도록 대기열 목록을 하드코딩하세요.

# Redis에 연결하기

<div class="content-ad"></div>

우리의 예제 앱에서는 dotenv gem과 도커 환경을 사용하여 환경 변수를 관리하고 있습니다. .env.development 파일은 다음과 같이 보일 것입니다:

```js
SIDEKIQ_REDIS_URL=redis://redis:6379/1
```

그냥 REDIS_PROVIDER를 SIDEKIQ_REDIS_URL을 가리키도록 설정해주시면 됩니다:

```js
REDIS_PROVIDER=SIDEKIQ_REDIS_URL
```

<div class="content-ad"></div>

이 간접성은 Redis 인스턴스가 다운되어 처리를 해야할 때 등 수동 장애 교대가 필요한 경우 편리합니다.

# Sidekiq 옵션 구성

이제 config/sidekiq.yml에 대해 이야기해 보겠습니다. 이 파일은 내장된 루비 코드를 사용할 수 있게 해줍니다:

```js
:concurrency: <%= ENV.fetch("SIDEKIQ_CONCURRENCY") { 5 } %>
:timeout: <%= ENV.fetch("SIDEKIK_TIMEOUT_SECONDS") { 25 } %>
:queues:
 - default
```

<div class="content-ad"></div>

만약 누군가가 병행성이나 제한 시간 설정을 다시 배포하지 않고 변경하고 싶을 때 (뉴그겐 그렇게 할 시간이 없잖아요?), 그들은 그냥 환경 변수를 조정할 수 있습니다.

# 웹 UI 구성

우리 설정 선데이에 선렁 맛있는 체리는 Sidekiq Web UI를 설정하는 것입니다. 이를 위해 config/routes.rb 파일의 맨 위에 다음 라인을 추가해주세요:

```js
require "sidekiq/web" # Sidekiq Web UI 가져오기

Rails.application.routes.draw do
  mount Sidekiq::Web => "/sidekiq" # /sidekiq URL에서 사용 가능합니다!
end
```

<div class="content-ad"></div>

서버를 다시 시작한 후 http://localhost:3000/sidekiq로 이동하면, Sidekiq이 자신의 역할을 얼마나 잘 (또는 잘못) 수행하고 있는지에 대한 다양한 통계를 보여주는 멋진 인터페이스가 표시됩니다.

<img src="/assets/img/2024-07-25-MasteringSidekiqABeginnersGuidetoBackgroundJobsinRubyonRails_1.png" />

팁: 민감한 정보를 노출시키면 혼란 (그리고 해커)이 발생할 수 있으므로 접근을 안전하게 설정하는 것을 잊지 마세요!

# 로컬에서 Sidekiq 실행하기

<div class="content-ad"></div>

만약 Procfile을 사용 중이라면, Procfile.dev을 열어 아래 내용을 추가해주세요:

```js
web: PORT=3000 bin/rails s
sidekiq: bundle exec sidekiq # 백그라운드 작업을 실행시켜볼게요!
```

# 코드를 백그라운드 작업으로 이동

안녕하세요 여러분! 이제 제가 가장 좋아하는 부분 중 하나인 긴 실행 시간이 소요되는 작업을 웹 요청 주기에서 사라지게 하고 사랑스러운 Sidekiq과 백그라운드 작업으로 옮기는 것에 대해 이야기해보겠습니다!

<div class="content-ad"></div>

주문 생성 로직을 가지고 있는데, 결제를 호출하고 이메일을 보내는 과정이 필요하다고 가정해 봅시다. 이러한 서비스가 응답할 때 사용자가 정말로 기다리지 않고 있나요? 마치 러시아 시간에 교통체증에 갇힌 것처럼. 그래서 새로운 작업인 CompleteOrderJob을 만들어 보겠습니다.

## CompleteOrderJob 생성하기

먼저 app/jobs/complete_order_job.rb에 작업 파일을 만들어 보겠습니다:

```js
class CompleteOrderJob
  include Sidekiq::Job
  
  def perform(order_id)
    order = Order.find(order_id) # 주문 찾기!
    OrderCreator.new.complete_order(order) # 처리를 위임합니다.
  end
end
```

<div class="content-ad"></div>

# 주문 생성 로직 업데이트

이제 app/services/order_creator.rb의 주문 생성 로직을 업데이트해 봅시다. 여기서 느린 서비스 호출을 제거하고 대신 새 작업을 대기열에 넣는 코드로 대체할 겁니다.

변경 후 예상 코드는 다음과 같습니다:

```js
def create_order(order)
  # 주문 처리를 위한 로직

  if order.save 
    CompleteOrderJob.perform_async(order.id) # 해당 작업을 대기열에 넣어요!
  end 
end
```

<div class="content-ad"></div>

위의 업데이트가 완료되었습니다. 앱을 다시 시작하고 사용자들이 영원히 기다리지 않고 커피를 즐기는 동안 주문이 비동기적으로 처리되는 것을 확인해 보세요!

# 테스트 시간!

드디어 테스트할 시간입니다! 모든 것이 예상대로 작동하는지 확인하고 싶습니다. 그러나 여기서 문제가 발생할 수 있습니다. 비동기 작업을 테스트하는 것은 고양이를 몰아내는 것 같은 느낌이 들 수 있습니다.

우리는 Sidekiq이 제공하는 두 가지 테스트 모드를 활용할 것입니다:

<div class="content-ad"></div>

- 가짜 모드: 작업은 대기열에 넣지만 실행되지 않습니다.
- 인라인 모드: 작업은 대기열에 넣자마자 즉시 실행됩니다.

대부분의 경우 - 우리의 경우를 포함해서 - 가짜 모드가 선호됩니다. 이 모드는 테스트를 구현 세부사항과 너무 강하게 결합하지 않고 실행 타이밍을 제어할 수 있기 때문입니다.

# 테스트 모드 활성화

테스트 헬퍼(spec/spec_helper.rb)에 다음 줄을 추가하세요.

<div class="content-ad"></div>

```js
require "sidekiq/testing" # 테스트 지원을 위해 작업을 활성화합니다.
```

그리고 이전 specs에서의 이전 작업을 모두 지우세요:

```js
# 이전 specs에서의 sidekiq 작업을 지웁니다.
RSpec.configure do |config|
  config.before(:each) do
    Sidekiq::Worker.clear_all
  end
end
```

그런 다음 각 관련 테스트 케이스에 주문을 넣은 후 .drain_all을 호출하여 모든 대기 중인 작업이 단언문 실행 전에 실행되도록합니다.

<div class="content-ad"></div>

모든 것이 순조롭게 진행되면서 — 정말로요 — 마치 무지개를 따라 뽀송뽀송 걸어다니는 유니콘처럼 모든 테스트가 통과되는 것을 보실 거에요!

그러니까요 — 루비 온 레일즈에서 Sidekiq를 효과적으로 구성하고 사용하는 완벽한 안내서가 여기 있습니다.