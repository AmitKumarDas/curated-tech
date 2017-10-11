## Requirements in the wild

This is a curated list of technical `programming` requirements. You may be able to find 
these in stack overflow or Google it. However, this personally curated content helps me in 
focusing without sacrificing my time bound metrics. This should improve my productivity while 
implementing these in a larger scheme of things.

### System Essentials

```bash
# CPUs = Threads per core X cores per socket X sockets

$ lscpu
Architecture:          x86_64
CPU op-mode(s):        32-bit, 64-bit
Byte Order:            Little Endian
CPU(s):                4
On-line CPU(s) list:   0-3
Thread(s) per core:    2
Core(s) per socket:    2
Socket(s):             1
...
```

### Designs & Implementations

```yaml
Constainer Storage Interface:
 - https://github.com/codedellemc/gocsi
 - https://github.com/codedellemc/csi-blockdevices
MultiTenancy in K8s:
 - https://www.reddit.com/r/kubernetes/comments/70lovh/is_k8s_still_not_ready_for_multitenancy/
Disruptions in K8s:
 - https://kubernetes.io/docs/concepts/workloads/pods/disruptions/
RBAC in K8s:
 - https://blog.heptio.com/security-matters-rbac-in-kubernetes-e369b483c8d8
Services & Gory Details of VIPs:
 - As of Kubernetes v1.0, Services are a “layer 4” (TCP/UDP over IP) construct
 - In Kubernetes v1.1 the Ingress API was added (beta) to represent “layer 7” (HTTP) services.
 - https://kubernetes.io/docs/concepts/services-networking/service/
Realtime Linting for K8s:
 - https://github.com/uswitch/klint
Chaos Monkey For K8s:
 - https://github.com/asobti/kube-monkey
Auto Scale:
 - https://github.com/kubernetes/autoscaler
Hunt for re-usable code / test-code that is simple k8s/e2e:
 https://github.com/kubernetes/kubernetes/blob/master/test/e2e/kubectl/kubectl.go
  - Run kubectl
  - Read YAML & emit bytes
  - Retry kubectl
 https://github.com/kubernetes/kubernetes/blob/master/test/e2e/e2e.go:
  - 3rd Party Libs:
   - Run tests using Ginkgo runner
   - If a "report directory" is specified, Unit test reports will be generated in this directory
  - Config:
   - Check configuration parameters (via flags)
  - POD related:
   - PARSE a pod.yaml & create a K8s Pod programmatically
   - Wait for POD to be READY based on a TIMEOUT
   - Create & TEAR Down using Go's defer func() {}
  - Cloud Provider Abstraction:
   - Check if AWS or GCE
  - Inference:
   - Metrics Grabber
   - Write JSON formatted metrics into a file
   - Human Readable Print
   - Dump the pod LOGS on your terminal
  - Non Functional
   - Disable SKIPPED tests  
 https://github.com/kubernetes/kubernetes/blob/master/test/e2e/apimachinery/etcd_failure.go
  - SSH Access
  - Disruptive:
   - Network Partition
   - SIGKILL
 https://github.com/kubernetes/kubernetes/blob/master/test/e2e/apimachinery/garbage_collector.go
  - Parallelism via Cron Jobs
 https://github.com/kubernetes/kubernetes/blob/master/test/e2e/apimachinery/table_conversion.go
  - Test the display table on terminal
 https://github.com/kubernetes/kubernetes/blob/master/test/e2e/chaosmonkey/chaosmonkey.go
  - Semaphore, Ready, Stop, Done, Channel, Wait
  - Ready for disruption to start
  - Singal that disruption is done
  - Wait for all tests to return
  - Do not wait if tests panic
  - Register a set of tests that should run during the disruption
Test K8s Programatically:
 - https://blog.heptio.com/straighten-out-your-kubernetes-client-go-dependencies-heptioprotip-8baeed46fe7d
Third Party Resources vs Custom Resource Definition:
 - https://github.com/metral/memhog-operator
Learn To Write Minimal K8s Code:
 - https://github.com/kubernetes/kubernetes/blob/master/plugin/pkg/admission/defaulttolerationseconds/admission.go
 - https://github.com/kubernetes/kubernetes/tree/master/plugin/pkg/admission
K8s Node Monitoring:
 - https://kubernetes.io/docs/concepts/architecture/nodes/
Additional runtime requirements for a Pod:
 - https://github.com/kubernetes-incubator/service-catalog/pull/1094/files#diff-b4ff134a727ba336b42671611bc2e6d1
Data Pipelines Using Docker & K8s:
 - https://github.com/pachyderm/pachyderm
Polling to Pod Lifecycle Event Watcher:
 - https://github.com/kubernetes/community/blob/master/contributors/design-proposals/pod-lifecycle-event-generator.md
Placement, Scheduling, Rescheduling, Spread, Priority, Controlled:
 - Reference Links:
  - https://kubernetes.io/docs/admin/admission-controllers/ & Navigate to Pod Node Selector
  - https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#per-pod-configurable-eviction-behavior-when-there-are-node-problems-alpha-feature
  - https://stackoverflow.com/questions/41644755/whats-the-purpose-of-kubernetes-daemonset-when-replication-controllers-have-nod
  - https://github.com/kubernetes/kubernetes/issues/11470
  - https://github.com/kubernetes/kubernetes/issues/41584
  - https://github.com/kubernetes/community/blob/master/contributors/design-proposals/podaffinity.md
  - https://github.com/kubernetes/community/blob/master/contributors/design-proposals/node-allocatable.md
  - https://github.com/kubernetes/community/blob/master/contributors/design-proposals/pod-priority-api.md
  - https://github.com/kubernetes/community/blob/master/contributors/design-proposals/resource-qos.md
  - https://github.com/kubernetes/kubernetes/issues/28928
  - https://github.com/kubernetes/community/blob/master/contributors/design-proposals/rescheduling.md
  - https://github.com/kubernetes/community/blob/master/contributors/design-proposals/rescheduler.md
  - https://kubernetes.io/docs/concepts/configuration/assign-pod-node/
  - https://kubernetes.io/docs/admin/multiple-zones/
  - https://kubernetes.io/docs/tasks/administer-cluster/guaranteed-scheduling-critical-addon-pods/
  - https://github.com/kubernetes/community/blob/master/contributors/design-proposals/taint-toleration-dedicated.md
Reclaim Policy & K8s:
 - https://github.com/kubernetes/kubernetes/issues/38192
Protobuf, gRPC Challenges:
Device Manager Proposal:
 - https://github.com/RenaudWasTaken/community/blob/f1ce8db07c68dc114338b94f581849b46facd7ae/contributors/design-proposals/device-plugin.md
K8s & Local Storage:
 - https://github.com/kubernetes/community/blob/master/contributors/design-proposals/local-storage-overview.md
K8s & Local Storage & Placements:
 - Since local PVs are only accessible from specific nodes:
  - scheduler needs to take into account a PV's node constraint when placing pods.
  - This can be generalized to a storage toplogy constraint:
   - which can also work with zones, and in the future: racks, clusters, etc.
Multi-Tenancy Around Storage:
 - https://github.com/kubernetes/kubernetes/issues/47326
Assigning PODs to Nodes:
 - https://kubernetes.io/docs/tasks/configure-pod-container/assign-pods-nodes/
Taints, Tolerations & Daemon Set:
 - https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#taints-and-tolerations-beta-feature
Cloud Services && Monitoring:
 - https://sysdig.com/blog/introducing-sysdig-teams/
On top of etcd, IP, leasing:
 - https://github.com/lclarkmichalek/etcdhcp
K8s & Watch & Security & Versioning:
 - https://github.com/kubernetes/community/blob/master/contributors/design-proposals/bulk_watch.md
Security Context & Pod:
 - https://kubernetes.io/docs/tasks/configure-pod-container/security-context/
Certificates:
 - https://blog.giantswarm.io/issuing-certs-for-kubernetes-with-cert-operator-using-vault-and-operatorkit/
Secure & Simple Stuff:
 - https://github.com/Boostport/kubernetes-vault
Cloud Services & Security or CLI & Security:
 - Temporary Certificates
 - https://developer.atlassian.com/blog/2017/07/kubetoken/
Kubernetes Extensibility:
 - Kubernetes 1.7 has a new features: 
  - API aggregation at runtime
  - Enables power users to add Kubernetes-style:
   - pre-built, 
   - third party or 
   - user-created application programming interfaces (API)s to their cluster
 - https://kubernetes.io/docs/concepts/api-extension/apiserver-aggregation/
```

