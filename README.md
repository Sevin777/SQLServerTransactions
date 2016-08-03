# SQL Server Transactions *(.NET Usage)*

### Isolation Levels
* [Microsoft Documentation](https://msdn.microsoft.com/en-us/library/ms709374(v=vs.85).aspx)
* Deafult is READ COMMITTED

### Insert with manual transaction
* Start a transaction
* Perform an insert operation
* On a separate transaction: select the item that was just inserted with isolation level read uncommitted
    * Result: item found
* On a separate transaction: select the item that was just inserted with normal isolation level
    * Result: query times out, it is blocked by the open transaction


### Modification with manual transaction and program is killed or exception is thrown before commit called
* Start a transaction
* Perform an insert operation
* (for testing) send `WAITFOR DELAY '00:01' /* wait for 1 minute */`
* Kill the app pool, or throw an exception
* Result: Transaction rolled back automatically


### Nested transactions
* Committing inner transactions is ignored by the SQL Server Database Engine. The transaction is either committed or rolled back based on the action taken at the end of the outermost transaction. If the outer transaction is committed, the inner nested transactions are also committed. If the outer transaction is rolled back, then all inner transactions are also rolled back, regardless of whether or not the inner transactions were individually committed.
* See https://technet.microsoft.com/en-us/library/ms189336%28v=sql.105%29.aspx?f=255&MSPPError=-2147217396


### Naming the transaction
* Naming the transaction adds no value unless you are working with nested transactions that need to be dealt with in a complex way
* see http://stackoverflow.com/a/1280701/1571103
