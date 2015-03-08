---
layout: default
title: 리소스
url: /resources
previous: /run-your-own-gem-server
next: /contributing
---

<em class="t-gray">훌륭한 블로그 글, 튜토리얼, 도움이 되는 다른 사이트들</em>

RubyGems에 관한 도움이 되는 소재의 모음입니다. 자유롭게
[포크](https://github.com/rubygems/guides)해서 넣어 보세요!

튜토리얼
---------

* [Making Ruby Gems](http://timelessrepo.com/making-ruby-gems)
* [Gemcutter & Jeweler](http://railscasts.com/episodes/183-gemcutter-jeweler)
* [MicroGems: five minute RubyGems](http://jeffkreeftmeijer.com/2011/microgems-five-minute-rubygems/) - gist에 넣을 수 있을 만큼 작은 gem.
* [Let's Write a Gem: Part 1](http://rakeroutes.com/blog/lets-write-a-gem-part-one/)과 [Part 2](http://rakeroutes.com/blog/lets-write-a-gem-part-two/)
* [Polishing Rubies](http://intridea.com/blog/tag/polishing%20rubies)
* [A Practical Guide to Using Signed Ruby Gems - Part 1: Bundler](http://blog.meldium.com/home/2013/3/3/signed-rubygems-part)
* [Basic RubyGem Development](http://tech.pro/tutorial/1226/basic-rubygem-development)와 [Intermediate RubyGem Development](http://tech.pro/tutorial/1277/intermediate-rubygem-development)
* [How to make a Rubygem](http://www.alexedwards.net/blog/how-to-make-a-rubygem)과 [How to make a Rubygem: Part Two](http://www.alexedwards.net/blog/how-to-make-a-rubygem-part-two)
* [Crafting Gems](http://railsconftutorials.com/2013/sessions/crafting_gems.html) - RailsConf 2013의 튜토리얼.
* [How to cryptographically sign your RubyGem](http://www.benjaminfleischer.com/2013/11/08/how-to-sign-your-rubygem-cert/) - 단계별 가이드

발표
-------------

* [Ruby gem 만들기](https://www.youtube.com/watch?v=UVCDgpKd8gQ)
* [Set up Ruby Project(한국어)](https://speakerdeck.com/nacyot/set-up-ruby-project)
* [History of RDoc and RubyGems](http://blog.segment7.net/2011/01/17/history-of-rdoc-and-rubygems)
* [Building a Gem](http://www.slideshare.net/sarah.allen/building-a-ruby-gem)
* [Gemology](http://www.slideshare.net/copiousfreetime/gemology)

철학
----------

* [Semantic Versioning](http://semver.org/)
* [Ruby Packaging Standard](http://chneukirchen.github.com/rps/)
* [Why `require 'rubygems'` Is Wrong](http://tomayko.com/writings/require-rubygems-antipattern)
* [How to Name Gems](http://blog.segment7.net/2010/11/15/how-to-name-gems)
* [Make the world a better place; put a license in your gemspec](http://www.benjaminfleischer.com/2013/07/12/make-the-world-a-better-place-put-a-license-in-your-gemspec/)

패턴
--------

* [Gem Packaging: Best Practices](http://weblog.rubyonrails.org/2009/9/1/gem-packaging-best-practices)
* [Rubygems Good Practice](http://yehudakatz.com/2009/07/24/rubygems-good-practice/)
* [Gem Development Best Practices](http://blog.carbonfive.com/2011/01/22/gem-development-best-practices/)

만들기
--------

gem을 빌드할 때 도움이 되는 도구들

* [gemerator](https://github.com/rkh/gemerator) - gem의 뼈대를 생성하는 아주 작은 도구
* [hoe](https://github.com/seattlerb/hoe) - Rake/RubyGems 헬퍼
* [Jeweler](https://github.com/technicalpickles/jeweler) - RubyGems를 관리하기 위한 독립적인 도구
* [micro-cutter](https://github.com/tjh/micro-cutter) - MicroGem의 베이스 파일을 빌드하기 위한 도구
* [newgem](https://github.com/drnic/newgem) - 새로운 gem 생성기
* [RStack](https://github.com/jrun/rstack) - 비공개 gem을 위한 생성기
* [rubygems-tasks](https://github.com/postmodern/rubygems-tasks) - 루비 gem을 빌드, 설치, 릴리스하는 Rake 작업 모음
* [ore](https://github.com/ruby-ore/ore) - 여러 템플릿이 있는 프로젝트 생성기
* [Omnibus](https://github.com/opscode/omnibus-ruby) - 루비 코드를 위한 풀 스택 인스톨러를 생성(독립 RubyGem을 패키지로 사용하는 것에 관한 지침은 [Omnibus tutorial](http://blog.scoutapp.com/articles/2013/06/21/omnibus-tutorial-package-a-standalone-ruby-gem)을 보세요.)

모니터링
----------

gem의 변경을 보기 위한 도구.

* [Gemnasium](https://gemnasium.com/) - GitHub 프로젝트를 분석해 무엇을 통지할지 학습합니다. 공개 저장소는 무료입니다.
* [Gemnasium gem](https://github.com/gemnasium/gemnasium-gem) - 비공개 저장소에 접근을 허가하지 않고 Gemnasium을 사용할 수 있습니다.
* [gemwhisperer](https://github.com/rubygems/gemwhisperer)

호스트와 서빙
-------------------

* [Geminabox](https://github.com/cwninja/geminabox)- 가지고 있는 gem을 rubygems 호환 API와 함께 호스트합니다.
* [Gem Mirror](https://github.com/YorickPeterse/gem-mirror) - 외부 gem 소스의 내부 미러를 운영합니다.
* [Gemfury](http://www.gemfury.com/) - 비공개 클라우드 기반 RubyGems 서버입니다. 기여자의 수를 기준으로 가격이 책정됩니다.

유틸리티
---------

* [gemnasium-parser](https://github.com/laserlemon/gemnasium-parser) - gemfile이나 gemspec의 루비 코드를 보지 않고 의존성을 파악합니다.
* [Gemrat](https://github.com/DruRly/gemrat) - 커맨드 라인에서 Gemfile에 최신 버전의 gem을 추가합니다.
