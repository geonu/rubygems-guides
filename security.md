---
layout: default
title: 보안
url: /security
previous: /publishing
next: /ssl-certificate-update
---

<em class="t-gray">gem을 암호화된 서명을 넣어 빌드하고 설치하기와 다른 보안 이야기.</em>

보안 수칙은 활발하게 논의되고 있습니다. 자주 확인해 주세요.

* [일반](#general)
* [gem 사용하기](#using-gems)
* [gem 빌드하기](#building-gems)
* [보안 취약점 보고하기](#reporting-security-vulnerabilities)

일반
-----------
{:#general}

gem을 설치하면 애플리케이션의 환경에서 gem의 코드를 실행하도록 합니다. 여기에
보안 문제가 있다는 것은 명백합니다. 악성 코드 gem을 서버에 설치하면 결국 해당
서버가 gem의 저자에게 완전히 침투당할 수 있습니다. 그래서, gem 코드의 보안은
루비 커뮤니티에서 활발히 논의되는 주제입니다.

RubyGems 0.8.11버전부터 [gem에 암호화된
서명](http://docs.seattlerb.org/rubygems/Gem/Security.html)을 넣을 수 있게
되었습니다. 서명은 `gem cert` 명령어를 사용해 키 쌍을 생성하고, 서명 데이터를
gem의 패키지에 포함하게 합니다. `gem install` 명령어는 선택적으로 보안 정책을
설정하게 할 수 있고, 이렇게 하면 gem을 설치하기 전에 서명 키를 검증할 수
있습니다.

하지만, gem을 보호하는 이 방법은 널리 사용되지 않습니다. 개발자는 [여러 번의
수작업](#building-gems)을 해야 하고, gem 서명 키에 대한 신뢰가 충분히 확립되지
않았기 때문입니다. X509, OpenPGP같은 새로운 서명 모델에 대한 토론이 [rubygems-trust
위키](https://github.com/rubygems-trust/rubygems.org/wiki/_pages), [RubyGems 개발자
그룹](https://groups.google.com/d/msg/rubygems-developers/lnnGTlfsuYo/TLDcJ2RPSDoJ)이나
[IRC](irc://chat.freenode.net/#rubygems-trust)에서 진행 중입니다. 이는
사용자에게는 투명하고 저자에게는 쓰기 쉽게 서명 시스템을
개선(혹은 교체)하는 것을 목표로 하고 있습니다.

gem 사용하기
--------------
{:#using-gems}

신뢰 정책을 사용해 설치해 봅시다.

  * `gem install gemname -P HighSecurity`: 모든 의존 gem은 서명되고 인증되어야
    합니다.

  * `gem install gemname -P MediumSecurity`: 모든 서명된 의존 gem은 인증되어야
    합니다.

  * `bundle --trust-policy MediumSecurity`: 대부분 위와 같지만, Bundler에서는
    `--trust-policy` 플래그만 인식하고 `-P`로 줄일 수는 없습니다.

  * *경고:* Gem 인증서는 전역적으로 신뢰됩니다. 그래서 gem 하나를 위해 cert.pem을
    추가하면 그 인증서로 서명된 모든 gem을 신뢰하게 됩니다.

가능할 때만 체크섬을 확인합니다.

    gem fetch gemname -v version
    ruby -rdigest/sha2 -e "puts Digest::SHA512.new.hexdigest(File.read('gemname-version.gem'))

[Benjamin Smith 님의 Hacking with Gems
talk](http://lanyrd.com/2013/rulu/scgxzr/)에서도 나오듯이 해킹 당할 위험이
있습니다.

gem 빌드하기
-----------------
{:#building-gems}

### `gem cert`로 서명하기

1) 자체 서명된 gem 인증서 만들기

    cd ~/.ssh
    gem cert --build your@email.com
    chmod 600 gem-p*

- gemspec에 적은 이메일 주소를 사용

2) gemspec에 인증서 설정하기

저장소에 인증서 공개 키 추가

    cd /path/to/your/gem
    mkdir certs
    cp ~/.ssh/gem-public_cert.pem certs/yourhandle.pem
    git add certs/yourhandle.pem

gemspec 인증서 경로 추가

    s.cert_chain  = ['certs/yourhandle.pem']
    s.signing_key = File.expand_path("~/.ssh/gem-private_key.pem") if $0 =~ /gem\z/

3) 승인 목록의 다른 인증서 사이에 방금 만든 인증서 추가하기

    gem cert --add certs/yourhandle.pem

4) gem 빌드하고 설치할 수 있는지 테스트하기

    gem build gemname.gemspec
    gem install gemname-version.gem -P HighSecurity
    # gem이 서명 안 된 gem을 참조하고 있다면 -P MediumSecurity

5) 설치 문서에 예제 넣기

> MetricFu는 암호화된 서명을 사용합니다. 설치할 gem이 손상되지 않았는지 확인하려면
>
> (이미 넣지 않았다면) 이 공개 키를 신뢰하는 인증서에 추가하세요.
>
> `gem cert --add <(curl -Ls https://raw.github.com/metricfu/metric_fu/master/certs/bf4.pem)`
>
> `gem install metric_fu -P MediumSecurity`
>
> MediumSecurity 신뢰 프로파일은 서명된 gem을 확인하지만, 서명되지 않은 의존성의 설치를 허용합니다.
>
> 이는 MetricFu의 모든 종속성이 서명되어 있지 않기 때문이며, 이런 이유로 HighSecurity는 사용하실 수 없습니다.

-------

### 저장소에 릴리스 된 gem의 체크섬 넣기

    require 'digest/sha2'
    built_gem_path = 'pkg/gemname-version.gem'
    checksum = Digest::SHA512.new.hexdigest(File.read(built_gem_path))
    checksum_path = 'checksum/gemname-version.gem.sha512'
    File.open(checksum_path, 'w' ) {|f| f.write(checksum) }
    # 'checksum_path' 추가하고 커밋

-------

### OpenPGP 서명은 [지원이 부족하므로 추천하지 않습니다](http://www.rubygems-openpgp-ca.org/blog/nobody-cares-about-signed-gems.html).

더 자세한 정보는 [Yorick
Peterse](https://github.com/rubygems/guides/pull/70#issuecomment-29007487) 님과의 
대화를 확인하세요.

보안 취약점 보고하기
--------------------------------------
{:#reporting-security-vulnerabilities}


### 다른 사람 gem의 보안 취약점 보고하기

다른 사람의 gem에서 취약점을 발견한다면, 제일 먼저 이게 알려진 취약점인지
확인해야 합니다.

새로운 취약점처럼 보인다면 저자에게 비공개(즉, 풀 리퀘스트나 공개 프로젝트의
이슈는 사용하면 안 됩니다.)로 연락해 어떻게 악용될 수 있는지 문제를 설명합니다.
물론, 해결책까지 제시할 수 있다면 더 좋습니다.

### 자신이 만든 gem의 보안 취약점 보고하기

먼저 cve-assign@mitre.org에 이메일을 보내 [CVE
아이디](https://en.wikipedia.org/wiki/Common_Vulnerabilities_and_Exposures)를
등록합니다. 이 아이디는 이야기할 때 취약점을 구별하기 쉽게 합니다.

그리고 gem을 사용하는 사람들에게 이 취약점을 해결하려면 어떻게 해야 하는지 설명
합니다. 패치된 버전을 릴리스해 사용자들에게 업그레이드를 권할 수도 있습니다.

마지막으로 취약점에 대해 사용자들에게 공지해야합니다. 현재 정보를 공유하기 좋은
통일된 채널은 없습니다만, 몇 군데 추천할 곳이 있습니다.

- Ruby Talk 메일링 리스트 (ruby-talk@ruby-lang.org)에 이메일을 보냅니다. 제목
  앞에 \[ANN]\[Security]를 붙이고 취약점을 요약해 gem의 어느 버전에 영향이 있고
  gem을 사용한다면 어떤 행동을 해야하는지를 적습니다.
- [OSVDB](http://osvdb.org/) 같은 오픈 소스 취약점 데이터베이스에 추가합니다.
  추가 하시려면 moderators@osvdb.org에 이메일을 보내거나 GitHub, Twitter의
  @osvdb로 메시지를 보내면 됩니다.

Credits
-------

이 가이드는 다음 출처에서 내용을 가져왔습니다.

* [How to cryptographically sign your RubyGem](http://www.benjaminfleischer.com/2013/11/08/how-to-sign-your-rubygem-cert/) - Step-by-step guide
* [Signing rubygems - Pasteable instructions](http://developer.zendesk.com/blog/2013/02/03/signing-gems/)
* [Twitter gem gemspec](https://github.com/sferik/twitter/blob/master/twitter.gemspec)
* [RubyGems Trust Model Overview](https://github.com/rubygems-trust/rubygems.org/wiki/Overview), [doc](http://goo.gl/ybFIO)
* [Let’s figure out a way to start signing RubyGems](http://tonyarcieri.com/lets-figure-out-a-way-to-start-signing-rubygems)
* [A Practical Guide to Using Signed Ruby Gems - Part 3: Signing your Own](http://blog.meldium.com/home/2013/3/6/signing-gems-how-to)
* [리소스]({{ site.baseurl }}/resources) 페이지도 확인하세요.
