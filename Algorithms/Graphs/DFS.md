## Deep first search
**Purpose** - visit all verticles but only once

1. Go as deep as you can
2. Store already visited places
3. If go to already visited then skip

```
finction dfs(at):
    if cache[at]:
        return cache[at]
    cache[at] = true
    neigbours = graph[at]
    for neighbour in neighbours:
        dfs(neighbour)
end
```

