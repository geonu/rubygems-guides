---
layout: default
title: 확장기능을 포함한 gem
url: /gems-with-extensions
previous: /make-your-own-gem
next: /name-your-gem
alias: /c-extensions
---

<em class="t-gray">설치할 때 빌드되는 확장기능을 가진 gem 만들기</em>

많은 gem이 c로 짜인 라이브러리를 루비로 래핑해서 사용합니다. 예를 들어
[nokogiri](https://rubygems.org/gems/nokogiri)는
[libxml2, libxslt](http://www.xmlsoft.org)을 래핑하고
[pg](https://rubygems.org/gems/pg)는 [PostgreSQL
데이터베이스](http://www.postgresql.org)의 인터페이스이고
[mysql](https://rubygems.org/gems/mysql),
[mysql2](https://rubygems.org/gems/mysql2) gem은
[MySQL 데이터베이스](http://www.mysql.com)의 인터페이스를 제공합니다.

확장기능을 사용하는 gem을 만들려면 몇 단계를 거쳐야 합니다. 이 가이드는 가능한
한 쉽게, 유지보수하기 좋게 하려면 무엇을 gemspec에 넣어야 하는지 설명하는 데에
중점을 둡니다. 이 가이드에 있는 확장기능은 C 표준 라이브러리의 `malloc()`과
`free()`를 래핑합니다.

Gem 레이아웃
----------

모든 gem은 개발자가 작업할 때 필요한 작업이 들어 있는 Rakefile부터 만들어야
합니다. 확장기능용 파일은 `ext/` 디렉토리 안의 확장기능 이름과 같은 디렉토리에
들어가야 합니다. 이 예제에서는 "my_malloc"이라는 이름을 사용하겠습니다.

어떤 확장기능은 일부는 C로, 일부는 루비로 구현됩니다. C, 자바 확장기능 같은
여러 언어를 지원할 생각이라면, C-specific 루비 파일을 `ext/` 디렉토리뿐만
아니라 `lib/` 디렉토리에도 넣으셔야 합니다.

    Rakefile
    ext/my_malloc/extconf.rb               # extension configuration
    ext/my_malloc/my_malloc.c              # extension source
    lib/my_malloc.rb                       # generic features

확장기능이 빌드될 때, `ext/my_malloc/lib/` 안의 파일이 `lib/` 디렉토리에
설치됩니다.

extconf.rb
----------

extconf.rb는 확장기능을 빌드할 Makefile을 설정합니다. extconf.rb는 확장기능에서
필요한 함수, 매크로, 공유 라이브러리를 체크해야 합니다. 이 중 빠진 것이 있으면
extconf.rb는 에러와 함께 종료되어야 합니다.

이 extconf.rb는 `malloc()`, `free()`를 체크하고, 확장기능을
`lib/my_malloc/my_malloc.so`에 설치 할 Makefile을 생성합니다.

    require "mkmf"

    abort "missing malloc()" unless have_func "malloc"
    abort "missing free()"   unless have_func "free"

    create_makefile "my_malloc/my_malloc"

extconf.rb의 작성과 관련 메소드의 더 자세한 정보는 [mkmf 문서][mkmf.rb]와
[README.EXT][README.EXT]를 살펴보세요.

C 확장기능
-----------

`malloc()`, `free()`를 래핑하는 C 확장기능은 `ext/my_malloc/my_malloc.c`에
넣습니다. 이런 내용입니다.

    #include <ruby.h>

    struct my_malloc {
      size_t size;
      void *ptr;
    };

    static void my_malloc_free(void *p) {
      struct my_malloc *ptr = p;

      if (ptr->size > 0)
        free(ptr->ptr);
    }

    static VALUE my_malloc_alloc(VALUE klass) {
      VALUE obj;
      struct my_malloc *ptr;

      obj = Data_Make_Struct(klass, struct my_malloc, NULL, my_malloc_free, ptr);

      ptr->size = 0;
      ptr->ptr  = NULL;

      return obj;
    }

    static VALUE my_malloc_init(VALUE self, VALUE size) {
      struct my_malloc *ptr;
      size_t requested = NUM2SIZET(size);

      if (0 == requested)
        rb_raise(rb_eArgError, "unable to allocate 0 bytes");

      Data_Get_Struct(self, struct my_malloc, ptr);

      ptr->ptr = malloc(requested);

      if (NULL == ptr->ptr)
        rb_raise(rb_eNoMemError, "unable to allocate %ld bytes", requested);

      ptr->size = requested;

      return self;
    }

    static VALUE my_malloc_release(VALUE self) {
      struct my_malloc *ptr;

      Data_Get_Struct(self, struct my_malloc, ptr);

      if (0 == ptr->size)
        return self;

      ptr->size = 0;
      free(ptr->ptr);

      return self;
    }

    void Init_my_malloc(void) {
      VALUE cMyMalloc;

      cMyMalloc = rb_const_get(rb_cObject, rb_intern("MyMalloc"));

      rb_define_alloc_func(cMyMalloc, my_malloc_alloc);
      rb_define_method(cMyMalloc, "initialize", my_malloc_init, 1);
      rb_define_method(cMyMalloc, "free", my_malloc_release, 0);
    }

이 확장기능은 간단한 몇 가지 기능만 있습니다.

* `struct my_malloc`는 할당된 메모리를 유지합니다.
* `my_malloc_free()`는 가비지 콜렉션 이후 할당된 메모리를 해제합니다.
* `my_malloc_alloc()`는 루비 래퍼 객체를 생성합니다.
* `my_malloc_init()`는 루비에서 메모리를 할당합니다.
* `my_malloc_release()`는 루비에서 메모리를 해제합니다.
* `Init_my_malloc()`는 `MyMalloc` 클래스에 함수를 등록합니다.

확장기능의 빌드는 이렇게 테스트 할 수 있습니다.

    $ cd ext/my_malloc
    $ ruby extconf.rb
    checking for malloc()... yes
    checking for free()... yes
    creating Makefile
    $ make
    compiling my_malloc.c
    linking shared-object my_malloc.bundle
    $ cd ../..
    $ ruby -Ilib:ext -r my_malloc -e "p MyMalloc.new(5).free"
    #<MyMalloc:0x007fed838addb0>

음... 따분한 일이네요. 자동화합시다!

rake-compiler
-------------

[rake-compiler][rake-compiler]는 확장기능 빌드를 자동화하기 위한 rake 작업의
모음입니다. rake-compiler는 같은 프로젝트 안의 C나 자바 확장기능과 함께
사용할 수 있습니다. (nokogiri가 이런 식으로 씁니다.)

rake-compiler는 쉽게 추가할 수 있습니다.

    require "rake/extensiontask"

    Rake::ExtensionTask.new "my_malloc" do |ext|
      ext.lib_dir = "lib/my_malloc"
    end

이제 `rake compile`으로 확장기능을 빌드할 수 있습니다. 다른 작업(테스트 같은)
사이에 컴파일 작업을 넣을 수 있습니다.

`lib_dir` 설정은 `lib/my_malloc/my_malloc.so`(또는 `.bundle`이나 `.dll`)에 공유
라이브러리를 둡니다. 이렇게 하면, gem의 최상위 파일은 루비파일이 되고,
루비를 쓰기에 알맞은 부분은 루비로 짤 수 있습니다.

예를 들어:

    class MyMalloc

      VERSION = "1.0"

    end

    require "my_malloc/my_malloc"

`lib_dir` 설정으로 여러 버전의 루비를 위해 빌드된 확장기능을 포함한 gem을 빌드할
수도 있습니다. (루비 1.9.3 확장기능은 루비 2.0.0의 확장기능과 같이 사용할 수
없습니다.) `lib/my_malloc.rb`은 설치할 때 맞는 공유 라이브러리를 고를 수
있습니다.

Gemspec
-----------------

gem을 만들기 위해 마지막으로 gemspec의 확장기능 목록에 extconf.rb를 추가합니다.

    Gem::Specification.new "my_malloc", "1.0" do |s|
      # [...]

      s.extensions = %w[ext/my_malloc/extconf.rb]
    end

이제 gem을 빌드하고 배포할 수 있습니다!

확장기능 이름짓기
----------------

gem 사이의 의도치 않은 상호작용을 피하기 위해, 각 gem의 파일을 한 디렉토리에
넣어두면 좋습니다. `<name>`이라는 gem을 아래와 같이 구성하는 것을 권장합니다.

1. `ext/<name>`은 `extconf.rb`, 소스 파일을 포함하는 디렉토리입니다.
2. `ext/<name>/<name>.c`는 메인 소스 파일(다른 파일도 있겠죠?)입니다.
3. `ext/<name>/<name>.c`는 `Init_<name>` 함수를 포함합니다. (require로 불러오려면
   `Init_` 함수 뒤의 이름은 확장기능의 이름과 정확히 같아야 합니다.
4. `ext/<name>/extconf.rb`는 확장기능을 컴파일하는 데 필요한 모든 요소가
   존재하는 경우에만 `create_makefile('<name>/<name>')`를 호출합니다.
5. gemspec에 `extensions = ['ext/<name>/extconf.rb']`와 필요한 확장기능의 소스
   파일을 전부 `files` 목록에 추가합니다.
6. `lib/<name>.rb`는 C 확장기능을 로드할 `require '<name>/<name>'`을 포함합니다.

더 읽을거리
---------------

* [my_malloc](https://github.com/rubygems/guides/tree/my_malloc)은 이 확장
  기능의 소스에 주석을 조금 덧붙여 놓았습니다.
* [README.EXT][README.EXT]는 훨씬 더 자세하게 어떻게 루비에서 확장기능을
  빌드하는지 설명합니다.
* [MakeMakefile][mkmf.rb]에는 mkmf.rb의 문서가 있습니다. mkmf.rb는 extconf.rb
  라이브러리에서 루비와 C 라이브러리 기능을 찾는데 사용합니다.
* [rake-compiler][rake-compiler]는 C와 자바 확장기능의 빌드를 자연스럽게
  Rakefile에 넣습니다.
* Aaron Patterson 님의 [C 확장기능 파트
  1](http://tenderlovemaking.com/2009/12/18/writing-ruby-c-extensions-part-1.html),
  [파트 2](http://tenderlovemaking.com/2010/12/11/writing-ruby-c-extensions-part-2.html)
* C 라이브러리의 인터페이스는 루비를 사용해 작성할 수 있습니다.
  [fiddle](http://ruby-doc.org/stdlib-2.0/libdoc/fiddle/rdoc/Fiddle.html)(표준
  라이브러리에 포함), [ruby-ffi](https://github.com/ffi/ffi)를 참조하세요.

[README.EXT]: https://github.com/ruby/ruby/blob/trunk/README.EXT
[mkmf.rb]: http://ruby-doc.org/stdlib-2.0/libdoc/mkmf/rdoc/MakeMakefile.html
[rake-compiler]: https://github.com/luislavena/rake-compiler
