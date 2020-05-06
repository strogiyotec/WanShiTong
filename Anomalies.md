## Concurrency control

### Non-repeatable read 

	When tx1 reads row twice and gets different results

### Phantom read
	
    Transaction executes query once and get result, then the same query returns another result. For    example Read commited will see rows commited by another transaction.
	
### Serialization anomaly

	When you execute queries concerrently you don't get the same result as you execute them serially
