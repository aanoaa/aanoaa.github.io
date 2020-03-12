---
layout: post
title: "Git 데이터 복구"
date: 2017-05-12 19:12:13 +0900
comments: true
categories: git reflog
---

오늘 실수로 고객의 한달간의 데이터를 잃어버렸습니다.

고객의 데이터를 git 으로 관리하는 프로젝트가 있는데,
지난 한달간의 unstaging 되어 있는 고객의 데이터를

    $ git add .
    $ git commit -m 'sync'
    $ git push

이런식으로 저장했습니다.
이후에 우리 말썽꾸러기가 이상하게 작업해놓은 이력을 발견하고 업무 쓰로잉 욕구가 생겨서,

    # 아 짱나넴 걍 원래대로 해놓고 알아서 하라고 해야겠다.
    $ git reset --hard HEAD^
    $ git push
    $ git push -f origin <current-branch>

를 했습니다.
이때에 방금 커밋한 이력을 날렸습니다.

순간, **아아아아 받은 돈 토해내고 마포대교 가야겠다** 라는 생각이 들었습니다.

하지만 곧 pointer 만 잃고 objects 는 그대로 있을 것 같아서 검색했습니다.

http://stackoverflow.com/questions/5788037/recover-from-git-reset-hard

    $ git reflog show
    d5d92ec HEAD@{0}: reset: moving to HEAD^
    3adf160 HEAD@{1}: commit: sync
    ...

이런 이력이 있어서,

    $ git show 3adf160

으로 커밋내용이랑 시간을 확인하고,

    git reset --hard 3adf160

해서 복구 했습니다.
뒤지는 줄 알았습니다.

웃겼던 것은 저 위에 스택오버플로 코멘트였습니다.

> This trick saved my life a couple of times.

> saved me just now! :D

> You saved me. Thanks a ton.

> saved me too - perfect

## See also

- [Git의 내부 - 운영 및 데이터 복구](https://git-scm.com/book/ko/v1/Git%EC%9D%98-%EB%82%B4%EB%B6%80-%EC%9A%B4%EC%98%81-%EB%B0%8F-%EB%8D%B0%EC%9D%B4%ED%84%B0-%EB%B3%B5%EA%B5%AC)
