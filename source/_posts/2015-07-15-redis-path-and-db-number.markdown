---
layout: post
title: "redis path and db number"
date: 2015-07-15 18:10:25 +0900
comments: true
categories: redis
---

`redis://localhost:6397/1` 흔한 redis-url 입니다.
`/1` 의 역할은 뭘까요? 뭐냐면 Database number 입니다.

    $ redis-cli --help
    redis-cli 3.0.2
    
    Usage: redis-cli [OPTIONS] [cmd [arg [arg ...]]]
      ...
      -n <db>            Database number.
      ...

`/1` 없이 연결하면 기본으로 `0` 번 database 를 사용합니다.

디버그 하면서 값을 확인하려고 제아무리 `keys *` 을 찍어도 암것도
안나와서 넘 답답했는데..

    127.0.0.1:6379> keys *
    (empty list or set)

`select` 명령으로 선택해주거나,

    127.0.0.1:6379> select 1
    OK
    127.0.0.1:6379[1]> keys *
    1) "user:aanoaa"
    2) "users"

`redis-cli` 에서 `-n` 옵션을 사용하거나

    $ redis-cli -n 1
    127.0.0.1:6379[1]> 

redis-url path 로 졸 검색했는데 암것도 안나옴.
