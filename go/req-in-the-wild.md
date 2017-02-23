## Requirements in the wild

This is a curated list of technical requirements. You may be able to find 
these in stack overflow or Google it. However, an active curated content
helps me always. Objective of writing it down is improving my productivity
while implementing these requirements.

### Requirement 1

> I am a programmer. Can I have some golang's design references before starting 
to code in golang.

#### Lifted from k8s/pkg/volume/volume.go file

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

#### Observations w.r.t k8s/pkg/volume/volume.go file

```yaml
 An `interface` can have:
   - method signature(s) & 
   - composes other interface(s) and hence their signatures
```

```yaml
A `base entity` can be an `interface`:
  - Should we call it as a **BASE INTERFACE**:
    - It has one or couple of properties:
        - e.g. Volume interface
    - It should be minimalistic
    - It may be injected with external `concerns`:
      - Concerns are otherwise known as aspects
        - e.g. Volume injects MetricsProvider
	- Is `injecting` here a good approach ?
	- Further Study: `Aspect Oriented Programming` & `Dependency Injection`
```

```yaml
An action can represent an `interface` too:
  - e.g. Mounter
  - These action interface(s) may compose its own base interface
```

```yaml
Input & output parameters :
  - Have been left out of brevity
  - Some of the questions we shoud ask:
    - Will it be good to have `standard types` for input & output parameters ?
    - Else mocking or new implementations will need to import external types !!!
    - However, sometimes it makes sense to define custom types & use them here.
      - e.g. types.NodeName than string,
      - e.g. resource.Quantity than int
    - Sometimes versioning may be required as types in these parameters.
      - e.g. v1.NodeAddress
      - e.g. v1.PersistentVolume
    - Does all these mean sacrificing simplicity ?
      - How have the language designers implemented the same ?
```

```yaml
Naming:
  - Does the name provides ample evidence of differentiating: 
    - `interface` from a `struct` ?
    - Can it be carried as a standard practice without being orthodox ?
  - How do you differentiate a Metrics (`struct`) vs. a Volume (`interface`):    
    - Does the suffix `s` play any role ?
    - Does this mean `Volumes` should be a struct ?
  - Is there any solution to `struct` vs `interface` naming ?
    - How about **JivaVolume** as a choice for struct ?
    - Similarly, **JivaMetrics** as a struct
    - We can have **StdMetrics**, **StdVolume** too.
    - We downplay the rule of `Opts`, `Props` or `s` as suffix
    - We vote for specialized entity as prefix
```

```yaml
Ideas:
  - Mostly same as above
  - Changes:
    - Metrics as interface
    - StdMetrics as struct
    - StdVolume as struct
    - JivaVolume as struct in specific volume
    - JivaMetrics as struct in specific volume
```

```yaml
Structs can be aggregation of common properties:
  - e.g. Attributes
  - Should these kind of structs be used in `interface` methods ?
  - Should we remember these as **STANDARD STRUCTS**
    - i.e. structs that are at same level as an interface
    - i.e. structs which are understood by operators & admins
    - i.e. structs which are used in communication between different stakeholders
  - Will it be good to name it as `AttributeProps` ?
    - Nah...
```

#### Contd.. from k8s/pkg/volume/plugins.go

