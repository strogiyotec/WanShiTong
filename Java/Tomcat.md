## Tomcat work
1. TCP handshake and file descriptor is put into the queue(acceptCount in Tomcat)
2. Tomcat's acceptor thread get a connection from queue and assign a worker thread
3. Worker thread processes thread and become free once response it writeen however if keep alive is configured then thread is still waiting for more data
4. If no threads are available and acceptCount queue is full then Tomcat will send 503 error


## Resources
1. [Netflix blog](https://netflixtechblog.com/tuning-tomcat-for-a-high-throughput-fail-fast-system-e4d7b2fc163f)
2. [Chinese blog](https://developpaper.com/tomcats-acceptcount-and-maxconnections/)
