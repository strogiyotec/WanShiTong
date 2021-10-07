## Volumes
By default storage doesn't persist
```
apiVersion: v1
kind: Pod
metadata:
  name: fortune
spec:
  containers:
    - image: strogiyotec/test                   
      name: html-generator                   
      volumeMounts:                          
      - name: html                           
        mountPath: /var/htdocs               
    - image: nginx:alpine                   
      name: web-server                        
      volumeMounts:                         
        - name: html                          
          mountPath: /usr/share/nginx/html    
          readOnly: true                      
      ports:
        - containerPort: 80
          protocol: TCP
  volumes:                 
  - name: html             
    emptyDir: {}
```
### hostPath
Volume that points to specific dir in Node file system
```
apiVersion: v1
kind: Pod
metadata:
  name: test-pd
spec:
  containers:
  - image: k8s.gcr.io/test-webserver
    name: test-container
    volumeMounts:
    - mountPath: /test-pd
      name: test-volume
  volumes:
  - name: test-volume
    hostPath:
      # directory location on host
      path: /data
      # this field is optional
      type: Directory
```
> Don't use it for db because it's mounted to Node
> If Pod will be deployed in another Node then it won't see the data

## Persistent Volume
A volume created by administrator for developers to use
```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: mongodb-pv
spec:
  capacity:                  
    storage: 1Gi             
  accessModes:                              
  - ReadWriteOnce                           
  - ReadOnlyMany                            
  persistentVolumeReclaimPolicy: Retain    
  gcePersistentDisk:                      
    pdName: mongodb                       
    fsType: ext4                          
```
It creates a volume with 1Gb that can be used by multiple readers and one writer nodes

### Volume Claim
Separate config that specifies what type of volume Pod needs, Kubernetes will choose available one among **PersistentVolumes**
```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mongodb-pvc # name of a claim
spec:
  resources:
    requests:                
      storage: 1Gi           
  accessModes:              
  - ReadWriteOnce           
  storageClassName: ""     
```
Here Claim asks for at least 1Gb of storage
To Use claim from Pod
```
apiVersion: v1
kind: Pod
metadata:
  name: mongodb 
spec:
  containers:
  - image: mongo
    name: mongodb
    volumeMounts:
    - name: mongodb-data
      mountPath: /data/db
    ports:
    - containerPort: 27017
      protocol: TCP
  volumes:
  - name: mongodb-data
    persistentVolumeClaim:       
      claimName: mongodb-pvc # claim name
```
Now Pod doesn't know which volume it uses , it just says what volume should be capable of
