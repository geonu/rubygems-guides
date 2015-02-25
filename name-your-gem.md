---
layout: default
title: gem 이름짓기
url: /name-your-gem
previous: /gems-with-extensions
next: /publishing
---

<em class="t-gray">gem의 이름에서 "_"와 "-"의 사용에 관한 권고</em>

이름짓기에 대한 권고 사항의 예를 몇 가지 들어 보겠습니다.

Gem 이름               | Require 문                       | 메인 클래스나 모듈
---------------------- | -------------------------------- | -----------------------
`ruby_parser`          | `require 'ruby_parser'`          | `RubyParser`
`rdoc-data`            | `require 'rdoc/data'`            | `RDoc::Data`
`net-http-persistent`  | `require 'net/http/persistent'`  | `Net::HTTP::Persistent`
`net-http-digest_auth` | `require 'net/http/digest_auth'` | `Net::HTTP::DigestAuth`

이런 권고의 목적은 gem에서 파일을 어떻게 require 하는지에 대한 힌트를 사용자에게
주기 위해서입니다. 이런 관례에 따르면 Bundler에서 추가 설정 없이 gem을 require
할 수도 있습니다.

[rubygems.org][rubygems]에 gem을 배포할 때, 불쾌한 이름이거나, 지적 재산권을
어기거나 gem의 내용물이 기준에 해당하면 삭제될 수도 있습니다. 이런 gem이 있다면
[RubyGems Support][rubygems-support] 사이트에서 신고하실 수 있습니다.

[rubygems]: http://rubygems.org
[rubygems-support]: http://help.rubygems.org

여러 단어에 밑줄 사용하기
----------------------------------

클래스나 모듈이 여러 단어를 사용한다면, 밑줄을 사용해 단어를 구분하세요. 이
단어는 require할 파일명과 같아서, 사용자가 gem을 사용하기 쉽게합니다.

확장기능에 대시 사용하기
-------------------------

다른 gem에 기능을 추가하였다면, 대시를 사용하세요. 이것은 보통 require 문 안에서
`/`에(따라서 gem의 디렉토리 구조에) 메인 클래스나 모듈에서는 `::`에 대응합니다.

적절하게 밑줄과 대시 섞어주기
----------------------------------------

클래스나 모듈에서 여러 단어를 사용하고 다른 gem에도 기능을 추가하였다면, 위에
있는 두 가지 규칙를 따르세요. 예를 들어, [`net-http-digest_auth`][digest-gem]는
[HTTP digest authentication][digest-standard]을 `net/http`에 넣습니다.
사용자는 (`Net::HTTP::DigestAuth` 클래스에 있는) 확장기능을 사용할 때
 `require 'net/http/digest_auth'`라고 하면 될 것입니다.

[digest-gem]: https://rubygems.org/gems/net-http-digest_auth
[digest-standard]: http://tools.ietf.org/html/rfc2617

대문자 사용하지 않기
---------------------------

OS X와 Windows는 기본적으로 대소문자 구별하지 않는 파일시스템을 채용하고
있습니다. 사용자는 실수로 gem을 대문자를 사용해 require할 수 있고, 이는 windows나
OS X가 아닌 시스템에서 호환성이 없습니다. 이는 대부분 초보자가 하는 실수가
되겠지만, 필요 이상으로 초보를 혼란스럽게 할 필요는 없을 것 같습니다.

Credits
-------

이 가이드는 Eric Hodel 님의 [How to Name Gems][how-to-name-gems]를 바탕으로
만들었습니다.

[how-to-name-gems]: https://web.archive.org/web/20130821183311/http://blog.segment7.net/2010/11/15/how-to-name-gems
