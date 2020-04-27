 Algorithms time complexity  
===========================
# Rate growth
Rate growth shows how fast a function grows with the input size

 `6n^2 + 100n +300`

![Graph](https://cdn.kastatic.org/ka-perseus-images/0642ea78ce621e53dbe7f45881a97786c7262635.png)

**n^2 is always bigger than (100n + 300)**


**n^b** - polunomial

**a^n** - exponential function

When we drop **costant coefficents**(300) and **less significant terms**(100n) we use **Asymptotic notation**

# Big-Theta
Example with **Linear Search** 

The total time would be `n+c` where 

	n- amount of elements
	c- constant factor like create var i inside loop
But constant factor **c** doesn't matter 

**Theta(n)** - when **n** is large enough then running time is at least **k1\*n** and at most **k2\*n** for some constants **k1 and k2**

**Big Theta** doesn't have time unit (like millis)






# Big O notation
**Big O notation** always gives assymptotic upper bound

# Big Omega
**Big Omega** gives lower bound

