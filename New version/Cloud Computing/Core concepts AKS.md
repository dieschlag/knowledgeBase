
Original page: https://learn.microsoft.com/en-us/azure/aks/core-aks-concepts

The cluster is divided into two main components: the control panel and the nodes.

![[Pasted image 20250615185758.png]]

The control panel consists in:
- `kube-apiserver`: server exposing the Kubernetes API to enable requests inside/outside the cluster
- `etcd`: highly available key-value store, helps maintain state of the cluster and config
- `kube-scheduler`: helps to make scheduling decisions, assigns nodes to pods
- `kube-controller-manager`: runs controller processes, notices + responds when node is down
- `cloud-controller-manager`: embeds cloud-specific control logic to run controllers specific to the cloud provider

For nodes:
- `kubelet`: makes sure that containers are running inside a pod
- `kube-proxy`: network proxy that maintains network rules on the pods
- `container runtime`: manages execution and lifecycles of containers

Node config:
- VM size and image: defines CPU, storage size/type (ssd or hard drive). Cluster nodes are based on Linux. SKU (Stock Keeping Unit = version name of the image) is defined dinamically if parameter is left blank when deploying.
- OS disks: default ones are used when not parameter is not defined when deploying
- resource reservation: AKS reserves the CPU and memory allocation to match the allocated ressource on the node and the allocatable ressources on AKS
- OS: two possibilities: Linux and Azure Linux. Linux is the default one.
- Container Runtime: software that executes the containers and images on a node. For Linux, containerd is being used, and as well for some windows nodes.

Node pools: nodes of the same config are groupes into node pools. They include the underlying vms that make the apps run.

Node ressource group: when creating an AKS cluster in an Azure resource group, another group is created known as the node ressource group. It contains all the infrastructure ressource associated with the cluster (vms, virtual machine scale set, storage, ...)

Note: Virtual machine scale sets: group of load balanced virtual machines

Namespaces: Kub ressources are grouped using namespaces:
- default: allows to start using the cluster without creating a new namespace
- kube-node-lease: enables nodes to communicate their availability to control plane.
- kube-public: can be used so that ressources are visible by all users across the cluster by any user.
- kube-system: used by Kub to manage its ressources, for ex. `coredns`, `konnectivity-agent`, `metrics-server`, our applications should not be uploaded here.

