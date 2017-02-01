### Requirements in the wild

> This is a curated list of technical requirements. You may not
find them directly in stack overflow. These are a bit better than
code snips.

- Handling metadata or kv pairs or unknown in CLI. 
  - Ref - nomad client & agent startup
- Initializing telemetry
  - Ref nomad agent startup
- Joining other services/daemon
  - Ref nomad agent startup
- Merging two configs & returning a new config (a new mem location)
  - Ref nomad config
- Data Driven Testing minus frameworks
  - Ref coreos/ignition @ config/types/filesystem_test.go
- Versioning with types
  - Ref coreos/ignition @ config/v1/
- Config parsing, deep copy, comparison, testing
  - Ref nomad
  - Ref ignition
- Polymorphism - Coding to Interfaces
  - Ref https://github.com/aws/aws-sdk-go/tree/master/aws/credentials
- Code to AWS - DIY in 5 minutes
  - Ref https://github.com/aws/aws-sdk-go/blob/master/aws/credentials/endpointcreds/provider.go
- Http Client coding - DIY in 5 minutes
  - Ref https://github.com/aws/aws-sdk-go/blob/master/aws/credentials/endpointcreds/provider.go
- Chain providers - if-elif-else in a better way - exclusive provider - run only valid
  - Ref https://github.com/aws/aws-sdk-go/blob/master/aws/credentials/chain_provider.go
