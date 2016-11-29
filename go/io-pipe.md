```go
// If your code or CLI accepts stdin as its args.
// You can unit test that.
// Check this piece of code.
func TestVolCommand_From_STDIN(t *testing.T) {
	stdinR, stdinW, err := os.Pipe()
	if err != nil {
		t.Fatalf("err: %s", err)
	}

	go func() {
		stdinW.WriteString(`
    job "job1" {
      type = "service"
      datacenters = [ "dc1" ]
      group "group1" {
        count = 1
        task "task1" {
          driver = "exec"
          resources = {
            cpu = 1000
            memory = 512
          }
        }
      }
    }`)
		stdinW.Close()
	}()

// ... rest of the code 
}
```
