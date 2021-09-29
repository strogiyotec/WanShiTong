# Links
1. [Resource management](Pods/ResourceManagement)
## Pods
Kubernetes groups multiple containers in a single Unit called **Pods**
1. All Containers in a POD always land on the same machine
2. Containers share same IP and ports(networking)
3. How to choose containers for POD - `Will these containers work correctly if they land on different machines`

## Pods Manifest
**Pods Manifest** is just a text file that describes Pod Object
Once scheduled , Pods do not move and must be explicitly destroyed/rescheduled

Manifest can be written with YAML or JSON

### Example
In order to deploy this container in POD
```
$ docker run -d --name kuard \
  --publish 8080:8080 \
  gcr.io/kuar-demo/kuard-amd64:blue
```

We can write a file `kuard-pod.yml` with the following content
```
apiVersion: v1
kind: Pod
metadata:
  name: kuard
spec:
  containers:
    - image: gcr.io/kuar-demo/kuard-amd64:blue
      name: kuard
      ports:
        - containerPort: 8080
          name: http
          protocol: TCP
```

1. To deploy run `kubectl apply -f kuard-pod.yml`
2. To check run `kubectl get pods`
3. To have a verbose output run `kubectl describe pods kuard`

### Delete Pod
In order to delete Pod there are two commands
1. `kubectl delete pods/kuard`
2. `kubectl delete -f kuard-pod.yml` - Using same file it was deployed with 

When deleted Pod has a termination period(default 30 sec). In this state Pod doesn't accept new requests but
proceed with existing ones

When Pod is deleted all associated data is deleted too
> In order to persist data you have to use **PersistentVolumes** 

### Interactions
1. For port forwarding `kubectl port-forward kuard 8080:8080`
2. To open shell `kubectl exec -it kuard -- ash`

### Copy files
1. From pod ` kubectl cp <pod-name>:/captures/capture3.txt ./capture3.txt`
2. To Pod `kubectl cp $HOME/config.txt <pod-name>:/config.txt`

### Liveness
Custom Logic to understand that Pod is healthy
```
apiVersion: v1
kind: Pod
metadata:
  name: kuard
spec:
  containers:
    - image: gcr.io/kuar-demo/kuard-amd64:blue
      name: kuard
      livenessProbe:
        httpGet:
          path: /healthy
          port: 8080
        initialDelaySeconds: 5
        timeoutSeconds: 1
        periodSeconds: 10
        failureThreshold: 3
      ports:
        - containerPort: 8080
```
Here it sends httpGet response to given endpoint
If It fails then Prob will be restarted, this behavior is configurable using `restartPolicy`
1. Always
2. OnFailure - restart only on failure
3. Never

