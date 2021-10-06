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
For DNS the pod has to call service by it's name

## Pods accessing external resource
If Pod wants to reach a service outside of the cluster the **ExternalName** service can be used
```
apiVersion: v1
kind: Service
metadata:
  name: external-service
spec:
  type: ExternalName                       
  externalName: someapi.somecompany.com     
  ports:
  - port: 80
```
Now when Pod tries to reach `external-service.default.svc.cluster.local ` it will be redirected to `somecompany.com`

## Pods accessible to outside cluster clients
### NodePort
Assign static IP address to service that can be used outside
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
```
❯ kubectl get service kubia-nodeport
NAME             TYPE       CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
kubia-nodeport   NodePort   10.96.34.169   <none>        80:30123/TCP   17s
```
Now external users will access pods using service ip and port 30123


## Ingress
When client sends request to Ingress, Ingress will forward it to service
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: kubia
spec:
  rules:
  - host: kubia.example.com             
    http:
      paths:
      - path: /                           
        backend:
          serviceName: kubia-nodeport     
          servicePort: 80                 
```
Now when external client reaches `kubia.example.com` , Ingress will get choose
a random Pod based on service and redirect the traffic

### Ingress Controller
For Ingress to work you have to deploy a controller, Controller is a service
that implements Ingress specification (nginx,Envoy etc)
