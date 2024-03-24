# Kubernetes Architecture

## Era Deployments
![evolution deployments](/images/container-evolution.svg)

* **Traditional deployment**: Organizations ran applications on physical servers.
* **Virtualized deployment**: it allows you to run multiple Virtual Mechines (VMs) on single physical server's CPU. Each VM is a full machine running all the components, including its own opratiing system.
* **Containers deployment**: Containers are similar to VMs, but they have isolation properties to share the Operating System (OS) among the applications. Similar to a VM, a container has its own filesystem, share of CPU, memory, process space, and more. As they are decoupled from the underlying infrastructure, they are portable across clouds and OS distributions.

## Kubernetes Provide you with
* Service discovery and load balancing
* Storage orchestration
* automated rollout and rollbacks
* Automatic bin packing
* Self-healing: restarts containers that fail, replaces containers, kill containers that respon health check
* Batch execution
* Horizontal scaling: Scale your aplication up and down or automatically based on CPU Usage.
* IPV4/IPv6 dual-stack
* Designed for extensibilitiy: Kubernetest cluster without changing upstream source code.


> Kubernetes is not a traditional, all-inclusive PaaS (Platform as a Service) system

## Components Kubernetes


## Cluster
**A Cluster** is a set of nodes grouped together. This way even if one node fails you have your application still accessible from the other nodes. Moreover having multiple nodes helps in sharing load as well.

![Cluster](https://kubernetes.io/images/docs/kubernetes-cluster-architecture.svg)

## Nodes
**A node** may be a virtual or physical machine, depending on the cluster. Each node is managed by the control plane and contains the services necessary to run Pods.

## Pods
**Pods** are the smallest deployable units of computing that you can create and manage in Kubernetes.

A Pod (as in a pod of whales or pea pod) is a group of one or more containers, with shared storage and network resources, and a specification for how to run the containers

As well as application containers, a Pod can contain init containers that run during Pod startup. You can also inject ephemeral containers for debugging a running Pod.

![pods concepts](/images/pods-concepts.png)

