```go

// Orange is used to access the orange-specific operations
type Orange struct {
	farmer *Farmer
}

// Orange returns a handle on the orange operations.
// NOTE: Farmer knows how to do the operations w.r.t orange
// e.g. o.List() will make use of o.farmer.exec("..") etc
func (f *Farmer) Orange() *Orange {
	return &Orange{farmer: f}
}
```
