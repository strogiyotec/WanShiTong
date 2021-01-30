## Persist
1. Returns void
2. Doesn't guarantee to set identifier
3. Doesn't call insert ,only after session is commited
4. Two times persist doesn't do anything
5. Persist on detached object throws an exception

## Save
1. Returns id


## Transactional tracks the state
```
@Transactional
void save(){
    var user = repo.get(1);
    user.setName("Almas")//don't need update
}
```
When you return object from transactional method it's in detached mode
>you must not share Hibernate-managed objects between threads.

When hibernate execute reads query it makes a flush which means all dirty changes
will be commited
