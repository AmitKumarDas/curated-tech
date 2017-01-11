### References

- https://play.golang.org/p/gwsvoiWWPr

```go
package main

import (
	"fmt"
)

// Showing how you correct https://play.golang.org/p/ZgsqYqcrid

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
