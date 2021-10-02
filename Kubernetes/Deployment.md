# Problem
> How to upadte running Pods to different version ? 
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
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```
Using label selector `matchLabels nginx` replica set understands which pods to replicate

## New State for Pods
In order to update Pod image
```
kubectl deployment.apps/nginx-deployment set image deployment.v1.apps/nginx-deployment nginx=nginx:1.16.1
```
