---
layout: default
title: SSL Certificate Update
url: /ssl-certificate-update
previous: /security
next: /patterns
---

# SSL 인증서 업데이트

**UPDATE 2014-12-21**: RubyGems 1.8.30, 2.0.15, 2.2.3이 릴리스 되었습니다.
이는 수동 설치가 필요합니다. 밑에 있는
[지침](#installing-using-update-packages-new)을 보세요.

---

안녕하세요.

이 페이지에 오셨다면 아마 RubyGems에서 업데이트를 하다 이 SSL 에러를 보셨기
때문일 겁니다.

    SSL_connect returned=1 errno=0 state=SSLv3 read server certificate B: certificate verify failed

이 에러는 rubygems.org 인프라 내부의 변경 때문입니다. 좀 더 이해하고 싶다면 계속
읽어주세요.

만약, *한 줄 요약*을 원하시면 가이드는 건너 뛰고 회피책만 읽으세요.

## 배경

SSL이나 인증서가 익숙지 않으신 분을 위해 설명하자면 콘텐츠를 안전하게 전달하기
위해서는 많은 것이 필요합니다.

SSL 인증서는 웹 사이트에서 사용됩니다. 이는 인증기관(CA)에서 얻은 값과 개인
키에서 생성된 것, 그리고 이들 각각의 서명으로 구성됩니다.

보통 몇 달 전까지는, 개인 키 서명은 키 자체를 배포하지 않고(키는 공유되는
정보가 아님을 기억하세요.) 개인 키의 다이제스트(혹은 체크섬)를 제공하는 방법으로
SHA-1을 사용했습니다.

SHA-1은 많은 약점이 발생했고, 많은 웹 서버와 사이트들이 브라우저의 변경에 대비해
SHA-2로(구체적으로는 SHA256 이상) 업그레이드했습니다.

## RubyGems의 문제

RubyGems(커맨드 라인 도구) 문제는 RubyGems과 서버와의 접속을 확립하기
위해 코드 안에 인증서를 넣을 필요가 있다는 것입니다. 심지어 기반 운영 체제의
신원을 확인 할 수 없더라도 말이죠.

몇 달 전까지만 해도, 이 인증서는 한 CA에서만 제공했습니다만, 새로운 인증서는
이제 다른 곳에서 제공합니다.

이런 이유로, 기존의 설치된 RubyGems는 인증서를 전환하기 전에 업데이트 되어야 하고
이 변경이 널리 퍼질 때까지(그리고 사람이 업데이트할 때까지) 기다려야 합니다.

소프트웨어란 게 일반적으로 그렇듯이, 아마 동기화는 이루어지기 힘들 것이고
rubygems.org 정도의 크기와 사용자 수를 이런 방법으로 감당하기는 불가능에
가깝습니다.

[이슈 #1050](https://github.com/rubygems/rubygems/issues/1050#issuecomment-61422934)에서
이를 설명했습니다.

우리는 IRC에서 의논하여, RubyGems의 모든 주요 브랜치(1.8, 2.0, 2.2, 2.4)에
패치와 백포트를 제공하기로 했습니다.

여기에서 이 변경들에 관한 커밋을 확인하실 수 있습니다.

- [1.8 브랜치](https://github.com/rubygems/rubygems/commit/74ee66395c8e1b9ad6a45ba2f292bee8c6ea1a50)
- [2.0 브랜치](https://github.com/rubygems/rubygems/commit/98f5f44c7141881c756003e4256b1a96b200b98e)
- [2.2 브랜치](https://github.com/rubygems/rubygems/commit/17d8922966051864a0c4bf768623e9d0c854de26)
- [2.4 브랜치](https://github.com/rubygems/rubygems/commit/5a31f092d483ea7ccd91adbf08a88593cf0fbbc7)

문제는 RubyGems 2.4.4만 릴리스되었고, 1.8, 2.0, 2.2에서의 루비 설치는 동작하지
않는 채로 있다는 점입니다.

특히, RubyGems 2.4부터 Windows에서 동작하지 않고 있습니다.

이 일은 누구에게나 일어날 있다는 것을 알아주시기 바랍니다. 짧은 기간에 여러
버전을 릴리스하는 *어떤* 소프트웨어에도 이런 일은 있을 수 있으며, 매우 복잡하고
시간에 민감한 문제입니다.

이 이슈를 해결하는 각 버전의 정식 릴리스가 있다고 해도, RubyGems를 통해 설치할
수 없습니다.(이전에 설명한 닭이 먼저냐 달걀 먼저냐 문제)

정식 릴리스가 나오면, 설치는 아마 간단해질 것입니다. 나오기 전에는 밑에 설명된
지침을 사용해서 진행하세요.

## 업데이트 패키지로 설치하기 (NEW)
{:#installing-using-update-packages-new}

RubyGems 1.8.x, 2.0.x, 2.2.x가 릴리스되었으니 이 버전으로 수동 업데이트하실 수
있습니다.

먼저 설치할 RubyGems의 바른 버전을 다운받습니다.(예를 들어, `1.8.28` 버전이라면
`1.8.30`를 다운로드합니다.)

*주의*: 사용중인 RubyGems의 버전을 알아보려면, 커맨드 라인에서 `gem
--version`을 실행하세요.

GitHub의 다운로드 링크는 
[Releases](https://github.com/rubygems/rubygems/releases)에서 찾으실 수 있습니다.

이제 `rubygems-update-X.Y.Z.gem`을 찾습니다. `X.Y.Z`는 업데이트 해야할
RubyGems의 버전이 됩니다.

- 1.8.x 사용 중: [1.8.30](https://github.com/rubygems/rubygems/releases/tag/v1.8.30) 다운로드
- 2.0.x 사용 중: [2.0.15](https://github.com/rubygems/rubygems/releases/tag/v2.0.15) 다운로드
- 2.2.x 사용 중: [2.2.3](https://github.com/rubygems/rubygems/releases/tag/v2.2.3) 다운로드

나중에 찾을 수 있는 디렉터리에 다운로드 하세요.(예를 들면, 하드 드라이브 루트
`C:\`)

이제 커맨드 프롬프트에 밑의 내용을 입력합니다.

```
C:\>gem install --local C:\rubygems-update-1.8.30.gem
C:\>update_rubygems --no-ri --no-rdoc
```

그 후, `gem --version`을 실행하면 업데이트된 새 버전을 확인할 수 있습니다.

이제 안전하게 `rubygems-update` gem을 제거 할 수 있습니다.

```
C:\>gem uninstall rubygems-update -x
Removing update_rubygems
Successfully uninstalled rubygems-update-2.2.3
```

## SSL 이슈의 수동 해결책

위에 설명된 이슈를 상세하게 읽어주셨다면 감사합니다.

이제, 설치된 RubyGems를 수동으로 고치고 싶으실 겁니다.

간단히 몇 단계만 거치면 됩니다.

- 1 단계: 새로운 트러스트 인증서 얻기
- 2 단계: 설치된 RubyGems 인증서 디렉터리를 찾기
- 3 단계: 새 트러스트 인증서 복사하기
- 4 단계: 끝

### 1 단계: 새로운 트러스트 인증서 얻기

이전 단락을 읽으셨다면, 이게 무슨 뜻인지 아셔야 합니다.(모르시겠다면 반성하세요.)

[AddTrustExternalCARoot-2048.pem](https://raw.githubusercontent.com/rubygems/rubygems/master/lib/rubygems/ssl_certs/AddTrustExternalCARoot-2048.pem)을 다운로드 하셔야 합니다.

위의 링크로 다운로드 받아 나중에 쉽게 찾을 수 있는 곳을 지정해
저장하세요.(예를 들어 바탕화면)

**중요**: 파일은 반드시 `.pem` 확장자를 가져야 합니다. 크롬 같은 브라우저는 일반
텍스트 파일로 저장하려 할 것입니다. 다운로드 후 파일 이름에 `.pem`이 있는지
확인하셔야 합니다.

### 2 단계: 설치된 RubyGems 인증서 디렉터리를 찾기

이 파일을 복사하려면, 어디에 복사해야 하는지 알아야 합니다.

설치된 루비가 어디 있냐에 따라, 디렉터리는 달라집니다.

루비 2.1.5가 기본적으로 설치되는 곳을 예로 들면 `C:\Ruby21`에 있습니다.

커맨드 프롬프트를 열고 다음을 입력하세요.

```
C:\>gem which rubygems
C:/Ruby21/lib/ruby/2.1.0/rubygems.rb
```

이제 디렉터리를 찾았습니다. 같은 창에서 슬래시 대신 백슬래시를 사용해 경로
부분을 입력합니다.

```
C:\>start C:\Ruby21\lib\ruby\2.1.0\rubygems
```

이 명령은 우리가 찾은 디렉터리 안에서 탐색기 창을 열 것입니다.

### 3 단계: 새 트러스트 인증서 복사하기

이제, `ssl_certs` 디렉터리를 찾아 이전 단계에서 받은 `.pem` 파일을 복사합니다.

이 파일은 `GeoTrustGlobalCA.pem` 같은 다른 파일과 함께 나열될 것입니다.

### 4 단계: 끝

4 단계는 사실 없습니다. 이제 아무 이슈없이 루비 gem을 설치할 수 있으실 겁니다.
