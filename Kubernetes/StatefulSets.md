# StatefulSets
### Motivation
ReplicaSet doesn't give guarantee on ordering and which persistence storage 
the Pod will use
If you need a state for a POD then StatefulSet can be used

### Pods
Pods in StatefulSet are given same names. For example for 3 replicas names would be
1. a-0
2. a-1
3. a-2

If a-1 dies then the same pod with same storage and IP will be replicated

### Headless Service
Database cluster can be deployed to StatefulSet but in case of Master+Slave
architecture each node has to know the address of another. For this
each pod needs a unique and permanent address provided by Headless Service

### Create StatefulSet
1. Persistent Volume
2. Headless Service
3. StatefulSet itself

PersistentVolume
```
kind: List                     
apiVersion: v1
items:
- apiVersion: v1
  kind: PersistentVolume       
  metadata:
    name: pv-a                
  spec:
    capacity:
      storage: 1Mi            
    accessModes:
      - ReadWriteOnce
    persistentVolumeReclaimPolicy: Recycle     
    gcePersistentDisk:         
      pdName: pv-a             
      fsType: nfs4                         
```

Headless Service
```
apiVersion: v1
kind: Service
metadata:
  name: kubia       
spec:
  clusterIP: None    
```

StatefulSet
```
apiVersion: apps/v1beta1
kind: StatefulSet
metadata:
  name: kubia
spec:
  serviceName: kubia
  replicas: 2
  template:
    metadata:
      labels:                  
        app: kubia             
    spec:
      containers:
      - name: kubia
        image: luksa/kubia-pet
        ports:
        - name: http
          containerPort: 8080
        volumeMounts:
        - name: data                  
          mountPath: /var/data        
  volumeClaimTemplates:
  - metadata:                  
      name: data               
    spec:                      
      resources:               
        requests:              
          storage: 1Mi         
      accessModes:             
      - ReadWriteOnce          
```


> StatefulSet works same way as Deployment 
> You make an update, one old Pod will be shutdown and new one will be spawn
