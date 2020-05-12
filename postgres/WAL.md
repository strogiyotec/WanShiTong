## WAL

Any changes to a Postgres database first of all saved in Write-Ahead log so they will never get lost (Durability). After that **dirty** (they need to be synced to disk) pages are flushed to disk

**Checkpoint** process dump dirty pages to disk. It also saves the position in WAL (**REDO POINT**) up to which all changes are sunchronized.

### Example  
Postgres crashed and then it will restore state by replaying the **WAL** records from **REDO POINT** 
