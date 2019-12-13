---
layout: post
title: "Private Git Repo With Dropbox"
date: 2013-12-23 18:18:35 +0900
comments: true
categories: [dropbox,git,todo,todo.txt]
---

개인적인 private repo 는 bitbucket 을 사용하고 있었는데, 아래 링크를
보고 Dropbox 를 사용하고 있습니다.

[Forget github limits: Free private git repositories with Dropbox](http://mrdanadams.com/2011/github-free-private-git-repositories-dropbox/)

근데 아쉬운거는 github repo 에는 issue board 도 있어서 프로젝트별로
할일을 관리할 수 있어서 참 좋은데 그런 기능이 필요했습니다.

해서 둘러보던 차에 [Todo.txt](http://todotxt.com/) CLI 를 사용해보고
있습니다.

    # todo.cfg
    export TODO_DIR=$HOME/Dropbox/todo/`basename $PWD`
    alias t=todo.sh

요래해서 사용중입니다.

프로젝트 root 에서 밖에 사용못하고 이름 중복에 대응할 순 없지만 아주
만족합니다.

`Dropbox`++, `Todo.txt`++

    $ cd ~/Desktop/t
    $ t add '요래 조래'
    1 요래 조래
    TODO: 1 added.
    ~/Desktop/t $ t add '이러쿵 저러쿵'
    2 이러쿵 저러쿵
    TODO: 2 added.
    ~/Desktop/t $ t add '어쩌고 저쩌고'
    3 어쩌고 저쩌고
    TODO: 3 added.
    ~/Desktop/t $ t ls
    3 어쩌고 저쩌고
    1 요래 조래
    2 이러쿵 저러쿵
    --
    TODO: 3 of 3 tasks shown
    ~/Desktop/t $ t do 3
    3 x 2013-12-23 어쩌고 저쩌고
    TODO: 3 marked as done.
    x 2013-12-23 어쩌고 저쩌고
    TODO: /home/hshong/Dropbox/todo/t/todo.txt archived.
    ~/Desktop/t $ t ls
    1 요래 조래
    2 이러쿵 저러쿵
    --
    TODO: 2 of 2 tasks shown

뭐 대략 이런느낌

Dropbox 에서 동기화 되기 땜시 다른 PC 로 옮겨서도 같은 프로젝트에서
같은 TODO list 를 관리할 수 있다는게 좋은 것 같습니다.
