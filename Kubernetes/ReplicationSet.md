## Replica Set - RS(Controller in old version)
Main job is to ensure that exact number of Pods are running
### Main Components
1. Label selector - which Pods to choose(by Pod labels)
2. Replica Cnt - Desired number of Pods
3. Pod template - let's say 2 pods are assigned to RS, but replicate cnt is 3 in this case RS needs template to create a new Pod
```
apiVersion: v1
kind: ReplicationController     
metadata:
  name: kubia                      
spec:
  replicas: 3                     
  selector:              
    app: kubia           
template:                        
    metadata:                      
      labels:                      
        app: kubia                 
    spec:                          
      containers:                  
      - name: kubia                
        image: luksa/kubia         
        ports:                     
        - containerPort: 8080      
```
> Do not specify label for pod template in RS because it has to match selector,
> If you don't specify then RS will assign the same one as in selector section
If you change a label of one Pod which is managed by RS then it will be out of scope of RS BUT RS will create a new one to match replication cnt

### Change RS selector
If you change selector, then existing Pods will exist by RS will create new Pods
that match a new selector

### Change Pod template
If you change a template in RS, then existing Pods will be running but
as soon as one of them fail, RS will create a new one from updated template


### Delete RS
When you delete RS then all pods are deleted too 
In order to avoid use 
```
kubectl delete rc kubia --cascade=false
```