```yaml
File: pkg/volume/plugins.go
Interfaces:
  - VolumePlugin:
    - Init(host VolumeHost) error
    - GetPluginName() string
    - GetVolumeName(spec *Spec) (string, error)
    - CanSupport(spec *Spec) bool
    - RequiresRemount() bool
    - NewMounter(spec *Spec, podRef *v1.Pod, opts VolumeOptions) (Mounter, error)
    - NewUnmounter(name string, podUID types.UID) (Unmounter, error)
    - ConstructVolumeSpec(volumeName, mountPath string) (*Spec, error)
  - PersistentVolumePlugin:
    - VolumePlugin
    - GetAccessModes() []v1.PersistentVolumeAccessMode
  - RecyclableVolumePlugin:
    - VolumePlugin
    - Recycle(pvName string, spec *Spec, eventRecorder RecycleEventRecorder) error
  - DeletableVolumePlugin:
    - VolumePlugin
    - NewDeleter(spec *Spec) (Deleter, error)
  - ProvisionableVolumePlugin:
    - VolumePlugin
    - NewProvisioner(options VolumeOptions) (Provisioner, error)
  - AttachableVolumePlugin:
    - VolumePlugin
    - NewAttacher() (Attacher, error)
    - NewDetacher() (Detacher, error)
    - GetDeviceMountRefs(deviceMountPath string) ([]string, error)
  - VolumeHost:
    - GetPluginDir(pluginName string) string
    - GetPodVolumeDir(podUID types.UID, pluginName string, volumeName string) string
    - GetPodPluginDir(podUID types.UID, pluginName string) string
    - GetKubeClient() clientset.Interface
    - NewWrapperMounter(volName string, spec Spec, pod *v1.Pod, opts VolumeOptions) (Mounter, error)
    - NewWrapperUnmounter(volName string, spec Spec, podUID types.UID) (Unmounter, error)
    - GetCloudProvider() cloudprovider.Interface
    - GetMounter() mount.Interface
    - GetWriter() io.Writer
    - GetHostName() string
    - GetHostIP() (net.IP, error)
    - GetNodeAllocatable() (v1.ResourceList, error)
    - GetSecretFunc() func(namespace, name string) (*v1.Secret, error)
Structs:
  - VolumeOptions:
    - PersistentVolumeReclaimPolicy v1.PersistentVolumeReclaimPolicy
    - PVName string
    - PVC *v1.PersistentVolumeClaim
    - ClusterName string
    - CloudTags *map[string]string
    - Parameters map[string]string
  - VolumePluginMgr:
    - mutex   sync.Mutex
    - plugins map[string]VolumePlugin
  - Spec:
    - Volume           *v1.Volume
    - PersistentVolume *v1.PersistentVolume
    - ReadOnly         bool
  - VolumeConfig:
    - RecyclerPodTemplate *v1.Pod
    - RecyclerMinimumTimeout int
    - RecyclerTimeoutIncrement int
    - PVName string
    - OtherAttributes map[string]string
    - ProvisioningEnabled bool
```

#### Observations w.r.t k8s/pkg/volume/plugins.go

```yaml
Notes:
  - All definitions to build the bridge
  - Some of the types are from volume/volume
  - Some of the types are from v1
  - VolumeHost bridges plugins to access the kubelet
  - VolumeHost is an aspect
  - The placement of structs & interfaces in respective .go files seems good
  - All **API volume types translate** to Spec
Important Notes:
  - VolumeConfig & initialization of plugin go hand-in-hand  
  - RecyclerPodTemplate is a *v1.Pod struct that determines the logic branch
    - template to logic
  - OtherAttributes map[string]string is opaque to the system
    - stores config as strings
    - is understood only by the plugin binary
Naming:
  - VolumePlugin vs. VolumeAdaptor
  - VolumeOptions vs. VolumePluginOptions
  - VolumeHost vs. VolumePluginHost
  - VolumePluginMgr vs. DefaultVolumePluginMgr
  - VolumeConfig vs. VolumePluginConfig
```

```yaml
Ideas:
  - OrchProviderPlugin
  - OrchProviderPluginOptions
```

#### Contd.. from k8s/pkg/volume/aws_ebs/aws_ebs.go

```yaml
File: volume/aws_ebs/aws_ebs.go
Interfaces:
  - ebsManager
    - CreateVolume(provisioner *awsElasticBlockStoreProvisioner) 
      - returns (volumeID aws.KubernetesVolumeID, volumeSizeGB int, labels map[string]string, err error)
    - DeleteVolume(deleter *awsElasticBlockStoreDeleter) error
Structs:
  - awsElasticBlockStorePlugin:
    - host volume.VolumeHost
  - awsElasticBlockStore:
    - volName string
    - podUID  types.UID
    - volumeID aws.KubernetesVolumeID
    - partition string
    - manager ebsManager
    - mounter mount.Interface
    - plugin  *awsElasticBlockStorePlugin
    - volume.MetricsProvider
  - awsElasticBlockStoreMounter:
    - *awsElasticBlockStore
    - fsType string
    - readOnly bool
    - diskMounter *mount.SafeFormatAndMount
  - awsElasticBlockStoreUnmounter:
    - *awsElasticBlockStore
  - awsElasticBlockStoreDeleter:
    - *awsElasticBlockStore
  - awsElasticBlockStoreProvisioner:
    - *awsElasticBlockStore
    - options   volume.VolumeOptions
    - namespace string
```

