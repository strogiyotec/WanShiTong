## PITR
Point in time recovery

### Config
1. In config file
    * `archieve=on`
	* `archieve_command='test ! -f /wal_archive/%f && cp %p /wal_archive/%f'`
	* `wal_senders=10`
	* Restart postgres
2. Make base backup `pg_basebackup -Umyuser -h127.0.0.1 --progress -D /basebackup/`

> Postgres needs transaction logs from point of time when transaction started
> and transaction log when transaction ended
> Postgres makes inconsistent copy of `data` dir
> and make it consistent using Log files

We can include WAL files to backup using `-X`
> Includes the required write-ahead log files (WAL files) in the backup.
> This will include all write-ahead logs generated during the backup. 

How to restore ? 

1. Stop postgres
2. Move all data from postgres to temp dir `mv /var/lib/pgsql/9.2/data/* /tmp/data.old/`
3. Copy backup to data dir(from tar)
4. Create `recovery.conf`
    * `restore_command = 'cp /var/lib/pgsql/pg_log_archive/%f %p'`
	* `recovery_target_lsn = '3/72658818'`
	* `recovery_target_action` - what to do after replaying WAL (pause,shutdown)
5. Delete all WALS
6. Now postgres will read archieve WAL directory

> Since Postgres 12 `recovery.conf` migrated to `postgres.conf`

## WAL archieve vs pg_receive_wal

### What is wrong with archieve
When postgres do WAL archieve it will do it only when Wal is full
which usually means when WAL is bigger than 16 Mb.
What will happen if our server will crash before WAL will be archieved?
We will lose Wal log for this period

### pg_receive_wal
In order to avoid problem above `pg_receive_wal` was introduced. It streams Wal from master to
replication server. All streamed data will be saved in `.partial` file. Only when this file is more than
16 Mb then it will be archieved

## To highlight
For PITR you need two things
1. Base backup
2. Archieve Wals since backup till now

## Resources
1. https://severalnines.com/database-blog/point-time-postgresql-database-restoration
