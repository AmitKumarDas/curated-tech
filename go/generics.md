
## Generics via interface

- Keep an eye on interface vs interface{}

```go

type Response struct {
	Values map[string]interface{}
}

// in some method
values := map[string]interface{}{}

if !removed {
  values = map[string]interface{}{
    OPT_SNAPSHOT_NAME:         snapshot.Name,
    "VolumeName":              volumeID,    
  }
} else {
  values = map[string]interface{}{
    OPT_SNAPSHOT_NAME: snapshot.Name,
    "VolumeName":      volumeID,
    "State":           "removed",
  }
}
```
