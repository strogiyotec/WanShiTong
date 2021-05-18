## Caches
Multiple level of caches.

## Cache line
When cpu stores values in it's cache then it stores a thing called cache line which is<br>
the smallest unit and usually is 64 or 128 Bytes

## False sharing
When two cores store the same cache line and one of them is modifing <br>
variable from this line, then all other cores will mark an entire cache line<br>
as dirty and reread it from main memory.<br>

## Contented annotation
In order to add a padding for field or class we can use Contented annotation

```
@Contended
    public static class ContendedTest2 {
        private Object plainField1;
        private Object plainField2;
        private Object plainField3;
        private Object plainField4;
    }
```
This will make a padding to entire object

```
 public static class ContendedTest1 {
        @Contended
        private Object contendedField1;
        private Object plainField1;
        private Object plainField2;
        private Object plainField3;
        private Object plainField4;
    }
```
This will put contFiedld1 into a separate cache line



## Resources
1. [About Java](https://alidg.me/blog/2020/5/1/false-sharing)
2. [Volatile vs Cache coherency](https://stackoverflow.com/questions/65037547/the-volatile-keyword-and-cpu-cache-coherence-protocol)
3. [Another volatile thing](https://stackoverflow.com/questions/37348390/java-jit-compiler-optimizations-is-jit-consistent-in-respect-to-volatile-varia)
