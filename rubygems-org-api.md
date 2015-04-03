---
layout: default
title: RubyGems.org API
url: /rubygems-org-api
previous: /command-reference
next: /run-your-own-gem-server
---

<em class="t-gray">HTTP를 통한 RubyGems.org와의 통신 자세히 보기</em>

> 주의: API는 작업 중이고, [도움이 필요합니다!](https://github.com/rubygems/gemcutter)
> RubyGems 자체와
> [gemcutter gem](https://rubygems.org/gems/gemcutter)은 gem을 넣고 소유자를
> 추가하고 그 밖의 다른 일을 하기 위해 API를 사용합니다.

* [API 인증](#api-authorization): RubyGems.org에 인증하는 방법
* [Gem 메소드](#gem-methods): 호스트할 gem을 만들거나 조회하기
* [Gem 버전 메소드](#gem-version-methods): 특정 gem의 버전에 관한 정보를
  조회하기
* [Gem 다운로드 메소드](#gem-download-methods): 다운로드 통계 조회하기
* [소유자 메소드](#owner-methods): gem의 소유자를 관리하기
* [웹훅 메소드](#webhook-methods): gem 푸시 알림 관리하기
* [활동 메소드](#activity-methods): 사이트 전체의 활동에 대한 정보 조회하기
* [기타 메소드](#misc-methods): 그 밖의 다양한 사이트와의 통신

API 인증
---------------------
{:#api-authorization}

어떤 API 통신은 인증 헤더가 필요합니다. API 키를 찾으려면,
[RubyGems.org](http://rubygems.org)에 로그인한 이후 당신의 유저 이름을 클릭해서
'Edit Profile'을 클릭합니다. 여기에 API 키를 사용하는 예가 있습니다.

    $ curl -H 'Authorization:YOUR_API_KEY' \
      https://rubygems.org/api/v1/some_api_call.json

루비 라이브러리
--------------

RubyGems.org와 루비로 통신 할 수도 있습니다.

[gems](https://rubygems.org/gems/gems) 클라이언트는 밑에 나열된 모든 방법의 루비
인터페이스를 제공합니다. 이 라이브러리는 README에 있는 기본 사용 예를 포함한
[전체 문서](http://rdoc.info/gems/gems)를 가지고 있습니다. 이 라이브러리는 다음
명령으로 설치할 수 있습니다.

    gem install gems

Gem 메소드
---------
{:#gem-methods}

### GET - `/api/v1/gems/[GEM NAME].(json|yaml)`

주어진 gem의 기본 정보를 반환합니다. 밑은 "rails" gem의 정보를 JSON 형식으로
응답한 예제입니다.

    $ curl https://rubygems.org/api/v1/gems/rails.json

    {
      "name": "rails",
      "downloads": 7528417,
      "version": "3.2.1",
      "version_downloads": 47602,
      "authors": "David Heinemeier Hansson",
      "info": "Ruby on Rails is a full-stack web framework optimized for programmer
              happiness and sustainable productivity. It encourages beautiful code
              by favoring convention over configuration.",
      "project_uri": "http://rubygems.org/gems/rails",
      "gem_uri": "http://rubygems.org/gems/rails-3.2.1.gem",
      "homepage_uri": "http://www.rubyonrails.org",
      "wiki_uri": "http://wiki.rubyonrails.org",
      "documentation_uri": "http://api.rubyonrails.org",
      "mailing_list_uri": "http://groups.google.com/group/rubyonrails-talk",
      "source_code_uri": "http://github.com/rails/rails",
      "bug_tracker_uri": "http://github.com/rails/rails/issues",
      "dependencies": {
        "development": [],
        "runtime": [
          {
            "name": "actionmailer",
            "requirements":"= 3.2.1"
          },
          {
            "name": "actionpack",
            "requirements": "= 3.2.1"
          },
          {
            "name": "activerecord",
            "requirements": "= 3.2.1"
          },
          {
            "name": "activeresource",
            "requirements": "= 3.2.1"
          },
          {
            "name": "activesupport",
            "requirements": "= 3.2.1"
          },
          {
            "name": "bundler",
            "requirements": "~> 1.0"
          },
          {
            "name": "railties",
            "requirements": "= 3.2.1"
          }
        ]
      }
    }
    }

### GET - `/api/v1/search.(json|yaml)?query=[YOUR QUERY]`

사이트에서 검색을 하는 것처럼 Gemcutter에 검색을 합니다. 일치한 gem의 정보를
JSON이나 YAML의 배열로 반환합니다.

    $ curl 'https://rubygems.org/api/v1/search.json?query=cucumber'

    $ curl 'https://rubygems.org/api/v1/search.yaml?query=cucumber'

결과는 페이지로 구분되며 API 호출은 처음 30개의 일치된 값만 반환합니다. 다음
결과를 얻으려면, 빈 결과가 나오기 전까지 페이지 쿼리 매개변수를 사용하시면
됩니다.

    $ curl 'https://rubygems.org/api/v1/search.json?query=cucumber&page=2'

### GET - `/api/v1/gems.(json|yaml)`

소유한 모든 gem을 나열합니다. 소유한 gem의 정보를 JSON이나 YAML의 배열로
반환합니다.

    $ curl -H 'Authorization:701243f217cdf23b1370c7b66b65ca97' \
              https://rubygems.org/api/v1/gems.json


### POST - `/api/v1/gems`

RubyGems.org에 gem을 보냅니다. 빌드한 루비 gem을 리퀘스트 보디에 넣어서
전송(post)해야 합니다.

    $ curl --data-binary @gemcutter-0.2.1.gem \
           -H 'Authorization:701243f217cdf23b1370c7b66b65ca97' \
           https://rubygems.org/api/v1/gems

    Successfully registered gem: gemcutter (0.2.1)

### DELETE - `/api/v1/gems/yank`

RubyGems.org의 목록에서 gem을 제거합니다. 플랫폼은 선택적입니다.

    $ curl -X DELETE -H 'Authorization:701243f217cdf23b1370c7b66b65ca97' \
           -d 'gem_name=bills' -d 'version=0.0.1' \
           -d 'platform=x86-darwin-10' \
           https://rubygems.org/api/v1/gems/yank

    Successfully yanked gem: bills (0.0.1)


### PUT - `/api/v1/gems/unyank`

제거된 gem을 다시 RubyGems.org의 목록으로 업데이트 합니다. 플랫폼은
선택적입니다.

    $ curl -X PUT -H 'Authorization:701243f217cdf23b1370c7b66b65ca97' \
           -d 'gem_name=bills' -d 'version=0.0.1' \
           -d 'platform=x86-darwin-10' \
           https://rubygems.org/api/v1/gems/unyank

    Successfully unyanked gem: bills (0.0.1)

Gem 버전 메소드
-----------------------
{:#gem-version-methods}

### GET - `/api/v1/versions/[GEM NAME].(json|yaml)`

밑에 나온 것처럼 gem 버전의 상세정보를 반환합니다.

    $ curl https://rubygems.org/api/v1/versions/coulda.json

    [
      {
        "authors" : "Evan David Light",
        "built_at" : "2011-08-08T04:00:00.000Z",
        "description" : "Behaviour Driven Development derived from Cucumber but as an internal DSL with methods for reuse",
        "downloads_count" : 2224,
        "number" : "0.7.1",
        "summary" : "Test::Unit-based acceptance testing DSL",
        "platform" : "ruby",
        "ruby_version" : nil,
        "prerelease" : false,
        "licenses" : nil,
        "requirements" : nil,
        "sha" : "777c3a7ed83e44198b0a624976ec99822eb6f4a44bf1513eafbc7c13997cd86c"
      }
    ]

Gem 다운로드 메소드
------------------------
{:#gem-download-methods}

### GET - `/api/v1/downloads.(json|yaml)`

RubyGems의 총 다운로드 수가 담긴 객체를 반환합니다.

    $ curl https://rubygems.org/api/v1/downloads.json

    {
      "total": 461672727
    }

### GET - `/api/v1/downloads/[GEM NAME]-[GEM VERSION].(json|yaml)`

특정 gem의 총 다운로드 수와 특정 버전의 총 다운로드 수가 담긴 객체를 반환합니다.

    $ curl https://rubygems.org/api/v1/downloads/rails_admin-0.0.0.json

    {
      "version_downloads": 3142,
      "total_downloads": 3142
    }

소유자 메소드
-----------------
{:#owner-methods}

### GET - `/api/v1/owners/[USER HANDLE]/gems.(json|yaml)`

사용자의 모든 gem을 봅니다. 이것은 사용자가 푸시할 수 있는 모든 gem입니다.

    $ curl https://rubygems.org/api/v1/owners/qrush/gems.json

    [
      {
        "name": "factory_girl",
    ...
      },
    ...
    ]


### GET - `/api/v1/gems/[GEM NAME]/owners.(json|yaml)`

gem의 모든 소유자를 봅니다. 이 사용자들은 이 gem을 푸시할 수 있습니다.

    $ curl https://rubygems.org/api/v1/gems/gemcutter/owners.json

    [
      {
        "email": "nick@gemcutter.org"
      },
      {
        "email": "ddollar@gmail.com"
      }
    ]

### POST - `/api/v1/gems/[GEM NAME]/owners`

소유하고 있는 루비 gem에 소유자를 추가합니다. 그 사용자에게 gem을 관리할 수 있는
권한을 줍니다.

    $ curl -H 'Authorization:701243f217cdf23b1370c7b66b65ca97' \
           -F 'email=josh@technicalpickles.com' \
           https://rubygems.org/api/v1/gems/gemcutter/owners

    Owner added successfully.

### DELETE - `/api/v1/gems/[GEM NAME]/owners`

소유하고 있는 루비 gem의 사용자 관리 권한을 제거합니다.

    $ curl -X DELETE -H 'Authorization:701243f217cdf23b1370c7b66b65ca97' \
            -d "email=josh@technicalpickles.com" \
            https://rubygems.org/api/v1/gems/gemcutter/owners

    Owner removed successfully.

웹훅 메소드
-------------------
{:#webhook-methods}

### GET - `/api/v1/web_hooks.(json|yaml)`

당신의 계정에 등록된 웹훅을 나열합니다.

    $ curl -H 'Authorization:701243f217cdf23b1370c7b66b65ca97' \
           https://rubygems.org/api/v1/web_hooks.json

    {
      "all gems": [
        {
          "url": "http://gemwhisperer.heroku.com",
          "failure_count": 1
        }
      ],
      "rails": [
        {
          "url": "http://example.com",
          "failure_count": 0
        }
      ]
    }

### POST - `/api/v1/web_hooks`

웹훅을 만듭니다. `gem_name`과 `url` 두 매개변수가 필요합니다. `gem_name`
매개변수에 `*`를 넣으면, 전체 gem의 훅에 적용할 수 있습니다.

    $ curl -H 'Authorization:701243f217cdf23b1370c7b66b65ca97' \
           -F 'gem_name=rails' -F 'url=http://example.com' \
           https://rubygems.org/api/v1/web_hooks

    Successfully created webhook for rails to http://example.com

    $ curl -H 'Authorization:701243f217cdf23b1370c7b66b65ca97' \
           -F 'gem_name=*' -F 'url=http://example.com' \
           https://rubygems.org/api/v1/web_hooks

    Successfully created webhook for all gems to http://example.com

### DELETE - `/api/v1/web_hooks/remove`

웹훅을 제거합니다. `gem_name`과 `url` 두 매개변수가 필요합니다. `gem_name`
매개변수에 `*`를 넣으면, 전체 gem의 훅에 적용할 수 있습니다.

    $ curl -X DELETE -H 'Authorization:701243f217cdf23b1370c7b66b65ca97' \
           -d 'gem_name=rails' -d 'url=http://example.com' \
           https://rubygems.org/api/v1/web_hooks/remove

    Successfully removed webhook for rails to http://example.com

    $ curl -X DELETE -H 'Authorization:701243f217cdf23b1370c7b66b65ca97' \
           -d 'gem_name=*' -d 'url=http://example.com' \
           https://rubygems.org/api/v1/web_hooks/remove

    Successfully removed webhook for all gems to http://example.com

### POST - `/api/v1/web_hooks/fire`

웹훅을 시험삼아 실행해 봅니다. 애플리케이션을 개발 중일 때와 같은 경우, 이를
사용해 언제나 엔드포인트를 테스트할 수 있습니다. `gem_name`과 `url` 두
매개변수가 필요합니다.`gem_name` 매개변수에 `*`를 넣으면, 전체 gem의 훅에
적용할 수 있습니다.

`Authorization` 헤더는 실행된 모든 웹훅에 포함되어 요청이 RubyGems.org로부터 온
것인지 확신할 수 있습니다. 이 헤더에는 gem 이름, gem 버전, API 키를 이어 붙여
SHA2로 해시된 값이 들어갑니다.

    $ curl -H 'Authorization:701243f217cdf23b1370c7b66b65ca97' \
           -F 'gem_name=rails' -F 'url=http://example.com' \
           https://rubygems.org/api/v1/web_hooks/fire

    Successfully deployed webhook for rails to http://example.com

    $ curl -H 'Authorization:701243f217cdf23b1370c7b66b65ca97' \
           -F 'gem_name=*' -F 'url=http://example.com' \
           https://rubygems.org/api/v1/web_hooks/fire

    Successfully deployed webhook for all gems to http://example.com

활동 메소드
--------------------
{:#activity-methods}

### GET - `/api/v1/activity/latest`

RubyGems.org에 (처음) 추가된 최신 gem을 50 개 가져옵니다. gem의 정보는 JSON이나
YAML의 배열로 반환합니다.

    $ curl 'https://rubygems.org/api/v1/activity/latest.json'

### GET - `/api/v1/activity/just_updated`

최근에 업데이트된 gem을 50 개 가져옵니다. gem 버전 정보를 JSON이나 YAML의
배열로 반환합니다.

    $ curl 'https://rubygems.org/api/v1/activity/just_updated.json'

기타 메소드
----------------
{:#misc-methods}

### GET - `/api/v1/api_key.(json|yaml)`

HTTP 기본 인증으로 API 키를 가져옵니다.

    $ curl -u "nick@gemcutter.org:schwwwwing" \
           https://rubygems.org/api/v1/api_key.json

    {
      "rubygems_api_key": "701243f217cdf23b1370c7b66b65ca97"
    }

### GET - `/api/v1/dependencies?gems=[COMMA DELIMITED GEM NAMES]`

주어진 gem의 모든 버전의 정보를 정렬된 해시 배열로 반환합니다. 각 해시는 gem
버전과 그 의존성을 포함합니다. 이 정보는 의존성을 해결할 때 유용합니다.

    $ ruby -ropen-uri -rpp -e \
      'pp Marshal.load(open("https://rubygems.org/api/v1/dependencies?gems=rails,thor"))'

    [{:platform=>"ruby",
      :dependencies=>
       [["bundler", "~> 1.0"],
        ["railties", "= 3.0.3"],
        ["actionmailer", "= 3.0.3"],
        ["activeresource", "= 3.0.3"],
        ["activerecord", "= 3.0.3"],
        ["actionpack", "= 3.0.3"],
        ["activesupport", "= 3.0.3"]],
      :name=>"rails",
      :number=>"3.0.3"},
    ...
    {:number=>"0.9.9", :platform=>"ruby", :dependencies=>[], :name=>"thor"}]
