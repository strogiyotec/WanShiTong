# Problem
> How to update running Pods to different version ? 
## Manual solution
1. Stop all pods and then start new ones
2. Start new ones and delete old ones
## Deployments
Declarative updates for Pods and ReplicaSets
Use cases
1. Rollout Replica Set
2. Declare new state for Pods

## Rollout Replica Set
```
kubectl rolling-update kubia-v1 kubia-v2 --image=luksa/kubia:v2
```
1. kubia-v1 - replica set name
2. kubia-v2 - new replica set name

### Algorithm
1. It will create a new replica set
2. It will destroy one pod from old and create one pod in new while all old pods won't be destroyed

## New State for Pods
In order to update Pod image
```
kubectl deployment.apps/nginx-deployment set image deployment.v1.apps/nginx-deployment nginx=nginx:1.16.1
```
## Deployment
In order to avoid manually creating a new replica set in order to replace an old one Deployment was introduced
```
apiVersion: apps/v1beta1          
kind: Deployment                  
metadata:
  name: kubia          
spec:
  replicas: 3
  template:
    metadata:
      name: kubia
      labels:
        app: kubia
    spec:
      containers:
      - image: luksa/kubia:v1
        name: nodejs
```
### Deployment strategies
1. RollingUpdate - remove 1 old pod and create a new one
2. Recreate - stops all old Pods and create new ones

To trigger update
```
$ kubectl set image deployment kubia nodejs=luksa/kubia:v2
```
It will change an image

### Undo
In order to undo deployment, by default the last two ReplicaSets are stored in Deployment
```
$ kubectl rollout undo deployment kubia
```
In order to deploy only a single pod while old ones will be running you can pause deployment
```
$ kubectl set image deployment kubia nodejs=luksa/kubia:v4
deployment "kubia" image updated
$ kubectl rollout pause deployment kubia
deployment "kubia" paused
```
It will pause deployment, but 1 new Pod was created

If deployments wasn't successful it can be rolled out
```
$ kubectl rollout undo deployment kubia
deployment "kubia" rolled back
```

