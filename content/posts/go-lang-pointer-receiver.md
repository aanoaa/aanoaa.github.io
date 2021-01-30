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
