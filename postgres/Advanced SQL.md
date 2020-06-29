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
