---
title: "Kubernetes Notes - Part 2"
layout: post
date: 2021-11-07
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

## Concepts and Architecture

### Master node components
- kube-apiserver: cordinates all the administratives tasks.
- kube-scheduler: is to assign new workload objects, such as pods, to nodes.
- controller managers: are control plane components on the master node running controllers to regulate the state of the Kubernetes cluster.
- kube-controller-manager: responsible to act when nodes become unavailable, to ensure pod counts are as expected, to create endpoints, service accounts, and API access tokens.
- cloud-controller-manager: interact with the underlying infrastructure of a cloud provider when nodes become unavailable, to manage storage volumes when provided by a cloud service, and to manage load balancing and routing.
- etcd: distributed key-value data store used to persist a Kubernetes cluster's state.

### Worker node
It provides a running environment for client applications

#### Components
- Container Runtime: to Manage a container's lifecycle, such as Docker, containered..
- Node Agent - kubelet: it is an agent running on each node and communicates with the control plane components from the master node.
- Proxy - kube-proxy: it is the network agent which runs on each node responsible for dynamic updates and maintenance of all networking rules on the node. It abstracts the details of Pods networking and forwards connection requests to Pods. 
- Addons for DNS, Dashboard user interface, cluster-level monitoring and logging.


### Network

We can have pods communicating with other pods, containers inside pods communicating with each other, deployments communicating with each other, nodes communicatins with each other...

![Kubernetes Networking](https://www.superbytehosting.com/wp-content/uploads/2020/07/Kubernetes-Networking-Model.jpg)

Kubernetes has an internal DNS
You can set a resolved DNS like `your-service-name.your-namespace-name.svc.cluster.local`. The `cluster.local` means the internal host.

#### Services

They are the way we can expose the services to outside from kubernetes, it's considerated as network abstraction, and kind a "physical portforward".

The services receives a selector with the label, entry and exit port. We can choose which deployment or other workload are gonna be inside of the service through labels, with that we can aggregate the apps inside the same IP

![Services](/assets/images/k8s-services.png){: .center-image }

##### Types of service

- ClusterIP: intern IP
- NodePort: opens a fixed port on the node for external access via IP
- ExternalName: expose the service with a defined name, it doesn't use proxy and it uses a DNS CNAME record
- LoadBalancer: creates a load balance solution and gives a external and fixed IP address

#### Ingress

Ingress is a network workload, it exposes the application to the external world, also there is a set of rules to allow the entry in the cluster. For default, all external request outside from kubernet are blocked, but all request to outside are allowed. Always is related to a service, it never comes alone. The service redirects the traffic to the pods. It can add a digital certificate

![Services](/assets/images/k8s-ingress.png){: .center-image }

#### Ingress Controllers
##### Reverse Proxy
we receive requests from the users in one place, and the reverse proxy forwards the requests that the place received to the service.

##### What is ingress controllers?
It's kind of a reverse proxy. It acts as a proxy and route to the pods. Also, it detects automatically new ingresses to add this new domain in an allowed list.

#### Port Forward
It gets a localhost port and forwards to an internal Kubernetes port


#### Namespaces
Namespaces are a logic division so that you can divide apps without they're split by nodes.
There are some default namespaces, such as kube-public, kube-system and default.

We can have different permissions for different roles as well

#### RBAC
RBAC means Role-based access control. It is a method of regulating access to computer or network resources based on the roles of individual users within your organization (access policies based per roles).

![Namespaces](/assets/images/k8s-namespaces.png){: .center-image }


#### Secrets
It's a file that you can add sensible data that will be encode to base-64.
You can use the secrets as:
- read envvars;
- as files in volume mount;
- download private images.


![Secrets as a file](/assets/images/k8s-secrets-as-file.png){: .center-image }
![Secrets as env](/assets/images/k8s-secrets-as-env.png){: .center-image }

#### Config Maps
Very similar to secrets, but it's simpler. The difference is the data isn't encoded

![ConfigMaps](/assets/images/k8s-configmaps.png){: .center-image }
![ConfigMaps as file](/assets/images/k8s-configmaps-as-file.png){: .center-image }
![ConfigMaps as env](/assets/images/k8s-configmaps-as-env.png){: .center-image }

the config maps propagate automatically to the pods. We can set the propagation type, the configmaps changes can be observer by the pods or it can have a TTL.


#### Sources:
- Post - [The Illustrated Children’s Guide to Kubernetes](https://www.cncf.io/phippy/the-childrens-illustrated-guide-to-kubernetes/)
- Youtube Video - [The Illustrated Children's Guide to Kubernetes](https://www.youtube.com/watch?v=4ht22ReBjno)
- Course - [Introduction to Kubernetes](https://www.edx.org/course/introduction-to-kubernetes)
- GitHub - [Descomplicando Kuberntes](https://github.com/badtuxx/DescomplicandoKubernetes)
- Bootcamp - [Maratona AKS: Tudo sobre Kubernetes de A a Z](https://channel9.msdn.com/Series/AKS-Bootcamp-From-zero-to-container-hero)
- Youtube Video - [Kubernetes Tutorial for Beginners - FULL COURSE in 4 Hours](https://youtu.be/X48VuDVv0do) 

---
