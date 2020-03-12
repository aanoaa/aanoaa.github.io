---
layout: post
title: "picasa upload script"
date: 2012-01-03 23:18
comments: true
categories: perl picasa
---

{% gist 1444684 %}

`$HOME/.google.conf` 에다가 자신의 계정 정보를 슥슥하고요.

**이중보안 설정한 계정은 업로드 안됩니다.**

```bash
$ picasa-upload.pl <PHOTO.png>
```

    https://lh4.googleusercontent.com/-mmv40wXKaM4/TwMP0BKUVhI/AAAAAAAAABA/AM3KBgCuf8Q/2011082200618_10.jpg

제가 샘플로 올린 이미지 퍼블릭 링크입니다.

`어쩌고저쩌고/s[SIZE]/파일명.jpg` 으로 바꾸면 파일크기가 바뀌어서
보여집니다. 굳 :+1:

    https://lh4.googleusercontent.com/-mmv40wXKaM4/TwMP0BKUVhI/AAAAAAAAABA/AM3KBgCuf8Q/s72/2011082200618_10.jpg
    https://lh4.googleusercontent.com/-mmv40wXKaM4/TwMP0BKUVhI/AAAAAAAAABA/AM3KBgCuf8Q/s144/2011082200618_10.jpg
    https://lh4.googleusercontent.com/-mmv40wXKaM4/TwMP0BKUVhI/AAAAAAAAABA/AM3KBgCuf8Q/s200/2011082200618_10.jpg
    https://lh4.googleusercontent.com/-mmv40wXKaM4/TwMP0BKUVhI/AAAAAAAAABA/AM3KBgCuf8Q/s320/2011082200618_10.jpg
    https://lh4.googleusercontent.com/-mmv40wXKaM4/TwMP0BKUVhI/AAAAAAAAABA/AM3KBgCuf8Q/s400/2011082200618_10.jpg
    https://lh4.googleusercontent.com/-mmv40wXKaM4/TwMP0BKUVhI/AAAAAAAAABA/AM3KBgCuf8Q/s512/2011082200618_10.jpg

- s72
- s144
- s200
- s320
- s400
- s512
- s576
- s640
- s720
- s800
- s912
- s1024
- s1152
- s1280
- s1440
- s1600

뭐 사이즈는 요로코롬 슥슥해서 쓰면 되겠죠.

![원본](https://lh4.googleusercontent.com/-mmv40wXKaM4/TwMP0BKUVhI/AAAAAAAAABA/AM3KBgCuf8Q/2011082200618_10.jpg)

원본이고요

![s72](https://lh4.googleusercontent.com/-mmv40wXKaM4/TwMP0BKUVhI/AAAAAAAAABA/AM3KBgCuf8Q/s72/2011082200618_10.jpg)
![s144](https://lh4.googleusercontent.com/-mmv40wXKaM4/TwMP0BKUVhI/AAAAAAAAABA/AM3KBgCuf8Q/s144/2011082200618_10.jpg)
![s200](https://lh4.googleusercontent.com/-mmv40wXKaM4/TwMP0BKUVhI/AAAAAAAAABA/AM3KBgCuf8Q/s200/2011082200618_10.jpg)
![s320](https://lh4.googleusercontent.com/-mmv40wXKaM4/TwMP0BKUVhI/AAAAAAAAABA/AM3KBgCuf8Q/s320/2011082200618_10.jpg)
![s400](https://lh4.googleusercontent.com/-mmv40wXKaM4/TwMP0BKUVhI/AAAAAAAAABA/AM3KBgCuf8Q/s400/2011082200618_10.jpg)
![s512](https://lh4.googleusercontent.com/-mmv40wXKaM4/TwMP0BKUVhI/AAAAAAAAABA/AM3KBgCuf8Q/s512/2011082200618_10.jpg)

72, 144, 200, 320, 400, 512 입니다.

파일 한개를 간편하게 올려서 요로코롬 크기를 조절해가면서 사용할 수
있습니다.
게다가 **무료**! google++

편리한 스크립트를 맨들어주시고 유용한 팁까지 알려주신
[@pung96](https://twitter.com/pung96)++ 님 감사합니다.
