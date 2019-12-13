---
layout: post
title: "p5-hubot onto heroku"
date: 2013-06-10 17:31
comments: true
categories: [hubot,heroku]
---

오리지날 [hubot](https://github.com/github/hubot) 에는
[heroku](https://www.heroku.com/) 어쩌고 저쩌고 하는 기능이 있습니다.

이제 [p5-hubot](https://github.com/aanoaa/p5-hubot) 에서도 됩니다.

### OVERVIEW ###

```bash
$ cpanm Hubot                      # requires Hubot >= 0.2.0
$ hubot --create /path/to/hubot    # 배치가능한 형태의 패키지 저장소를 생성합니다
$ cd /path/to/hubot
$ git init ; git add . ; git commit -m "init commit"

$ heroku create --stack cedar --buildpack https://github.com/aanoaa/heroku-buildpack-perl.git

$ heroku config:add HEROKU_URL=http://your-herokuapp.herokuapp.com
$ heroku config:add HUBOT_IRC_ROOMS='#channel'
$ heroku config:add HUBOT_IRC_SERVER='irc.freenode.net'

$ git push heroku master
```

가장 기본은 이렇습니다만, 확장 스크립트를 추가하시려면
`hubot-scripts.json` 파일에 적어주시고 그에 따른 모듈 의존성은
`cpanfile` 에 적어서 해결해야 합니다.

### CREATE A DEPLOYABLE HUBOT ###

[CPAN](http://search.cpan.org/) 을 이용한다면,

    $ cpanm Hubot
    $ hubot -c ~/Desktop/hubot

명령어로 deploy 저장소를 만들 수 있습니다.

    $ cd ~/Desktop/hubot/
    ~/Desktop/hubot $ find
    .
    ./.gitignore
    ./cpanfile
    ./hubot-scripts.json
    ./bin
    ./bin/hubot
    ./README.md
    ./lib
    ./lib/Hubot
    ./lib/Hubot/EventEmitter.pm
    ./lib/Hubot/Scripts
    ./lib/Hubot/Scripts/help.pm
    ./lib/Hubot/Scripts/ascii.pm
    ./lib/Hubot/Scripts/shorten.pm
    ./lib/Hubot/Scripts/tweet.pm
    ./lib/Hubot/Scripts/roles.pm
    ./lib/Hubot/Response.pm
    ./lib/Hubot/Robot.pm
    ./lib/Hubot/User.pm
    ./lib/Hubot/Adapter.pm
    ./lib/Hubot/TextListener.pm
    ./lib/Hubot/Listener.pm
    ./lib/Hubot/Adapter
    ./lib/Hubot/Adapter/Irc.pm
    ./lib/Hubot/Adapter/Shell.pm
    ./lib/Hubot/Adapter/Campfire.pm
    ./lib/Hubot/Message.pm
    ./lib/Hubot/Brain.pm
    ./lib/Hubot/Creator.pm
    ./lib/Hubot.pm
    ./Procfile

이 디렉토리를 git 저장소로 만들고요, `heroku` tool 을 사용해서 heroku
에서 돌아갈 서비스 저장소로 만듭니다.

`Procfile` 은 heroku 에서 사용하는 process 타입별로 실행 명령어를
담고 있는 어쩌고 저쩌고;;
[Process Types and the Procfile](https://devcenter.heroku.com/articles/procfile)

`cpanfile` 은 모듈의존성을 해결하기 위해 꼭 필요합니다. 그렇지 않으면
아래와 비슷한 오류가 발생되면서 프로그램이 실행되지 않습니다.

    Can't locate xxx.pm in @INC (@INC contains: ~~~) at line 1.

나머지는 p5-hubot core 모듈입니다.(서비스엔 필요없는것도 잇지만..)

### CREATE A HEROKU INSTANCE? ###

    $ heroku create --stack cedar --buildpack https://github.com/aanoaa/heroku-buildpack-perl.git

`heroku` 라는 `remote` 를 등록하고 `BUILDPACK_URL` 설정변수를 추가하는
것 정도가 아닐까 추측 합니다.

BUILDPACK_URL 은 git 저장소인데,

- `bin/detect`
- `bin/compile`
- `bin/release`

를 가지고 있습니다. 자세한 내용은
[Creating a Buildpack](https://devcenter.heroku.com/articles/buildpacks#creating-a-buildpack)
을 참고하시고, 이부분의 매직은 `bin/compile` 이 수행될때 `cpanm` 을
이용해서 `./local` 에 의존모듈을 설치합니다. 그리고 실행할때에
`-Ilocal/perl5/lib` `PERL5LIB` 에 추가합니다. 풀어쓰면 아래와
같습니다.

    $ cpanm --quiet --notest -l $BUILD_DIR/local --installdeps .

[@miyagawa](https://twitter.com/miyagawa)++ 입니다.

그리고 heroku 에 push 하면 알아서 돌아갑니다(별일 없으면..)

### SEE ALSO ###

- [Deploying Hubot onto Heroku](https://github.com/github/hubot/wiki/Deploying-Hubot-onto-Heroku)
- [heroku-buildpack-perl for Perl/hubot](https://github.com/aanoaa/heroku-buildpack-perl)
- [Process Types and the Procfile](https://devcenter.heroku.com/articles/procfile)
