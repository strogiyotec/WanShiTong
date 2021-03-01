1. First make an adjacent map
2. Then make a queue where first element is node and the second is cost
3. Add starting point with cost zero to queue
4. Make a cache array to avoid undirected dependencies
5. Put into a queue adjacent verticles with cost equals to verticle cost + current node cost
6. Keep counter and decrement it after each iteration
7. If counter is non zero then it's impossible to reach all nodes
