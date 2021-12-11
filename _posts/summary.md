Pods: the smallest unit in Kubernetes, you can run one or more containers. Single instance of the application.
Labels: "nametags" to organize and select a subset of objects, such as Pods, ReplicaSets, Nodes, Namespaces, Persistent Volumes
ReplicationController: it ensures that a specified number of pod replicas are always up and available.
ReplicaSet: guarantees set of replica pods available. It ensures the number of the current replicas state matches the number of desire replicas
Deployments: it provides updates for pods and ReplicaSets. It creates new ReplicaSet or removes existing deployments and adopts all their resources with new deployaments.
Namespaces: group of services, pods, replication controllers, and volumes.