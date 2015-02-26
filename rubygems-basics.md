---
layout: default
title: RubyGems 기초
url: /rubygems-basics
previous: /
next: /what-is-a-gem
---

<em class="t-gray">일반적인 RubyGems 명령어 사용하기</em>

`gem` 명령어로 RubyGems와 상호작용할 수 있습니다.

루비 1.9 이상의 버전에서는 RubyGems가 동봉되어 있습니다만, 아마도 버그 수정이나
새로운 기능을 위해 업그레이드 하실 필요가 있을 겁니다. RubyGems를 처음으로
업그레이드하거나 설치하신다면 (그리고 루비 1.9를 사용해야한다면)
[다운로드](https://rubygems.org/pages/download) 페이지에 가보세요.

gem에서 어떻게 파일을 요청하는지 알고 싶으시면, [gem이란
무엇인가?]({{ site.baseurl }}/what-is-a-gem)를 먼저 보세요.

* [gem 찾기](#finding-gems)
* [gem 설치하기](#installing-gems)
* [코드 require하기](#requiring-code)
* [설치된 gem 목록보기](#listing-installed-gems)
* [gem 제거하기](#uninstalling-gems)
* [문서 보기](#viewing-documentation)
* [gem 가져오고 언팩하기](#fetching-and-unpacking-gems)
* [더 읽을거리](#further-reading)

gem 찾기
----------------
{:#finding-gems}

`search` 명령어를 사용하면, 리모트 gem을 이름으로 검색할 수 있습니다. 쿼리에서
정규표현식을 사용할 수도 있습니다.

    $ gem search ^rails

    *** REMOTE GEMS ***

    rails (4.0.0)
    rails-3-settings (0.1.1)
    rails-action-args (0.1.1)
    rails-admin (0.0.0)
    rails-ajax (0.2.0.20130731)
    [...]

gem에 대한 더 자세한 정보를 원하신다면 details 옵션을 넣으시면 됩니다.
details가 포함된 gem 리스트는 파일을 더 많이 다운로드해야 하므로, 적은 수의 gem만
검색하시는 것이 좋습니다.

    $ gem search ^rails$ -d

    *** REMOTE GEMS ***

    rails (4.0.0)
        Author: David Heinemeier Hansson
        Homepage: http://www.rubyonrails.org

        Full-stack web application framework.

rubygems.org에서도 gem을 검색하실 수 있습니다. 이 [링크는
rake](https://rubygems.org/search?utf8=✓&query=rake)를 검색합니다.

gem 설치하기
-------------------
{:#installing-gems}

`install` 명령어는 gem과 필요한 의존성을 다운로드 후 설치하고, 설치된 gem의
문서를 빌드합니다.

    $ gem install drip
    Fetching: rbtree-0.4.1.gem (100%)
    Building native extensions.  This could take a while...
    Successfully installed rbtree-0.4.1
    Fetching: drip-0.0.2.gem (100%)
    Successfully installed drip-0.0.2
    Parsing documentation for rbtree-0.4.1
    Installing ri documentation for rbtree-0.4.1
    Parsing documentation for drip-0.0.2
    Installing ri documentation for drip-0.0.2
    Done installing documentation for rbtree, drip after 0 seconds
    2 gems installed

이 drip 명령어는 확장을 가지고 있는 rbtree gem에 의존합니다. 루비는 의존관계의
rbtree를 설치하고 그 확장을 빌드하고 drip gem을 설치하고 설치된 gem들의 문서를
빌드합니다.

gem을 설치할 때 `--no-doc` 인자를 이용해 문서 생성을 하지 않을 수 있습니다.

코드 require하기
------------------
{:#requiring-code}

RubyGems는 `require`문에서 루비 코드를 찾는 루비 로드 패스를 수정합니다. gem을
`require` 할 때, 사실은 그냥 `$LOAD_PATH`에 gem의 `lib` 디렉터리를 붙일 뿐입니다.
루비에서 `pretty_print` 라이브러리가 어떻게 인클루드되는지 이해하기 위해
`irb`에서 한번 시험해 봅시다.

*팁: `irb`에 `-r`을 넘기면 irb가 로드될 때 자동으로 라이브러리를 require합니다.*

    % irb -rpp
    >> pp $LOAD_PATH
    [".../lib/ruby/site_ruby/1.9.1",
     ".../lib/ruby/site_ruby",
     ".../lib/ruby/vendor_ruby/1.9.1",
     ".../lib/ruby/vendor_ruby",
     ".../lib/ruby/1.9.1",
     "."]

기본적으로 로드 패스는 조금의 시스템 디렉터리와 루비 스탠다드 라이브러리만
가지고 있습니다. 로드 패스에 awesome_print의 디렉터리를 추가하면,
awesome_print의 파일 중 하나를 require 할 수 있게 됩니다.

    % irb -rpp
    >> require 'ap'
    => true
    >> pp $LOAD_PATH.first
    ".../gems/awesome_print-1.0.2/lib"

주의: 루비 1.8에서는 gem을 require 하기 전에 `require 'rubygems'`을 해야합니다.

한 번 `ap`를 require 하면, RubyGems는 자동으로 `$LOAD_PATH`에 `lib` 디렉터리를
붙입니다.

기본적으로 gem 안에 있는 것은 이것뿐입니다. 루비 코드를 `lib`에 넣고
루비 파일의 이름을 gem의 이름과 같게 지으면, ("freewill" gem이라면 파일은
`freewill.rb`이 되어야 합니다, [gem 이름짓기]({{ site.baseurl }}/name-your-gem)
도 보세요.) RubyGems에서 로드됩니다.

`lib` 디렉터리 자체는 보통 하나의 `.rb` 파일과 다른 모든 파일을 가지고 있는
같은 이름의 디렉터리를 가지고 있습니다.

예를 들어:

    % tree freewill/
    freewill/
    └── lib/
        ├── freewill/
        │   ├── user.rb
        │   ├── widget.rb
        │   └── ...
        └── freewill.rb

설치된 gem 목록보기
--------------------------
{:#listing-installed-gems}

`list` 명령어는 로컬에 설치된 gem을 보여줍니다.

    $ gem list

    *** LOCAL GEMS ***

    bigdecimal (1.2.0)
    drip (0.0.2)
    io-console (0.4.2)
    json (1.7.7)
    minitest (4.3.2)
    psych (2.0.0)
    rake (0.9.6)
    rbtree (0.4.1)
    rdoc (4.0.0)
    test-unit (2.0.0.0)

(루비에는 몇 가지 gem이 기본으로 포함되어 있습니다. 루비 2.0.0에는 bigdecimal,
io-console, json, minitest, psych, rake, rdoc, test-unit가 들어있습니다.)

gem 제거하기
---------------------
{:#uninstalling-gems}

`uninstall` 명령어는 설치한 gem을 제거합니다.

    $ gem uninstall drip
    Successfully uninstalled drip-0.0.2

의존성이 있는 gem을 제거하려면, RubyGems에서 정말로 지울 건지 확인합니다.

    $ gem uninstall rbtree

    You have requested to uninstall the gem:
      rbtree-0.4.1

    drip-0.0.2 depends on rbtree (>= 0)
    If you remove this gem, these dependencies will not be met.
    Continue with Uninstall? [yN]  n
    ERROR:  While executing gem ... (Gem::DependencyRemovalException)
        Uninstallation aborted due to dependent gem(s)

문서 보기
-------------------------
{:#viewing-documentation}

설치된 gem의 문서를 `ri`를 이용해 볼 수 있습니다.

    $ ri RBTree
    RBTree < MultiRBTree

    (from gem rbtree-0.4.0)
    -------------------------------------------
    A sorted associative collection that cannot
    contain duplicate keys. RBTree is a
    subclass of MultiRBTree.
    -------------------------------------------

설치된 gem의 문서를 브라우저에서 보려면 `server` 명령어로 볼 수 있습니다.

    $ gem server
    Server started at http://0.0.0.0:8808
    Server started at http://[::]:8808

문서는 http://localhost:8808 에서 보실 수 있습니다.

gem 가져오고 언팩하기
-------------------------------
{:#fetching-and-unpacking-gems}

gem을 설치하지 않고 내용을 보고 싶은 경우, `fetch` 명령어를 사용해 .gem 파일을
다운로드해 `unpack` 명령어로 내용을 추출할 수 있습니다.

    $ gem fetch malice
    Fetching: malice-13.gem (100%)
    Downloaded malice-13
    $ gem unpack malice-13.gem
    Fetching: malice-13.gem (100%)
    Unpacked gem: '.../malice-13'
    $ more malice-13/README

    Malice v. 13

    DESCRIPTION

    A small, malicious library.

    [...]
    $ rm -r malice-13*

설치된 gem도 unpack해서, 파일을 조금 수정해, 설치된 것 대신에 수정된 gem을 사용할
수도 있습니다.

    $ gem unpack rake
    Unpacked gem: '.../rake-10.1.0'
    $ vim rake-10.1.0/lib/rake/...
    $ ruby -I rake-10.1.0/lib -S rake some_rake_task
    [...]

`-I` 인자는 루비 `$LOAD_PATH`에 언팩된 rake를 추가해 RubyGems가 gem 버전(혹은
디폴트 버전)을 로드하는 것을 방지합니다. `-S` 인자는 전체 패스를 적지 않아도
되도록 셸의 `$PATH`에서 `rake`를 찾습니다.

더 읽을거리
-------------------
{:#further-reading}

이 가이드는 `gem`의 기본적인 사용법만 보여줄 뿐입니다. gem의 내부의 동작과
설치한 gem을 어떻게 사용하는지는 다음 장인 [gem이란
무엇인가?]({{ site.baseurl }}/what-is-a-gem)를 읽어보세요. gem 명령어의 완전한
레퍼런스를 보시려면 [명령어 레퍼런스]({{ site.baseurl }}/command-reference)를
보세요.
