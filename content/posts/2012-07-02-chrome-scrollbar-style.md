---
layout: post
title: "chrome scrollbar style"
date: 2012-07-02 12:50
comments: true
categories: chrome
---

`$HOME/.config/google-chrome/Default/User StyleSheets/Custom.css` 에다가
아래 내용을 추가하면 scroll-bar 가 얄상하니 이쁩니다.

근데 불편해서 안쓸라고.. 나중에 검색하기 힘들까봐 로그 남겨 둡니다.

```css $HOME/.config/google-chrome/Default/User StyleSheets/Custom.css
::-webkit-scrollbar-track-piece{
    background-color:#fff;
    -webkit-border-radius:0;
}
::-webkit-scrollbar{
    width:12px;
    height:8px;
}
::-webkit-scrollbar-thumb{
    height:50px;
    background-color:#999;
    -webkit-border-radius:4px;
    outline:2px solid #fff;
    outline-offset:-2px;
    border: 2px solid #fff;
}
::-webkit-scrollbar-thumb:hover{
    height:50px;
    background-color:#9f9f9f;
    -webkit-border-radius:4px;
}
```
