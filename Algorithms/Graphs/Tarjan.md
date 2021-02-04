## Low link value
Is a smallest node id reachable from given node including given node itself
If this node is smallest itself then it's a Low link for given node
Algorithm to find the strong connected component

1. Make a dfs and store current id in an array
2. When dfs is finished check if currentId is smaller than verticle id if so update current
3. If previous value of current was smaller than verticle than we found a bridge
