## Shared buffers
1. Shared buffers are used as a cache for postgres.
2. Each buffer page is 8 kb block
3. When you do insert that data is stored in cache and `is_dirty` field set to true(durty means not in the disk)

### Eviction algorithm
Every buffer contains a metadata like a `usage counter`. Using this counter
postgres decided to evict the cache(This is the reason why postgres doesn't use OS cache, because of metadata)
#### Ring Buffer 
Let's say we read a lot of data from disk. Without Ring Buffers all cache will be evicted
to give enough space for this huge blob of data. In order to prevent it Ring Buffers
is used when more than 32 blocks of data is retrieved from DB(Ring Buffer is also a cache)


