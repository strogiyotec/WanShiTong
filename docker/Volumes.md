## Volumes
Volumes are directory or file in host machine which is accessible 
within docker container

### Example
1. `docker run --it name test -v /data ubuntu bin/bash`
2. In The host - `docker volume inspect 3fe05e` - it will give the directory in the host machine 
3. If you create a file in host machine then it will be accesible in container

We can also
1. Specify specific directory `docker run -v /home/adrian/data:/data debian ls /data` 
2. Share valumes between containers `docker run -it -h NEWCONTAINER --volumes-from vol-test debian /bin/bash`
