```yaml
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
Json, Hcl & GoLang Structs
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
