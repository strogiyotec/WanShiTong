:java:concurrency:
## Concurrent Hash Map

	CHM never locks the whole Map, instead it it divides the map into segments 
	and locking is done on these segments. CHM is separated into different reginos(16 default)
	and locks are applied to them.
	When update happens in particular segment, then it doesn't block
	updated in different segments
	

