---
title: yasnippet 안의 elisp
date: 2021-02-01
categories: lisp elisp emacs snippet go
---

typescript 의 시작을 emacs 로 했었는데, 이때 lsp 을 사용해서 넘 경쾌하게 썼었다.
근데 IDE 써봐야지 하면서 억지로 vscode 썼는데 이것도 쓰다보니 훌륭.

그러다 보니 go 작업할때에 설정없이 간편하게 시작할 수 있어서 vscode 를 썼었다.
버튼 누르면 알아서 다되더라고.

그러다가 go 를 script 처럼 쓰려고 다시 emacs 에 설정해서 쓰는데 경쾌함이 다르다.
[go-mode.el](https://github.com/dominikh/go-mode.el) 이 외부 툴 연동도 훌륭하고,
vscode 에서의 답답함이 없다.

맥부기라서 그랬나..

답답함을 느꼈던 건 vscode on macOS
지금은 emacs on linux

아무튼 다시 설정 지옥에 빠져서 "그래 반복되는 타이핑을 줄이자" snippet 을 작성.
남의 snippet 중에 뭐 쓸거 없나 구경하던차에..

``` emacs-lisp
package ${1:`(car (last (split-string (file-name-directory buffer-file-name) "/") 2))`}
```

요런게 보였음.
alias 로 걸어둔 `pkg` 또는 `package` 를 입력하고 tab 누르면

```
package <현재디렉토리명>
```

이 뙇

```
${1:`()`}
```

back quote 로 감싼 괄호 안에서 elisp 표현식을 써서 default 값을 지정할 수 있음

``` go
Example_foo() {

    // Output:
    // .
}
```

이거 매번 작성하기 조띠 귀찮아서 perl 로 template 만들어서 사용했는데 그럴 필요가 없었던 거임.
그래서 저 lisp 좀만 변경하면 어케 될 거 같아서 스슥.

``` emacs-lisp
(car (last (split-string (file-name-directory buffer-file-name) "/") 2))
```

ㅋㅋㅋ lisp 는 어케 매번 새로움

``` emacs-lisp
(buffer-file-name) ;; 현재 버퍼의 파일명 foo_test.go 여기서 내가 필요한게 foo
(file-name-directory FILE) ;; 전체 경로 주는 건데 이건 필요없음
(split-string STRING &optional SEPARATOR...) ;; 구분자로 잘라주겠거니..
```

어케어케 해서 `foo_test.go` 파일에서 `foo` 가져와야 하는데 `foo_test` 까진 됐음

``` emacs-lisp
(car (last (split-string (car (last (split-string (buffer-file-name) "\\.") 2)) "/")))
```

영역잡고 괄호열면 양 끝에 괄호 열어주고 괄호 닫으면 pair 괄호 닫아주고 신통방통

이번에 하나 얻은건 표현식 값을 확인하는 방법
지금까지는 젤 마지막 괄호에 대해서만 동작하는 줄 알았는데, 중간 괄호에 대고 `C-x C-e` eval-last-sexp 해주면 이게 동작해서 잘라서 확인해볼 수 있음

``` emacs-lisp
(car (last (split-string (car (last (split-string (buffer-file-name) "\\.") 2)) "/"))) ;; a  <- current-buffer 가 a.el 이라서


(last (split-string (buffer-file-name) "\\.") 2) ;; ("..." "el")
```

뭐 이런식으로 넓혔다가 좁혔다가 하고 eldoc 보면서 어케어케

암튼 go 개발환경으로써 emacs 조띠 좋음
