# JIT

### Tiered compilation
In Java 8 by default JVM started in Tiered mode when C1(client) is used first
and then code is compiled using C2(server)




## Hot reload
1. Add these two JVM options `-Xverify:none -XX:TieredStopAtLevel=1`
2. Run app in debug mode, when class is changed press `Ctrl+Shift+F9`


## Code cache
1. InitialCodeCacheSize
2. ReservedCodeCacheSize
3. CodeCacheExpansionSize

Method/Loop is complied if it was executed certain amount of time.\
**-XX:CompileThreshold** - specifies this threashold.
1. 1500 for client
2. 10_000 for server

When method is going to be complied it's placed in the queue
and then each element of this queue is compiled

## Jit optimizations

### Inline
Inline method calls
```
Point p = ...
p.setA(p.getA) // no inline
p.A = ... //inline
```
Methods that are less than **-MaxInlineSize** and that are hot will be inlined

### Deoptimization
Let's say that method has condition which is always true, then JIT will remove this condition.
But if once the condition is false then JIT will deoptimize this method.

## JVM Optimization




## Resources 
1. https://phauer.com/2017/increase-jvm-development-productivity/
2. [Book about jit](https://www.oreilly.com/library/view/java-performance-the/9781449363512/ch04.html)
3. https://docs.oracle.com/javase/8/embedded/develop-apps-platforms/codecache.htm
