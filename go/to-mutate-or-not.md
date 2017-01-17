### References

- https://nathanleclaire.com/blog/2014/08/09/dont-get-bitten-by-pointer-vs-non-pointer-method-receivers-in-golang/

```go
package main

import "fmt"

type Mutatable struct {
    a int
    b int
}

// This will not mutate the fields of the struct
func (m Mutatable) DoNotMutate() {
    m.a = 5
    m.b = 7
}

// This will mutate the fields of the struct
func (m *Mutatable) Mutate() {
    m.a = 5
    m.b = 7
}

func main() {
    m := &Mutatable{0, 0}
    fmt.Println(m)
    m.DoNotMutate()
    fmt.Println(m)
    m.Mutate()
    fmt.Println(m)
}
```
