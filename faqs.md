---
layout: default
title: 자주 묻는 질문들
url: /faqs
previous: /contributing
next: /plugins
---

<em class="t-gray">"어떻게"보단 "왜죠"나 "이런 미친"에 대한 내용들</em>

RubyGems 개발 팀은 여러 해에 걸쳐 많은 지원 요청을 받고 있습니다. 다음은
사용자의 경험과 관계없이 자주 나오는 질문들입니다.

* [`--user-install`로 gem을 설치했더니 명령어를 사용할 수 없습니다.](#user-install)
* [어떻게 자동으로 다운로드된 gem 코드를 신뢰할 수 있나요?](#security)
* [왜 `require 'some-gem'`이 실패하죠?](#require-fail)
* [왜 gem에서 파일을 로드할 때 require가 false를 반환하나요?](#require-false)


또한 [RubyGems Support](http://help.rubygems.org/) 사이트와 #rubygems IRC에서도
질문에 대답하고 있습니다. support 사이트에서 찾을 수 있는 정보에는 이런 것들이
있습니다.

* [Installing gems with no network](http://help.rubygems.org/kb/rubygems/installing-gems-with-no-network)
* [Why do I get HTTP Response 302 or 301 when installing a gem?](http://help.rubygems.org/kb/rubygems/why-do-i-get-http-response-302-or-301-when-installing-a-gem)
* [RubyGems Upgrade Issues](http://help.rubygems.org/kb/rubygems/rubygems-upgrade-issues)

<a id="user-install"></a>

`--user-install`로 gem을 설치했더니 명령어를 사용할 수 없습니다.
---------------------------------------------------------------------------

`--user-install` 옵션을 사용할 때, RubyGems은 gem을 `~/.gem/ruby/1.9.1` 같은 홈
디렉터리 안의 디렉터리에 설치합니다. 설치된 gem의 명령어는
`~/.gem/ruby/1.9.1/bin`에 들어갑니다. 여기에 설치된 프로그램을 사용하려면,
`PATH` 환경 변수에 `~/.gem/ruby/1.9.1/bin`을 추가할 필요가 있습니다.

예를 들어, bash를 사용한다면 `~/.bashrc` 파일에 이 코드를 넣어서 디렉터리를
`PATH`에 추가할 수 있습니다.

    if which ruby >/dev/null && which gem >/dev/null; then
        PATH="$(ruby -rubygems -e 'puts Gem.user_dir')/bin:$PATH"
    fi

`~/.bashrc` 파일에 이 코드를 추가한 다음, 셸을 다시 시작해야 변경 사항이
반영됩니다. 새로운 터미널 윈도우를 열거나 이미 열린 윈도우에서 `exec $SHELL`을
실행해서 재시작 할 수 있습니다.

<a id="security"></a>

어떻게 자동으로 다운로드된 gem 코드를 신뢰할 수 있나요?
---------------------------------------------------------

인터넷에서 설치한 다른 코드와 같은 방법으로 신뢰하면 됩니다. 궁극적으로는, 할 수
없습니다. 당신은 사용하는 gem의 소스를 알아야 할 책임이 있습니다. 보안이 중요한
곳에서는 알려진 좋은 gem만 사용해야 합니다. 그리고 가능하면 gem 코드의 보안
감사는 직접 수행하시는 게 좋습니다.

루비 커뮤니티는 앞으로 공개 키 인프라를 사용해 gem 코드를 보다 안전하게 하는
방법에 대해 토론하고 있습니다. 이 논의의 진행 상활을 보시려면, GitHub에 있는
[rubygems-trust](https://github.com/rubygems-trust) 조직을 방문하세요.

<a id="require-fail"></a>

왜 `require 'some-gem'`이 실패하죠?
-----------------------------------

모든 라이브러리가 gem의 이름과 require 해야 할 파일 이름이 엄격하게 일치하지는
않습니다. 먼저 파일이 일치하는지 확인하셔야 합니다.

    $ gem list RedCloth

    *** LOCAL GEMS ***

    RedCloth (4.1.1)
    $ ruby -rubygems -e 'require "RedCloth"'
    /Library/Ruby/Site/1.8/rubygems/custom_require.rb:31:in `gem_original_require': no such file to load -- RedCloth (LoadError)
      from /Library/Ruby/Site/1.8/rubygems/custom_require.rb:31:in `require'
      from -e:1
    $ gem contents --no-prefix RedCloth | grep lib
    lib/case_sensitive_require/RedCloth.rb
    lib/redcloth/erb_extension.rb
    lib/redcloth/formatters/base.rb
    lib/redcloth/formatters/html.rb
    lib/redcloth/formatters/latex.rb
    lib/redcloth/formatters/latex_entities.yml
    lib/redcloth/textile_doc.rb
    lib/redcloth/version.rb
    lib/redcloth.rb
    $ ruby -rubygems -e 'require "redcloth"'
    $ # success!

맞는 파일을 require 했다면, 아마 `gem`이 `ruby`와는 다른 루비를 사용하고 있을
수 있습니다.

    $ which ruby
    /usr/local/bin/ruby
    $ gem env | grep 'RUBY EXECUTABLE'
       - RUBY EXECUTABLE: /usr/local/bin/ruby1.9

이 인스턴스에서는 설치된 루비가 두 개입니다. 그래서 `gem`이 사용하는 루비가
`ruby`와 다르죠. 이는 symlink를 조정해서 바르게 고칠 수 있습니다.

    $ ls -l /usr/local/bin/ruby*
    lrwxr-xr-x  1 root  wheel       76 Jan 20  2010 /usr/local/bin/ruby@ -> /usr/local/bin/ruby1.8
    -rwxr-xr-x  1 root  wheel  1213160 Jul 15 16:36 /usr/local/bin/ruby1.8*
    -rwxr-xr-x  1 root  wheel  2698624 Jul  6 19:30 /usr/local/bin/ruby1.9*
    $ ls -l /usr/local/bin/gem*
    lrwxr-xr-x  1 root  wheel   76 Jan 20  2010 /usr/local/bin/gem@ -> /usr/local/bin/gem1.9
    -rwxr-xr-x  1 root  wheel  550 Jul 15 16:36 /usr/local/bin/gem1.8*
    -rwxr-xr-x  1 root  wheel  550 Jul  6 19:30 /usr/local/bin/gem1.9*

아마 `irb`도 같은 조치가 필요할 겁니다.

<a id="require-false"></a>

왜 gem에서 파일을 로드할 때 require가 false를 반환하나요?
-------------------------------------------------------------

로드가 맞게 되었다면 보통 require는 true를 반환합니다. gem에서 파일을 로드할 때
require가 false를 반환한다면 무엇이 잘못된 걸까요?

잘못된 건 없습니다. 음, 있을 순 있어요. 하지만 걱정할 정도는 아닙니다.

require 메소드가 false를 반환하는 것은 에러가 아닙니다. 그냥 그 파일은 이미
로드되어 있다는 의미입니다.

RubyGems에는 gem이 활성화될 때(예를 들면 선택될 때) 파일을 자동으로 로드하는
기능이 있습니다. 비활성화된 gem에서 파일을 require할 때, RubyGems 소프트웨어는
그 gem을 활성화합니다. 활성화하는 동안, 모든 자동으로 로드된 파일들이
로드됩니다.

그래서 require 문은 실제로 파일을 로드하는 작업을 수행하고, 이는 gem을 활성화할
때 이미 자동으로 로드되어, 이 구문은 false를 반환하게 됩니다.

