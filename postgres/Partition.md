## Partition
Splitting one table into multiple

```
CREATE TABLE measurement (
    city_id         int not null,
    logdate         date not null,
    peaktemp        int,
    unitsales       int
) PARTITION BY RANGE (logdate);
```
It will create a virtual table that doesn't have a storage
Then create a partition
```
CREATE TABLE measurement_y2006m02 PARTITION OF measurement
    FOR VALUES FROM ('2006-02-01') TO ('2006-03-01');
```

1. If you insert a row that doesn't match any partition it will throw an error
2. Create index on table will create index for all partitions
3. You can detach partition to run some analytical queries on it

## Limits
1. Both the primary key and all unique keys must include partition key in their definition. Make sure that the unique key still makes sense.`primary key (id, created_at),unique (external_id, created_at)`

## Resources
1. [Limitations](https://alexey-soshin.medium.com/dealing-with-partitions-in-postgres-11-fa9cc5ecf466)
