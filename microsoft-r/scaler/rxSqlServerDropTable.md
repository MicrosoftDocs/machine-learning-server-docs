#rxSqlServerDropTable

Executes a SQL statement that deletes an existing table.  

## Usage

`rxSqlServerDropTable(table, connectionString)`

## Arguments

_table_: A string specifying a table name or an RxSqlServerData data source that uses the _table_ argument.

_connectionString_: A string specifying the connection information.  If NULL, the connection string from the currently active compute context will be used.


## Remarks
A SQL DDL statement is prepared and passed to the ODBC driver.

## Return Value
TRUE if the table is successfully dropped; FALSE if the table did not exist or could not be dropped.

## Example

The following example checks whether a table exists and deletes the table if it does exist. 
~~~~
if (rxSqlServerTableExists(tempTable)) rxSqlServerDropTable(tempTable)
~~~~


## See Also
[Comparison of rx Functions and CRAN R Functions](compare-base-r-scaler-functions.md)

[ScaleR Functions for Working with SQL Server Data](functions-for-sql-server-data.md)