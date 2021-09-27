# Garbage collector

### Generations
The vast majority of objects are allocated in Younf generation (and died here)
When this area is full it causes a **minor GC**
Typically some fraction of of survived objects are moved to old generation
When old generation is full it causes a **major GC**

### Young Generation
Younf generation is divided into 
1. Eden
2. Two survival spaces
Eden space is where new objects are created.
When it's full Gc performs minor GC when it ping-pong dead-live objects from one
survival space to another. Why ? In order to avoid fragmentation(Contiguos memory)
Those object that were saved into Eden space enough times are moved into the Old generation\
**The bigger the Young generation the lesser Major gc occur**\
Setting **-XX:NewRatio=3** means that the ratio between the young and old generation is 1:3.
In other words, the combined size of the eden and survivor spaces will be one-fourth of the total heap size. 

## Parallel collector
Uses multiple threads to collect young generation.\
The number of garbage collector threads can be controlled with the command-line option **-XX:ParallelGCThreads**\
### Config
1. Max pause times
		By default not specified. If specified then GC tries to do GC not exceeding this limit
2. Throughtout 
    1. Time spend in GC and time spend to app
    2. -XX:GCTimeRatio=<N> sets ratio to 1/(1+N)
    3. Example: ratio is 19 then 1/1+19 = 1/20 = 5% of total time in GC
If more than 98% of the total time is spent in garbage collection and less than 2% of the heap is recovered,
then an OutOfMemoryError, is thrown

## G1
Use cases
1. Huge heap size
2. Predictable pause time
G1 is generation garbage collector. G1 devides memory into the regions 2048(Eden,Survivor,Old)
One particular is called Humongous(when object occupies more than 50% of memory)
The G1 principle is to free regions with most garbage.For G1 regions don't have to be
contiguous
**Algorithm**
1. Start putting objects into Eden spaces
2. When all eden spaces are full => Young generation Collector
3. Unless ParGC, G1 doesn't clean all heap, instead it clears region that most likely have most dead objects
4. G1 tracks free regions and during GC moves live objects into these regions
5. G1 tries to use specified pause time in order to determine which regions it can free during this time
**Don't specify NewRatio because it's dynamic**


## References
1. [Gc log format](https://blog.gceasy.io/2016/07/07/understanding-g1-gc-log-format/)
2. https://www.reddit.com/r/java/comments/pkjj5e/digging_into_java_garbage_collection/
3. https://docs.oracle.com/en/java/javase/13/gctuning/garbage-first-garbage-collector.html#GUID-DA6296DD-9AAB-4955-8B5B-683651936155
4. https://www.oracle.com/technetwork/tutorials/tutorials-1876574.html
