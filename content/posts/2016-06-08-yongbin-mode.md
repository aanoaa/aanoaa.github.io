---
layout: post
title: "yongbin-mode"
date: 2016-06-08 00:22:00 +0900
comments: true
categories: [elisp]
---

## 외래어의 한글표기 ##

보통 외래서를 한글로 적을때, `e` 는 `ㅔ` 로, `a` 또는 `ae` 는 `ㅐ` 로
적습니다.

그런데 [@yongbin](https://twitter.com/yongbin)님은 바꿔씁니다.
`helper` 를 한글로 적으면 보통은 `헬퍼`이지만 @yongbin 님은 `핼퍼`
입니다. 오래된 이야기입니다.

디랙토리, 핼퍼, 렌덤 등등..

`생일`도 `셍일`로 적고, `요새`도 `요세` 적는걸 보면, 외래어만 그런 것 같진 않습니다.


## *-mode ##

제 작은소원은 조국의 통일도 편경장의 팔모가지도 아닙니다.
제가 필요한 emacs의 mode 를 만드는 것입니다.
elisp 관련 책도 읽다가 포기한 적도 많습니다.

그런데 몇일 전
[github-comment](https://github.com/opencloset/monitor/issues/94#issuecomment-223536395) 에서

> 핼퍼에 구현된 코드중에

라는 코멘트가 결정적인 계기가 되어서 `yongbin-mode` 를 만들어보자 라고
결심하게 되었습니다.

### yongbin-mode ###

아이디어는 이랬습니다.

emacs 내에서 `ㅔ` 를 누르면 `ㅐ` 가 입력되고 `ㅐ`를 누르면 `ㅔ`가
입력되도록 하자.

`pre-command-hook` 에서 스슥 하면 될 것 같았는데, multibyte 입력은
이벤트가 발생되지 않았습니다.

(아직도 왜 그런지 모르겠고, 어디서 무얼 찾아야 하는지도
모르겠습니다. elisp 도사님 찾아가서 제자 하고 싶음)

여튼 꼼수를 써서 익숙한 perl 로 프로그램을 만들고 emacs 에서는 호출만
하자 싶었습니다.

해서 `yb` 라는 perl 스크립트를 만들어서 `ㅐ`와 `ㅔ` 를 바꾸도록
하였습니다.

    $ echo "안녕하세요" | yb
    안녕하새요

그리고 `yongbin-mode` 라는 minor mode 를 만들어서 채팅할때 알아서
변환되게 하고 싶었습니다.(저는 irc 채팅을 emacs 에서 합니다)

아무것도 몰라서(지금도 모름) 구글링하고 문서읽고 다른 코드들을 참고하면서

- yongbin-mode : 마이너 모드(yongbin-mode)를 켜고끈다
- yongbinize-buffer : 현재버퍼의 텍스트를 용빈화시킨다
- yongbinize-region(start, end) : 영역의 텍스트를 용빈화시킨다
- yongbinize-line : 현재 라인을 용빈화 시킨다
- erc-yongbinize : irc 의 입력을 용빈화 시킨다

기능을 맨들었습니다.

그걸 여차저차 해서 irc 에서

    <aanoaa> '-']/ 안녕하세요<RET>

하면,

    <aanoaa> '-']/ 안녕하새요

로 됩니다.

https://github.com/aanoaa/yongbin-mode

## 느낀점 ##

개발환경이 넘 구립니다. 코드 변경하고 `M-x eval-buffer` 해서 다시 컴파일
하고 어디어디 찾아가서 `C-x C-e` 해서 한줄만 실행하고 등등..

뭘 모르는데 뭘 어떻게 해야할지가 젤 어렵고,
또 어떻게 디버깅 해야 할지가 어렵습니다.
괄호가 넘 많고 `if else` 문에 대한 문서를 봐도 어케 쓰는지 모르겠능;;
한글처리에 관한 문서는??

등등 어려움이 넘 많습니다.

하지만 context 가 오묘하달까?

    (with-current-buffer yongbinize-buffer-name
      (goto-char (point-min))
      (setq yongbinized-text (...)))
      
이런거?

`point-min` 이라는걸 buffer 마다 가지고 있는 모양인데, 그게 context 에 따라
선택된다는게 참 신통방통

perl 뉴비 시절에 perl-kr 커뮤니티에서 도움을 많이 받았던게 넘 좋았는데,
elisp 는 한국사용자 커뮤니티가 없는 것 같습니다(?)
한글문서도 오래되었거나 거의 없는 것 같고..
closure 는 있는데 이건 elisp 가 아니고..

elisp 에 관심있는 emacs 사용자들에게서 뭔가 도움을 구해보고 싶습니다.
관련된 커뮤니티를 알고 계시면 알려주세요.

## 레알 결론 ##

`ㅔ` 와 `ㅐ` 가 바뀐게 보이면 너무 꼴보기 싫었습니다.
이제 몇주동안 저도 똑같이 바꿔쓰면서 고통을 주도록 하겠습니다.
