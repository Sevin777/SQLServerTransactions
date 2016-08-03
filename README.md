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


## Modification with manual transaction and program is killed or exception is thrown before commit called
* Start a transaction
* Perform an insert operation
* (for testing) send `WAITFOR DELAY '00:01' /* wait for 1 minute */`
* Kill the app pool, or throw an exception
* Result: Transaction rolled back automatically
* 

##Naming the transaction
* Naming the transaction adds no value unless you are working with nested transactions that need to be dealt with in a complex way
* see http://stackoverflow.com/a/1280701/1571103
