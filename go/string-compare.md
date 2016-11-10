```go

import (
    "io/ioutil"
    "os"
    "strings"
    "testing"

    "github.com/mitchellh/cli"
)

// inside some function

if out := ui.ErrorWriter.String(); !strings.Contains(out, "Error getting vol struct") {
  // ...
}
```
