
> My thoughts on K8s

### K8s Persistent Volumes

- K8s **emptyDir** Volume
  - An emptyDir volume is **first created** when a Pod is assigned to a Node, and exists as long as that Pod is running on that node. 
  - It is initially empty
  - Volume can be mounted at the same or different paths in each container in the pod
  - When a Pod is removed from a node for any reason, the data in the emptyDir is **deleted forever**
  - When emptyDir.medium field to "Memory" to tell Kubernetes to mount a tmpfs (RAM-backed filesystem)

- K8s **hostPath** Volume
  - Mounts a file or directory **existing** from the host node’s filesystem into your pod
  - usage: running a container that needs access to Docker internals; use a hostPath of /var/lib/docker
  - usage: running cAdvisor in a container; use a hostPath of /dev/cgroups
  - `DISADVANTAGES`:
    - When K8s adds resource-aware scheduling, as is planned, it will not be able to account for resources used by a hostPath
    - Either need to **run process as root** in a privileged container _`or`_
    - **Modify the file permissions** on the host to be able to write

- K8s **gcePersistentDisk** Volume
  - Contents of a PD are preserved and the volume is merely unmounted. 
  - A PD can be pre-populated with data, and that data can be “handed off” between pods.
  - `DISADVANTAGES`:
    - Need to create the PD separately using gce cli etc
    - The nodes on which pods are running must be GCE VMs
    - Those VMs need to be in the same GCE project and zone as the PD

- K8s **awsElasticBlockStore** Volume
  - DISADVANTAGES
    - Need to create the volume separatehy using aws cli etc
    - The nodes on which pods are running must be AWS EC2 instances
    - Those instances need to be in the same region and availability-zone as the EBS volume
    - EBS only supports a single EC2 instance mounting a volume

- K8s **gitRepo** Volume
  -  It mounts an empty directory and clones a git repository into it for your pod to use.

- K8s **secret** Volume
  - A secret volume is used to pass sensitive information, such as passwords, to pods.
  - You can store secrets in the Kubernetes API and mount them as files for use by pods. 
  - Secret volumes are backed by tmpfs (a RAM-backed filesystem) so they are never written to non-volatile storage.

- K8s **persistentVolumeClaim** Volume
  - Is used to mount a PersistentVolume into a pod
  - Used to claim durable storage without knowing the details of a particular environment

### Dynamic storage provisioning via StorageClass & PVC

- Intent, Yaml or Config file references:
  - https://docs.openshift.org/latest/install_config/persistent_storage/dynamically_provisioning_pvs.html

- Operator commands - via - oc
  - https://docs.openshift.org/latest/install_config/storage_examples/storage_classes_dynamic_provisioning.html#install-config-storage-examples-storage-classes-dynamic-provisioning

- Operator commands - via- kubectl
  - https://kubernetes.io/docs/tasks/configure-pod-container/configure-persistent-volume-storage/
