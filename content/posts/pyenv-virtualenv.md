---
title: pyenv, virtualenv
date: 2020-02-25
---

요새 업무때문에 python 을 만집니다.
사용하는 솔루션의 공식 sdk 가 python 2.7 을 사용해서 그걸 사용해야 합니다.

rbenv 나 plenv 처럼 pyenv 있으니 그걸 쓰면 되겠지 싶었는데..

-   venv
-   pyvenv
-   pyenv
-   virtualenv
-   virtualenvwrapper
-   pipenv
-   etc..

홀리쉿

지나가는 얘기로 섞어써야 한다는 얘기를 들었던 기억이 있습니다.
오늘에야 왜 섞어쓰는지 알았습니다.

system python 을 더럽히기 싫다면 [pyenv](https://github.com/pyenv/pyenv) 를 설치합니다.
여기에 2.x, 3.x 버전을 설치합니다.

각 버전에서 pip 로 [virtualenv](https://virtualenv.pypa.io/en/stable/) 를 설치할 수 있습니다.

    $ pyenv global 3.7.1
    $ pip install virtualenv

    $ pyenv global 2.7.15
    $ pip install virtualenv

여튼 이렇게 각 버전별로 virtualenv 를 설치하고 난 이후에는 이제 프로젝트 디렉토리에 들어가서 사용할 python 버전을 고른뒤에,
virtualenv 를 사용해서 프로젝트별 독립된 환경을 만듭니다.
rbenv, plenv 에는 없는 기능을 virtualenv 가 하는 것 입니다.
사용자가 버전별 python 을 설치하고 pip 를 사용해서 모듈을 설치하면 그때에는 해당 버전의 python 의 global 영역에 모듈이 install 되는데,
virtualenv 를 사용하면 nodejs 의 node\_modules 처럼 프로젝트 단위로 module 이 install 되는 공간을 격리 시킬 수 있습니다.

    $ cd /path/to/project
    $ pyenv local 3.7.1    # 원하는 python version 을 고른다
    $ virtualenv venv
    $ source venv/bin/activate

편리한 점은 명령어로 의존성 모듈을 관리할 수 있습니다.

    $ pip freeze > requirements.txt

perl 로 따지면.. 요런식이랄까..

    $ cpanm -L extlib Foo    # install Foo and all non-core deps into extlib

    # a.pl
    # use lib 'extlib';
    # use Foo;

시스템 영역이 아닌 사용자의 버전별 python 을 설치할때에는 pyenv.
프로젝트 단위로 의존성 모듈을 격리시켜서 사용하고자 할때에는 virtualenv 를 활용.

아래 포스트에 virtualenv 사용법이 잘 나와 있습니다.

<https://dgkim5360.tistory.com/entry/python-virtualenv-on-linux-ubuntu-and-windows>
