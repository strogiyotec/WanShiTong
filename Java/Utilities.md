## CyclicBarrier
**CyclicBarrier** - A CyclicBarrier is a synchronizer that allows a set of threads to wait for each other to reach a common execution point, also called a barrier.
Constructor takes integer which represents how many threads have to call await, if number was reached then all threads continue execution

```
preparedBarrier = new CyclicBarrier(number,
            new Runnable() {
                public void run() {
            // Callback
                }
            });
```
1. The callback will be called when the last thread will call await(). 
2. We can reset CycclicBarrier calling **reset()**

Threads could be interrupted using **BrokenBarrierException** when
1. One of the threads was interrupted
2. reset method was called


