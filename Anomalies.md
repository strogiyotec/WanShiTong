## Concurrency control

### Non-repeatable read 

	When tx1 reads row twice and gets different results
Let's say we have a table `account(id,name,amount)` and two transactions. Here is the state.
```
id    name    amount
1     Bob     200 
2     Bob     800 
```
One transaction executes `update account set amount=amount-100 where id =2` another one does `update account set amount=amount*1.01 where user have total_amount>=1000`.
In this case second transaction will wait until first transaction will commit. After commit of first and second the state will be

```
id    name    amount
1     Bob     202 
2     Bob     707 
```
So the second transaction updated amounts even if their sum is less than 1000. Why? Because changes from first transaction can't be lost 
### Phantom read
		Transaction executes query once to get set of rows , 
		then the same query returns another set of rows. 

   **Example**: customer can't have more than three accounts

	
### Serialization anomaly

	When you execute queries concerrently you don't get the same result as you execute them serially
	
	
## Repeatable read limitations

Repeatable read prevents non-repeatable read and phantom read. However it doesn't prevent two other anomalies

### Inconsistent write

**Example**:  

		negative amounts on customer account are allowed if the total amount on all accounts of that customer is non-negative

```
 UPDATE accounts SET amount = amount - 600.00 WHERE id = 2  | UPDATE accounts SET amount = amount - 600.00 WHERE id = 3;
 
 id | number | client | amount 
----+--------+--------+---------
  2 | 2001   | bob    | -400.00
  3 | 2002   | bob    | 100.00
```
As two transactions update two different rows ,they violated consistency and it's allowed in **Repeatable read**

### Read only transaction anomaly 

Let's say that this is the state of our table

```
 id | number | client | amount
----+--------+--------+--------
  3 | 2002   | bob    | 100.00
  2 | 2001   | bob    | 900.00
```
Then first transaction does

```
BEGIN ISOLATION LEVEL REPEATABLE READ; -- 1
=> UPDATE accounts SET amount = amount + (
  SELECT sum(amount) FROM accounts WHERE client = 'bob'
) * 0.01
WHERE id = 2;
```
Then another transaction withdraw money from id 3 and **COMMIT CHANGES** while first transaction is still opened.
```
BEGIN ISOLATION LEVEL REPEATABLE READ; -- 2
UPDATE accounts SET amount = amount - 100.00 WHERE id = 3;
COMMIT;
```
And then the **third** transaction was opened and after that **First** transaction was commited.
As **Second** transaction was commited before **third** was opened, then third one see changes if second one.
```
id | number | client | amount
|   ----+--------+--------+--------
|     2 | 2001   | bob    | 900.00
|     3 | 2002   | bob    | 0.00
```
However , these data is not valid because second transaction was commited and id=2 should be 910.



### Read only anomaly in 
