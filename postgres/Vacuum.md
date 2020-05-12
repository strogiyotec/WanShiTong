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