### Storage Testing

```yaml
Local PVs & K8s:
 - local PVs can only provide semi-persistence
 - are only suitable for specific use cases that need:
  - performance, data gravity and can tolerate data loss
 - If the node or PV fails, then:
  - either the pod cannot run, or
  - the pod has to give up on the local PV and find a new one.
 - Failure scenarios can be handled by:
  - unbinding the PVC from the local PV, and forcing the pod to reschedule and find a new PV.
 Debugging / Tracing:
  - https://sysdig.com/blog/container-isolation-gone-wrong/
  - https://sysdig.com/blog/monitoring-docker-kubernetes-wayblazer/
  - https://github.com/iovisor/bcc
  - https://qmonnet.github.io/whirl-offload/2016/09/01/dive-into-bpf/
```


### UI / CLI

```yaml
Inline CSS at Khan Academy:
 - http://engineering.khanacademy.org/posts/aphrodite-inline-css.htm
CSS in JS:
  - https://medium.freecodecamp.com/
  - css-in-javascript-the-future-of-component-based-styling
K8s & UI:
 - https://netsil.com/blog/kubernetes-monitoring-needs-maps/
Making CLI Cool:
 - https://github.com/cloudnativelabs/kube-shell
 - https://github.com/c-bata/kube-prompt
```

