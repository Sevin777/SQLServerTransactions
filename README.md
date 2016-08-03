# SQL Server Transactions

## Isolation Levels
[Microsoft Documentation](https://msdn.microsoft.com/en-us/library/ms709374(v=vs.85).aspx)
* Deafult is READ COMMITTED

## Insert with manual transaction
* Start a transaction
* Perform an insert operation
* On a separate transaction: select the item that was just inserted with isolation level read uncommitted
    * Result: item found
* On a separate transaction: select the item that was just inserted with normal isolation level
    * Result: query times out, it is blocked by the open transaction


## Modification with manual transaction and program is killed
* Start a transaction
* Perform an insert operation
* (for testing) send `WAITFOR DELAY '00:01' /* wait for 1 minute */`
* Kill the app pool
* Result: Transaction rolled back automatically
