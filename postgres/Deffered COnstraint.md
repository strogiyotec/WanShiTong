#Deferred Constraint
Disable check until commit.Let's say one table point to another with id 2. With **deffered** we could update id to 3 from table one and only then the actual id in the second table

**Imediate** - checked at the end of the statement   
**Deffered** - checked at the end of transaction
