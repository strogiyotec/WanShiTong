## Versioning

**Postgres** maintains both the past and the lastest state of each row in each table.  
Each table has two hidden fields: **xmin** and **xmax**

## Example

```
SELECT attname, format_type (atttypid, atttypmod)
FROM pg_attribute
WHERE attrelid::regclass::text='table_name'
```

### Output

```
attname  |      format_type       
----------+------------------------
 tableoid | oid
cmax     | cid
xmax     | xid
cmin     | cid
xmin     | xid
ctid     | tid
emp_id   | integer
emp_name | character varying(100)
dept_id  | integer
```

**xmin** - when a row is created the value is set equal to the id of the transaction that perfomr insert.  
**xmax** - when a row is deleted the value of the current version is labeled with the id of the transaction that performed *DELETE*.

**UPDATE** command actually performs two subsequent operations **DELETE** and **INSERT**. In the curent version of the row **xmax** is set equal to ID of transaction that performed **UPDATE**. Then the new version of the same row is created in which the value of **xmin** is the same as **xmax** of the previous version of this row.