### Git Snips

```yaml
Development Workflow:
 - https://github.com/istio/istio/blob/master/devel/README.md
Just Enough:
  - http://zabanaa.github.io/notes/mind-blowing-git-tips.html
Rename Branch:
 - https://multiplestates.wordpress.com/2015/02/05/rename-a-local-and-remote-branch-in-git/
```

### CI Snips

```yaml
CI In the Age of Containers & K8s:
  - https://github.com/kubernetes/ingress/tree/master/examples
More in CI in Age of Containers & K8s:
  - https://github.com/coreos/alb-ingress-controller/tree/master/examples
  - https://github.com/coreos/alb-ingress-controller/blob/master/main.go
  - https://github.com/coreos/alb-ingress-controller/blob/master/Makefile
  - https://github.com/coreos/alb-ingress-controller/blob/master/Dockerfile
More:
  - https://github.com/kubernetes/dns/tree/master/pkg/e2e
```

### Sample CURLs to K8s API Server

```bash
TOKEN="$(cat /var/run/secrets/kubernetes.io/serviceaccount/token)"

curl --cacert /var/run/secrets/kubernetes.io/serviceaccount/ca.crt \
    -H "Authorization: Bearer $TOKEN" \
    -H "Content-Type: application/yaml" \
    -XPOST -d"$(cat controller.yaml)" \
    "https://172.28.128.3:6443/apis/extensions/v1beta1/namespaces/default/deployments"

curl --cacert /var/run/secrets/kubernetes.io/serviceaccount/ca.crt \
    -H "Authorization: Bearer $TOKEN" \
    "https://172.28.128.3:6443/apis/extensions/v1beta1/namespaces/default/deployments"

curl --cacert /var/run/secrets/kubernetes.io/serviceaccount/ca.crt \
    -H "Authorization: Bearer $TOKEN" \
    "https://172.28.128.3:6443/api/v1/namespaces/default/services"

curl --cacert /var/run/secrets/kubernetes.io/serviceaccount/ca.crt \
    -H "Authorization: Bearer $TOKEN" \
    -H "Content-Type: application/yaml" \
    -XPOST -d"$(cat service.yaml)" \
    "https://172.28.128.3:6443/api/v1/namespaces/default/services"

curl -X DELETE \
  localhost:8001/apis/extensions/v1beta1/namespaces/default/deployments/nginx-deployment \
  -d '{"kind":"DeleteOptions","apiVersion":"v1","propagationPolicy":"Background"}' \
  -H "Content-Type: application/json"
```

### K8s Beginners

