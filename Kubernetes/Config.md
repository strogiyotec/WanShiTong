## Configuration
### 1. Env varialbles
In order to pass environment variables
```
kind: Pod
spec:
 containers:
 - image: luksa/fortune:env
   env:                        
   - name: INTERVAL            
     value: "30"               
   name: html-generator
```

### 2. Config Map
Config file that stores key->value
```
kubectl create configmap fortune-config --from-literal=sleep-interval=25
```
1. Creates a config map with a name **fortune-config**
2. Creates a key sleep-interval with a value 25

Create from a file
```
kubectl create configmap my-config --from-file=customkey=config-file.conf
```
To create Config as a resource
```yml
apiVersion: v1
kind: ConfigMap
metadata:
    name: config-for-prod
data: 
    some-config-file: |
        first line
        second line
    

```
How to use **ConfigMap** from Pod
```
apiVersion: v1
kind: Pod
metadata:
  name: fortune-env-from-configmap
spec:
  containers:
  - image: luksa/fortune:env
    env:                             
    - name: INTERVAL                 
      valueFrom:                       
        configMapKeyRef:               
          name: fortune-config  # name of a config
          key: sleep-interval   # key in this config
```
This will setup ENV varialbe **INTERVAL** with a value from a config file
#### How to set all config map entries as env varialbles ? 
```
spec:
  containers:
  - image: some-image
    envFrom:                
    - prefix: CONFIG_             
      configMapRef:              
        name: my-config-map      
```
In this case all config keys will be available by name with prefix **CONFIG_**

#### We can store files as config map values

```
kubectl create configmap fortune-config --from-file=nginx.config.conf
```
Here we store name of a file as a key and value is a content

```
apiVersion: v1
kind: Pod
metadata:
  name: fortune-configmap-volume
spec:
  containers:
  - image: nginx:alpine
    name: web-server
    volumeMounts:
    - name: config
      mountPath: /etc/nginx/conf.d  # path in Pod
      readOnly: true
    ...
  volumes:
  ...
  - name: config              
    configMap:                 
      name: fortune-config     
      items:
      - key: my-nginx-config
      - path: gzip.conf # the file content will be stored in file gzip.conf
```

> Updating configmap will update env variable in POD

### 3. Secrets
Not encrypted and stored in memory
```
kind: Secret
apiVersion: v1
stringData:           
  foo: plain text      
data:
  https.cert: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSURCekNDQ...
  https.key: LS0tLS1CRUdJTiBSU0EgUFJJVkFURSBLRVktLS0tLQpNSUlFcE...
```
For secrtes all values has to be base64 encoded
However, for string values , `stringData` can be used which then will be base64 encoded by Kuber
```
apiVersion: v1
kind: Pod
metadata:
  name: fortune-https
spec:
  containers:
  - image: nginx:alpine
    name: web-server
    volumeMounts:
    - name: certs           
      mountPath: /etc/nginx/certs/        
      readOnly: true                      
 volumes:
  - name: certs                            
    secret:                                
      secretName: fortune-https            
      items:
        - key: https-file
        - path: https.conf
```
