## Pointer vs value 
### Initialize
1. Using new keyword or & sign
2. * operator can be used to dereference a pointer which means getting the value at address stored in pointer
3. & used to get an address of the variable 
```
a :=2
b :=&a
fmt.printl(*b)//2
```
> * can be used to store new value in given address
```
 a:= 2
 b =&a
 *b = 3
 fmt.Println(a)//3
```
