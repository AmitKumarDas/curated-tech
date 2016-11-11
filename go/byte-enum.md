```go
type OrangeType byte

const (
	orangeOrange     OrangeType = 0x01
	blackOrange                 = 0x02
)

// a single byte
buf := make([]byte, 1)

// logic to fill the byte

if OrangeType(buf[0]) != blackOrange {		
		return
}
```