```yaml
Manage K8s:
 - https://github.com/kris-nova/kubicorn
Selector vs. Labels:
 - selector: "color=green, env=test, service=front"
 - labels:   map[string]string{"color": "green", "env": "test", "service": "front"}
Labels & Selector:
 - a label:
  - toplogyKey: kubernetes.io/hostname
 - somewhere else:
  - nodeSelectorTerms:
   - matchExpressions:
    - key: kubernetes.io/hostname
      operator: In
      values:
      - node-1
Metrics Code Snips:
 - https://github.com/kubernetes/kubernetes/pull/49117/commits/7e76c17e9ae3789cfa04c3ab8f927393232b6a91
Templating:
 - https://blog.openshift.com/kubernetes-state-app-templating/
K8s 1.7 Release:
 - http://www.zdnet.com/article/kubernetes-1-7-released/
If K8s && DNS:
 - then hostname / fqdn will be <servicename>.<namespace>.svc.cluster.local
K8s on Google Container Engine:
 - https://github.com/openebs/openebs/blob/master/k8s/hyperconverged/tutorial-configure-openebs-gke.md
Get the Basics Right:
 - https://hackernoon.com/the-curious-case-of-pid-namespaces-1ce86b6bc900
Rule Snips:
 - "rules": [
  - {"apiGroups": ["*"], "resources": ["*"], "verbs": ["*"]},
  - {"nonResourceURLs": ["*"], "verbs": ["*"]}
 - ]
Thoughts on Rule:
 - Look at nonResourceURLs
Kubectl Cheatsheet:
 - https://kubernetes.io/docs/user-guide/kubectl-cheatsheet/
Access Kubernetes API:
 - The easiest way is to configure a proxy
 - Luckily, kubectl comes with an option to configure it
  - kubectl proxy --port=8000
Athorization Role, ClusterRole & RoleBinding:
 - https://kubernetes.io/docs/admin/authorization/rbac/
 - Understand by looking at existing:
  - Role & RoleBinding
  - ClusterRole & ClusterRoleBinding
Flow:
 - User creates a Pod via the API Server and the API server writes it to etcd.
 - Scheduler notices an “unbound” Pod and decides which node to run that Pod on. 
  - It writes that binding back to the API Server.
 - Kubelet notices a change in the set of Pods that are bound to its node. 
  - It, in turn, runs the container via the container runtime (i.e. Docker).
 - Kubelet monitors the status of the Pod via the container runtime. 
  - As things change, the Kubelet will reflect the current status back to the API Server.
Watches in etcd:
 - Watch in etcd is critical for how Kubernetes works.
 - Allows clients to perform subscription for changes to parts of the key namespace.
 - Used as a coordination mechanism between components of the distributed system. 
 - One component can write to etcd and other componenents can immediately react to that change.
 - The common pattern is for clients to mirror a subset of the database in memory:
  - Then react to changes of that database
 - Watches are used as an efficient mechanism to keep that cache up to date. 
 - If the watch fails, client can fall back to polling:
  - At the cost of increased load, network traffic and latency.
Streamlined K8s Development:
 - label: CI
 - https://github.com/Azure/draft
K8s Users:
 - 2 user categories - managed outside K8s or managed by K8s API
 - Normal Users:
  - admin distributing private keys
  - use store e.g. Keystone, Google Accounts
  - file having username & passwords
  - are for humans
 - Service Accounts / Managed by K8s API:
  - are bound to namespaces
  - created automatically or manually by K8s API
  - are tied to a set of credentials stored as Secrets
  - secrets are mounted into pods
  - are for processes which run in pod
 - API requests can be tied with:
  - normal users or service accounts or treated as anonymous requests
Human ACCESS to cluster (e.g. using kubectl):
 - You are authenticated by the apiserver as a particular User Account
 - More authentication stuff at:
  - https://kubernetes.io/docs/user-guide/kubectl-cheatsheet/
Processes in containers inside pods contacting the apiserver:
 - When they do, they are authenticated as a particular Service Account 
 - (e.g. default).
K8s Authentication strategies:
 - client certificates
 - bearer tokens
 - authentication proxy
 - HTTP basic auth
K8s Certs Authentication:
 - is enabled by passing --client-ca-file=SOMEFILE option to API server
K8s Static Bearer Token File based Authentication:
 - is enabled by passing --token-auth-file=$SOMEFILE
 - token file is a .csv file with token, username & uid columns
 - using bearer token in http client:
  - Authorization header with a value of Bearer actual-token-value
  - e.g. Authorization: Bearer 31ada4fd-adec-460c-809a-9e56ceb75269
K8s Static Password File based Authentication:
 - enabled by passing --basic-auth-file=SOMEFILE
 - basic auth file:
  - password, username, uid, "group1, group2"
 - basic auth from http client
  - Authorization: Basic BASE64ENCODED(USER:PASSWORD)
K8s Service Account Token based Authentication:
 - enabled by passing:
  - --service-account-key-file
  - --service-account-lookup
 - username is system:serviceaccount:(NAMESPACE):(SERVICEACCOUNT)
 - groups are system:serviceaccounts and system:serviceaccounts:(NAMESPACE)
Things to Note about Service Accounts:
 - Service accounts are stored as secrets
 - Any user with read access to those secrets can authenticate as the service account
 - Be careful granting permissions to service accounts & read capabilities for secrets
Anonymous Requests:
 - username is system:anonymous
 - group is system:unauthenticated
K8s Authentication in 1.6 and above:
  - anonymous access is default
  - anonymous access can be disabled by passing the --anonymous-auth=false
  - ABAC & RBAC authorizers require explicit authorization of below:
   - system:anonymous user or
   - system:unauthenticated group
Infra-As-Code:
 - https://github.com/ksonnet
CRI:
 - http://blog.kubernetes.io/2016/12/container-runtime-interface-cri-in-kubernetes.html
Version Promotion:
 - A group first appears as v1alpha1 &
 - is then promoted to v1beta1 &
 - finally graduates to v1
K8s Client-GO & Versioning:
 - Types are managed at github.com/kubernetes/client-go/blob/v2.0.0/pkg/api
 - Others are managed at github.com/kubernetes/client-go/blob/v2.0.0/kubernetes/
Network Policy Controller:
 - https://cloudnativelabs.github.io/post/2017-05-1-kube-network-policies/
Distributed Load Balancer, Firewall & Router:
 - Kube Router vs. Kube Proxy
 - Kube Router vs. Flannel, Calico, Weave, etc
 - operational simplicity
 - combines all networking functionality available at the node into one cohesive solution
 - https://github.com/cloudnativelabs/kube-router
 - cross node pod to pod connectivity
 - service discovery & load balancing
K8s Operator's POV:
 - https://carlosbecker.com/posts/k8s-sandbox-costs/
Kubernetes Services:
  - An abstraction for pods, providing a stable, virtual IP (VIP) address. 
  - As pods may come and go, services allow clients to reliably connect to pod containers using VIP. 
  - Multiple pods running across multiple nodes of the cluster can be exposed as a service.
  - Service has an IP address and port combination along with a name:
    - Refer to a service either its IP address or its name
  - VIP:
    - Not an actual IP address connected to a network interface 
    - Its purpose is to forward traffic to one or more pods. 
    - Up-to-date mapping between the VIP and the pods is the job of kube-proxy
Expose Services in Various Forms:
  - Internal, External & Load Balanced
How VIP forwards the traffic:
  - via IPTables
  - kube-proxy adds rules to the IPTables
  - i.e. TCP traffic to VIP:port should be forwarded ContainerIP:port
  - sudo iptables-save | grep <servicename>
Kube-Proxy:
  - A process that runs on every node, which queries the API server 
  - Learns about new services in the cluster
ConfigMap, Environment Variables:
  - https://crondev.com/kubernetes-environment-file/
  - Kubernetes Downward API, which lets us pass environment variables into the containers
All you Wanted to Know about K8s:
  - https://github.com/hobby-kube/guide
hostPort:
  - expose a specific port on the host
StatefulSet:
  - Assigns persistent DNS names to pods
  - Allows to re-attach storage volume to another machine where the pod migrated to
  - A headless service that points to each member of pod:
    - It does not create cluster IP for load balancing
    - i.e. it does not have ClusterIP & NodePort specified
    - Its purpose is to trigger DNS name creation for PODs that will be launched
  - PODs are launched strictly one after the other
service.alpha.kubernetes.io/tolerate-unready-endpoints "true":
  - creates the pod endpoint ignoring its state
  - create the endpoint even if service is not healthy
  - e.g. MongoDB replica sets want to reach each other even before initialization
pod.alpha.kubernetes.io/init-containers: 
  - state the containers that need to be started before the main pod
```

