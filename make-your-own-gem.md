---
layout: default
title: gem 만들어 보기
url: /make-your-own-gem
previous: /what-is-a-gem
next: /gems-with-extensions
---

<em class="t-gray">처음부터 끝까지, 루비 코드를 어떻게 gem으로 패키징 하는지 배우기</em>

* [서론](#introduction)
* [당신의 첫 번째 gem](#your-first-gem)
* [파일을 더 require해 보기](#requiring-more-files)
* [실행 파일 추가하기](#adding-an-executable)
* [테스트 작성하기](#writing-tests)
* [코드 문서화하기](#documenting-your-code)
* [마치며](#wrapup)

서론
----------------
{:#introduction}

직접 만든 gem을 만들고 배포하는 것은 RubyGems에 들어있는 툴 덕분에 간단합니다.
간단한 "hello world" gem을 만들어 봅시다. 가볍게 집에서 함께해보세요! 여기서
만들 gem의 코드는 [GitHub](https://github.com/qrush/hola)에 올려두었습니다.

당신의 첫 번째 gem
------------------
{:#your-first-gem}

`hola` gem은 단 하나의 루비 파일과 gemspec으로 시작합니다.
아마 배포하시려면 (`hola_yourusername`같은) 새로운 이름이 필요할 것입니다. gem의
이름을 지으실 때는 [기본 추천 사항]({{ site.baseurl }}/patterns/#consistent-naming)을
위한 명명 패턴 가이드를 살펴보세요.

    % tree
    .
    ├── hola.gemspec
    └── lib
        └── hola.rb

패키지의 코드는 `lib` 안에 들어갑니다. `require 'hola'`가 실행될 때 로드되기 때문에,
관례에서는 일반적으로 gem의 이름과 *같은* 루비 파일 *하나*만 가지고 있길
권장합니다. 그 파일은 API와 gem 코드의 설정을 담당합니다.

`lib/hola.rb` 안의 코드는 뼈대만 있습니다. 이 코드는 그저 gem에서 어떤 출력을
생성할 수 있다는 것을 보여줄 뿐입니다.

    % cat lib/hola.rb
    class Hola
      def self.hi
        puts "Hello world!"
      end
    end

gemspec은 gem에 무엇이 있고, 누가 만들었고, 버전이 무엇인지 정의합니다.
gemspec은 [RubyGems.org](http://rubygems.org)에 대한 인터페이스이기도 합니다. gem
페이지에서 확인할 수 있는 (예를 들어 [jekyll](http://rubygems.org/gems/jekyll)의)
모든 정보는 gemspec 파일에서 옵니다.

    % cat hola.gemspec
    Gem::Specification.new do |s|
      s.name        = 'hola'
      s.version     = '0.0.0'
      s.date        = '2010-04-28'
      s.summary     = "Hola!"
      s.description = "A simple hello world gem"
      s.authors     = ["Nick Quaranto"]
      s.email       = 'nick@quaran.to'
      s.files       = ["lib/hola.rb"]
      s.homepage    =
        'http://rubygems.org/gems/hola'
      s.license       = 'MIT'
    end

> description 항목은 이 예제에서 보는 것보다 훨씬 길어질 수 있습니다. description이
> `/^== [A-Z]/`에 매치하면, RubyGems의 웹 사이트에 표시할 때는 [RDoc의 마크업
> 포매터](https://github.com/rdoc/rdoc)를 통해 실행됩니다. 데이터를 사용하는
> 다른 곳은 이 마크업을 이해하지 못할 수도 있으므로 주의하세요.

친숙해 보이나요? gemspec도 루비입니다. 그래서 스크립트를 감싸 파일 이름을
생성하고 버전을 올릴 수 있습니다. gemspec에 넣을 수 있는 필드는 매우 많습니다.
전부 다 보고 싶으시면 전체 [레퍼런스]({{ site.baseurl }}/specification-reference)를
참조하세요.

gemspec을 만들었으면, 거기에서 gem을 빌드 할 수 있습니다. 그리고 생성된 gem을
테스트하기 위해 로컬에 설치해 볼 수 있습니다.

    % gem build hola.gemspec
    Successfully built RubyGem
    Name: hola
    Version: 0.0.0
    File: hola-0.0.0.gem

    % gem install ./hola-0.0.0.gem
    Successfully installed hola-0.0.0
    1 gem installed

물론, 테스트가 끝난 건 아닙니다. 마지막으로 gem을 `require` 하고 사용해 보죠.

    % irb
    >> require 'hola'
    => true
    >> Hola.hi
    Hello world!

> 1.9.2보다 이전 루비를 사용한다면, 셰션을 시작할 때 `irb -rubygems`로 시작하거나
> irb를 기동한 후에 rubygems 라이브러리를 require해야 합니다.

이제 hola를 루비 커뮤니티에 공유할 수 있습니다. RubyGems.org 사이트에 계정을
가지고 있다면 명령어 한 줄로 배포할 수 있습니다. RubyGems 계정을 컴퓨터에
설정하는 법은 다음과 같습니다.

    $ curl -u qrush https://rubygems.org/api/v1/api_key.yaml >
    ~/.gem/credentials; chmod 0600 ~/.gem/credentials

    Enter host password for user 'qrush':

> curl, OpenSSL, 인증에 문제가 있다면, 위에 있는 URL을 브라우저의 주소창에
> 넣으시면 됩니다. 브라우저는 RubyGems.org에 로그인할지 물어보고, 유저이름과
> 패스워드를 입력하면, api_key.yaml 파일을 다운로드하려 합니다. 이 파일을
> 'credentials'이라 이름 지어 ~/.gem에 저장하시면 됩니다.

이 설정이 끝나면, gem을 RubyGems에 올릴 수 있습니다.

    % gem push hola-0.0.0.gem
    Pushing gem to RubyGems.org...
    Successfully registered gem: hola (0.0.0)

이 짧은 시간에(보통은 1분 이하) gem은 누구나 설치할 수 있게 될 것입니다.
[RubyGems.org 사이트](https://rubygems.org/gems/hola)에서 확인하거나 RubyGems가
깔린 어떤 컴퓨터에서든 받을 수 있습니다.

    % gem list -r hola

    *** REMOTE GEMS ***

    hola (0.0.0)

    % gem install hola
    Successfully installed hola-0.0.0
    1 gem installed

루비와 RubyGems로 정말 쉽게 코드를 공유할 수 있습니다.

파일을 더 require해 보기
------------------------
{:#requiring-more-files}

모든 것을 한 파일에 넣으면 확장이 어렵습니다. 이 gem에 코드를 좀 더 넣어보죠.

    % cat lib/hola.rb
    class Hola
      def self.hi(language = "english")
        translator = Translator.new(language)
        translator.hi
      end
    end

    class Hola::Translator
      def initialize(language)
        @language = language
      end

      def hi
        case @language
        when "spanish"
          "hola mundo"
        else
          "hello world"
        end
      end
    end

파일이 점점 혼잡해지네요. `Translator`를 새 파일로 쪼개봅시다. 전에 말했듯이
gem의 루트 파일은 gem 코드의 로드를 담당합니다. 다른 파일은 보통 `lib` 아래의
gem과 같은 이름의 디렉토리에 넣습니다. 이 파일은 이렇게 쪼갤 수 있습니다.

    % tree
    .
    ├── hola.gemspec
    └── lib
        ├── hola
        │   └── translator.rb
        └── hola.rb

`Translator`는 이제 `lib/hola` 안에 있습니다. 이 파일은 `lib/hola.rb`에서 쉽게
`require`문으로 가져올 수 있습니다. `Translator`의 코드는 그렇게 많이 바뀌지
않습니다.

    % cat lib/hola/translator.rb
    class Translator
      def initialize(language)
        @language = language
      end

      def hi
        case @language
        when "spanish"
          "hola mundo"
        else
          "hello world"
        end
      end
    end

하지만 `hola.rb` 파일에는 `Translator`를 로드하기 위한 코드가 조금 추가되었습니다.

    % cat lib/hola.rb
    class Hola
      def self.hi(language = "english")
        translator = Translator.new(language)
        translator.hi
      end
    end

    require 'hola/translator'

> Gotcha:
> 새로 만들어진 폴더/파일을 위해 hola.gemspec 파일에 항목을 추가하는 것을 잊지
> 마세요.

    % cat hola.gemspec
    Gem::Specification.new do |s|
    ...
    s.files       = ["lib/hola.rb", "lib/hola/translator.rb"]
    ...
    end

> 위와 같이 수정하지 않으면, 새 폴더는 설치된 gem 파일에 포함되지 않을 것입니다.

이걸 한번 해봅시다. 먼저 `irb`를 기동합니다.

    % irb -Ilib -rhola
    irb(main):001:0> Hola.hi("english")
    => "hello world"
    irb(main):002:0> Hola.hi("spanish")
    => "hola mundo"

우리는 여기서 `-Ilib`이라는 낯선 커맨드라인 플래그를 사용합니다. 보통 RubyGems는
`lib` 디렉토리를 인클루드해 주어서, 사용자는 로드 패스에 대해 신경 쓸 필요가
없습니다. 하지만, RubyGems 없이 코드를 실행한다면, 직접 설정을 해야 합니다.
`$LOAD_PATH`를 코드 안에서 조작할 수도 있지만, 대부분은 안티패턴으로
간주합니다. [이 가이드]({{ site.baseurl }}/patterns)에는, 좀 더 많은 gem을 위한
안티 패턴(과 좋은 패턴)이 설명되어 있습니다.

gem에 더 많은 파일을 추가하실 거면, 배포하기 전에 그 파일들이 gemspec의 `files`
배열에 들어 있는지 확인하셔야 합니다! 이런 이유로(다른 이유도 있지만) 많은
개발자가 이 작업을
[Hoe](https://github.com/seattlerb/hoe),
[Jeweler](https://github.com/technicalpickles/jeweler),
[Rake](https://github.com/jimweirich/rake),
[Bundler](http://railscasts.com/episodes/245-new-gem-with-bundler)나
[그냥 동적 gemspec](https://github.com/wycats/newgem-template/blob/master/newgem.gemspec)으로
자동화합니다.

여기에 더 많은 코드와 디렉토리를 추가하는 것은 비슷하게 진행됩니다. 그럴듯할 때
파일을 쪼개세요. 프로젝트를 합리적으로 진행하면 당신과 미래에 이 코드를
유지보수하게 될 사람을 두통에서 구할 수 있습니다.

실행 파일 추가하기
------------------------
{:#adding-an-executable}

루비 코드의 라이브러리 말고도, gem은 하나 이상의 실행 파일을 셸의 `PATH`에
추가할 수 있습니다. 이런 식으로 사용하는 가장 잘 알려진 예로 `rake`가 있습니다.
[JSON](http://rubygems.org/gems/json)(루비 1.9에 기본으로 포함) gem에 포함된
`prettify_json.rb`도 굉장히 유용합니다. 이는 JSON를 읽을 수 있게 포매팅해줍니다.
여기 예제가 있습니다.

    % curl -s http://jsonip.com/ | \
      prettify_json.rb
    {
      "ip": "24.60.248.134"
    }

실행 파일을 gem에 추가하는 것은 간단합니다. gem의 `bin` 디렉토리에 파일을 넣고,
gemspec 실행 파일의 목록에 추가하면 됩니다. Hola gem에 하나 추가해 봅시다.
먼저 파일을 작성해서 실행파일로 만드세요.

    % mkdir bin
    % touch bin/hola
    % chmod a+x bin/hola

실행 파일 자체는 어떤 프로그램으로 실행할지 알아내기 위한
[shebang](http://www.catb.org/jargon/html/S/shebang.html)만 있으면 됩니다.
Hola의 실행 파일은 이렇습니다.

    % cat bin/hola
    #!/usr/bin/env ruby

    require 'hola'
    puts Hola.hi(ARGV[0])

이제, gem을 로드하고 인사할 언어를 첫 번째 커맨드 라인 인자로 넣으면 됩니다.
여기 실행 예가 있습니다.

    % ruby -Ilib ./bin/hola
    hello world

    % ruby -Ilib ./bin/hola spanish
    hola mundo

마지막으로 gem을 올릴 때 Hola의 실행 파일을 포함시키기 위해, gemspec에 이런
내용을 추가해야 합니다.

    % head -4 hola.gemspec
    Gem::Specification.new do |s|
      s.name        = 'hola'
      s.version     = '0.0.1'
      s.executables << 'hola'

새로운 gem을 올리면 직접 만든 커맨드 라인 유틸리티가 배포됩니다! 필요에 따라
실행 파일을 좀 더 `bin` 디렉토리에 추가하고 gemspec의 `executables` 배열에도
추가할 수 있습니다.

> 새로 릴리스할 때 gem의 버전을 변경해야 하는 것에 주의하세요.
> gem 버전에 관한 더 자세한 정보는 [패턴 가이드]({{ site.baseurl }}/patterns/#semantic-versioning)를
> 보세요.

테스트 작성하기
-----------------
{:#writing-tests}

gem을 테스트하는 것은 매우 중요합니다. 코드의 동작을 보장할 뿐만 아니라, 다른
사람이 코드의 동작을 이해하는 데에도 도움이 됩니다. gem을 평가할 때, 루비
개발자는 코드를 신뢰성을 평가하는 기준으로 테스트가 잘 되어 있는지(아니면
없는지)를 보는 경향이 있습니다.

gem은 패키지에 테스트 파일을 포함시켜, 다운로드 후에 테스트를 돌려 볼 수 있게
합니다. 전체 커뮤니티의 노력으로 [GemTesters](http://test.rubygems.org/)이라는
사이트가 생겼습니다. 여기서는 어떻게 루비 인터프리터가 다른 아키텍처에서 gem을
테스트하고 있는지 문서화하고 있습니다.

한 줄 요약: **gem을 테스트하세요!**

`Test::Unit`는 루비에 포함된 테스트 프레임워크입니다. 그리고 사용법에 관한 아주
[많은](http://www.mikeperham.com/2012/09/25/minitest-ruby-1-9s-test-framework/)
[튜토리얼](https://github.com/seattlerb/minitest/blob/master/README.txt)이
온라인에 있습니다. 루비에는 다른 테스트 프레임워크도 많이 있습니다.
[RSpec](http://rspec.info/)이 대중적으로 많이 하는 선택입니다. 결국, 무엇을
사용하는가는 중요하지 않습니다. 그냥 **테스트**하세요!

Hola에 테스트를 추가해 봅시다. `Rakefile`이라는 파일과 `test` 디렉토리를 새로
더 추가할 필요가 있습니다.

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

`Rakefile`은 테스트의 실행을 간단히 자동화합니다.

    % cat Rakefile
    require 'rake/testtask'

    Rake::TestTask.new do |t|
      t.libs << 'test'
    end

    desc "Run tests"
    task :default => :test

이제 `rake test`나 그냥 `rake`를 실행하면 테스트를 실행할 수 있습니다. 아!
hola를 위한 기본 테스트 파일은 여기 있습니다.

    % cat test/test_hola.rb
    require 'minitest/autorun'
    require 'hola'

    class HolaTest < Minitest::Test
      def test_english_hello
        assert_equal "hello world",
          Hola.hi("english")
      end

      def test_any_hello
        assert_equal "hello world",
          Hola.hi("ruby")
      end

      def test_spanish_hello
        assert_equal "hola mundo",
          Hola.hi("spanish")
      end
    end

마지막으로 테스트를 실행해 봅시다.

    % rake test
    (in /Users/qrush/Dev/ruby/hola)
    Loaded suite
    Started
    ...
    Finished in 0.000736 seconds.

    3 tests, 3 assertions, 0 failures, 0 errors, 0 skips

    Test run options: --seed 15331

녹색이네요! 뭐, 셸의 색상에 따라 다르긴 합니다. 좀 더 훌륭한 예제를 보시려면
[GitHub](https://github.com/search?utf8=%E2%9C%93&q=stars%3A%3E100+forks%3A%3E10&type=Repositories&ref=advsearch&l=Ruby)에서
다른 코드를 찾아서 읽어보는 것이 최고입니다.

코드 문서화하기
-------------------------
{:#documenting-your-code}

기본적으로 대부분의 gem은 문서를 생성하기 위해 RDoc을 사용합니다. 코드를 RDoc에
맞춰 작성하기 위한 [좋은 튜토리얼](http://docs.seattlerb.org/rdoc/RDoc/Markup.html)이
아주 많이 있습니다.
여기에 간단한 예를 들죠.

    # The main Hola driver
    class Hola
      # Say hi to the world!
      #
      # Example:
      #   >> Hola.hi("spanish")
      #   => hola mundo
      #
      # Arguments:
      #   language: (String)

      def self.hi(language = "english")
        translator = Translator.new(language)
        puts translator.hi
      end
    end

[YARD](http://yardoc.org/)도 gem을 올리면 [RubyDoc.info](http://rubydoc.info/)에서
자동으로 gem을 위한 YARDocs을 만들어주기 때문에 문서화를 위한 좋은 선택이 될 수
있습니다. YARD는 RDoc과의 하위호환성을 유지하고 무엇이 다르고 어떻게 시작하는지에
관한 [좋은 소개 글](http://rubydoc.info/docs/yard/file/docs/GettingStarted.md)이
있습니다.

마치며
----------
{:#wrapup}

여기서 배운 기본 이해로 RubyGem을 직접 만들 수 있길 바랍니다! 다음에 나오는
가이드는 gem을 만들 때의 패턴과 RubyGems 시스템의 다른 기능들을 다룹니다.

Credits
-------

이 튜토리얼은 [Gem Sawyer, Modern Day Ruby
Warrior](http://rubylearning.com/blog/2010/10/06/gem-sawyer-modern-day-ruby-warrior/)
에서 차용했습니다. 이 gem의 코드는 [GitHub](https://github.com/qrush/hola)에서
찾으실 수 있습니다.
