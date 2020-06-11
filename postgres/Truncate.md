## Truncate

**Truncate** removes all rows from a set of tables. It's faster on huge amount of data because it doesn't scan tables. It reclaim disk space immediately. 

- Truncate doesn't have delete condition
- Truncate can restart related sequences (**RESTART IDENTITY**)
- Truncate can delete referenced tables as well (**CASCADE**)
- Truncate is not MVCC safe  
   - Truncate lock whole table for all operations
   - Parallel transactions will see empty row  
			
		  Let's	say two serializable transactions, first one truncate table and second one read.
		  Second one wil be locked and then if first transaction is commited 
		  then second one will see empty table. While in delete case second transaction will
		  see old snapshot
- Truncate is Transaction save.  

		If transaction that performs truncate was rollbacked , 
		then Truncate is rollbacked as well.
- Truncate create new page completely
