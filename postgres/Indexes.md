> If field that has index wasn't updated then 
> UPDATE doesn't change index as well

## Cordinality
Amount of different values in a columns.
Primary key has higher cordnilaity because each PK is unique

## Bitmap Scan
### Use case 1
Rows number is too big for index scan and too smal for Sequential
1. Let's say we run `select * from users where id > 700` and we have 1000 pages(not rows in table).
In this case Postgres creates an `byte[1000]` then uses indexes to set `byte[1]` to `true`.
1. Then we have an array with pages to fetch.
2. Postgres then sequentially reads page locations from array and retrive them from disk.
3. This is much faster cause array is sorted(sorted means `byte[0]` is first page and so on) and will use Sequential scan

### Use case 2
When Query plan is using multiple indexes
1. `select * where a > 500 and b < 4500`
2. In this case two byte arrays will be created and filled using index filters
3. Then this tow arrays will be merged `first_array[i] = first_array[i] && second_array[i]`
