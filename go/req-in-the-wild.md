## Requirements in the wild

This is a curated list of technical requirements. You may be able to find 
these in stack overflow or Google it. However, an active curated content
helps me always. Objective of writing it down is improving my productivity
while implementing these requirements.

### I want a programming language's design reference ?

- Lifted from k8s/pkg/volume/volume.go file

```yaml
File: pkg/volume/volume
Interfaces:
  - Volume:
    - GetPath()
    - MetricsProvider
  - MetricsProvider:
    - GetMetrics()
  - Mounter:
    - Volume
    - CanMount()
    - SetUp()
    - SetUpAt()
    - GetAttributes()
  - Unmounter:
    - Volume
    - TearDown()
    - TearDownAt()
  - Recycler:
    - Volume
    - Recycle()
  - Provisioner:
    - Provision()
  - Deleter:
    - Volume
    - Delete()
  - Attacher:
    - Attach()
    - VolumesAreAttached()
    - WaitForAttach()
    - GetDeviceMountPath()
    - MountDevice()
  - Detacher:
    - Detach()
    - UnmountDevice()
Structs:
  - Metrics:
    - Used
    - Capacity
    - Available
    - InodesUsed
    - Inodes
    - InodesFree
  - Attributes:
    - ReadOnly
    - Managed
    - SupportsSELinux
```

- Points to note
  - An interface has
    - method signature & 
    - composes other interface(s)
  - A `base entity` can be an `interface`
    - What is a **`base interface`** ?
      - It has a bare minimal property
      - It may represents common stuff
      - e.g. Volume
  - An action can represent an `interface` too
    - e.g. Mounter
    - These action interface(s) may compose its base interface
  - Input & output parameters 
    - NOTE: Have been left out of brevity
    - These are some of the questions we shoud ask ?
      - Will it be good to have standard types for input & output parameters ?
      - Else mocking or new implementations will need to import external types !!!
      - However, sometimes it makes sense to define custom types & use them here.
        - e.g. types.NodeName than string,
        - e.g. resource.Quantity than int
      - Sometimes versioning may be required as types in these parameters.
        - e.g. v1.NodeAddress
        - e.g. v1.PersistentVolume
      - Does all these mean sacrificing simplicity ?
        - How have the language designers implemented the same ?
  - Reflect over the naming conventions
    - Does the name provides ample evidence of differentiating an 
    	- `interface` from a `struct` ?
    - Can it be carried as a standard practice without being orthodox ?
  - How do you differentiate a Metrics (`struct`) vs. a Volume (`interface`)
    - Will `MetricProperties` or `MetricOptions` be a better name ?
    - This was not `Metrics vs. Volumes` rather `Metrics vs. Volume`
    - Does the suffix `s` play any role ?
    - Does this mean `Volumes` should be a struct ?
    - Will `VolumeProps` be a better choice as a struct ?
  - Can we downplay the `xxxs` & `xxxProps` ?
      - How about `JivaVolume` as a choice for struct ?
      - Similarly, `JivaMetrics` as a struct
      - We downplay the rule of `Opts`, `Props` or `s` as suffix
      - We vote for specialized entity as prefix
  - Structs can be aggregation of common properties 
    - e.g. Attributes
    - Should these kind of structs be used in `interface` methods ?
    - Should we remember these as `common structs`
      - i.e. structs that are at same level as an interface
    - Will it be good to name it as `AttributeProps` ?
      - Nah...

- Lifted from k8s/pkg/cloudprovider/cloud.go file

```yaml
File: pkg/cloudprovider/cloud
Interfaces:
  - Interface:
    - LoadBalancer() (LoadBalancer, bool)
    - Instance() (Instances, bool)
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

