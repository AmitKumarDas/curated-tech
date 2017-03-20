## Requirements in the wild

This is a curated list of technical `golang programming` requirements. You may be able to find 
these in stack overflow or Google it. However, this personally curated content helps me in 
focusing without sacrificing my time bound metrics. This should improve my productivity while 
implementing these or similar requirements.

### Requirement 1

> I learnt the basics stuff in golang. Any cookbook recipes or snippets that helps
me program faster. It should be golang technical implementations.

```yaml
Requirements:
  - Get all IP address from CIDR in golang
    - https://gist.github.com/kotakanbe/d3059af990252ba89a82	
  - I want a gossip implementation:
    - https://github.com/weaveworks/mesh
    - provides:
      - membership
      - unicast
      - broadcast
      - eventually consistent semantics
    - works in:
      - NAT
      - firewalls
      - across clouds
      - across DCs
      - can hop when there is no direct connection between peers
    - authentication:
      - shared secret auth & encryption
    - scale:
      - 1 till 100 peers
    - bootstraping:
      - easily done by providing a single existing peer
  - Learnings from nomad/structs/structs:
    - Standard golang types:
      - Solid focus on standard types
      - Validation
      - Hence DDD
    - Good Naming Conventions:
      - Type vs. Status vs. Mode vs. State
    - Solid Templating:
      - Modeling in struct
      - Rendering it for a given task
      - Re-Render:
        - ChangeSignal
        - ChangeMode
      - Policy coded as golang structs
    - You get to see lot of typical **GOLANG CODE**:
      - strong sense of Domain Driven Development (DDD) programming
      - validation
      - error handling
      - operators
      - copy
      - slice
      - mapping & associations
      - good reference on use of constants
      - grouping of related constants 
      - customized hashing for equals checking
  - Reverse Proxy, Load balancer, Metrics, Admin Console
    - https://github.com/containous/traefik
    - Traefik can listen to your service registry/orchestrator API, 
    - & knows each time a microservice is added, removed, killed or upgraded, 
    - & can generate its configuration automatically. 
    - Routes to your services will be created instantly.
    - TWIST: Can this be made as a Metrics & Admin Console UI
  - new vs make:
    - Code:
      - fmt.Printf("%T  %v\n", new([10]int), new([10]int))
      - fmt.Printf("%T  %v\n", make([]int, 10), make([]int, 10))
    - O/P:
      - *[10]int  &[0 0 0 0 0 0 0 0 0 0]
      - []int  [0 0 0 0 0 0 0 0 0 0]
  - Type printing:
    - i := 10
    - s := strconv.Itoa(i)
    - fmt.Printf("%T, %v\n", s, s)
    - // string, 10
  - What do you learn from follwing :
    - func (d *dumper) Dump(xyz) abc
    - type dumper struct:
      - w       io.Writer
    - func Dumper(w io.Writer) io.WriteCloser:
      - return &dumper{w: w}
    - Learnings:
      - Composition of interface
      - Private struct
      - Public initializer
      - Simple naming convention:
        - NewDumper vs. Dumper
  - Templating    
    - import text/template    
    - strtmpl string, obj interface{}
    - tmpl, err := template.New("template").Parse(strtmpl)
    - err = tmpl.Execute(&buf, obj)
  - Forced Compilation Check
    - // ensure kubeletVolumeHost implements VolumeHost interface
    - var _ volume.VolumeHost = &kubeletVolumeHost{}
  - Collecting Warnings
    - https://github.com/go-warnings/warnings/commit/8a331561fe74dadba6edfc59f3be66c22c3b065d
  - Export Metrics
    - github.com/armon/go-metrics
  - Fuzzy Testing
    - random values
    - github.com/google/gofuzz
  - Marshall & UnMarshall minus Reflection
    - github.com/mailru/easyjson
  - Json, Hcl & golang Structs - compatibility
    - hashicorp/nomad/api/compose_test.go
    - hashicorp/nomad/api/jobs_testing.go
```



## Below needs to be fit somewhere else !!

### Can I ever understand a filesystem ?

```yaml
Ref: https://github.com/spf13/afero
```


### The day I got stumped with init()

```
  - Need to understand the initialization order of .go files
  - The .go files are initialized in alphabetical order
  - **GOOGLY**: 
    - There are two files a.go & c.go in the same package
    - init() function of a.go depends on init() function of c.go
    - However, init of c.go has not yet happened when init() of a.go is invoked
```

