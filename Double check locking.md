## Double check locking

**What is it** ? 
```
class Help{
	static Help help;
	static get(){
		if(help== null){
			synchronized(Help.class){
				if(help == null){
					help = new Help();
				}
			}
		}
		return help;
	}
}
```
**Why it doesn't work ?**

When **Help** is initialized then the following steps happen

1. **local_ptr = malloc(sizeof(Help))**
2. **help = local_ptr**
3. **Help::ctor(help)**

Between step one and three another thread could call **get()** and first condition `if(help == null)` will be false because reference already exist but **Constructor** hasn't been called which means that thread could see object in invalid state. 

### Solution

When field is declared as volatile then **read/write** operations are atomic which means object will be fully constructed. For example first and second steps were executed and new thread calls
**get()** method, in this case first condition will return **true** and second thread will wait on synch block

