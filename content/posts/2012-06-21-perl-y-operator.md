---
layout: post
title: "perl y operator"
date: 2012-06-21 13:59
comments: true
categories: perl
---

## 사건의 발단 ##

어느날 오후 @freenode #perl-kr 에서 일어난 일입니다.

```
<a3r0_> 오랜만에 퀴즈
<a3r0_> 구글 입사문제인데
<a3r0_> 1에서 100000 사이에 8이 몇번 나오는지 맞춰라~
<a3r0_> 답은 perl 원라이너로만 받음
```

저는 손으로 세봤습니다. 100 까지 세는데, 한참 걸렸습니다.

```
8,18,28,38,48,58,68,78,80,81,82,83,84,85,86,87,88,89,98 
20개 (88에서 8이 두개 입니다)
```

이렇게 세어보고 다시 irc 화면을 봤는데..


```perl
<EnGoon> eval my $count =0; for (0..100000) { /(8)/; $count += $+;} print $count;
<hongbot> 799944
<a3r0_> 구글 입사 불합격 하셨습니다.
<ascendo> for i in $(seq 1 100000); do echo $i; done | egrep '8' | wc -l # syntax error
```

떡밥이 흥하질 않자 `a3r0` 님이 정답을 공개해 주셧습니다.

```perl
<a3r0_> $ perl -E 'for(1..100000) { $c+=@{[$_ =~ m/8/g]} }; say $c'
<a3r0_> 50000
<a3r0_> $ perl -E 'for(1..100000) { $c+=(tr/8//) }; say $c'
<a3r0_> 50000
```

이렇게 묻히나 싶었는데, 뒤늦게 보신 `nol2ter_work` 님의 bash oneliner

```bash
<nol2ter_work> 오홍.. bash 로는.. expr `echo {1..100000} | sed -r "s/[^8]//g" | wc -m` - 1 로 되네영..
<a3r0_> python으로 해보셈
```

**여기서 python 의 대반격**

```python
<nol2ter_work> 피똥으로.. str(range(100000)).count('8')
<nol2ter_work> 아.. 이건 편법일라나요 캬캬캬
<a3r0_> str 함수가 문자열 이어붙여주나여?
<nol2ter_work> >>> str(range(100000)).count('8')
<nol2ter_work> 50000
<nol2ter_work> 라고 나오긴 하네용
<a3r0_> 오
<jeen___> 피똥이 갑이네..
<jeen___> 피똥배워야지...
```

분위기가 술렁이기 시작했습니다.
파이썬으로 저렇게 그레이스풀한 방법이 잇엇나 싶기도 했습니다.
거기에 더해 `진사마`는 파이썬으로 갈아탈 준비를 마친 상태.

```perl
<a3r0_> say "@{[0..100000]}"=~y/8//
<a3r0_> str(range(100000)).count('8')
<a3r0_> 2글자 승?
<a3r0_> say 빼면 6글자 승?  [13:40]
<a3r0_> $ perl -E 'say "@{[0..100000]}"=~y/8//'
<a3r0_> 50000
<a3r0_> 줄이기 최종버젼은 "@{[0..1E5]}"=~y/8//
<nol2ter_work> 달달달
```

찰나 `a3r0` 님의 신의 한수
결국 `1E5` 까지쓰며 더 줄일 수 없도록 만들었습니다.

```
<jeen___> 1E5 ㅋㅋㅋㅋ
```

저만 여기서 터진게 아니엇습니다.
이때, 원라이너의 귀재 `pung96`님의 등장

```
*** pung96 (~pung96@46.162.162.115) has joined channel #perl-kr
<a3r0_> 원라이너의 귀재 pung96님 오셨
```

같은 문제를 내어 봤는데 역시나 금방 답이 나오더군요.

```perl
<pung96> 그냥,, 초 단순하게 생각하면,,
<pung96> map{$s+=()=/8/g}1..100000;say$s 이럼 되는데
```

이쯤에서 짧은 답안 공개

```perl
<a3r0_> "@{[0..1E5]}"=~y/8//
<pung96> 헐,
<pung96> 1E5 ...
```

ㅋㅋㅋ `1E5`

계속되는 덕질

```perl
<a3r0_> map{$s+=()=/8/g}1..100000
<a3r0_> map{$s+=@{[/8/g]}}1..100000
<pung96> map{$s+=y/8//} 이럼,, 좀더 짧아지는군요.
<a3r0_> map{$s+=y/8//}1..1E5
<a3r0_> "@{[1..1E5]}"=~y/8//
```

결국 `pung96`님의 타이기록

```perl
<pung96> map$s+=y/8//,1..1E5
```

`pung96`님의 신기록!

저는 이미 예전에 gg

```perl
<jeen___> use Alias ( map => m, print => p );
<pung96> jeen___: ++
```

폭풍같은 시간이었습니다.



## 분석 ##

```perl
"@{[0..1E5]}"=~y/8//; # 50000
```

아.. 아릅답습니다.

먼저 `y` operator 를 `perldoc` 에서 찾아보면,

`The transliteration operator. Same as tr///. See Quote and Quote-like Operators in perlop.`

`tr` operator 와 같다고 나옵니다.
`tr` 의미는 Transliteration(바꿔 씀) 입니다.

```
tr/SEARCHLIST/REPLACEMENTLIST/cdsr
```

`cdsr` modifier 는 `perldoc` 에서 못찾았습니다.
그래서 검색했더니 아래와 같은 내용이 있어서 붙입니다.

```
c - is used to specify that the SEARCHLIST character set is complemented
d - is used to delete found but not replaced characters
s - is used to specify that the sequences of characters that were transliterated to the same character are squashed down to a single
instance of the character

c	Complement the SEARCHLIST.
d	Delete found but unreplaced characters.
s	Squash duplicate replaced characters.
```

예제

```perl
$ARGV[1] =~ tr/A-Z/a-z/;    # canonicalize to lower case 
$cnt = tr/*/*/;             # count the stars in $_ 
$cnt = $sky =~ tr/*/*/;     # count the stars in $sky 
$cnt = tr/0-9//;            # count the digits in $_ 
tr/a-zA-Z//s;               # bookkeeper -> bokeper 
($HOST = $host) =~ tr/a-z/A-Z/; 
tr/a-zA-Z/ /cs;             # change non-alphas to single space 
tr [\200-\377] 
   [\000-\177];             # delete 8th bit
```

`tr|y` operator 는 `SEARCHLIST` 가 매칭된 갯수를 반환하는 걸
알겠습니다.

```perl
"@{[0..1E5]}"=~y/8//; # 50000
```

```perl
"@{[0..1E5]}"
```

- `1E5` 는 10의 5 제곱입니다. 해서 -> 1..100000
- `@{[]}` 는 array 를 reference 하고 다시 dereference 하는 과정입니다.
  이유는 문자열상수 안에서 perl code 를 evaluate 하고 다시 집어넣는
  보간(interpolation) 하기 위한 꼼수 입니다.
- `@{[1..100000]}` 은 `(1, 2, 3, ..., 100000)` 을 가지고 있는 array
  입니다.
- `"(1,2,3,...,100000)"` array 가 `"` 로 묶여 있으면 ` `(SPACE) 를
  구분자로 삽입한 scalar 자료형이 됩니다. 즉, '1 2 3 4 ... 100000'
- "@{[0..1E5]}" 는 '1 2 3 4 ... 100000' 이 됩니다.

```perl
'1 2 3 ... 100000'=~y/8//;
```

- 문자열 `1 2 3 ... 100000` 에 `8` 이 몇번 나오는지 돌려줍니다.


오늘도 즐거운 하루중입니다.
