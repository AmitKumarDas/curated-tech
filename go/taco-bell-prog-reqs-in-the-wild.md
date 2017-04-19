```yaml
ELK with nginx:
  - https://qbox.io/blog/elasticsearch-logstash-kibana-manage-nginx-logs
Log Pilot/Operator:
  - https://github.com/AliyunContainerService/fluentd-pilot
KV Abstraction:
  - https://github.com/docker/libkv
Dockerfiles & More Dockerfiles:
  - https://github.com/vimagick/dockerfiles
Release to Github Minus TravisCI:
Make use of GO than Bash or Shell:
  - github.com/apprenda/kismatic/blob/master/release.go
  - github.com/apprenda/kismatic/blob/master/RELEASE.md#release-script-usage
Interactive CLI:
  - https://github.com/asciimoo/wuzz
CLI - Look Good:
  - https://github.com/renstrom/dedent
Event & Agent:
  - https://github.com/muesli/beehive
HTTP Introspection:
  - https://github.com/asciimoo/wuzz
Build dynamic DNS service:
  - http://mkaczanowski.com/golang-build-dynamic-dns-service-go/
Slices Internals:
  - https://blog.golang.org/go-slices-usage-and-internals
Handling metadata or kv pairs or unknown in CLI:
  - nomad client & agent startup
Initializing Telemetry:
  - Ref nomad agent startup
Merging two configs & returning a new config:
  - Ref nomad config
Chain:
  - if-elif-else in a better way
  - https://github.com/aws/aws-sdk-go/blob/master/aws/credentials/chain_provider.go
Deep Pretty Printer for Go Data Structures:
  - Aid in Debugging  
  - https://github.com/davecgh/go-spew/
Network Programming - Did You Understand This In Your College:
  - https://jannewmarch.gitbooks.io/network-programming-with-go-golang-/content/
  - https://blog.packagecloud.io/eng/2017/02/06/monitoring-tuning-linux-networking-stack-sending-data/
Query language for JSON:
  - http://jmespath.org/
  - https://github.com/jmespath/go-jmespath
Monitoring:
  - http://www.brendangregg.com/blog/2017-02-05/file-system-flame-graph.html
  - https://news.ycombinator.com/item?id=13574825
Low Level OS Interfacing:
  - golang.org/x/sys/unix
Security:
  - https://github.com/cloudflare/gokeyless
Load Testing:
  - https://github.com/tsenart/vegeta
  - reports, histogram, metrics
CLI & Ease of Use Reference Library:
  - https://github.com/wallix/awless
  - Rules of Supportability
Networking Library Reference:
  - https://github.com/meshbird/meshbird  
Http Inspection Library:
  - https://github.com/asciimoo/wuzz
UI Reference Library:
  - https://github.com/botpress/botpress
  - Modularization, Composition, Extensibility
  - Build from some foundation than thinking all over from scratch
  - This is JavaScript based & no golang based
QEMU/KVM Provider Library:
  - https://github.com/coreos/matchbox/tree/master/scripts
  - How a KVM provider can be scripted in bash
Tough Nut - Reflect, byte, string & interface:
  - Ref https://github.com/aws/aws-sdk-go/blob/master/aws/awsutil/string_value.go
IO customization via std GoLang IO Interfaces:
  - https://github.com/aws/aws-sdk-go/blob/master/aws/types.go
Why select & channel go hand-in-hand:
  - https://blog.quickmediasolutions.com/2015/09/13/non-blocking-channels-in-go.html
Local Filesystem Abstracted:
  - https://github.com/spf13/afero
Simple Dockerized GO Webapp for K8s Demos:
  - Can refer this as a pattern to implement CI usecase
  - A pattern to implement one CI usecase at a time
  - https://github.com/apprenda/kdemo
Smoke Test a K8s Install Using Docker Images:
  - https://github.com/apprenda/kuberang
Continuous Deployment of K8s objects:
  - https://github.com/box/kube-applier
Orchestrate Containers for Development with:
  - Docker Compose
  - https://blog.codeship.com/orchestrate-containers-for-development-with-docker-compose/
  - Rocker Compose
  - https://github.com/grammarly/rocker-compose
HighWaterMark:
  - to track the maximum value seen for some quantity
  - refer: K8s code base
Collecting Warnings:
  - https://github.com/go-warnings/warnings/commit/8a331561fe74dadba6edfc59f3be66c22c3b065d
Export Metrics:
  - github.com/armon/go-metrics
Fuzzy, Random Value Testing:
  - github.com/google/gofuzz
Marshall & UnMarshall minus Reflection:
  - github.com/mailru/easyjson
Json, Hcl & GoLang Structs:
  - hashicorp/nomad/api/compose_test.go
  - hashicorp/nomad/api/jobs_testing.go
Custom routines, Thread Safety:
  - https://github.com/containous/traefik/tree/master/safe
Http Requests Middleware:
  - auth, metrics, prometheus, stats, etc
  - https://github.com/containous/traefik/tree/master/middlewares
JSON/YAML to Consul KV:
  - https://github.com/lewispeckover/consulator
Yaml/GO based task runner:
  - https://github.com/kcmerrill/alfred/
Run curl from yaml:
  - https://github.com/fabiofalci/gohit
Command line parser for yaml:
  - https://github.com/mikefarah/yaml
Interface, Noop Impl, List Impl:
  - https://github.com/containerd/containerd/blob/master/plugin/monitor.go
Slice Tricks:
  - https://github.com/golang/go/wiki/SliceTricks
Million container challenge:
  - https://blog.codeship.com/running-1000-containers-in-docker-swarm/
ANN / ML in go:
  - http://mlexplore.org/2017/03/12/hopfield-networks-in-go/
io.Reader to line-by-line reader
  - https://github.com/msoap/byline
Vagrant vs K8s:
Dev to QA to Prod:
  - https://blog.branch.io/beta-architecture-scaling-developer-environments-with-kubernetes/
IP Utility & CNI:
  - https://github.com/containernetworking/cni/
IP Utility:
  - https://github.com/ziutek/utils/tree/master/netaddr
Get all IP address from CIDR in golang:
  - https://gist.github.com/kotakanbe/d3059af990252ba89a82
A gossip implementation:
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
Learnings from nomad/structs/structs:
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
  - Customized hashing for equals checking
Reverse Proxy, Load balancer, Metrics, Admin Console:
  - https://github.com/containous/traefik
  - Traefik can listen to your service registry/orchestrator API, 
  - & knows each time a microservice is added, removed, killed or upgraded, 
  - & can generate its configuration automatically. 
  - Routes to your services will be created instantly.
  - TWIST: Can this be made as a Metrics & Admin Console UI
Admin UI, Interact, Manage, Start, Stop Containers:
  - https://github.com/weaveworks/scope
Dynamic Flags via etcd / K8s, avoid restart, config files, tuning, chaos testing, load testing:
  - https://github.com/mwitkow/go-flagz
Kubernetes VM, Third Party Resource:
  - https://github.com/kubevirt/kubevirt
Kubernetes Chaos Testing:
  - https://github.com/asobti/kube-monkey
Kubernetes Deployments:
  - https://github.com/apprenda/kismatic
Chaos Testing Tool for Dockers:
  - https://github.com/gaia-adm/pumba
Code Generation, CMS & GO:
  - https://github.com/ponzu-cms/ponzu
Supervisor in golang:
  - caters to continuous deployment
  - http://immortal.run/about/
Network IO in golang:
  - https://morsmachine.dk/netpoller
Profiling with golang:
  - http://vinceyuan.github.io/profiling-memory-usage-of-a-go-app/
Monitoring with golang:
  - https://blog.dekstroza.io/monitoring-software-written-in-golang/
Containers vs. Zones vs. Jails:
  - https://blog.jessfraz.com/post/containers-zones-jails-vms/
Ansible like in golang:
  - https://github.com/pressly/sup
Ascii Mathematics:
  - https://github.com/asciimath/asciimathml
Accidental Complexity vs. Essential Complexity:
  - "Is it really 'Complex'? Or did we just make it 'Complicated'?"
Pretty Printing:
  - https://github.com/olekukonko/tablewriter
CLI over CLI:
  - Self documenting CLI programs from YAML
  - https://github.com/ahoy-cli/ahoy
ML Articles:
  - https://www.infoq.com/presentations/ml-predictability
  - https://www.infoq.com/presentations/score-ml-stock-market
Starting with GO: 
  - hello world -to- concurrent REST-ish server & self contained binary with std go libs:
    - http://howistart.org/posts/go/1/
    - https://github.com/peterbourgon/how-i-start-go
Git Currying:
  - https://github.com/kamranahmedse/git-standup
Crash course in Docker Networking:
  - https://medium.com/@loginoff/debugging-a-docker-heisenbug-in-production-586ccb265f7c
Consensus Algorithms:
  - http://rystsov.info/2017/02/15/simple-consensus.html
Kubernetes APIs:
  - http://k8s.uk/kubectl-vs-http-api.html
Go & Concurrency:
  - https://www.infoq.com/presentations/go-race-detector
Vagrant is too heavy:
  - https://github.com/lxdock/lxdock
Collect, Display & Report system metrics:
  - https://github.com/ostrost/ostent
Multi-Pod or Multi-Container-Within-A-Pod log tailing:
  - https://github.com/wercker/stern
Vagrant & OpenStack:
  - https://github.com/ggiamarchi/vagrant-openstack-provider
Transform CLI commands to HTTP endpoints:
  - https://github.com/adnanh/webhook
Compliment Ansible:
  - Ansible Tower, or
  - https://github.com/purpleidea/mgmt or
  - https://github.com/dmsimard/ara
UseCases on Vagrant & Ansible:
  - https://github.com/geerlingguy/ansible-vagrant-examples
Install, Upgrade & Manage Clusters of OpenShift:
  - https://github.com/openshift/openshift-ansible
Launch K8s on AWS via AMI & CLoudFormation:
  - https://github.com/weaveworks/kubernetes-ami
Overview of real-time metrics for multiple containers:
  - https://github.com/bcicen/ctop
Learn Ansible:
  - https://github.com/leucos/ansible-tuto
Open Source Alternative to Ansible Tower:
  - UI & GoLang & Anisble & CD
  - https://github.com/ansible-semaphore/semaphore
Monitor your system:
  - https://nicolargo.github.io/glances/
```

### Docker

```yaml
Container's PID 1
  - When the executable is container's PID 1 then it can receive Unix signals
  - Hence, container can receive a SIGTERM from docker stop <container>
```

### Kubelet

```yaml
Why kubelet:
  - Preserve node stability when available compute resources are low
Proactive Monitoring:
  - To prevent against total starvation of compute resource
  - Can terminate/evict one or more pods
Kubelet Supported Filesystems:
  - nodefs for volumes, daemon logs, etc
  - imagefs used by container runtime for storing images & container writable layers
  - kubelet does not care about any other filesystems
Eviction Signals:
  - memory.available, 
  - nodefs.available, nodefs.inodesFree, 
  - imagefs.available, imagefs.inodesFree
```
