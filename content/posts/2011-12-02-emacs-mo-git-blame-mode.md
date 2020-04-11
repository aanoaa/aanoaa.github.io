---
layout: post
title: "emacs mo-git-blame mode"
date: 2011-12-02 15:07
comments: true
categories: emacs
---

프로젝트 환경
------------

[silex][silex]라는 조그마한 회사를 댕기고 있습니다.

소스관리는 [git][git]사용하고 있고요, 내부에도 [gitosis][gitosis]를
운영하고 있긴 하지만, 주요 프로젝트는 [github][github] 유료 서비스를
사용하고 있습니다. 이슈 관리도 [github][github]에서
합니다. (버그질라를 사용하는 프로젝트도 있습니다)


코드리뷰
-------

아주 느슨합니다.
repo 접근 권한이 있다면 누구나 commit, push 할 수 있습니다.
코드 리뷰는 push후에 프로젝트 참여자가 알아서 합니다.

> 이건 요래서 그지같고, 저건 조래서 다시해라, 발로 썻냐 등등..

좋은 얘기도 합니다만, 지적질이 대세를 이루죠.

몇일전에는 바로 옆에서 [jeen][jeen]님이 그지같이 썻다고 해서
소주병 나발을 불기도 하고 뭐 그랬습니다.


git blame
---------

누가 무슨 짓 했는지 다 알 수 있습니다.

    519aaa9b (yongbin       2011-07-23 23:23:33 +0900 1) #!/bin/sh
    686cb0fb (Yongbin Yu    2011-09-24 15:58:03 +0900 2) TRUNNER_CONFDIR=t/selenium \
    2b3e6736 (Hyungsuk Hong 2011-10-13 11:25:22 +0900 3) plackup -Ilib -s Starman -p 3000 -a best_web.psgi --access-log /dev/null


문제점 그리고 새로운 인력의 투입
----------------------------

- push 했을때 review 하지 못하고 그냥 넘길때가 있습니다(많습니다)
- 프로젝트 중간에 커밋터 보급받았을때

남이 써놓은 코드를 열었는데 그지 같으면 말해줘서 바로 잡아야 합니다.
쓰다보니 거지 같아서 `git blame`으로 살폈는데 제 코드일 수도 있습니다.

뭐 어찌됐든 이 일련의 과정을 커맨트라인으로 하려면 불편합니다.

    while(1) {
        $ git blame oops
        $ git log 72ddf38
    }

[vim][vim] vs [emacs][emacs]
----------------------------

> @JEEN_LEE,  about 2 hours ago:
  예를 들어 vim 으로 특정 코드부분을 주시한다고 할 때, 어.. 이거 누가
  고쳤지 하는 부분이 있는데... 그때마다 blame 으로 파일을 지정해서
  찾아보는 데, 그 라인에서 어떻게 바로 뿅하고
  알 수 있게끔 하는 거 없을까.. // from Twitter for iPhone [Seoul,
  Korea]

조금 찾아 봤는데 뭐 그런거 없습니다.

> @semtlnori,  18 minutes ago:
  @JEEN_LEE :exe "!git blame -L".line(".").",".line(".")." -- %" 이거
  단축키 바인딩해서 쓰는 방법도 대충 될듯요. // from web [Seoul] in
  reply to JEEN_LEE

@semtlnori: ㄱㅅ

회사에서 저만 [emacs][emacs]씁니다.
`해서`, 찾아봤더니 [mo-git-blame][mo-git-blame]이 있었습니다.


mo-git-blame
------------

### 설치

`.emacs.d/vendor/mo-git-blame/mo-git-blame.el`을 두었습니다.

### 설정

    ;; mo-git-blame
    (add-to-list 'load-path "~/.emacs.d/vendor/mo-git-blame")
    (require 'mo-git-blame)
    (global-set-key [?\C-c ?g ?c] 'mo-git-blame-current)
    (global-set-key [?\C-c ?g ?f] 'mo-git-blame-file)

### 사용법

- 파일을 열고 작업을 합니다.
- 거지같은 코드를 발견합니다.
- `C-c g c`(command git current 의 약자인듯..)
- 커맨드를 마구마구 입력하면서 확인합니다.

      TAB
      l
      s
      Ctrl + n
      Ctrl + n
      Ctrl + n
      TAB
      l
      ...

- 내가 했던거면 말없이 수정하고, 남이 한거면 *Scratch* 에 메세지를
  남겨두거나 바로 지적질 합니다.(아직 어케 할지 몰겟네요)


결론
====

#vim 이 뭐져?


[git]:          http://git-scm.com/
[github]:       https://github.com/
[gitosis]:      http://scie.nti.st/2007/11/14/hosting-git-repositories-the-easy-and-secure-way
[jeen]:         https://twitter.com/#!/jeen_lee
[vim]:          http://www.vim.org/
[emacs]:        http://www.gnu.org/s/emacs/
[silex]:        http://www.facebook.com/silexkr
[mo-git-blame]: https://github.com/voins/mo-git-blame
