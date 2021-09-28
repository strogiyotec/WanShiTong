## Resource management
Two types
1. Resource requests - min amount of resources to run app
2. Resource Limits - Max amount app can consume

### Min Resource
Can specify CPU and memory
```
apiVersion: v1
kind: Pod
metadata:
  name: kuard
spec:
  containers:
    - image: gcr.io/kuar-demo/kuard-amd64:blue
      name: kuard
      resources:
        requests:
          cpu: "500m"
          memory: "128Mi"
      ports:
        - containerPort: 8080
          name: http
          protocol: TCP
```
500m for cpu means 50% of CPU
> Resources are requested per container not per Pod 
> Resources for Pod is the sum of resources for all containers