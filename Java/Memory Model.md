## Java Memory model notes
1. [Happens before](Hb)

Java execution path **source_code->javac->JVM->processor** .In JVM step code started being optimized which means that code is transformed and **Java memory model** set boundaries which tells how code could be transformed

Example:

```
	public void foo(){
	 f+=2;
	 b+=1;
	 f++;
	}
```

How does it look in terms of memory without optimization? 

* CPU reads **f** from RAM into cache
* Adds 2 to **f**
* Write value back to RAM
* CPU reads **b** from RAM into cache
* Adds 1 to **b**
* Write value back to RAM
* And the same for the last **f**

This is why JVM could reorder execution to 

```
	public void foo(){
		f+=3;
		b+=1;
	}
```

As a result at the same time the values could be : 

```
	{f=3,b=0}
	{f=3,b=1}
```	
which is impossible if all instruction will be executed from top to bottom 

