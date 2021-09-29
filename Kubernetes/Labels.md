## Labels
Labels are used in conjunction with selectors to identify groups of related resources
### Example
```
apiVersion: v1
kind: Pod
metadata:
  name: label-demo
  labels:
    environment: production
    app: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort: 80
```
Creates a pod with label environment and app
It can be filtered then using selector
```
kubectl get pods --selector="app in (nginx)"
```


## Annotations
Almost the same as labels, but Kubernetes doesn't care about information in annotations
As such, annotation keys and values have no constraints. Thus, if you want to add information for other humans about a given resource, then annotations are a better choice.