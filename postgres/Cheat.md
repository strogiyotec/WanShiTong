## Connections
**Idle connections** - select count(*),state,datname from pg_stat_activity  group by state,datname;

**Condition on left join  **
```
test=# SELECT * FROM a LEFT JOIN b ON (aid = bid AND bid = 2);
 aid | bid
-----+-----
   1 |
   2 |   2
```
As you can see bid null is still here

#Deferred Constraint
Disable check until commit.Let's say one table point to another with id 2. With **deffered** we could update id to 3 from table one and only then the actual id in the second table

**Imediate** - checked at the end of the statement   
**Deffered** - checked at the end of transaction

# Csv manipulation
- Output	
	
	`Copy (Select * From foo) To '/tmp/test.csv' With CSV DELIMITER ',' HEADER;`
- 

# Filter grouping sets
Let's say that you write query with aggregate function and group by clause.
But you want aggregate function to peek only specific rows.

Example:
```
select sum(score) as tops
```
you want to calculate only those scores whose values is bigger than 10. 	
In this case the query will look like this
```
select sum(score) FILTER (WHERE score >10) as tops
```
You can combine FILTERS in one query

```
select sum(score) FILTER (WHERE score >10) as tops, 
		sum(scores) FILTER (WHERE score < 10) as bottom
```

> **FILTER is only useful if the data left by the WHERE 
> clause is not needed by each aggregate**
