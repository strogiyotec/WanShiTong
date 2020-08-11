## Theory
Compare stack with synchronized and CAS
1. On single thread CAS wins.
2. On multiple threads synchronized wins.

If we add `Blackhole.consume` to both algorithms then no difference between single thread.\
The code for benchmark
```
@Benchmark
void test(){
 stack.push()
 stack.pop();
}
```
Let's change code to increase amount of reads
```
@Benchmark
void test(){
 stack.push()
repeat(10){stack.peek}
}
```
In this case perfomance of both algorithms is the same.

>Lock free algorithms better for active reads
>If we increase amount of reads from 10 to 100 then
>Lock free out perform lock

Usually it's not just stack read, we usually do some operations after read.
In this case , lock free outperform synchronization because threads are busy 
