## WAL

Any changes to a Postgres database first of all saved in Write-Ahead log so they will never get lost (Durability). After that **dirty** (they need to be synced to disk) pages are flushed to disk

**Checkpoint** process dump dirty pages to disk. It also saves the position in WAL (**REDO POINT**) up to which all changes are sunchronized.

WAL uses internal cache, when sql is executed, the state is saved in cache and when transaction is commited
Postgres saves data from WAL cache to file