### Get your coding right

```
  - https://blog.skyliner.io/ship-small-diffs-741308bec0d1#.8zz7zx26f
  - https://www.goinggo.net/2017/02/design-philosophy-on-integrity.html
  - https://www.goinggo.net/2014/10/error-handling-in-go-part-i.html
  - https://www.goinggo.net/2014/11/error-handling-in-go-part-ii.html
  - https://github.com/ardanlabs/gotraining/tree/master/topics/api/error_handling
  - https://dave.cheney.net/2013/06/09/writing-table-driven-tests-in-go
  - https://nathanleclaire.com/blog/2015/10/10/interfaces-and-composition-for-effective-unit-testing-in-golang/
```

### Learn you some unit testing

```
  - https://nathanleclaire.com/blog/2015/10/10/interfaces-and-composition-for-effective-unit-testing-in-golang/
  - Scenario: When a struct exposes method to consumers & also relies on these methods internally
    - How do you unit test above ?
    - Hint: Compose large interfaces from smaller ones by embedding
  - Conclusion: Simple modularization is not enough for effective unit testing
  - Designing Interfaces - https://github.com/kubernetes/kubernetes/blob/master/pkg/cloudprovider/cloud.go
  - Design Mocks - https://github.com/kubernetes/kubernetes/tree/master/pkg/cloudprovider/providers/fake
```

### Iteration, Chain Of Execution, Handlers, Injection / Hooks at various points

```
  - definition - https://github.com/aws/aws-sdk-go/blob/master/aws/request/handlers.go
  - usage - https://github.com/aws/aws-sdk-go/blob/master/aws/request/request.go
```

### Living with interfaces

```
  - Interfaces all the way down
  - https://github.com/kubernetes/kubernetes/blob/master/pkg/cloudprovider/cloud.go
```

### Plugins, Providers & Factory concept

```
  - https://github.com/kubernetes/kubernetes/blob/master/pkg/cloudprovider/plugins.go
```

### Have you ever come across a pkg that needs to do dual stuff ?

```
  - e.g. service & api
    - service will be used by the callers or clients or this pkg
    - api is something that this pkg uses to achieve its objective
  - ref - https://github.com/aws/aws-sdk-go/tree/master/aws/ec2metadata
```

### Why select & channel go hand-in-hand ?

```
  - https://blog.quickmediasolutions.com/2015/09/13/non-blocking-channels-in-go.html
```

### Why json in struct ?

```
  - Printing a golang struct with fields & values
  - http://stackoverflow.com/questions/24512112/golang-how-to-print-struct-variables-in-console
```

### Pointer to Value, Pointers to Values & vice-versa

```
  - https://github.com/aws/aws-sdk-go/blob/master/aws/convert_types.go
```

### Provider snippet
	
  - https://github.com/kubernetes/kubernetes/tree/master/pkg/cloudprovider/providers

### IO customization via std golang io interfaces

  - https://github.com/aws/aws-sdk-go/blob/master/aws/types.go

### Serialization Snippet

```go
  b := &bytes.Buffer{}
	_, err := io.Copy(b, r.HTTPResponse.Body)
	if err != nil {
    return fmt.Errorf("Serialization error: %s", err.Error())
  }
```

### Interactive CLI
  
  - https://github.com/asciimoo/wuzz

### CLI - Look Good

  - https://github.com/renstrom/dedent

### Event & Agent

  - https://github.com/muesli/beehive

### HTTP Introspection
  
  - https://github.com/asciimoo/wuzz

### Build dynamic DNS service
  
  - http://mkaczanowski.com/golang-build-dynamic-dns-service-go/

### slices internals
  
  - Ref - https://blog.golang.org/go-slices-usage-and-internals

### Handling metadata or kv pairs or unknown in CLI. 
  
  - Ref - nomad client & agent startup

### Initializing telemetry
  
  - Ref nomad agent startup

### Joining other services/daemon
  
  - Ref nomad agent startup

### Merging two configs & returning a new config (a new mem location)
  
  - Ref nomad config

