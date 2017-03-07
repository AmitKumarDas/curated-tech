## Requirements in the wild

This is a curated list of technical requirements. You may be able to find 
these in stack overflow or Google it. However, an active curated content
helps me always. Objective of writing it down is improving my productivity
while implementing these requirements.

### Requirement 1

> I am a programmer. Can I have some golang's design references before starting 
to code in golang.

#### Observations w.r.t nomad/structs/structs.go

```yaml
Notes:
  - ApiMajorVersion is returned as part of Status.Version request
  - RPCInfo represents the common info about query
  - It is all about grouping of attributes
  - As the name of the file suggests, it is all about structs
  - Not sure if the versioning strategy helps ?
```

```yaml
Ideas:
  - Good to import this file & have the entire Nomad specs
  - Standard golang types:
    - Solid focus on standard types
    - Validation
    - Hence DDD
  - Naming:
    - Type
    - Status
    - Mode
    - State
  - Template:
    - Modeling in struct
    - Rendering it for a given task
    - Re-Render:
      - ChangeSignal
      - ChangeMode
  - Customized hashing for equals checking
  - Data modeling of **service** check
  - Policy coded as golang structs
  - Grouping of related constants 
  - You get a strong sense of Domain Driven Development (DDD) programming.
  - You get to see lot of typical **GOLANG CODE**:
    - validation
    - error handling
    - operators
    - copy
    - slice
    - mapping & associations
  - Good reference on use of constants
  - Good idea on the attributes to use for entities:
    - Allocation
      - Placement
    - Evaluation
      - Applying business logic
    - Operand
    - Template
    - Artifact
    - Job:
      - Task Group
      - Parallel
      - Meta
      - Token
      - Constraints
      - Priority
      - Parent
      - Raft:
        - CreateIndex
	- ModifyIndex
    - Node
    - Resource
    - NetworkResource
```

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

#### Lifted from k8s/pkg/volume/plugins.go

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

#### Lifted from k8s/pkg/volume/aws_ebs/aws_ebs.go

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

#### Lifted from k8s/pkg/cloudprovider/cloud.go file

```yaml
File: pkg/cloudprovider/cloud
Interfaces:
  - Interface:
    - LoadBalancer() (LoadBalancer, bool)
    - Instance() (Instances, bool)
    - Zones() (Zones, bool)
    - ProviderName() string
    - ScrubDNS(nameservers, searches []string) (nsOut, srchOut []string)
  - Instances:
    - NodeAddresses(name types.NodeName) ([]v1.NodeAddress, error)
    - ExternalID(nodeName types.NodeName) (string, error)
    - InstanceID(nodeName types.NodeName) (string, error)
    - InstanceType(name types.NodeName) (string, error)
    - AddSSHKeyToAllInstances(user string, keyData []byte) error
    - CurrentNodeName(hostname string) (types.NodeName, error)
  - LoadBalancer:
    - GetLoadBalancer(clusterName string, service *v1.Service) 
      - returns (status *v1.LoadBalancerStatus, exists bool, err error)
    - EnsureLoadBalancer(clusterName string, service *v1.Service, nodes []*v1.Node) 
      - returns (*v1.LoadBalancerStatus, error)
    - UpdateLoadBalancer(clusterName string, service *v1.Service, nodes []*v1.Node) error
    - EnsureLoadBalancerDeleted(clusterName string, service *v1.Service) error
  - Clusters:
    - ListClusters() ([]string, error)
    - Master(clusterName string) (string, error)
```

#### Lifted from k8s/pkg/cloudprovider/plugins.go

```yaml
Custom Type:
  - type Factory func(config io.Reader) (Interface, error)
Global Var:
  - providersMutex sync.Mutex
  - providers      = make(map[string]Factory)
Public Functions:
  - RegisterCloudProvider(name string, cloud Factory)
  - IsCloudProvider(name string) bool
  - CloudProviders() []string
  - GetCloudProvider(name string, config io.Reader) (Interface, error)
  - InitCloudProvider(name string, configFilePath string) (Interface, error)
```

#### Lifted from k8s/pkg/cloudprovider/aws/aws.go

```yaml
File: cloudprovider/aws/aws.go
Interfaces:
  - Services
    - Compute(region string) (EC2, error)
    - LoadBalancing(region string) (ELB, error)
    - Autoscaling(region string) (ASG, error)
    - Metadata() (EC2Metadata, error)
  - EC2
    - DescribeInstances(request *ec2.DescribeInstancesInput) ([]*ec2.Instance, error)
    - AttachVolume(*ec2.AttachVolumeInput) (*ec2.VolumeAttachment, error)
    - DetachVolume(request *ec2.DetachVolumeInput) (resp *ec2.VolumeAttachment, err error)
    - DescribeVolumes(request *ec2.DescribeVolumesInput) ([]*ec2.Volume, error)
    - CreateVolume(request *ec2.CreateVolumeInput) (resp *ec2.Volume, err error)
    - DeleteVolume(*ec2.DeleteVolumeInput) (*ec2.DeleteVolumeOutput, error)
  - EC2Metadata
    - GetMetadata(path string) (string, error)
Structs:
  - CloudConfig
  - awsSdkEC2
    - ec2 *ec2.EC2
  - awsSDKProvider
    - creds *credentials.Credentials
    - mutex sync.Mutex
    - regionDelayers map[string]*CrossRequestRetryDelay
  - Cloud
    - ec2 		EC2
    - metadata 		EC2Metadata
    - cfg 		*CloudConfig
    - region 		string
    - filterTags 	map[string]string
    - mutex 		sync.Mutex
    - attachingMutex 	sync.Mutex
    - attaching		map[types.NodeName]map[mountDevice]awsVolumeID
    - deviceAllocators 	map[types.NodeName]DeviceAllocator
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

#### Lifted from hashicorp/nomad/jobspec

```yaml
```

#### Lifted from K8s/pkg/api/validation

```yaml
```

### Requirement 2

> Alright I learnt the design stuff. Any cookbook recipes or snippets that helps
me program faster. I should be golang technical Implementations.

```yaml
Requirements:
  - Naming, Composition, Function vs. Method
    - func Dump(data []byte) string
      - var buf bytes.Buffer
      - dumper := Dumper(&buf)
      - dumper.Write(data)
      - dumper.Close()
      - return buf.String()
    - func Dumper(w io.Writer) io.WriteCloser
      - return &dumper{w: w}   
    - type dumper struct
      - w       io.Writer
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

