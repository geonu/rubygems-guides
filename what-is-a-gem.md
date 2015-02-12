---
layout: default
title: gem이란 무엇인가?
url: /what-is-a-gem
previous: /rubygems-basics
next: /make-your-own-gem
---

<em class="t-gray">RubyGem안에 무엇이 들어있는지 미스터리 파해치기.</em>

gem의 구조
------------------

각 gem은 이름, 버전, 플렛폼을 가지고 있습니다. 예를 들어
[rake](http://rubygems.org/gems/rake) gem은 `0.8.7` 버전 (2009년 5월 기준)을
가지고 있고 플렛폼은 `ruby`입니다. 이 말은 Ruby가 돌아가는 어떤 플렛폼에서도
사용가능하다는 의미입니다.

플렛폼은 CPU 아키텍쳐, 운영체제 타입이나 가끔은 운영체제 버전에 기반합니다.
예를 들어 플렛폼에는 "x86-mingw32" 나 "java" 같은게 있습니다. 플렛폼은
같은 플렛폼에서 빌드한 루비에서만 gem이 동작하는 것을 나타냅니다. RubyGems은
자동으로 당신의 플렛폼에 맞는 버전을 다운로드합니다. 자세한 내용은
`gem help platform`을 보세요.

gem의 내부에는 다음과 같은 구성요소가 있습니다.

* (테스트와 지원 유틸리티를 포함한) 코드
* 문서
* gemspec

각 gem은 같은 코드 구성의 표준 구조를 따릅니다.

    % tree freewill
    freewill/
    ├── bin/
    │   └── freewill
    ├── lib/
    │   └── freewill.rb
    ├── test/
    │   └── test_freewill.rb
    ├── README
    ├── Rakefile
    └── freewill.gemspec

여기 gem의 주요 구성요소를 보실수 있습니다.

* `lib` 디렉토리는 gem을 위한 코드를 포함합니다.
* 개발자가 어떤 테스트 프레임워크를 사용하냐에 따라, `test`나 `spec` 디렉토리는
  테스트를 포함합니다.
* gem은 보통 [rake](http://rake.rubyforge.org/) 프로그램에서 테스트 자동화, 코드
  생성, 다른 작업을 수행하는데 사용하는  `Rakefile`을 가지고 있습니다.
* 이 gem은 `bin` 디렉토리안에 실행 파일도 가지고 있습니다. `bin` 디렉토리는 gem이
  인스톨 될 때 유저의 `PATH`에 로드될 것 입니다.
* 문서는 보통 `README`파일이나 코드 안에 포합됩니다. gem을 인스톨하면, 문서는
  자동으로 생성됩니다. 대부분의 gem은 [RDoc](http://rdoc.sourceforge.net/doc/)
  문서를 사용하지만, [YARD](http://yardoc.org/) 문서를 사용하는 것도 있습니다.
* 마지막 조각은 gem에 관한 정보를 가지고 있는 gemspec입니다.
  gem의 파일들, 테스트 정보, 플렛폼, 버전 번호 등이 저자의 이메일과 이름과 함께
  여기에 배치됩니다.

[gemspec 파일에 대한 더 자세한 정보]({{ site.baseurl }}/specification-reference/)

[gem 만들어 보기]({{ site.baseurl }}/make-your-own-gem/)

Gemspec
-----------

6개월 후에 당신과 애플리케이션, gem의 사용자는 누가 언제 gem을 작성했고 무엇을
하는지를 알고 싶어할 것입니다. gemspec은 이런 정보를 가지고 있습니다.

여기 gemspec 파일의 예제가 있습니다. 더 자세한 내용은 [gem 만들어
보기]({{ site.baseurl }}/make-your-own-gem)에서 배우실 수 있습니다.

    % cat freewill.gemspec
    Gem::Specification.new do |s|
      s.name        = 'freewill'
      s.version     = '1.0.0'
      s.summary     = "Freewill!"
      s.description = "I will choose Freewill!"
      s.authors     = ["Nick Quaranto"]
      s.email       = 'nick@quaran.to'
      s.homepage    = 'http://example.com/freewill'
      s.files       = ["lib/freewill.rb", ...]
    end

gemspec에 관한 더 자세한 정보는 전체 [사양 레퍼런스]({{ site.baseurl }}/specification-reference)의
각 메타데이터의 상세정보를 확인하세요.

Credits
-------

이 가이드는 [Gonçalo Silva](https://twitter.com/#!/goncalossilva)님이
docs.rubygems.org에 올린 오리지널 투토리얼과
Gem Sawyer, Modern Day Ruby Warrior를 차용했습니다.
