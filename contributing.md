---
layout: default
title: RubyGems에 기여하기
url: /contributing
previous: /resources
next: /faqs
---

<em class="t-gray">RubyGems와 주변 생태 개선을 도와주기</em>

RubyGems에 기여하려 하시나요? 잘 찾아 오셨습니다!
지금도 개발이 많이 이루어지고 있고, 당신의 도움을 필요로 합니다. 밑의 링크를 타고
기여를 시작하시거나 프로젝트의 메인테이너에 연락하세요.

* [핵심 프로젝트](#core-projects)
* [생태계 프로젝트](#ecosystem-projects)
* [아이디어 추가하기](#add-your-own-idea)

핵심 프로젝트
-----------------
{:#core-projects}

이들 프로젝트는 핵심 [RubyGems 팀](https://github.com/rubygems/)의 영향 아래
있습니다.

<a class="project__name is-first" href="https://github.com/rubygems/rubygems">RubyGems</a>

루비에서 가장 많이 쓰는 패키지 시스템입니다. 루비 1.9 이상에 포함되어 있으며,
루비 1.8에서도 사용할 수 있습니다. 커맨드 라인에서 `gem`을 실행할 때 이
프로젝트를 사용합니다.

<div class="project__links">
  <a class="project__link t-link" href="https://github.com/rubygems/rubygems/issues">이슈</a>
  <a class="project__link t-link" href="https://groups.google.com/forum/#!forum/rubygems-developers">메일링 리스트</a>
</div>

<p class="avatars">
  <a href="https://github.com/drbrain">
    <img src="https://secure.gravatar.com/avatar/58479f76374a3ba3c69b9804163f39f4?s=32" title="Eric Hodel">
  </a>
  <a href="https://github.com/zenspider">
    <img src="https://secure.gravatar.com/avatar/16c4b19d8670085a428787f8b2438223?s=32" title="Ryan Davis">
  </a>
  <a href="https://github.com/jbarnette">
    <img src="https://secure.gravatar.com/avatar/c237cf537a06b60921c97804679e3b15?s=32" title="John Barnette">
  </a>
  <a href="https://github.com/evanphx">
    <img src="https://secure.gravatar.com/avatar/540cb3b3712ffe045113cb03bab616a2?s=32" title="Evan Phoenix">
  </a>
</p>

<em class="t-gray t-uppercase">코드 가이드라인:</em>

* 새 기능은 테스트와 함께 추가되어야 합니다.
* 코드가 기존의 것과 잘 섞여야 합니다.(예를 들어, 줄 뒤의 공백은 없어야 하고,
  들여쓰기랑 코딩 스타일도 맞아야 합니다.)
* 이력 파일과 버전 번호는 수정하지 않습니다.
* 질문이 있으면 IRC의 #rubygems 채널이나 [이슈][1]로 남기세요.

[0]: https://github.com/rubygems/rubygems
[1]: https://github.com/rubygems/rubygems/issues
[2]: http://help.rubygems.org

<a class="project__name" href="https://github.com/rubygems/rubygems.org">RubyGems.org</a>

루비 커뮤니티의 gem 호스트 서비스입니다. 깔끔하고 이용하기 편한 프로젝트 페이지로
gem을 찾고, 배포하고, 관리하기에 더 나은 API를 제공합니다.

<div class="project__links">
  <a class="project__link t-link" href="https://rubygems.org">사이트</a>
  <a class="project__link t-link" href="https://github.com/rubygems/rubygems.org/issues">이슈</a>
  <a class="project__link t-link" href="https://groups.google.com/forum/#!forum/gemcutter">메일링 리스트</a>
</div>

<p class="avatars">
  <a href="https://github.com/qrush">
    <img src="https://secure.gravatar.com/avatar/eb8975af8e49e19e3dd6b6b84a542e26?s=32" title="Nick Quaranto">
  </a>
  <a href="https://github.com/sferik">
    <img src="https://secure.gravatar.com/avatar/1f74b13f1e5c6c69cb5d7fbaabb1e2cb?s=32" title="Erik Michaels-Ober">
  </a>
  <a href="https://github.com/dwradcliffe">
    <img src="https://secure.gravatar.com/avatar/013fd4dfb0e29744d5f37cf9068ba930?s=32" title="David Radcliffe">
  </a>
  <a href="https://github.com/evanphx">
    <img src="https://secure.gravatar.com/avatar/540cb3b3712ffe045113cb03bab616a2?s=32" title="Evan Phoenix">
  </a>
</p>

<a class="project__name" href="https://github.com/rubygems/rubygems-infrastructure">RubyGems 인프라</a>

AWS에 있는 Rubygems.org를 설정하고 관리하기 위한 Chef 쿡북과 부트스트랩
스크립트입니다.

<div class="project__links">
  <a class="project__link t-link" href="https://github.com/rubygems/rubygems-infrastructure/wiki">위키</a>
  <a class="project__link t-link" href="https://trello.com/b/cd2HqKnE/infrastructure">Trello</a>
</div>

<p class="avatars">
  <a href="https://github.com/skottler">
    <img src="https://secure.gravatar.com/avatar/ee9182ab4e45d446dfa05c20c341371f?s=32" title="Sam Kottler">
  </a>
  <a href="https://github.com/dwradcliffe">
    <img src="https://secure.gravatar.com/avatar/013fd4dfb0e29744d5f37cf9068ba930?s=32" title="David Radcliffe">
  </a>
  <a href="https://github.com/evanphx">
    <img src="https://secure.gravatar.com/avatar/540cb3b3712ffe045113cb03bab616a2?s=32" title="Evan Phoenix">
  </a>
</p>

<a class="project__name" href="https://github.com/rubygems/rubygems-status">RubyGems 상태</a>

rubygems.org 인프라의 상태를 보여주는 간단한 레일즈 앱입니다.

<div class="project__links">
  <a class="project__link t-link" href="http://status.rubygems.org">사이트</a>
  <a class="project__link t-link" href="https://github.com/rubygems/rubygems-status/issues">이슈</a>
</div>

<p class="avatars">
  <a href="https://github.com/sferik">
    <img src="https://secure.gravatar.com/avatar/1f74b13f1e5c6c69cb5d7fbaabb1e2cb?s=32" title="Erik Michaels-Ober">
  </a>
  <a href="https://github.com/dwradcliffe">
    <img src="https://secure.gravatar.com/avatar/013fd4dfb0e29744d5f37cf9068ba930?s=32" title="David Radcliffe">
  </a>
</p>

<a class="project__name" href="https://github.com/rubygems/guides">RubyGems 가이드</a>

튜토리얼과 레퍼런스를 가지고 있는 RubyGems 문서의 공식 페이지입니다.
사용자의 가이드 기여는 매우 환영하고 또 권장합니다!

<div class="project__links">
  <a class="project__link t-link" href="http://guides.rubygems.org">사이트</a>
  <a class="project__link t-link" href="https://github.com/rubygems/guides/issues">이슈</a>
</div>

<p class="avatars">
  <a href="https://github.com/qrush">
    <img src="https://secure.gravatar.com/avatar/eb8975af8e49e19e3dd6b6b84a542e26?s=32" title="Nick Quaranto">
  </a>
  <a href="https://github.com/sandal">
    <img src="https://secure.gravatar.com/avatar/31e038e4e9330f6c75ccfd1fca8010ee?s=32" title="Gregory Brown">
  </a>
  <a href="https://github.com/ffmike">
    <img src="https://secure.gravatar.com/avatar/a54251b745d59735ea5e9f0656a5d58d?s=32" title="Mike Gunderloy">
  </a>
</p>

<a class="project__name" href="https://github.com/rubygems/rubygems-test">RubyGems 테스터</a>

커뮤니티의 노력으로 만든 여러 환경에서의 여러 gem의 테스트 결과입니다.

<div class="project__links">
  <a class="project__link t-link" href="http://test.rubygems.org/">사이트</a>
  <a class="project__link t-link" href="https://github.com/rubygems/rubygems-test/issues">이슈</a>
</div>

<p class="avatars">
  <a href="https://github.com/bluepojo">
    <img src="https://secure.gravatar.com/avatar/4b1e87301a43b027903617a98d61831a?s=32" title="Josiah Kiehl">
  </a>
  <a href="https://github.com/erikh">
    <img src="https://secure.gravatar.com/avatar/1b641a79b2717f2d582ad455b40d5b89?s=32" title="Erik Hollensbe">
  </a>
</p>

<a class="project__name" href="https://github.com/rubygems/gemwhisperer">Gem Whisperer</a>

푸시되는 모든 gem을 감시하기 위한 [RubyGems.org 웹훅](http://guides.rubygems.org/rubygems-org-api/#webhook)의
사용례입니다. 현재 웹훅으로 [m.rubygems.org](http://m.rubygems.org)와 [@rubygems](http://twitter.com/rubygems)를
움직입니다.

<div class="project__links">
  <a class="project__link t-link" href="http://m.rubygems.org/">사이트</a>
  <a class="project__link t-link" href="https://github.com/rubygems/gemwhisperer/issues">이슈</a>
</div>

<p class="avatars">
  <a href="https://github.com/qrush">
    <img src="https://secure.gravatar.com/avatar/eb8975af8e49e19e3dd6b6b84a542e26?s=32" title="Nick Quaranto">
  </a>
  <a href="https://github.com/laserlemon">
    <img src="https://secure.gravatar.com/avatar/0887991a8846577a6aa85433d6ab3ea2?s=32" title="Steve Richert">
  </a>
</p>

<a class="project__name" href="https://github.com/rubygems/gems">RubyGems.org API 라이브러리</a>

RubyGems.org에서 사용할 수 있는 여러 API 단말의 루비 구현체입니다.
루비로 커뮤니티에서 사용 가능한 gem과 상호작용하는 서비스를 만든다면,
살펴보세요!

<div class="project__links">
  <a class="project__link t-link" href="https://github.com/rubygems/gems/issues">이슈</a>
</div>

<p class="avatars">
  <a href="https://github.com/sferik">
    <img src="https://secure.gravatar.com/avatar/1f74b13f1e5c6c69cb5d7fbaabb1e2cb?s=32" title="Erik Michaels-Ober">
  </a>
</p>

<a class="project__name" href="https://github.com/rubygems/rubygems-mirror">RubyGems 미러</a>

RubyGems 미러의 현재 상태는 솔직히 좋지 않습니다. RubyGems를 전 세계에 걸쳐
언제나 사용가능하게 해야할 필요가 있습니다. 이제 더 이상 변명은 없습니다!
[rubygems-mirror 위키](https://github.com/rubygems/rubygems-mirror/wiki/Mirroring-2.0)에서
어떻게 개선할지에 대한 토론이 진행 중입니다.

<div class="project__links">
  <a class="project__link t-link" href="https://github.com/rubygems/rubygems-mirror/issues">이슈</a>
</div>

<p class="avatars">
  <a href="https://github.com/raggi">
    <img src="https://secure.gravatar.com/avatar/b19b02a49b433c9e2e6e6c43785d2bfb?s=32" title="James Tucker">
  </a>
</p>

<a class="project__name" href="https://github.com/rubygems/rubygems-verification">RubyGems Verification</a>

서드 파티가 수집한 체크섬을 기반으로 rubygems.org에 있는 gem의 무결성을 검증하기
위한 도구와 데이터의 모음입니다.

<div class="project__links">
  <a class="project__link t-link" href="https://github.com/rubygems/rubygems-verification/issues">이슈</a>
</div>

## 생태계 프로젝트
{:#ecosystem-projects}

이들 프로젝트는 RubyGems의 핵심이 아니지만, 모두의 사용자 경험을 증진시키기 위해
RubyGems와 긴밀하게 협력합니다.

<a class="project__name is-first" href="https://github.com/bundler/bundler">Bundler</a>

Bundler는 체계적이고 반복적으로 많은 기기에 걸쳐 애플리케이션 전체 생명의
의존성을 관리합니다.

<div class="project__links">
  <a class="project__link t-link" href="http://bundler.io">사이트</a>
  <a class="project__link t-link" href="https://github.com/bundler/bundler/issues">이슈</a>
  <a class="project__link t-link" href="https://groups.google.com/forum/#!forum/ruby-bundler">메일링 리스트</a>
</div>

<p class="avatars">
  <a href="https://github.com/indirect">
    <img src="https://secure.gravatar.com/avatar/fb389f1e8b98d5d03be29e9dd309b3be?s=32" title="Andre Arko">
  </a>
  <a href="https://github.com/hone">
    <img src="https://secure.gravatar.com/avatar/efb7c66871043330ce1310a9bdd0aaf6?s=32" title="Terence Lee">
  </a>
  <a href="https://github.com/wycats">
    <img src="https://secure.gravatar.com/avatar/428167a3ec72235ba971162924492609?s=32" title="Yehuda Katz">
  </a>
  <a href="https://github.com/carllerche">
    <img src="https://secure.gravatar.com/avatar/da5274b27cc6c0f505495bf5d504575d?s=32" title="Carl Lerche">
  </a>
</p>

<a class="project__name" href="https://github.com/jbarnette/isolate">Isolate</a>

애플리케이션이 require한 정확한 gem 버전을 가지고 있는지 확인하는 간단한
gem 샌드박스입니다. Bundler처럼 의존성의 확인을 수행하지는 않습니다.

<div class="project__links">
  <a class="project__link t-link" href="https://github.com/jbarnette/isolate/issues">이슈</a>
</div>

<p class="avatars">
  <a href="https://github.com/jbarnette">
    <img src="https://secure.gravatar.com/avatar/c237cf537a06b60921c97804679e3b15?s=32" title="John Barnette">
  </a>
</p>

<a class="project__name" href="https://github.com/lsegal/rubydoc.info">RubyDoc.info</a>

문서화된 모든 RubyGem의 [YARD](http://yardoc.org) 문서를 제공하는 곳입니다.
gem을 넣는 즉시 문서가 생성됩니다! RubyGems.org는 이 사이트를 링크하며,
이 사이트는 [RubyGems.org의 웹훅](http://guides.rubygems.org/rubygems-org-api/#webhook)을
사용합니다.

<div class="project__links">
  <a class="project__link t-link" href="http://rubydoc.info">사이트</a>
  <a class="project__link t-link" href="https://github.com/lsegal/rubydoc.info/issues">이슈</a>
  <a class="project__link t-link" href="https://groups.google.com/forum/#!forum/yardoc">메일링 리스트</a>
</div>

<p class="avatars">
  <a href="https://github.com/indirect">
    <img src="https://secure.gravatar.com/avatar/fb389f1e8b98d5d03be29e9dd309b3be?s=32" title="Andre Arko">
  </a>
  <a href="https://github.com/hone">
    <img src="https://secure.gravatar.com/avatar/efb7c66871043330ce1310a9bdd0aaf6?s=32" title="Terence Lee">
  </a>
</p>

<a class="project__name" href="https://github.com/copiousfreetime/stickler">Stickler</a>

Stickler는 조직 내의 내부 gem 서버를 운영하고 구성하는 훌륭한 방법입니다.
Stickler는 gem을 미러링하고 내부이거나 사유 재산인 gem 소스를 제공하게 도울 수
있습니다.

<div class="project__links">
  <a class="project__link t-link" href="https://github.com/copiousfreetime/stickler/issues">이슈</a>
</div>

<p class="avatars">
  <a href="https://github.com/copiousfreetime">
    <img src="https://secure.gravatar.com/avatar/cff2d90ae70bbbb5d4865d8412159f85?s=32" title="Jeremy Hinegardner">
  </a>
</p>

<a class="project__name" href="https://github.com/cwninja/geminabox">Geminabox</a>

간단한 RubyGems 호스트가 필요하신가요? Geminabox로 만들 수 있습니다! 이
프로젝트는 복잡한 과정 없이 내부 RubyGems를 간단히 설정하고 gem을 올릴 수
있게 합니다.

<div class="project__links">
  <a class="project__link t-link" href="https://github.com/cwninja/geminabox/issues">이슈</a>
</div>

<p class="avatars">
  <a href="https://github.com/cwninja">
    <img src="https://secure.gravatar.com/avatar/f61c5838432c656ea88dd77a56a40f52?s=32" title="Tom Leal">
  </a>
</p>

아이디어 추가하기
---------------------
{:#add-your-own-idea}

이 목록에 새로운 아이디어를 추가해 주셨으면 합니다. RubyGems에 관련된 프로젝트를
하신다면 그냥 [이 저장소를 포크](https://github.com/rubygems/guides)해 링크를
추가해 주세요!
