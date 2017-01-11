
### References

- https://github.com/hashicorp/nomad/blob/master/nomad/structs/config/consul.go

```go
// Copy returns a copy of this Consul config.
func (c *ConsulConfig) Copy() *ConsulConfig {
	if c == nil {
		return nil
	}

  // A new address
	nc := new(ConsulConfig)
  // Setting the values from original
	*nc = *c
	return nc
}
```
