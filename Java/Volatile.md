# Volatile

**Thread interference** - data corruption when two threads execute the same code

- Read and writes are atomic for reference variables and for most primitive variables (except for double and long)   
- Read and writes are atomic for all variables declared as volatile
- Atomic actions can't be interleaved, 
- Any write to volatile variable establish happens before relationship with other threads.Changes to volatile variables are always visible to other threads

