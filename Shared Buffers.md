## Shared buffers

**Shared buffers** is simple an array of 8KB blocks. Before postgres checks out the data from the disk it first does a lookup for the pages in the shared buffers.


## Buffer eviction

In order to evict page Postgres keeps usage count if page usage is 0 then it's evicted from cache 
## Example 

```
CREATE EXTENSION pg_buffercache;
SELECT bufferid,
  CASE relforknumber
    WHEN 0 THEN 'main'
    WHEN 1 THEN 'fsm'
    WHEN 2 THEN 'vm'
  END relfork,
  relblocknumber,
  isdirty,
  usagecount,
  pinning_backends
FROM pg_buffercache
WHERE relfilenode = pg_relation_filenode('cacheme'::regclass);
```


In case of massive select or update only 32 blocks are allocated for caching 