### Ansible Snips

```yaml
To use Ansible:
  - PYTHONPATH must be set to include the lib and lib64 directories
  - PYTHONPATH=<>/lib/python2.7/site-packages/:<>/lib64/python2.7/site-packages bin/ansible
ANSIBLE_CONFIG:
  - /etc/ansible/ansible.cfg
  - INI format
pipelining=True:
  - Reduces the number of SSH operations to execute a module on the remote server
```

### Vagrant Verbose

```ruby
# Array of Structs
boxes = [
  {
    :name => "master01",
    :mem => "1024"
  },
  {
    :name => "worker01",
    :mem => "1024"
  }
]

# Add SSH Public Key to the Node
config.vm.provision "shell" do |s|
  ssh_pub_key = File.readlines("#{Dir.home}/.ssh/kismaticuser.key.pub").first.strip
  s.inline = <<-SHELL
    mkdir -p /root/.ssh
    echo #{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys
    echo #{ssh_pub_key} >> /root/.ssh/authorized_keys
  SHELL
end
```

### Vagrant Snips

```yaml
Vagrant && Insert SSH Key:
  - config.ssh.insert_key = true is default
  - Vagrant automatically inserts a keypair to use for SSH
  - Replaces Vagrant's default insecure key inside the machine if detected.
Turn off shared folders:
  - config.vm.synced_folder ".", "/vagrant", id: "vagrant-root", disabled: true
CentOS & Networking:
  - config.vm.network :private_network, ip: 1.1.12.1
  - config.vm.provision "shell", inline: "nmcli connection reload; systemctl restart network.service"
```

