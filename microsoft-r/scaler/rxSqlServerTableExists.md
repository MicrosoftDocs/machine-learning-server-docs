#rxSqlServerTableExists

Returns a Boolean that indicates whether the specified database table exists.

## Usage

`rxSqlServerTableExists(table, connectionString)`

## Arguments

_table_: A string containing a table name or the name of an `RxSqlServerData` data source that specifies a table.

_connectionString_: A string containing connection information.  If NULL, the connection string from the currently active compute context will be used.

## Remarks
A SQL query is prepared and passed to the ODBC driver.

## Return Value
TRUE if the table exists, FALSE if the table does not exists or was not found.

## Example

The following example checks for the existence of a table and drops it if it exists.
~~~~
if (rxSqlServerTableExists(tempTable)) rxSqlServerDropTable(tempTable)
~~~~

## See Also
[Comparison of rx Functions and CRAN R Functions](compare-base-r-scaler-functions.md)

[ScaleR Functions for Working with SQL Server Data](functions-for-sql-server-data.md)