---
layout: default
title: 플러그인
url: /plugins
previous: /faqs
next: /credits
---

<em class="t-gray">RubyGems 플러그인 API를 사용하는 확장기능</em>

RubyGems 1.3.2부터 RubyGems는 $LOAD\_PATH나 gems에 설치한 플러그인을 로드합니다.
플러그인은 'rubygems\_plugin'(.rb, .so 등)이라는 이름이어야 하며 gem #require\_path의
루트에 있어야 합니다. 플러그인은 로드 이후 Gem::find\_files로 찾을 수 있습니다.
플러그인을 구현하실 때에는 여러 버전의 플러그인이 설치되어 있다면 플러그인
파일이 여러 번 로드될 수 있음에 주의하셔야 합니다.

다음 RubyGems 플러그인의 목록은 완전하지 않습니다. 빠진 플러그인을 알고 있다면,
편하게 이 페이지를 업데이트 해주세요.

* [executable-hooks](#executablehooks)
* [gem-browse](#gembrowse)
* [gem-ctags](#gemctags)
* [gem-empty](#gemempty)
* [gem\_info](#geminfo)
* [gem-init](#geminit)
* [gem-compare](#gemcompare)
* [gem-man](#gemman)
* [gem-nice-install](#gemniceinstall)
* [gem-orphan](#gemorphan)
* [gem-patch](#gempatch)
* [gem-toolbox](#gemtoolbox)
* [gem-wrappers](#gemwrappers)
* [graph](#graph)
* [maven-gem](#mavengem)
* [open-gem](#opengem)
* [PushSafety](#pushsafety)
* [rbenv-rehash](#rbenvrehash)
* [rubygems-desc](#rubygemsdesc)
* [rubygems-openpgp](#rubygemsopenpgp)
* [rubygems-sandbox](#rubygemssandbox)
* [rubygems\_snapshot](#rubygemssnapshot)
* [rubygems-tasks](#rubygemstasks)

<a id="executablehooks"> </a>

## executable-hooks

[https://github.com/mpapis/executable-hooks](https://github.com/mpapis/executable-hooks)

executables 플러그인을 지원하도록 rubygems를 확장합니다.

gem의 lib 디렉터리 안에 rubygems\_executable\_plugin.rb를 만드세요.

    Gem.execute do |original_file|
      warn("Executing: #{original_file}")
    end


<a id="gembrowse"> </a>

## gem-browse

[https://github.com/tpope/gem-browse](https://github.com/tpope/gem-browse)

다음 명령을 추가합니다.

- `gem edit` 편집기에서 gem을 열기
- `gem open` 이름으로 편집기에서 gem을 열기
- `gem clone` GitHub에서 gem을 클론하기
- `gem browse` 브라우저에서 gem의 홈페이지를 열기

<a id="gemempty"> </a>

## gem-empty

[https://github.com/rvm/gem-empty](https://github.com/rvm/gem-empty)

현재 `GEM_HOME`에서 모든 gem을 제거하는 `gem empty` 명령을 추가합니다.

<a id="gemctags"> </a>

## gem-ctags

[https://github.com/tpope/gem-ctags](https://github.com/tpope/gem-ctags)


이미 설치된 gem에 ctags를 추가하고, gem을 설치할 때 ctags를 호출하는 `gem ctags`
명령을 추가합니다.

<a id="geminfo"> </a>

## gem\_info

[https://github.com/oggy/gem\_info](https://github.com/oggy/gem_info)

이름과 버전으로 퍼지 매칭(fuzzy matching)을 하는 `gem info` 명령을 추가합니다.
스크립트에서 사용하도록 설계되었습니다.

<a id="geminit"> </a>

## gem-init

[https://github.com/mwhuss/gem-init](https://github.com/mwhuss/gem-init)

뼈대 gem을 만드는 `gem init`를 추가합니다.

<a id="gemcompare"> </a>

## gem-compare

[https://github.com/strzibny/gem-compare](https://github.com/strzibny/gem-compare)

gemspec 값, gemspec, Gemfile 의존성, 파일을 비교해 릴리스된 .gem 파일의 업스트림
변경을 확인하는 `gem compare` 명령을 추가합니다.


<a id="gemman"> </a>

## gem-man

[https://github.com/defunkt/gem-man](https://github.com/defunkt/gem-man)

`gem man` 명령으로 gem의 man 페이지를 확인할 수 있습니다.

<a id="gemniceinstall"> </a>

## gem-nice-install

[https://github.com/voxik/gem-nice-install](https://github.com/voxik/gem-nice-install)

표준 `gem install` 명령을 사용해 gem의 바이너리 확장에 필요한 시스템 의존성을
설치하려 합니다. 현재 페도라에서만 동작하지만 지원하는 곳이 늘어나길 기대합니다.

<a id="gemorphan"> </a>

## gem-orphan

[https://github.com/sakuro/gem-orphan](https://github.com/sakuro/gem-orphan)

다른 gem과 의존성이 없는 gem을 찾아 나열하는 `gem orphan` 명령을 추가합니다.

<a id="gempatch"> </a>

## gem-patch

[https://github.com/strzibny/gem-patch](https://github.com/strzibny/gem-patch)

`.gem` 파일에 직접 패치를 적용할 수 있는 `gem patch` 명령을 추가합니다.
RubyGems 1.8과 2.0 양쪽 다 지원합니다.

<a id="gemtoolbox"> </a>

## gem-toolbox

[https://github.com/gudleik/gem-toolbox](https://github.com/gudleik/gem-toolbox)

여섯 개의 명령을 추가합니다.

- `gem open` - 기본 편집기에서 gem을 열기
- `gem cd` - 작업 디렉터리를 gem의 소스 루트로 변경
- `gem readme` - gem의 readme 파일을 찾아서 표시하기
- `gem history` - gem의 changelog를 찾아서 표시하기
- `gem doc` - 기본 브라우저에서 gem의 문서를 열기
- `gem visit` - 기본 브라우저에서 gem의 홈페이지를 열기

<a id="gemwrappers"> </a>

## gem-wrappers

[https://github.com/rvm/gem-wrappers](https://github.com/rvm/gem-wrappers)

cron이나 다른 시스템에서 gem을 쉽게 사용하기 위해 gem 래퍼를 만듭니다.
기본적으로 gem이 설치될 때 래퍼도 설치됩니다.

이 명령어들을 추가합니다.

- `gem wrappers regenerate` - 모든 gem 실행파일 래퍼를 강제로 재작성하기
- `gem wrappers` - 현재 설정을 보여주기

<a id="graph"> </a>

## graph

[https://github.com/seattlerb/graph](https://github.com/seattlerb/graph)

gem 의존성 그래프를 graphviz의 dot 형식으로 출력하는 `gem graph` 명령을
추가합니다.

<a id="mavengem"> </a>

## maven\_gem

[https://github.com/jruby/maven\_gem](https://github.com/jruby/maven_gem)

Maven으로 배포되는 자바 라이브러리를 gem처럼 설치하는 `gem maven`을 추가합니다.

<a id="opengem"> </a>

## open\_gem

[https://github.com/adamsanderson/open\_gem](https://github.com/adamsanderson/open_gem)

두 개의 명령을 추가합니다.

- `gem open` 기본 편집기에서 gem을 열기
- `gem read` 기본 브라우저에서 gem의 rdoc 열기

<a id="pushsafety"> </a>

## PushSafety

[https://github.com/jdleesmiller/push\_safety](https://github.com/jdleesmiller/push_safety)

실수로 비공개 gem을 공개 RubyGems 저장소에 `gem push` 하는 것을 방지하도록
화이트리스트에 적용합니다.

<a id="rbenvrehash"> </a>

## rbenv-rehash

[https://github.com/scoz/rbenv-rehash](https://github.com/scoz/rbenv-rehash)

gem을 설치하고 제거할 때 자동으로 `rbenv rehash`를 실행해줍니다.

<a id="rubygemsdesc"> </a>

## rubygems-desc

[https://github.com/chad/rubygems-desc](https://github.com/chad/rubygems-desc)

gem을 이름으로 설명해주는 `gem desc`를 추가합니다.

<a id="rubygemsopenpgp"> </a>

## rubygems-openpgp

[https://github.com/grant-olson/rubygems-openpgp](https://github.com/grant-olson/rubygems-openpgp)

gem에 OpenPGP 서명을 하기 위한 명령어와 플래그를 추가합니다.

- `gem sign foo.gem`으로 gem 서명
- `gem verify foo.gem --trust`로 gem 인증
- `gem build foo.gemspec --sign`으로 빌드할 때 서명
- `gem install foo --verify --trust`로 설치할 때 인증

<a id="rubygemssandbox"> </a>

## rubygems-sandbox

[https://github.com/seattlerb/rubygems-sandbox](https://github.com/seattlerb/rubygems-sandbox)

커맨드 라인 gem 도구와 의존성을 `gem sandbox` 명령어로 관리합니다. 이것으로
글로벌 rubygems 저장소 밖에 flay와 rdoc을 설치할 수 있습니다.

<a id="rubygemssnapshot"> </a>

## rubygems\_snapshot

[https://github.com/rogerleite/rubygems\_snapshot](https://github.com/rogerleite/rubygems_snapshot)

나중에 가져오기 편하게 현재 gem을 하나의 파일로 추출해주는 `gem snapshot`을
추가합니다.

<a id="rubygemstasks"> </a>

## rubygems-tasks

[https://github.com/postmodern/rubygems-tasks](https://github.com/postmodern/rubygems-tasks#readme)

rubygems-tasks는 독립적이고 거슬리지 않게 루비 gem을 빌드, 설치, 릴리스하는 Rake
작업를 제공합니다.
