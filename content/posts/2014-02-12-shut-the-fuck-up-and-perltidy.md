---
layout: post
title: "shut the fuck up and perltidy"
date: 2014-02-12 17:07:08 +0900
comments: true
categories: perltidy
---

`$HOME/.perltidyrc`

    # https://github.com/kraih/mojo/blob/master/.perltidyrc
    -pbp # Start with Perl Best Practices
    -w # Show all warnings
    -iob # Ignore old breakpoints
    -l=79 # 79 characters per line
    -mbl=2 # No more than 2 blank lines
    -i=4 # Indentation is 4 columns
    -ci=2 # Continuation indentation is 2 columns
    -vt=0 # Less vertical tightness
    -pt=2 # High parenthesis tightness
    -bt=2 # High brace tightness
    -sbt=2 # High square bracket tightness
    -isbc # Don't indent comments without leading space

여기에 emacs 에다가
[perltidy-mode.el](http://search.cpan.org/~jjore/perltidy-mode/) load
하고 

    (eval-after-load "cperl-mode"
      '(add-hook 'cperl-mode-hook 'perltidy-mode))

하고 저장하면 저장할때마다 알아서 `perltidy` 되기 땜시 굳입니다
팀에서 통일해서 쓰면 좋겟습니다.

### SEE ALSO ###

[tidyall](http://aanoaa.github.io/blog/2012/09/12/tidyall/)
