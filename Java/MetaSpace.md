## MetaSpace

MetaSpace is a replacement for PermGen . Why
1. The size of PermGen was hard to predict
2. Gc now works on Metaspace

Metaspace doesn't have reflection info.\
Metaspace contains annotations and counters for JIT
Gc will clean Metaspace(unload classes)

## References
1. [Explanation](https://www.youtube.com/watch?v=jsJtZdYhQuE)
