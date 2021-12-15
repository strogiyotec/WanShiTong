Header is 12 bytes
1. mark word - 64 bits
    1. 25 - unused
    2. 31 - hashcode First call to System.idendityHashCode() computes the i-hash and stores it into the upper bits of the header
    3. 4 - age
2. klass reference - 4 bytes(or 8 when compression is disabled)


## Resources
1. [In depth](https://www.fatalerrors.org/a/in-depth-understanding-of-java-object-header-mark-word.html)
2. [Inline classes](https://realjenius.com/2021/05/09/value-classes/)
