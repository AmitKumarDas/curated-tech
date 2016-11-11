```go
type OrangeType byte

const (
	orangeOrange     OrangeType = 0x01
	blackOrange                 = 0x02
)

// a single byte
buf := make([]byte, 1)

// some logic to fill the byte

// Switch on the byte
switch OrangeType(buf[0]) {
case orangeOrange:
  // logic

case blackOrange:
  // logic

default:
  // logic
}
```