#### Observations from k8s/pkg/volume/aws_ebs/aws_ebs.go

```yaml
Notes:
  - awsElastiBlockStorePlugin struct will implement VolumePlugin methods
    - is private
  - ebsManager interface implies further granularity in control
    - is private
    - is implemented as &AWSDiskUtil{}
      - a separate file that binds to particular cloud provider & its method
  - Interface contract methods can be further implemented in unit testable manner
    - e.g. call up `contractMethod`Internal with additional parameters
    - these additional parameters can be derived from struct properties
    - or these parameters can be injected with default instantiations
    - this is just for **unit testing** convenience
```

```yaml
Ideas:
  - jivaVolumePlugin
  - jivaManager
  - jivaVolume
  - JivaStorageUtil from AWSDiskUtil
```

#### Lifted from k8s/pkg/cloudprovider/cloud.go file

```yaml
File: pkg/cloudprovider/cloud
Interfaces:
  - Interface:
    - LoadBalancer() (LoadBalancer, bool)
    - Instance() (Instances, bool)
```

#### Observations w.r.t k8s/pkg/cloudprovider/cloud.go

```yaml
```

#### Lifted from aws-sdk-go/aws/credentials

```yaml
File: credentials/credentials.go
Interfaces:
  - Provider:
    - Retrieve() 	(Value, error)
    - IsExpired() 	bool
Structs:
  - Expiry:
    - expiration 	time.Time
    - CurrentTime 	func() time.Time
  - Value:
    - AccessKeyID 	string
    - SecretAccessKey	string
    - SessionToken 	string
    - ProviderName 	string
  - Credentials:
    - creds		Value
    - forceRefresh	bool
    - m			sync.Mutex
    - provider		Provider
```

#### Observations w.r.t aws-sdk-go/aws/credentials

```yaml
Aggregated properties in a struct:
  - e.g. **Value** represents the `sought after properties`
  - Can `Attributes` be a better naming choice ?
    - Value is better as credential is better understood with its value.
```

```yaml
Shared struct:
  - e.g. **Expiry** can be be embedded anonymously within any other struct
  - It can provide shared logic
```

```yaml
Mock properties of a struct:
  - e.g. CurrentTime property of Expiry can be mocked
  - Suitable for unit test coverage
  - Is set as a public property
```

```yaml
A `manager entity` tied to its provider:
  - Here the manager entity is a struct i.e. Credentials
  - Can we think of it as a **MANAGER ENTITY** ?
  - It embeds Provider interface:
  - Typical Instantion:
    - NewCredentials(provider *Provider) *Credentials
  - Will expose methods that are wrappers over Provider
  - Usage of mutex implies safety across multiple goroutines
    - Each provider need not manage synchronous state  
```

#### Lifted from aws/aws-sdk-go/aws/types.go

```yaml
```

#### Observations w.r.t aws/aws-sdk-go/aws/types.go

```yaml
```

#### Lifted from aws-sdk-go/aws/config.go

```yaml
```

#### Observations w.r.t aws-sdk-go/aws/config.go

```yaml
```

#### Lifted from aws-sdk-go/aws/convert_types.go

```yaml
```

#### Observations from aws-sdk-go/aws/convert_types.go

```yaml
```

### Requirement 2

> Alright I learnt the design stuff. Any cookbook recipes or snippets that helps
me program faster. I should be golang technical Implementations.

```yaml
Requirements:
  - templating    
      - import text/template    
      - strtmpl string, obj interface{}
      - tmpl, err := template.New("template").Parse(strtmpl)
      - err = tmpl.Execute(&buf, obj)
```

## Below needs to be fit somewhere else !!

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

