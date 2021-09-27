## Timestamp with timezone
1. When you insert time with specified offset, offset will convert it to uts, and when you
select it , it will be converted to current session UTC
2. If you want to display time with user's timezone then `SET time zone ""` in the beginning of the transaction using user's timezone

## References
1. [Blog post about time types](https://tapoueh.org/blog/2018/04/postgresql-data-types-date-timestamp-and-time-zones/)
