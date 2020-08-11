# Volatile

**Thread interference** - data corruption when two threads execute the same code

- Read and writes are atomic for reference variables and for most primitive variables (except for double and long)   
- Read and writes are atomic for all variables declared as volatile
- Atomic actions can't be interleaved, 
- Any write to volatile variable establish happens before relationship with other threads.Changes to volatile variables are always visible to other threads


## Volatile happens before

Volatile variables guarantees happens before relationship between writes and reads

## Writes
```
nonVol1 = 200
nonVol2 = 300
vol = 400
```
When we assign new value to vol then this value is synch with main memory and **values of two non volatile variables will be synch with main memory**

### Reordering
In example above nonVol 1 and 2 are not alowed to happen after write to **vol**.
However they can be reordered before **vol**
```
nonVol2 = 300
nonVol1 = 200
vol =400
```

## Reads
When you read volatile then you read directly from the main memory.**Moreover**\
all variable reads after volatile read are also read from main memory
```
int vol = this.vol;
int nonVol1 = this.nonVol1;
int nonVol2 = this.nonVol2;
```
Here when we read **this.vol** we read it from memory and we read both non volatile variables from memory as well

### Reordering
A read from volatile variable will happen before any subsequent non volatile reads. It meams that when we read from **this.vol** then all reads belove can't be reordered to happen before volatile read. So this reordering in invalid
```
int nonVol1 = this.nonVol1;
int vol = this.vol;
int nonVol2 = this.nonVol2;
```
But reordering after volatile read is **valid**
```
int vol = this.vol;
int nonVol2 = this.nonVol2;
int nonVol1 = this.nonVol1;
```

## Example

```
class VolatileExample {
  int x = 0;
  volatile boolean v = false;
  public void writer() {
    x = 42;
    v = true;
  }

  public void reader() {
    if (v == true) {
      //uses x - guaranteed to see 42.
    }
  }
}
```
Here when **v** is assigned to true then **reader()** will see it **AND** reader will see new value for **x** which is 42

## Referrences
1. https://www.cs.umd.edu/users/pugh/java/memoryModel/jsr-133-faq.html#volatile
2. http://tutorials.jenkov.com/java-concurrency/java-happens-before-guarantee.html#the-java-volatile-visibility-guarantee 
