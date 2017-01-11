### References

- https://play.golang.org/p/gwsvoiWWPr
- https://play.golang.org/p/zP7hR_jemD

```go
package main

import (
	"fmt"
)

// Showing how you correct below:
// Value wise
// https://play.golang.org/p/ZgsqYqcrid
// Address wise
// https://play.golang.org/p/6z4cKBC8FO

func main() {
	m := map[string]interface{}{
		"a": nil,
		"b": nil,
		"c": nil,
	}

	var funcs []func()
	for k, _ := range m {
		// Copy the value to a new variable (and therefore
		// new memory address) $$ IMPORTANT $$
		x := k
		
		funcs = append(funcs, func() {
			fmt.Println(x)
		})
	}

	for _, f := range funcs {
		f()
	}
}
```

- Complex Way (reminds me of JavaScript)

```go
package main

import (
	"fmt"
)

// Showing a common gotcha for Go programmers. This is well
// documented and experienced Go programmers should know
// about this! 

func main() {
	m := map[string]interface{}{
		"a": nil,
		"b": nil,
		"c": nil,
	}

	var funcs []func()
	
	// Here is the gotcha: the key/value variables in a for loop
	// are reused (values just change, not their address). If
	// you close over them, you'll get whatever value it happens
	// to hold at the time the closure is called.
	for k, _ := range m {
		funcs = append(funcs, func(x string) func() {		
                        return func() { fmt.Println(x) }
		}(k))
	}

	for _, f := range funcs {
		f()
	}
}
```
