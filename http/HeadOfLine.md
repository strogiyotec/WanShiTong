## Http pipelining
Instead of opening a lot of connections to server browser can use persistance connection. `Keep-Alive:true`. 

In this case browser can send multiple requests using the same connection. 
All of them will be ordered. Not all browsers support it 
This introduced problem below. If first request is slow then all other requests will wait for it

## Head of line blocking
In order to avoid it browser opens 6 connections to single server.
Some times it's inefficient because TCP requires time for max efficiency(because bandwidth is increased geometrically and starts from low to high)
