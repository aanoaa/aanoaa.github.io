---
layout: post
title: "backbon.js jsonp collection sync"
date: 2012-02-27 05:07
comments: true
categories: [backbone.js, jsonp]
---

외부 도메인을 사용하고자 할때 `jsonp` 를 이용해야 하는데,
이때, `Backbone.sync` 메소드를 override 해서 쓸 수 있습니다.

jquery `$.ajax` 메소드의 옵션을 보면 `dataType`을 `jsonp`로 설정하면
`url` 꽁지에 자동으로 `?callback=jqueryblablablabla_timestamp`
query_parameter 를 추가해줍니다.

```coffeescript
class MyCollection extends Backbone.Collection
  model: Log
  sync: (method, model, options) ->
    options.dataType = 'jsonp'
    Backbone.sync(method, model, options)

collection = new MyCollection
collection.url = 'http://otherdomain.com'
collection.fetch # 요래하면 GET/ http://otherdomain.com?callback=jqueryblablablabla_timestamp
  success: (collection, response) ->
    # do something
  error: (collection, response) ->
    # error handling
```

서버로부터 응답은 요래 와야..

```
jqueryblablablabla_timestamp(<stringifiedJSON>)
```
