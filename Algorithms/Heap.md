Heap
=======

## Heap Definition

* **Heap** - for every node **v** other than root , the key stored at T is greater than key stored at v's parent 

* In heap,every node is smaller that it's children(both)
* `heap_key[i] < heap_key[2i+1] if 2i+1 < n` where **n** is the number of elements
* `heap_key[i] < heap[2i+2] if 2i+2 < n`

## Insert example

![Heap insert example](heap.png)

## Delete example


![Heap delete example](heap_delete.png)

1. Move the last element to the root(the last is 21)
2. Call max heapify in root

## Build heap complexity

In order to build heap from array
1. Insert all elements from array to tree(just one by one)
2. Starting from array.length/2 to 0 call max heapify on each tree[i]
3. Time complexity is O(n)


[Fibonacci heap](FibonacciHeap)
