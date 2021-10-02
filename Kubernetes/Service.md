## Services
### Problem
1. Pods are not permanent
2. Kubernetes assigns IP address to POD after it was scheduled to Node - users can't know IP in advance
3. ReplicaSet creates multiple PODS, client shoudln't care which of them to choose

### Solution - Service
You create a service for a set of Pods using selector and then expose IP address of the service.
Service will do load balancing among Pods it manages
```
apiVersion: v1
kind: Service
metadata:
    name: kubia
spec:
ports:
   -port: 80
    targetPort: 8080
selector:
    app: kubia
```
1. port - service port
2. targetPort - pod port
```
kubectl get svc
```
Two IPS cluster and external.

> The primary goal of services is to expose group of Pods to other Pods

Service then can be reached by it's Cluster IP and port. It then redirects 
traffic to selected Pods

## How to find Service IP ? 
1. Environmental variables
2. Kubernetes DNS server

## External access
In order for external API to reach a cluster you have three options
### NodePort
```
apiVersion: v1
kind: Service
metadata:
    name: kubia-nodeport
spec:
    type: NodePort
ports:
    - port: 80
    targetPort: 8080
    nodePort: 30123
selector:
app: kubia
```
In this case all Nodes will be accesible through port `nodePort`

### Ingress
TODO: 
