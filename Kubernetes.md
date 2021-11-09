## Kubernetes
1. [Kind](Kubernetes/Kind)
2. [Components](Kubernetes/Components)
3. [Pods](Kubernetes/Pods)
4. [ReplicationSet](Kubernetes/ReplicationSet)
5. [Labels and Annotations](Kubernetes/Labels)
6. [Service](Kubernetes/Service)
7. [Deployments](Kubernetes/Deployment)
9. [Namespaces](Kubernetes/Namespaces)
10. [Volumes](Kubernetes/Volumes)
11. [Config](Kubernetes/Config)
12. [StatefulSets](Kubernetes/StatefulSets)
13. [Resources](Kubernetes/Resources)

## Big picture of how Kubernetes works
1. Each node runs **kubelet** which sends requests to Api server
    1. It sends manifests when `kubectl apply` is used
    2. It sends health metrics about each POD running on this node
2. When **Api server** gets pod manifest from `kubelet` it will create a new entry in etcd(this entry is usually a deployment)
3. **Control Manager** listens for events from `etcd` once it got a new deployment manifest it will
    1. Create a ReplicaSet manifest in etcd
    2. Pod manifests in etcd
4. **Scheduler** will listen for new Pod manifests in `etcd` and once new manifest is created it will
    1. Check healthy nods 
    2. Choose the most appropriate node according to available resources and create a new entry<br> in etcd `Pod 1-> Node1`
5. **kubelet** listens for new etcd events with `Pod 1-> Node 1` type once it finds an event with it's own Node it will deploy this pod in the node it's running on and change the status of this POD in etcd as running
6. **kubelet** will notify etcd through API that Pod that was running is down
7. **Control Manager** will periodically check how many Pods in etcd are running and how many are expected according to ReplicaSet manifest if Pods are less than expected then it will create a new Pod manifest in etcd that then will be processed by **Scheduler** and **kubeletet**

