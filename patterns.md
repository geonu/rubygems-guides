---
layout: default
title: 패턴
url: /patterns
previous: /ssl-certificate-update
next: /specification-reference
---

<em class="t-gray">gem 사용자와 다른 개발자의 인생을 쉽게 할 일반적인 기법</em>

* [일관된 이름짓기](#consistent-naming)
* [유의적 버전](#semantic-versioning)
* [의존관계 선언](#declaring-dependencies)
* [코드 로드하기](#loading-code)
* [gem 사전 릴리스](#prerelease-gems)

일관된 이름짓기
---------------------
{:#consistent-naming}

> 컴퓨터 과학에서 어려운 것은 캐시 무효화와 이름짓기 단 두 가지뿐이다.
> -[Phil Karlton](http://martinfowler.com/bliki/TwoHardThings.html)

### 파일 이름

`lib`과 `bin` 안의 gem 파일 이름은 일치하도록 짓습니다. [gem 만들어
보기]({{ site.baseurl }}/make-your-own-gem) 가이드에서 사용했던
[hola](https://github.com/qrush/hola) gem은 훌륭한 예제입니다.

    % tree
    .
    ├── Rakefile
    ├── bin
    │   └── hola
    ├── hola.gemspec
    ├── lib
    │   ├── hola
    │   │   └── translator.rb
    │   └── hola.rb
    └── test
        └── test_hola.rb

실행 파일과 `lib` 안의 초기 파일은 같은 이름으로 지었습니다. 개발자는 아무
문제 없이 `require 'hola'`를 호출해서 쉽게 넘어갈 수 있습니다.

### gem 이름짓기

gem 이름짓기는 중요합니다. gem의 이름을 고르기 전에
[RubyGems.org](http://rubygems.org)와 [GitHub](https://github.com/search)에서
이미 사용 중인지 살펴봅시다. 모든 배포되는 gem의 이름은 반드시 유일해야 합니다.
좋은 이름을 찾았다면 [이름짓기에 관한
권장사항]({{ site.baseurl }}/name-your-gem)을 읽어보세요.

유의적 버전
-----------------------
{:#semantic-versioning}

버전 관리 정책은 버전 번호를 할당하기 위한 규칙의 집합일 뿐입니다. 간단할 수도
있고(예를 들어, 버전 번호가 1부터 시작하고 순서대로 증가하는 버전) 굉장히
이상할 수도 있습니다.(예를 들어, Knuth의 TeX 프로젝트는 파이에 숫자를 순서대로
더하는 3, 3.1, 3.14, 3.141, 3.1415 같은 버전 번호사용합니다.)

RubyGems 팀은 gem 개발자에게 gem 버전을 [유의적
버전](http://semver.org/lang/ko/) 표준에 맞추도록 강력히 권장하고 있습니다.
RubyGems 라이브러리 자체는 엄격한 버전 정책을 강제하지는 않습니다만, "변칙적인"
정책은 gem을 사용하는 커뮤니티에 민폐가 될 수 있습니다.

`Stack` 클래스에 `push`와 `pop` 기능을 제공하는 'stack' gem을 운영하고 있다고
해봅시다. 유의적 버전을 사용할 경우, `CHANGELOG`는 이렇게 될 것입니다.

* **Version 0.0.1**: The initial `Stack` class is released.
* **Version 0.0.2**: Switched to a linked list implementation because it is
  cooler.
* **Version 0.1.0**: Added a `depth` method.
* **Version 1.0.0**: Added `top` and made `pop` return `nil` (`pop` used to
  return the old top item).
* **Version 1.1.0**: `push` now returns the value pushed (it used it return
  `nil`).
* **Version 1.1.1**: Fixed a bug in the linked list implementation.
* **Version 1.1.2**: Fixed a bug introduced in the last fix.

유의적 버전은 이렇게 요약됩니다.

* **수(PATCH)** `0.0.x` 수준 변경은 작은 버그 수정 같은 기존 버전과 호환되는 구현
  수정입니다.
* **부(MINOR)** `0.x.0` 수준 변경은 새로운 기능 추가 같은 기존 버전과 호환되는 API
  변경입니다.
* **주(MAJOR)** `x.0.0` 수준 변경은 업데이트 하면 기존 사용자의 코드를 망가트리는
  기존 버전과 *호환되지 않는* API 변경입니다.

의존관계 선언
--------------------------
{:#declaring-dependencies}

gem은 다른 gem과 함께 동작합니다. 여기에 다른 것과 함께 사용해야 할 때 확인해야
할 몇 가지 팁을 소개하겠습니다.

### 런타임과 개발

RubyGems의 의존성은 런타임 의존성과 개발 의존성, 크게 두 가지 "타입"이 있습니다.
런타임 의존성은([레일즈](http://rubygems.org/gems/rails)는
[activesupport](http://rubygems.org/gems/activesupport)가 필요한 것처럼) gem이
동작할 때 필요한 것입니다.

개발 의존성은 다른 사람이 gem을 수정하려 할 때 유용한 것입니다. 개발 의존성을
지정하면, 다른 개발자는 `gem install --dev your_gem`을 실행해 RubyGems가 양쪽
(런타임, 개발)의존성을 다 가져오게 할 수 있습니다. 일반적으로 개발 의존성에는
테스트 프레임워크와 빌드 시스템이 들어갑니다.

gemspec에 의존성을 설정하는 것은 간단합니다. 그냥 `add_runtime_dependency`와
`add_development_dependency`를 사용하시면 됩니다.

    Gem::Specification.new do |s|
      s.name = "hola"
      s.version = "2.0.0"
      s.add_runtime_dependency "daemons",
        ["= 1.1.0"]
      s.add_development_dependency "bourne",
        [">= 0"]

### `gem`을 gem 안에서 사용하지 말 것

특정 gem 버전을 사용하고 있는지 확인하기 위해 이런 코드를 사용하고 계실 수도
있습니다.

    gem "extlib", ">= 1.0.8"
    require "extlib"

이것은 gem을 사용하는 애플리케이션에서는 합리적으로 보일 수 있습니다.(물론
[Bundler](http://bundler.io) 같은 도구를 사용할 수도 있습니다만) 하지만
gem에서는 이렇게 하면 **안 됩니다**. 대신 RubyGems가 사용자 대신 의존성 로드를
관리할 수 있도록 gemspec의 의존성을 사용해야 합니다.

### 비관적(Pessimistic) 버전 제한

gem이 버전 관리 방식에 [유의적 버전](http://semver.org/lang/ko/)을 잘 따르고
있다면, 다른 루비 개발자도 애플리케이션에서 사용할 gem을 제한하는 데 이를 활용할
수 있습니다.

이런 gem의 릴리스가 있다고 해봅시다.

* **Version 2.1.0** — Baseline
* **Version 2.2.0** — Introduced some new (backward compatible) features.
* **Version 2.2.1** — Removed some bugs
* **Version 2.2.2** — Streamlined your code
* **Version 2.3.0** — More new features (but still backwards compatible).
* **Version 3.0.0** — Reworked the interface. Code written to version 2.x might
  not work.

gem을 사용할 사람은 2.2.0 버전이 자신의 소프트웨어에서 동작하는 것을 확인 했지만
2.1.0 버전에는 필요한 기능이 없었습니다. gem에 넣을 의존성(아니면 Bundler의
`Gemfile`에 넣을)은 아마 이렇게 될 것입니다.

    # gemspec
    spec.add_runtime_dependency 'library',
      '>= 2.2.0'

    # bundler
    gem 'library', '>= 2.2.0'

이것은 "낙관적인(optimistic)" 버전 제약입니다. 2.x부터의 모든 변경은 내
소프트웨어에서 동작 *할 것이라는* 이야기입니다만, 3.0.0 버전에서는 그렇지 않을
것입니다.

여기서 "비관적으로" 할 수도 있습니다. 이 예제는 명시적으로 코드를 망가트릴 수
있는 버전을 제외하고 있습니다.

    # gemspec
    spec.add_runtime_dependency 'library',
      ['>= 2.2.0', '< 3.0']

    # bundler
    gem 'library', '>= 2.2.0', '< 3.0'

RubyGems에는 일반적으로 [twiddle-wakka](http://robots.thoughtbot.com/post/2508037841/twiddle-wakka)라
알려진 단축 표기법이 있습니다.

    # gemspec
    spec.add_runtime_dependency 'library',
      '~> 2.2'

    # bundler
    gem 'library', '~> 2.2'

버전 번호의 `PATCH` 수준을 적지 않은 것에 주목하세요. `~> 2.2.0`이라 했다면,
`['>= 2.2.0', '< 2.3.0']`과 같은 뜻이 됩니다.

새로운 하위호환 버전을 사용하고 싶지만 특정 버그 픽스도 필요하다면 복합
요구사항을 사용할 수 있습니다.

    # gemspec
    spec.add_runtime_dependency 'library', '~> 2.2', '>= 2.2.1'

    # bundler
    gem 'library', '~> 2.2', '>= 2.2.1'

여기서 중요한 점은 다른 사람이 당신의 gem을 사용할 수도 있다는 것입니다.
그러므로 앞으로의 릴리스에서 잠제적인 버그/실패로부터 자유로우려면 가능한 한
`>=` 대신 `~>`를 사용하세요.

> 애플리케이션에서 많은 의존성을 관리한다면, [Bundler](http://bundler.io)나
> [Isolate](https://github.com/jbarnette/isolate) 같은 여러 gem의 복잡한
> 버전 명세를 잘 관리해주는 툴을 살펴보시길 바랍니다.

사전 릴리스와 통상 릴리스를 동시에 허용하려면 복합 요구사항을 사용하면 됩니다.

    # gemspec
    spec.add_runtime_dependency 'library', '>= 2.0.0.a', '< 3'

사전 릴리스 버전에 `~>`를 사용하면 사전 릴리스 버전만으로 제한하게 됩니다.

### RubyGems require하기

한줄 요약: 하지마세요.

이 줄...

    require 'rubygems'

...은 RubyGems는 이미 gem을 require할 때 로드했기 때문에 gem 코드에서
필요 없습니다. 코드에서 `require 'rubygems'`가 없는 것은 gem이 RubyGems
클라이언트 없이도 쉽게 사용할 수 있다는 의미입니다.

더 자세한 정보는 [Ryan Tomayko
님의](http://tomayko.com/writings/require-rubygems-antipattern) 원문을
읽어보세요.

코드 로드하기
----------------
{:#loading-code}

RubyGems의 핵심은 RubyGems는 루비의 `require` 구문이 새로운 코드를 가져올 때
사용하는 `$LOAD_PATH`의 관리를 도와주기 위해 존재한다는 것입니다. 코드를
올바르게 로드하고 있는지 확인하기 위해 할 수 있는 것이 몇 가지 있습니다.

### 전역 로드 경로 존중하기

gem 파일을 꾸릴 때, `lib` 디렉터리 안에 있는 것을 조심해야 합니다. 설치된
모든 gem은 자신의 `lib` 디렉터리를 `$LOAD_PATH` 안에 넣습니다. 이 말은 `lib`
디렉터리의 최상위 레벨의 어떤 파일도 require 될 수 있다는 이야기입니다.

예를 들어, 이런 구조의 `foo` gem이 있다고 합시다.

    .
    └── lib
        ├── foo
        │   └── cgi.rb
        ├── erb.rb
        ├── foo.rb
        └── set.rb

이는 커스텀 `erb`와 `set` 파일이 gem 안에 있기 때문에 무해해 보일 수 있습니다.
하지만, 전혀 무해하지 않습니다. 이 gem을 require하는 사람은
[ERB](http://ruby-doc.org/stdlib/libdoc/erb/rdoc/classes/ERB.html)나 루비 표준
라이브러리의 [Set](http://www.ruby-doc.org/stdlib/libdoc/set/rdoc/classes/Set.html)
클래스를 가져올 수 없게 됩니다.

이것을 피하는 가장 좋은 방법은 `lib` 아래의 다른 디렉터리에 파일을 두는
것입니다. 일반적인 관례는 일관성 있게 gem의 이름과 같은 폴더에 파일을 넣는
것입니다. 예를 들자면 `lib/foo/cgi.rb` 같은 식으로요.

### 연관된 파일 require하기

gem은 안에서 다른 루비 파일을 가져오기 위해 `__FILE__`을 사용할 필요가 없습니다.
이런 코드는 gem 안에서 놀랄만큼 일반적입니다.

    require File.join(
              File.dirname(__FILE__),
              "foo", "bar")

아니면 이런 것도요.

    require File.expand_path(File.join(
              File.dirname(__FILE__),
              "foo", "bar"))

고치기는 쉽습니다. 그냥 로드 경로에 적절하게 파일을 require하면 됩니다.

    require 'foo/bar'

아니면 require_relative를 사용하세요.

    require_relative 'foo/bar'

[gem 만들어 보기]({{ site.baseurl }}/make-your-own-gem) 가이드에 실제로 동작하는
테스트를 포함한 좋은 예제가 있습니다. 그 gem의 코드는
[GitHub](https://github.com/qrush/hola)에도 있습니다.

### 로드 경로 망치기

gem은 `$LOAD_PATH` 변수를 고치면 안 됩니다. RubyGems가 관리해 드립니다. 이런
코드는 필요하지 않습니다.

    lp = File.expand_path(File.dirname(__FILE__))
    unless $LOAD_PATH.include?(lp)
      $LOAD_PATH.unshift(lp)
    end

아니면 이런 코드도...

    __DIR__ = File.dirname(__FILE__)

    $LOAD_PATH.unshift __DIR__ unless
      $LOAD_PATH.include?(__DIR__) ||
      $LOAD_PATH.include?(File.expand_path(__DIR__))

RubyGems가 gem을 활성화할 때, 패키지의 `lib` 폴더를 `$LOAD_PATH`에 추가해 다른
라이브러리나 애플리케이션에서 require할 수 있도록 준비해 줍니다. `lib` 폴더의
파일은 전부 로드할 수 있다고 생각하는 것이 안전합니다.

gem 사전 릴리스
-------------------
{:#prerelease-gems}

많은 gem 개발자가 큰 gem 릴리스 전에 릴리스 준비가 되었는지 테스트하기 위한 gem
버전을 두거나 "edge" 릴리스를 합니다. RubyGems에는 "사전 릴리스" 개념이 있습니다.
이는 베타, 알파를 포함해 뭐든 정규 릴리스가 아닌 것에 사용할 수 있습니다.

이것을 활용하기는 쉽습니다. gem 버전에 한두 단어를 추가하기만 하면 됩니다.
예를 들어, 사전 릴리스 gemspec의 `version` 필드는 아마 이렇게 될 것입니다.

    Gem::Specification.new do |s|
      s.name = "hola"
      s.version = "1.0.0.pre"

사전 릴리스 버전의 다른 예에는 아마 `2.0.0.rc1`, `1.5.0.beta.3` 같은 것이 있을 수
있습니다. 그냥 안에 문자를 넣도록 설정하면 됩니다. 이런 gem은 `--pre`를 붙여서
설치할 수 있습니다. 이렇게요.

    % gem list factory_girl -r --pre

    *** REMOTE GEMS ***

    factory_girl (2.0.0.beta2, 2.0.0.beta1)
    factory_girl_rails (1.1.beta1)

    % gem install factory_girl --pre
    Successfully installed factory_girl-2.0.0.beta2
    1 gem installed

Credits
-------

이 가이드는 다음 출처에서 내용을 가져왔습니다.

* [Rubygems Good Practice](http://yehudakatz.com/2009/07/24/rubygems-good-practice/)
* [Gem Packaging: Best Practices](http://weblog.rubyonrails.org/2009/9/1/gem-packaging-best-practices)
