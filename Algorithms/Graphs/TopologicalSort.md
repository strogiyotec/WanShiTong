## Use cases
Some events has to occure before others
1. Class prerequisites
2. Program dependencies
3. Event scheduling


**Topological sort** - linear ordering of all its verticles Where
for each directed edge from node A to node B , node A appears before node B
![Topological sort](topological.png)


## Topological order
When nodes are lined up(Can't be made when node has cycles)
> Every tree has topological sort because it doesn't have cycles

### How to check ? 
Do DFS
1. Peek up the node
2. Put it into the cache
3. Go to adjacent nodes and call dfs
4. If node is in cache then it has cyclic dependencies
