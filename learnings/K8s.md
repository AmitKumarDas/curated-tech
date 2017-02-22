
> My thoughts on K8s

## Design Doc

- https://github.com/kubernetes/community/blob/master/contributors/design-proposals/persistent-storage.md

## Troubleshoot

### Volume Plugin Error

- https://github.com/kubernetes/kubernetes/issues/16585
  - Was the volume plugin installed ?
  - How to install the volume plugin ?
  - kubelet logs ?
  - kube-controller-manager logs ?
  - What is the verbose mode you are operating at ?
    - --v=4 for any kube-* component

## Theory

### Build docker image

```yaml
File:
  - build/make-build-image.sh
```

### cluster ubuntu essentials

```yaml
Config:
  - ubuntu/config-default.sh
 
```

### kubeadm essentials

```yaml
Configs:
  - /etc/systemd/system/kubelet.service.d/10-kubeadm.conf
Notes:
  - cloudprovider integrations:
    - --cloud-provider must be set for all kubelets in the cluster
  - kubeReleaseBucketURL:
    - https://storage.googleapis.com/kubernetes-release/release
  - clientcmd pkg
    - builds a working client from:
      - a fixed config,
      - a .kubeconfig file,
      - command line flags, or
      - from any merged combination
```

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

- Access Control - via - GID
  - https://kubernetes.io/docs/tasks/configure-pod-container/configure-persistent-volume-storage/

### Config Map

Many applications require configuration via some combination of config files, command line arguments, and environment variables. These configuration artifacts should be decoupled from image content in order to keep containerized applications portable. The ConfigMap API resource provides mechanisms to inject containers with configuration data while keeping containers agnostic of Kubernetes.

### Flexvolume

Flexvolume enables users to mount vendor volumes into kubernetes. It expects vendor drivers are installed in the volume plugin path on every kubelet node. It allows for vendors to develop their own drivers to mount volumes on nodes.

Have a look at lvm driver in below repo:

Ref - https://github.com/fabric8io/gitcontroller/tree/master/vendor/k8s.io/kubernetes/examples/flexvolume
Ref - https://github.com/kubernetes/kubernetes/issues/32543
