1. [Auditing](Hibernate/Auditing)
2. [States](Hibernate/States)

## Multiple bags exception
Let's say we have entites
```
class First{
		List<Second> second
}
class Second{
		List<Third> third;
}
```
In order to get first with list of second and list of third we should
1. Put FetchMode Subselect under each collection
2. Make Fetch Type Eager
3. Now when we select First , it will make 3 requests
    - to fetch first
	- subselect to fetch second
	- subselect to fetch third

## Use field from another table
Let's say we have a user and user has a company using company_id as a FK<br>
If we wanna use a field from company in User then we can use **@Formula** annotation<br>
It will send a separate request to get a field
### Example
```
 @Formula("(select c.company_key from company c where c.uidpk=company_id)")
    private String companyKey;
```
>Note that value inside annotation is a native SQL and it has to be surrounded by brackets<br>
>company_id here is a field inside User class , the name should match Database name,<br>
>not a field name


## If transaction alive ? 
If you wanna check if transaction in a method is active use this
```
TransactionSynchronizationManager.isActualTransactionActive()
```
## Multiple join conditions
```
    @JoinColumnOrFormula(
        column = @JoinColumn(
            name = "section_id", referencedColumnName = "uidpk", insertable = true, updatable = true
        )
    )
    @JoinColumnOrFormula(
        formula = @JoinFormula(
            value = "'SINGLE'",//value
            referencedColumnName = "type"//column name
        )
    )

```


