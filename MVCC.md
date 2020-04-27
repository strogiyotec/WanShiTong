## Concurrency control

### Non-repeatable read 

	When tx1 reads row twice and gets different results

### Phantom read
	
	Tx1 sees row in stmt2 that wasn't reported in stmt1 as txt2 deleted it
	
### Serialization anomaly

	When you execute queries concerrently you don't get the same result as you execute them serially
