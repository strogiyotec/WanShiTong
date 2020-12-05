## Cheets

### Multiple bugs exception
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

