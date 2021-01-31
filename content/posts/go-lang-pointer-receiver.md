---
title: golang pointer receiver
date: 2021-01-30
categories: go pointer receiver
---

golang 을 만지다가 이게 뭔일이여 싶어서..
pointer receiver method 안에서 address 가 아닌 value 로 사용해도 동작한다?

https://tour.golang.org/methods/4 여기에서 예제를 보니까 pointer receiver 메소드 안에서,
value 를 사용하길래 어? 이상하다 했는데 테스트해보니까 그러함

``` go
package main

import (
    "fmt"
    "math"
)

type Vertex struct {
    X, Y float64
}

func (v Vertex) Abs() float64 {
    return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func (v *Vertex) Scale(f float64) {
    v.X = v.X * f
    v.Y = v.Y * f
}

func main() {
    v := Vertex{3, 4}
    v.Scale(10)
    fmt.Println(v.Abs())
}
```

모르겠는 부분은 여기

``` go
func (v *Vertex) Scale(f float64) {
    v.X = v.X * f
    v.Y = v.Y * f

    // 왜 이게 아니고..
    // (*v).X = (*v).X * f
    // (*v).Y = (*v).Y * f
}
```

아래는 테스트 해본거

``` go
package test

type multiset struct {
    values []int
}

// pointer receiver (1)
func (m *multiset) append(val int) {
    for _, v := range (*m).values {
        if v == val {
            return
        }
    }

    (*m).values = append((*m).values, val)
}

// pointer receiver (2)    <- 왜 됨?
func (m *multiset) append(val int) {
    for _, v := range m.values {
        if v == val {
            return
        }
    }

    m.values = append(m.values, val)
}

// value receiver (3)
func (m multiset) append(val int) {
    for _, v := range m.values {
        if v == val {
            return
        }
    }

    m.values = append(m.values, val)
}
```

``` go
package test

import "fmt"

func Example_pr() {
    var ms multiset
    ms = multiset{[]int{1, 2, 3}}
    fmt.Println(ms) // 123

    ms.append(4)
    fmt.Println(ms) // 1234

    ms.append(4)
    fmt.Println(ms) // 1234

    // Output:
    // {[1 2 3]}
    // {[1 2 3 4]}
    // {[1 2 3 4]}
}
```

각 케이스 별로 위 테스트 돌리면..

- (1) 성공
- (2) 실패
- (3) 실패

로 예상했는데

- (1) 성공
- (2) 성공 <- ???
- (3) 실패

가 됨

왜죠?

---

이것은 포인트 리시버와 무관하고, struct 에 대한 내용이다.
그리고 그것을 사용하는 표현식.

    some := mystruct{}

some 은 struct 의 값인데, 배열/슬라이스와 마찬가지로 다른 데이터들의 집합을 가리키는 값을 가진다.
(struct 가 기본자료형의 alias 가 아니라면)

struct 와 struct pointer 둘다 dot 표기법 사용이 가능하다.
뭘 써야 되는지는 아직 모르겠지만..

https://tour.golang.org/methods/4 예제에서는 간결한 표현을 위해서인지 struct 의 value 대신에 struct 의 pointer 를 사용.

이거..

``` go
func (v *Vertex) Scale(f float64) {
    v.X = v.X * f
    v.Y = v.Y * f
}
```

어느 구루께서 명쾌하게 이거써~ 하면 그거 쓸텐데 일단 이렇게 써야..

이건 caller 가 struct data type 이 아니고 내부 자료형일때에..
헛갈리지 않는다.

``` go
type myint int

func (m *myint) increase() myint {
    fmt.Println("  m", m)  //  m 0xc000014168
    fmt.Println(" *m", *m) // *m 1
    (*m)++
    return (*m)
}

func Example_myint() {
    var mi myint
    mi = myint(1)
    fmt.Println(mi)  // 1
    fmt.Println(&mi) // 0xc000014168

    mi.increase()
    fmt.Println(mi) // 2

    // Output:
    // 1
    // 0xc000014168
    //   m 0xc000014168
    //  *m 1
    // 2
}
```

이건 caller 가 struct data type

`fmt` 에서 struct 의 address 는 실제 주소대신에 `&{x}` 이런식으로 표현해주어서 `%p` 포매터로 확인

``` go
type mystruct struct {
    foo int
}

func (m *mystruct) increase() int {
    fmt.Printf("  m %d %p\n", m, m) //  m &{1} 0xc0000a0050
    fmt.Println(" *m", *m)          // *m {1}

    m.foo++
    return m.foo
}

func Example_mystruct() {
    var ms mystruct
    ms = mystruct{1}
    fmt.Println(ms)  // {1}
    fmt.Println(&ms) // &{1}

    ms.increase()
    fmt.Println(ms) // 2

    // Output:
    // {1}
    // &{1}
    //   m &{1} 0xc0000a0050
    //  *m {1}
    // {2}
}
```

struct 의 pointer 와 struct 의 첫번째 멤버의 pointer 가 같음
값은 같지만 자료형에 따라 사용방법이 달라짐

``` go
func Example_mystruct2() {
    var ms mystruct
    ms = mystruct{1}

    var msp *mystruct
    var foop *int

    msp = &ms
    foop = &(ms.foo)

    fmt.Printf("%d %p\n", msp, msp)                                // &{1} 0xc0000141a8
    fmt.Println(foop)                                              // 0xc0000141a8
    fmt.Println(fmt.Sprintf("%p", msp) == fmt.Sprintf("%p", foop)) // true

    // 일반적인 사용법
    ms.foo++

    // compile error! invalid operation
    // *msp 는 *foo 와 같지만 내부에 metadata 가 있어서 구별하는 듯
    (*msp)++    // 요거 에러남

    // msp 는 ms 의 pointer 이지만 dot notation 으로 사용가능
    // 왜 가능? 그렇게 만들어놨으니까 가능. 이건 내부 구현을 몰라서 되는갑다 하능..
    // pointer reciever method 에서 이렇게 사용
    msp.foo++

    (*msp).foo++ // 이제 이게 요상해보이네..
    (*foop)++    // struct 내부의 값을 직접변경

    fmt.Println(ms)

    // Output:
    // &{1} 0xc00010c050
    // 0xc00010c050
    // true
    // {5}
}
```

struct 의 pointer 에서 struct 처럼 dot notation 을 사용할 수 있다.
정도가 되겠다.

다른 예외케이스가 있는지 모르겠지만..

컴파일러가 알아서 해주겠지.. 믿습니다.
