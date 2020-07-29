**Condition on left join  **
```
test=# SELECT * FROM a LEFT JOIN b ON (aid = bid AND bid = 2);
 aid | bid
-----+-----
   1 |
   2 |   2
```
As you can see bid null is still here
