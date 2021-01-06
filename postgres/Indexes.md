## Bitmap Scan
Let's say we have a separate index for **x** and **y** . To Combine them Postgres
create multiple **BitMaps** and then OR or AND them. In this case pages from disk will
be fetched in disk order and index order will be lost