### Shell Snips

```yaml
Fixes to Permission related Issues:
  - Permission Denied:
  - Command Not Found:
  - Operation Not Permitted:
    - Run the command with sudo
    - chmod + x path/to/bin
uname -s:
  - Linux
uname -m:
  - x86_64
Using uname:
  - $ curl -L "../releases/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
IPv4 Addr for a Network Device Named eth0:
  - ip -4 addr show scope global dev eth0 | grep inet | awk '{print \$2}' | cut -d / -f 1
Interpolation:
  - ETCD_NAME=${1:-"default"}
  - ETCD_LISTEN_IP=${2:-"0.0.0.0"}
  - ETCD_INITIAL_CLUSTER=${3:-}
Variable:
  - etcd_data_dir=/var/lib/etcd
  - mkdir -p ${etcd_data_dir}
Error Handling:
  - http://linuxcommand.org/lc3_wss0140.php
Which Program/Script:
  - PROGNAME=$(basename $0)
Line Number an Environment Variable:
  - $LINENO
  - helpful in getting the error prone line number
If Undefined Then Substitute:
  - ${1:-"Unknown Error"}
Interpolation, Nested Interpolation:
  - echo "${PROGNAME}: ${1:-"Unknown Error"}" 1>&2
OR Operator:
  - Avoid IF ELSE e.g. if [ "$?" = "0" ]; then ...
  - Functional Programming
  - Refer - http://linuxcommand.org/lc3_wss0140.php
  - cd $some_directory || error_exit "Cannot change directory! Aborting"
Execute a Command On Receiving Some Signals:
  - trap "echo "bye..bye..."; exit" SIGHUP SIGINT SIGTERM
SIGKILL As The Last Resort:
  - kill -9
  - Does not provide any chance for the program to execute cleanup actions
  - Can lead to locks
Prefer temp file at user s local tmp:
  - if [ -d "~/tmp" ]; then
  -  TEMP_DIR=~/tmp
  - else
  -  TEMP_DIR=/tmp
  - fi
Resistance to RACE attacks:
  - TEMP_FILE=$TEMP_DIR/printfile.$$.$RANDOM
Know What the Script Is Doing:
  - Turn on tracing at the beginning set -x
  - Turn off tracing at the end set +x
```

### Go GoLang Snips

