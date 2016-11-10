```go

func (c *Config) SetAddr(address string, tlsEnabled bool) *Config {
	scheme := "http"
	if tlsEnabled {
		scheme = "https"
	}
	config := &Config{
		Address:    fmt.Sprintf("%s://%s", scheme, address),		
	}

	return config
}
```
