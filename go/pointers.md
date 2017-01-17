### References

- https://gobyexample.com/pointers

```go
// Accept an address
func zeroptr(iptr *int) {
    // set the value against the address
    *iptr = 0
}

func main() {
    i := 1
    zeroptr(&i)
    // Print the value
    fmt.Println("zeroptr:", i)
    // i.e. 0
    // Print the address   
    fmt.Println("pointer:", &i)
    // e.g. 0x42131100
}
```

```go

func ABC(){
		if err := parseAdvertise(&result.AdvertiseAddrs, o); err != nil {
			return multierror.Prefix(err, "advertise ->")
		}
}

// Accept a double pointer, to set an address
func parseAdvertise(result **AdvertiseAddrs, list *ast.ObjectList) error {
  var advertise AdvertiseAddrs
	if err := mapstructure.WeakDecode(m, &advertise); err != nil {
		return err
	}
  
  // You are setting an address not a value
	*result = &advertise
	return nil
}
```
