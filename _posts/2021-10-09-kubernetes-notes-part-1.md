---
title: "Kubernetes Notes - Part 1"
layout: post
date: 2021-10-09
headerImage: false
tag:
- kubernetes
- notes
- studies
published: true
category: blog
author: beatrizuezu
description: 
---

Some kubernetes studies notes 🎉

---
# Kubernetes

## What do I need to know first to undestand Kubernetes?
### Containers
- To provide portable and isolated virtual environment
- Need to be managed and connected to external world
- Must be scheduled, distribuited and load balanced and the data persisted somewhere

### Containers Orchestration


## What is Kubernetes
- “Kubernetes” is the Greek word which means helmsman or ship pilot. With this analogy in mind, we can think of Kubernetes as the pilot on a ship of containers.
- Referred to as k8s
- The Kubernetes project focuses on building a robust platform (a fault-tolerant and scalable solution) for running thousands of containers in production.
- Can be achieved by creating a single controller/management unit, after connecting multiple nodes together. This controller/management unit is generally referred to as a container orchestrator. 

## Features
- Automatic bin packing
- Self-healing: Kubernetes automatically replaces and reschedules containers from failed nodes
- Horizontal scaling: scaled manually or automatically based on CPU or custom metrics utilization.
- Service discovery and Load balancing
- Automated rollouts and rollbacks: to prevent any downtime
- Secret and configuration management
- Storage orchestration
- Batch execution: supports batch execution, long-running jobs, and replaces failed containers.  

## Why use Kubernetes?
- Can be deployed in many environments such as local or remote Virtual Machines, bare metal, or in public/private/hybrid/multi-cloud setups
- Kubernetes' architecture is modular and pluggable
- Kubernetes' functionality can be extended

## Importants terms
- Node: or workers, it runs proccess. It contains the app, proxy and kubelet, it can has one or more pods.
- Kubelet (Node Agent): responsible to inform to control plane how many cpu, memory the node has.
- Control plane: virtual machine, it run a lot of services, such as scheduler service, API server, node controller and state storage (etcd).
- etcd: it is a distributed key-value store which only holds cluster state related data, no client workload data.
- Kubectl: Kubernetes CLI, point of control for all apps based in Kubernetes, based in configuration files.
- Workloads: everthing you can put inside of kubernetes that generate a work rules, like apps and DNS. The workloads are:
  - Pod
  - Deployment
  - ReplicaSet
  - Secret
  - Configmap
  - HorizontalPodAutoscaler
  - Service
  - Ingress
  - ...
- Pods: the smallest unit in Kubernetes, you can run one or more containers. Each pod contains an internal IP, it runs the application in the container and it can has or not a volume. Volume is a pre-existing data structure to use in the pod to share files or to use a temporary files.
![Pods](/assets/images/k8s-pods.png){: .center-image }
- Labels: as “nametags” to identify things, it can be used to query based on the labels.
- Deployments: deployments manages and adds "intelligence" to pods, and it can group pods by an label. Also, you can define the number of replicas inside of the deployment
  
![Deployment](/assets/images/k8s-deployment.png){: .center-image }

![Deployment File example](/assets/images/k8s-deployment-file-example.png){: .center-image }

- Replicaset: it created by a deployment. It manage the number of replicas inside of the cluster. Each one contains a different image version, it is able to create versions of images. It always keep the desired number state.

- Replication Controllers: responsible to manage the numbers of pods, pods' lifecycle, scaling up and down, rolling deployments, and monitoring.
- Services: Responsible to tell to the rest of the Kubernetes environment what services your application provides, the service IP addresses and ports remain the same.
- Volume: Location where containers can access and store information.
- Namespaces: group of services, pods, replication controllers, and volumes.

![Some Kubernetes terms](/assets/images/k8s-concepts.png){: .center-image }



#### Sources:
- Post - [The Illustrated Children’s Guide to Kubernetes](https://www.cncf.io/phippy/the-childrens-illustrated-guide-to-kubernetes/)
- Youtube Video - [The Illustrated Children's Guide to Kubernetes](https://www.youtube.com/watch?v=4ht22ReBjno)
- Course - [Introduction to Kubernetes](https://www.edx.org/course/introduction-to-kubernetes)
- GitHub - [Descomplicando Kuberntes](https://github.com/badtuxx/DescomplicandoKubernetes)
- Bootcamp - [Maratona AKS: Tudo sobre Kubernetes de A a Z](https://channel9.msdn.com/Series/AKS-Bootcamp-From-zero-to-container-hero)
- Youtube Video - [Kubernetes Tutorial for Beginners - FULL COURSE in 4 Hours](https://youtu.be/X48VuDVv0do) 

---