### Data Driven Testing minus frameworks
  
  - Ref coreos/ignition @ config/types/filesystem_test.go
  - [Table Driven Testing](https://dave.cheney.net/2013/06/09/writing-table-driven-tests-in-go)

### Versioning with types
  
  - Ref coreos/ignition @ config/v1/

### Config parsing, deep copy, comparison, testing
  
  - Ref nomad
  - Ref ignition

### Polymorphism - Coding to Interfaces

  - Ref https://github.com/aws/aws-sdk-go/tree/master/aws/credentials

### Code to AWS - DIY in 5 minutes
  
  - Ref https://github.com/aws/aws-sdk-go/blob/master/aws/credentials/endpointcreds/provider.go

### Http Client coding - DIY in 5 minutes
  
  - Ref https://github.com/aws/aws-sdk-go/blob/master/aws/credentials/endpointcreds/provider.go

### Chain providers - if-elif-else in a better way - exclusive provider - run only valid
  
  - Ref https://github.com/aws/aws-sdk-go/blob/master/aws/credentials/chain_provider.go

### A deep pretty printer for Go data structures to aid in debugging
  
  - Ref https://github.com/davecgh/go-spew/

### Which **DESIGN** to use ?
  
  - Ref https://appliedgo.net/generics/

### Decoding arbitrary data
  
  - Case for **empty interface** i.e. interface{}
  - Every go type implements empty interface
  - Ref https://blog.golang.org/json-and-go

### Network Programming - Did you understand this in your college ?
  
  - https://jannewmarch.gitbooks.io/network-programming-with-go-golang-/content/
  - https://blog.packagecloud.io/eng/2017/02/06/monitoring-tuning-linux-networking-stack-sending-data/

### Query language for JSON
  
  - http://jmespath.org/
  - https://github.com/jmespath/go-jmespath

### Monitoring
  
  - http://www.brendangregg.com/blog/2017-02-05/file-system-flame-graph.html
  - https://news.ycombinator.com/item?id=13574825

### Low Level OS Interfacing
  
  - golang.org/x/sys/unix

### Security
  
  - https://github.com/cloudflare/gokeyless

### Load testing
  
  - https://github.com/tsenart/vegeta
  - reports, histogram, metrics

### CLI & Ease of Use Reference Library - awless

  - https://github.com/wallix/awless
  - AWS, 
  - **CLI** (auto-completion), tabular format, etc
    - If you loved OpenStack CLI then you will feel at home here as well
  - Scriptable Templates,  **DSL**
  - Since a RDF model of the infrastructure (stored locally) is implemented, can answer many advanced queries easily
    - RDF is used to sync & model the cloud/remote resources locally
  - Local **history** & **versioning** of changes
  - SSH support
  - Inspection
  - **SUPPORTABILITY** - A big YEAH !!!

### w.r.t K8s

  - use apimachinery packages instead of client-go packages

### Networking Library Reference - meshbird

  - https://github.com/meshbird/meshbird
  - Distributed private networking

### Http Inspection Library - wuzz

  - https://github.com/asciimoo/wuzz
  - A CLI for http inspection

### UI Reference Library - botpress

  - https://github.com/botpress/botpress
  - Modularization, Composition, Extensibility
  - Build from some foundation than thinking all over from scratch
  - This is JavaScript based & no golang based

### Versioning Reference Library - K8s

  - Fitting version when exposed as services
  - Fitting namespacing when exposed as services
  - https://github.com/kubernetes/community/blob/master/contributors/design-proposals/persistent-storage.md#persistentvolume-api

### Makefile Reference - K8s's makefile

  - Good CLI with various options
  - Help / Usage
  - A good tutorial on just appropriate usage of shell scripting

### Vagrant Provider Library - K8s's cluster provider

  - A good tutorial on just appropriate usage of shell scripting
  - How a Vagrant provider can be scripted in bash

### QEMU/KVM Provider Library - CoreOS's matchbox

  - https://github.com/coreos/matchbox/tree/master/scripts
  - How a KVM provider can be scripted in bash

### Installer Reference Library - CoreOS's matchbox

  - https://github.com/coreos/matchbox/blob/master/Documentation/deployment.md
  - You need to read the internal used libraries of CoreOS to make it fit for other usages

### gopkg.in/gcfg.v1

  - Package gcfg reads "INI-style" text-based configuration files with "name=value" pairs 
  - These pairs are grouped into sections.

### Supportability Commands - Reckoner

  - http://ablagoev.github.io/linux/adventures/commands/2017/02/19/adventures-in-usr-bin.html

## Yet to understand the requirements !!!

### Tough Nut - Reflect, byte, string & interface
  
  - Ref https://github.com/aws/aws-sdk-go/blob/master/aws/awsutil/string_value.go

