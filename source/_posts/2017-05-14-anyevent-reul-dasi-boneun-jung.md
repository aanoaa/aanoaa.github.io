---
layout: post
title: "AnyEvent 를 다시 보는 중"
date: 2017-05-14 04:57:19 +0900
comments: true
categories: [AnyEvent]
---

꽤 오래전부터 `#perl-kr@freenode` 채널에 hongbot(제가 운영하는 irc 봇)이 들어오지
않았습니다.

heroku 에서 Deprecated 된 Cedar-10 Stack? 을 사용했던게 문제였던 것 같습니다.
여차저차 문서보고 Cedar-16? 으로 마이그레이숀 했습니다.

아무튼 맨날 열린옷장 웹서비스만 하다가 오래간만에 irc bot 을 디버깅 하니까 재미가
있었습니다.

헌데, 모듈 의존성이 너무 많고 테스트가 어려운게 거슬려서

- 의존성도 확 줄이고
- 테스트코드를 추가(강화?)

해서 lightweight hongbot 으로 운영하고 싶어졌습니다.
요즘 들여다보면서 고치고 있습니다.

머릿속에서 AnyEvent 가 싹 잊혀져서, 문서를 다시 처음부터 보고 있습니다.(조따 괴로운
일입니다.)

-------------------------------

아무튼 오늘은 4시간 동안 삽질했습니다.

뭐냐면, heroku 에서 idle 상태에 빠지지 않기 위해서 스스로 ping/pong 하는 기능의 테스트
코드를 작성하던 중에 제대로 동작하지 않아서 괴로웠습니다.

AnyEvent::HTTPD 에 등록된 콜백을 AnyEvent main loop 안에서 Blocking 연결로 요청하면
동작하지 아니합니다.

아래는 1초 뒤에 `GET http://localhost:5000/ping` 을 요청하는 간단한 코드입니다.

``` perl
use utf8;
use strict;
use warnings;

use AnyEvent;
use AnyEvent::HTTPD;
use HTTP::Tiny;

my $httpd = AnyEvent::HTTPD->new( host => '0.0.0.0', port => 5000 );
$httpd->reg_cb(
    '/ping' => sub {
        my ( $httpd, $req ) = @_;
        $req->respond( { content => [ 'text/plain', "pong" ] } );
    }
);

my $wait = AnyEvent->condvar;
my $t    = AnyEvent->timer(
    after => 1,   # 1sec
    cb    => sub {
        my $http = HTTP::Tiny->new( timeout => 1 );
        my $url = 'http://localhost:5000/ping';
        print "GET $url\n";

        my $res = $http->get($url); # <- 여기서 멈춤
        print "$res->{status}: $res->{reason}\n";

        ## my $res = $http->get('https://www.google.com'); # 이거는 됨
        ##
        ## or
        ##
        ## AnyEvent::HTTP::ScopedClient->new($url)->get(
        ##     sub {
        ##         my ( $body, $hdr ) = @_;
        ##         print "$body\n"; # pong
        ##         $wait->send;
        ##     }
        ## );

        $wait->send;
    }
);

$wait->recv;
print "done\n";
```

1. 비동기 요청을 하도록 바꾸거나,
2. AnyEvent::HTTPD 의 문제를 해결해야겠지요.

뭐가 문제인지 들여다보고 싶은 맘은 없습니다.
쉬운 1번 으로 해결할 것 같습니다.

## See also ##

- [https://metacpan.org/pod/AnyEvent](AnyEvent)
