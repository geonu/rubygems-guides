---
layout: default
title: gem 서버 운영하기
url: /run-your-own-gem-server
previous: /rubygems-org-api
next: /resources
---

<em class="t-gray">사설, 조직을 위해 gem을 제공해야 할 때</em>

스스로 gem 서버를 실행하고 싶을 때가 있습니다. 인터넷이 연결되지 않았을 때
동료에게 gem을 공유하고 싶을 수도 있죠. 조직 내부에서만 사용하는, 공개하지 않은
채 gem으로 배포, 관리하고 싶은 개인적인 코드가 있을 수도 있습니다.

조직 안에서 gem을 호스팅하기 위한 서버를 설정하는 몇 가지 방법이 있습니다. 이
가이드에서는 `gem server` 명령어와 [Gem in a
Box](https://github.com/geminabox/geminabox) 프로젝트를 설명하려 합니다. 개발할
때 이런 서버를 gem 소스로 사용하는 방법도 다룹니다.

## 내장 gem 서버 운영하기

RubyGems을 설치할 때, 시스템에 `gem server` 명령어도 추가됩니다.
이는 gem의 호스팅을 하는 가장 빠른 방법입니다. 그냥 명령어를 실행하세요.

    gem server

이는 모든 로컬 머신에 설치된 모든 gem을
[http://localhost:8808](http://localhost:8808)에서 제공합니다. 브라우저에서 이
url을 방문하면 `gem server` 명령어가 제공하는 HTML 문서 인덱스를 보실 수
있습니다.

새로운 gem을 설치하면, 자동으로 내장 gem 서버에서도 사용 가능하게 됩니다.

쓸 수 있는 옵션을 모두 보시려면 이 명령을 실행하세요.

    gem server --help

다른 옵션 중에는, gem을 제공할 port 변경과 설치된 gem을 검색할 디렉터리 지정
등이 있습니다.

## Gem in a Box 실행하기

서버에 gem을 넣는 기능을 포함해 더 많은 것을 원한다면, [Gem in a
Box](https://github.com/geminabox/geminabox) 프로젝트를 시험해 보세요.

시작하려면 `geminabox`를 설치합니다.

    [~/dev/geminabox] gem install geminabox

gem을 저장하기 위한 data 디렉터리를 만듭니다.

    [~/dev/geminabox] mkdir data

밑의 내용을 `config.ru` 파일에 넣습니다.

    [~/dev/geminabox] cat config.ru
    require "rubygems"
    require "geminabox"

    Geminabox.data = "./data"
    run Geminabox::Server

서버를 실행합니다.

    [~/dev/geminabox] rackup
    [2011-05-19 12:09:40] INFO  WEBrick 1.3.1
    [2011-05-19 12:09:40] INFO  ruby 1.9.2 (2011-02-18) [x86_64-darwin10.5.0]
    [2011-05-19 12:09:40] INFO  WEBrick::HTTPServer#start: pid=60941 port=9292

이제 `gem inabox` 명령어를 사용해 gem을 넣을 수 있습니다. 이걸 처음 할 때, gem
서버의 위치를 묻는 메시지가 표시됩니다.

    [~/dev/secretgem] gem build secretgem.gemspec
      Successfully built RubyGem
      Name: secretgem
      Version: 0.0.1
      File: secretgem-0.0.1.gem
    [~/dev/secretgem] gem inabox ./secretgem-0.0.1.gem
    Enter the root url for your personal geminabox instance. (E.g. http://gems/)
    Host:  http://localhost:9292
    Pushing secretgem-0.0.1.gem to http://localhost:9292/...

[http://localhost:9292](http://localhost:9292)에 표시되는 웹 인터페이스도
있습니다. 더 자세한 정보는 [Gem in a
box](https://github.com/geminabox/geminabox)의 README를 읽으세요.

## 서버에서 gem 사용하기

`gem server`, Gem in a Box, 아니면 다른 gem 서버를 사용하면 RubyGems를
[http://rubygems.org](http://rubygems.org) 같은 다른 소스와 함께 로컬, 내부
소스도 사용하도록 RubyGems를 설정할 수 있습니다.

`gem sources` 명령어를 사용해 시스템 전체 gem 소스에 gem 서버를 추가할 수
있습니다. 밑의 URL은 `rackup`을 통해 Gem in a Box를 실행 했을 때의 기본값입니다.

    gem sources --add http://localhost:9292

그리고 평소처럼 gem을 설치합니다.

    [~] gem install secretgem
    Successfully installed secretgem-0.0.1
    1 gem installed

[Bundler](http://bundler.io)를 사용한다면, `Gemfile`에 이 서버를 gem 소스로
지정할 수 있습니다.

    [~/dev/myapp] cat Gemfile
    source "http://localhost:9292"
    gem "secretgem"

    [~/dev/myapp] bundle
    Using secretgem (0.0.1)
    Using bundler (1.0.13)
    Your bundle is complete! Use `bundle show [gemname]` to see where a bundled gem is installed.
