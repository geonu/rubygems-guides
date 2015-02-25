---
layout: default
title: Publishing your gem
url: /publishing
previous: /name-your-gem
next: /security
---

<em class="t-gray">아이디어로 시작해, 루비 코드의 배포 패키지로 끝내기</em>

다른 사용자에게 gem을 공유하는 방법.

* [서론](#introduction)
* [소스 코드 공유하기](#sharing-source-code)
* [직접 gem 서빙하기](#serving-your-own-gems)
* [RubyGems.org에 배포하기](#publishing-to-rubygemsorg)
* [RubyGems.org의 push 권한](#push-permissions-on-rubygemsorg)
* [Gem 보안](#gem-security)

서론
----------------
{:#introduction}


이제 [gem을 만들었으니]({{ site.baseurl }}/make-your-own-gem), 공유할 준비도
되었을 겁니다. 대규모의 비공개 프로젝트에서 코드를 정리하기 위해 비공개 gem을
것도 아주 합리적이긴 하지만, 보통은 여러 프로젝트에서 사용하기 위해 gem을 만드는
경우가 더 일반적입니다. 이 가이드는 세상에 gem을 공유하는 여러 방법에 대해
이야기합니다.

소스 코드 공유하기
-----------------------
{:#sharing-source-code}

(제 관점에서) gem을 공유하는 가장 간단한 방법은 소스 코드 채로 공유하는
방법입니다. gem의 전체 소스 코드를 공개 git 저장소에(항상 그렇진 않더라도
대부분은 [GitHub](https://github.com)에) 올려두면, 다른 사용자가 [Bundler의 git
기능](http://bundler.io/git.html)([번역](http://ruby-korea.github.io/bundler-site/git.html))을
이용해 설치할 수 있습니다.

예를 들어, Gemfile에 밑의 줄을 추가하여 `wicked_pdf` gem의 최신 코드를
프로젝트에 설치할 수 있습니다.

    gem "wicked_pdf", :git => "git://github.com/mileszs/wicked_pdf.git"

> gem을 git 저장소에서 바로 설치하는 것은 RubyGems의 기능이 아니라 Bundler의 기능입니다.
> Gem을 이런 식으로 설치하면 `gem list`를 했을 때 목록에 나타나지 않습니다.

직접 gem 서빙하기
-------------------------
{:#serving-your-own-gems}

gem을 설치권한을 관리하고 싶거나, gem 주변의 활동을 직접 관리하고 싶다면, 사설
gem 서버를 설정하고 싶을 수도 있습니다. 직접 [gem 서버를
설정]({{ site.baseurl }}/run-your-own-gem-server)하거나,
[Gemfury](http://www.gemfury.com/) 같은 상용 서비스를 이용할 수도 있습니다.

RubyGems 2.2.0 이상의 버전에서는 하나의 호스트로만 gem을 올릴 수 있게 하기 위해
`allowed_push_host` 메타데이터 값을 제공합니다. 비공개 gem을 배포한다면 실수로
rubygems.org에 올리는 것을 방지하기 위해 이 값을 설정하셔야 합니다.

    Gem::Specification.new 'my_gem', '1.0' do |s|
      # ...
      s.metadata['allowed_push_host'] = 'https://gems.my-company.example'
    end

비공개 gem 서버를 위한 최신 옵션 목록을 보시려면
[리소스]({{ site.baseurl }}/resources) 가이드를 참조하세요.

RubyGems.org에 배포하기
-----------------------------
{:#publishing-to-rubygemsorg}

공개적으로 gem을 배포하기 위한 가장 간단한 방법은 [RubyGems.org](https://rubygems.org/)를
사용하는 것입니다. RubyGems.org에서 배포하는 gem은 `gem install` 명령어나
Isolate, Bundler 같은 툴을 통해 설치할 수 있게 됩니다.

먼저, RubyGems.org에 계정을 만드실 필요가 있습니다. [가입](https://rubygems.org/users/new)
페이지에 가서 이메일 주소, 이름 (사용자명), 비밀번호를 입력하세요.

계정을 만들었으면, 이메일과 패스워드를 gem을 올릴 때 사용할 수 있습니다.
(RubyGems은 인증서를 ~/.gem/credentials에 저장해서 로그인은 한 번만 해도 됩니다.)

'squid-utils'이라는 새 gem의 버전 0.1.0을 배포하려면 이렇게 하면 됩니다.

    $ gem push squid-utils-0.1.0.gem
    Enter your RubyGems.org credentials.
    Don't have an account yet? Create one at https://rubygems.org/sign_up
       Email:   gem_author@example
    Password:
    Signed in.
    Pushing gem to RubyGems.org...
    Successfully registered gem: squid-utils (0.1.0)

축하합니다! 당신의 새 gem은 이제 세계의 어느 루비 사용자도 설치할 수 있게
되었습니다!

RubyGems.org의 push 권한
-----------------------------------
{:#push-permissions-on-rubygemsorg}

gem에 여러 관리자가 있다면, 다른 관리자에게 [gem owner
명령어]({{ site.baseurl }}/command-reference/#gem_owner)로 rubygems.org에 올릴
수 있는 권한을 부여할 수 있습니다.

Gem 보안
----------------
{:#gem-security}

[보안]({{ site.baseurl }}/security) 페이지를 보세요.