```yaml
os exec utility:
 - https://github.com/kubernetes/test-infra/blob/master/kubetest/util.go
So where do you point your GOPATH? 
 - A common preference:
  - GOPATH=$HOME
  - Since $HOME/bin is already on your PATH
Simple Yet Effective Code Organization:
 - https://github.com/boltdb/bolt
 - https://talks.golang.org/2014/organizeio.slide
Closures & Handlers go hand-in-hand:
 - https://gist.github.com/tsenart/5fc18c659814c078378d
When in DOUBT about Go:
 - https://github.com/golang/go/wiki/whygo
Nil Checks:
 - ref - https://golang.org/ref/spec#The_zero_value
 - When storage is allocated for a variable, either through a declaration or a call of new, or 
 - when a new value is created, either through a composite literal or a call of make, and 
 - no explicit initialization is provided, 
  - the variable or value is given a default value. 
 - Each element of such a variable or value is set to the zero value for its type: 
  - false for booleans, 
  - 0 for integers, 
  - 0.0 for floats, 
  - "" for strings, and 
  - nil for pointers, functions, interfaces, slices, channels, and maps. 
 - This initialization is done recursively, 
  - so for instance each element of an array of structs will have its fields zeroed if no value is specified.
Best of Designs from GoLang Gurus:
 - https://blog.golang.org/index
Lock Free:
 - https://texlution.com/post/golang-lock-free-values-with-atomic-value/
Nil vs. Empty:
  - var nilSlice []int
  - emptySlice1 := make([]int, 0)
  - emptySlice2 := []int{}
  - fmt.Println("nilSlice == nil)    // true
  - fmt.Println("emptySlice1 == nil) // false
  - fmt.Println("emptySlice2 == nil) // false
  - As for all three slices len and cap are 0
Advanced Items in Simple ways:
 - https://blog.bluematador.com/posts/golang-pros-cons-for-devops-part-1-goroutines-panics-errors/
GoLang DataStructure Pretty Printer:
  - https://github.com/davecgh/go-spew/
Semantic Versioning Lib:
  - https://github.com/blang/semver
Resuable Building Blocks:
  - SDK, Lib Kind of Development:
  - github.com/kubernetes/kubernetes/tree/master/pkg/util
  - https://github.com/kubernetes/kubernetes/tree/master/pkg/ssh
  - https://github.com/coreos/pkg
  - https://github.com/kubernetes/kops/tree/master/util/pkg
IPTables & GoLang:
  - github.com/suedadam/GoWall
  - github.com/coreos/go-iptables
  - github.com/kubernetes/kubernetes/tree/master/pkg/util/iptables
Config from CLI or Config With Sync from K8s ConfigMap:
  - Adapts to ConfigMap updates
  - https://github.com/kubernetes/dns/tree/master/pkg/dns/config
State of GoLang Vendoring:
  - https://github.com/kubernetes/client-go/blob/master/INSTALL.md
Server Beyond a HTTP Server:
  - https://github.com/coreos/etcd/blob/master/etcdserver/server.go
Simplest Main, Flag, Log, Config, Http Serve, Metrics, Initialization:
  - github.com/coreos/alb-ingress-controller/blob/master/main.go
A GoLang Logging Utility:
  - https://github.com/coreos/pkg/tree/master/capnslog
Etcd Metrics & Prometheus:
  - https://github.com/coreos/etcd/blob/master/etcdserver/metrics.go
Cluster Discovery:
  - Refer discovery package of etcd
  - An implementation of DNS discovery
Auth, Role, Users, Permission:
  - Refer auth package of etcd
New vs Make:
  - fmt.Printf("%T  %v\n", new([10]int), new([10]int))
    - *[10]int  &[0 0 0 0 0 0 0 0 0 0]
  - fmt.Printf("%T  %v\n", make([]int, 10), make([]int, 10))   
    - []int  [0 0 0 0 0 0 0 0 0 0]
Type vs. Value Printing:
  - i := 10
  - s := strconv.Itoa(i)
  - fmt.Printf("%T, %v\n", s, s) will o/p string, 10
Composing Interfaces, Private Struct, Public Initializer, Naming of Initializer:
  - func (d *dumper) Dump(xyz) abc
  - type dumper struct composed of w io.Writer
  - func Dumper(w io.Writer) io.WriteCloser returns &dumper{w: w}
Templating:
  - import text/template    
  - strtmpl string, obj interface{}
  - tmpl, err := template.New("template").Parse(strtmpl)
  - err = tmpl.Execute(&buf, obj)
Forced Compilation Check:
  - // ensure kubeletVolumeHost implements VolumeHost interface
  - var _ volume.VolumeHost = &kubeletVolumeHost{}  
```

### Docker Verbose

```bash
# --install-option flag tells pip to install ansible package to /ansible
# The /ansible directory is Container volume that maps to Host's $(pwd)/out

docker run --rm -v $(pwd)/out:/ansible apprenda/vendor-ansible \
    pip install --install-option="--prefix=/ansible" ansible==2.1.2.0

```

### Docker Snips

```yaml
Connect Dockers On Multiple Hosts:
 - https://goldmann.pl/blog/2014/01/21/connecting-docker-containers-on-multiple-hosts/
DOcker With OpenVSwitch:
 - http://fbevmware.blogspot.in/2013/12/coupling-docker-and-open-vswitch.html
Docker & Whats Ephemeral Storage:
  - Docker containers use their root disk as ephemeral storage
  - A chunk of disk space from the host filesystem which runs the container
  - This disk space can’t be shared with other processes, nor easily mgrated to a new host.
Docker & Whats Docker Volume:
  - Allows to run a container with dedicated volume mounted.
  - This volume comprises another chunk of space from a host machine
  - This is persistent and independent from container lifecycle
  - It can be Network Storage, or Shared Filesystem Mount:
    - Depends on the storage plugin being used
Remove all Exited Containers:
  - sudo docker rm $(sudo docker ps -a -q -f "status=exited*")
Remove all Images:
  - sudo docker rmi $(sudo docker images -q)
Stop all Containers:
  - sudo docker stop $(sudo docker ps -a -q)
Remove all Containers & its Associated Volumes:
  - sudo docker rm -v $(sudo docker ps -a -q)
Using Docker to Store Vendored Packages:
  - github.com/apprenda/kismatic/tree/master/vendor-ansible#using-docker-to-create-vendored-package
Expose Port 80 of Container Without Publishing Port to the Host system’s Interfaces:
  - docker run --expose 80 ubuntu bash
Bind Port 8080 of Container to Port 80 on 127.0.0.1 of Host Machine:
  - docker run -p 127.0.0.1:80:8080 ubuntu bash
Connect Container to a Network:
  - docker run -itd --network=my-net busybox
Choose an IP and Network:
  - docker run -itd --network=my-net --ip=10.10.9.75 busybox
Add A Host into a Container s /etc/hosts:
  - docker run --add-host=dockerize:10.20.1.2 --rm -it debian
  - Login to Above Container:
    - root@f38c87f2a42d:/# ping dockerize
```

