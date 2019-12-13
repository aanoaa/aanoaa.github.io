---
layout: post
title: "keep simple dist.ini"
date: 2014-02-12 17:20:06 +0900
comments: true
categories: Dist::Zilla, dzil
---

[Dist::Zilla](http://search.cpan.org/~rjbs/Dist-Zilla/lib/Dist/Zilla.pm)
는 매우 편리한 배포도구 입니다.

기능이 넘 많아서 잘 모르고 써왔는데, 이게 뭔지도 모르는걸 쓰다보니
헛갈려서 요새는 레알 간단하게만 쓰고 있습니다.

갠적으로 딱 필요한 메타파일은 이거 3개 입니다.

- dist.ini
- MANIFEST.SKIP
- cpanfile

`cpanfile` 이 없을적에는 `Makefile.PL` 로 부터 의존성문제를 해결했기
때문에 복잡했는데 이제는 `cpanm --installdeps .` 덕분에 몹.시.편.리.

배포할때 빼고 싶은 파일은 `MANIFEST.SKIP` 에 적어주고, `dist.ini` 로
build 하고 배포

좋은 플러그인이 많은데 나중되면 어케 쓰는지도 몰겟고 의존성문제로 인해
불편하기도 해서 그냥 자잘구레한 작업은 직접 손으로 하는걸로..

`dist.ini`

```ini
name = mymodule
abstract = wat
author = aanoaa <wat@wat.wat>
license = Perl_5
copyright_holder = aanoaa
copyright_year = 2014
version = v0.0.1
[@Basic]
[PkgVersion]
```

`MANIFEST.SKIP`

    # Makemaker generated files and dirs.
    ^MANIFEST\.
    ^Makefile$
    ^blib/
    
    # version-control
    ^.git/
    ^.gitignore
    
    ^dist.ini
    ^README.md


이렇게 쓰고 있습니다.
제일 간단한 설정이라고 생각합니다.

전에는 dzil profile 같은 것도 만들고 그랬는데 이제는 그냥 예전거
복사해다 쓰고 그럽니다.
