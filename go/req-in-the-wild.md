### Requirements in the wild

This is a curated list of technical requirements. You may be able to find 
these in stack overflow or Google it. However, an active curated content
helps me always.

```
# The day I got stumped with init()
  - Need to understand the initialization order of .go files
  - The .go files are initialized in alphabetical order
  - GOOGLY: 
    - There are two files a.go & c.go in the same package
    - init() function of a.go depends on init() function of c.go
    - However, init of c.go has not yet happened when init() of a.go is invoked
  
# slices internals
  - Ref - https://blog.golang.org/go-slices-usage-and-internals

# Handling metadata or kv pairs or unknown in CLI. 
  - Ref - nomad client & agent startup

# Initializing telemetry
  - Ref nomad agent startup

# Joining other services/daemon
  - Ref nomad agent startup

# Merging two configs & returning a new config (a new mem location)
  - Ref nomad config

# Data Driven Testing minus frameworks
  - Ref coreos/ignition @ config/types/filesystem_test.go

# Versioning with types
  - Ref coreos/ignition @ config/v1/

# Config parsing, deep copy, comparison, testing
  - Ref nomad
  - Ref ignition

# Polymorphism - Coding to Interfaces
  - Ref https://github.com/aws/aws-sdk-go/tree/master/aws/credentials

# Code to AWS - DIY in 5 minutes
  - Ref https://github.com/aws/aws-sdk-go/blob/master/aws/credentials/endpointcreds/provider.go

# Http Client coding - DIY in 5 minutes
  - Ref https://github.com/aws/aws-sdk-go/blob/master/aws/credentials/endpointcreds/provider.go

# Chain providers - if-elif-else in a better way - exclusive provider - run only valid
  - Ref https://github.com/aws/aws-sdk-go/blob/master/aws/credentials/chain_provider.go

# A deep pretty printer for Go data structures to aid in debugging
  - Ref https://github.com/davecgh/go-spew/

# Which **DESIGN** to use ?
  - Ref https://appliedgo.net/generics/

# Decoding arbitrary data
  - Case for **empty interface** i.e. interface{}
  - Every go type implements empty interface
  - Ref https://blog.golang.org/json-and-go

# Network Programming - Did you understand this in your college ?
  - https://jannewmarch.gitbooks.io/network-programming-with-go-golang-/content/
  - https://blog.packagecloud.io/eng/2017/02/06/monitoring-tuning-linux-networking-stack-sending-data/

# Query language for JSON
  - http://jmespath.org/
  - https://github.com/jmespath/go-jmespath

# Monitoring
  - http://www.brendangregg.com/blog/2017-02-05/file-system-flame-graph.html
  - https://news.ycombinator.com/item?id=13574825

# Low Level OS Interfacing
  - golang.org/x/sys/unix

# Security
  - https://github.com/cloudflare/gokeyless

# Load testing
  - https://github.com/tsenart/vegeta
  - reports, histogram, metrics
```

### Yet to understand the requirements !!!

```
# Tough Nut - Reflect, byte, string & interface
  - Ref https://github.com/aws/aws-sdk-go/blob/master/aws/awsutil/string_value.go
```
