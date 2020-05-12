## Checkpoint 

Where to start playing back **WAL** records during recovery ? 

### Checkpointer

Periodically a process called **checkpointer** reads all dirty pages and flush them to disk (but they are still in shared buffers). When all dirty pages are in disk then this time is the point to start recovery at. 


The dirty buffers can also be written by server processes - whichever reachers the buffer first. 


### Background writer

If a backend needs a buffer but all of them are busy and buffer to be evicted is dirty then backend will have to write the page on disk on its own and this situation will block user.  
That is why we need background write or **bgwriter**, this writer runs ahead of eviction and finds those buffers that are more likely to be evicted soon. 
