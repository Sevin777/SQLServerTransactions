# SQL Server Transactions *(.NET Usage)*

### Isolation Levels
* [Microsoft Documentation](https://msdn.microsoft.com/en-us/library/ms709374(v=vs.85).aspx)
* Deafult is READ COMMITTED

### Manual transaction blocking tests
* Start a transaction
* Perform an insert operation
    * *Note: performing update operation appears to have exact same results in preliminary testing*
    * *Note: I think that performing a delete operation would also have the same results, but need to test*
    * *Note: performing a select operation has DIFFERENT results, need to test more (not super relevant due to lack of need to select in a manual transaction)*
* On a separate transaction: select the item that was just inserted with isolation level read uncommitted
    * Result: item found
* On a separate transaction: select the item that was just inserted with normal isolation level
    * Result: query times out, it is blocked by the open transaction
* On a separate transaction: insert another item in the same table
    * Result: insert works fine (not blocked by open transaction)
* On a separate transaction: run ANY update query on the same table
    * Result: query times out, it is blocked by the open transaction. This occurs regardless of transaction isolation level, and regardless of the scope of the update query (even if the update query is only updating one item and it would not affect the open transaction)


### Modification with manual transaction and program is killed or exception is thrown before commit called
* Start a transaction
* Perform an insert operation
    * *Note: update and delete likely have the exact same results, have not tested*
* (for testing) send `WAITFOR DELAY '00:01' /* wait for 1 minute */`
* Kill the app pool, or throw an exception
* Result: Transaction rolled back automatically


### Nested transactions
* Committing inner transactions is ignored by the SQL Server Database Engine. The transaction is either committed or rolled back based on the action taken at the end of the outermost transaction. If the outer transaction is committed, the inner nested transactions are also committed. If the outer transaction is rolled back, then all inner transactions are also rolled back, regardless of whether or not the inner transactions were individually committed.
* See https://technet.microsoft.com/en-us/library/ms189336%28v=sql.105%29.aspx?f=255&MSPPError=-2147217396


### Naming the transaction
* Naming the transaction adds no value unless you are working with nested transactions that need to be dealt with in a complex way
* see http://stackoverflow.com/a/1280701/1571103
