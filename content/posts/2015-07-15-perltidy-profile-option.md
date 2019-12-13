---
layout: post
title: "perltidy profile option"
date: 2015-07-15 16:31:41 +0900
comments: true
categories: [perl,perltidy]
---

    $ perldoc perltidy

    ...
    -pro=filename or --profile=filename
        To simplify testing and switching .perltidyrc files, this command
        may be used to specify a configuration file which will override the
        default name of .perltidyrc. There must not be a space on either
        side of the '=' sign. For example, the line

           perltidy -pro=testcfg

        would cause file testcfg to be used instead of the default
        .perltidyrc.

        A pathname begins with three dots, e.g. ".../.perltidyrc", indicates
        that the file should be searched for starting in the current
        directory and working upwards. This makes it easier to have multiple
        projects each with their own .perltidyrc in their root directories.
    ...

여기서 중요한것은 `...` 으로 시작하면 현재 디렉토리에서부터 위로
올라가면서 이름으로 프로파일을 찾는다는 것

global 설정은 `$HOME/.perltidyrc` 로 두고 각 프로젝트 root 마다
`.perltidyrc` 파일을 두면 신경쓰지 않고 사용할 수 있다는 겁니다.

굳

alias 걸어두고 사용하면 편리하겠습니다.
emacs 를 사용하는 인간이라면 `perltidy-mode` 의 `perltidy-bin` 을
수정해주면 되겠습니다.

선조들은 훌륭합니다.
