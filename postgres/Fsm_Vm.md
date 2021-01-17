## Free space map
If dead row is visible to old transactions then it's still stored in table file.
Otherwise it's aarked(by Vaccum) as non richable inside FSM so later on this space can be reused

## Visiblity Map
Contain boolean variable if specific page has dead tuples. If 
it doesn't then this page is not checked by Vacuum