### Dockerfile

```yaml
Wrong vs. Right:
  - Does not work RUN [ "echo", "$HOME" ]
  - Works RUN [ "sh", "-c", "echo $HOME" ]
Wrong vs. Right:
  - Does not work CMD ["echo", "$HOME"]
  - Works CMD ["sh", "-c", "echo $HOME"]
Build Image using Current Directory as Context:
  - docker build .
Build Directory Best Practices:
  - Start with an empty directory that will host the Dockerfile
  - All the other files will form the context to this image building process
Tagging Image into Multiple Repositories:
  - docker build -t openebs/jiva:1.2.2 -t openebs/jiva:latest .
Running Dockerfile Instructions during Build Process:
  - Each run of instruction is independent
  - Result of each instruction is commited to tbe newly built image
Instruction Naming Practices:
  - They are UPPERCASE by convention
  - FROM, COPY, ENV, WORKDIR, ADD, COPY, EXPOSE, LABEL, USER, VOLUME, STOPSIGNAL, ONBUILD, RUN
Parser Directives:
  - Special type of comment in the form # directive=value
  - e.g.
  - # escape=\
  - # escape=`
Default Escape Character:
  - \
  - \$foo translates to $foo
  - \${foo} translates to ${foo}
RUN <command>:
  - runs a shell with /bin/sh -c on Linux
  - runs a shell with cmd /S /C -c on Windows
RUN ["executable", "param1", "param2"]:
  - runs in exec form
  - e.g. RUN ["/bin/bash", "-c", "echo hello"]
RUN vs CMD:
  - RUN runs a command & commits the result into the image
  - CMD does not execute anything at build time
  - CMD is used along with ENTRYPOINT
LABEL <key>=<value> <key>=<value> <key>=<value> ...:
  - Adds metadata to an image
  - An image's label(s) can be inspected via docker inspect command
EXPOSE:
  - This tells Docker to let container listen on the specified network port(s) at runtime
  - This does not expose the port(s) of container accessible to the host
ENV:
  - Can be set on each line
  - Can be set together
  - Setting multiple k=v together is preferred as it produces a single cache layer
  - These values will persist when when a container is run from resulting image
  - View these using docker inspect command
  - Change them using docker run --env <key>=<val>
  - To set for a single command RUN <key>=<value> <command>
  - Can cause unexpected side effects
ENTRYPOINT:
  - Has two forms
  - ENTRYPOINT ["executable", "param1", "param2"]
  - ENTRYPOINT command param1 param2
  - e.g. docker run -i -t --rm -p 80:80 nginx
  - CLI args to docker run <image> will be appended after all elements in an exec form ENTRYPOINT
  - Above will also override all elements specified using CMD
ENTRYPOINT vs. CMD:
  - fairly stable default commands are set in ENTRYPOINT
  - additional defaults that are likely to change in CMD
  - When you run the container the ENTRYPOINT executable is the ONLY PROCESS RUNNING
  - Verify via docker run -it --rm --name test top -H
  - Alternatively verify later via docker exec -it test ps aux
ENTRYPOINT vs. CMD:
  - ENTRYPOINT should be defined when using the container as executable
  - CMD can be used to set default args for ENTRYPOINT command
  - CMD can be used to run ad-hoc commands in a container
  - CMD will be overridden when running the container with alternative arguments
VOLUME:
  - Creates a mountpoint with the specified name
```

### Design Snips

```yaml
Aggregator Interface:
 - Composes a list of interfaces:
 - Each interface specializing in their work areas
Getter Interface:
 - Has a GetXXX contract that returns Aggregator interface
A struct for Aggregator Interface:
 - will implement above aggregator interface
A struct that embeds Interface:
 - Interface is refered as a struct property
 - This enables dynamic injection of this interface
- Aggregator References:
 - https://github.com/kubernetes/client-go/
 - blob/v2.0.0/kubernetes/typed/core/v1/core_client.go
```

### References

- https://github.com/apprenda/kismatic/
