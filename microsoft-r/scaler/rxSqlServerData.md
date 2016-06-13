---

# required metadata
title: "ScaleR Functions RxSqlServerData"
description: "ScaleR Functions: RxSqlServerData"
keywords: "RevoScaleR, ScaleR, RxSqlServerData"
author: "j-martens"
manager: "Paulette.McKay"
ms.date: "06/13/2016"
ms.topic: "article"
ms.prod: "microsoftr"
ms.service: ""
ms.assetid: ""

# optional metadata
ROBOTS: ""
audience: ""
ms.devlang: ""
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.technology: ""
ms.custom: ""

---

#RxSqlServerData

Generates a SQL Server data source object. 

## Usage

`RxSqlServerData(table = NULL, sqlQuery = NULL, connectionString = NULL, rowBuffering = TRUE, returnDataFrame = TRUE, stringsAsFactors = FALSE, colClasses = NULL, colInfo = NULL, rowsPerRead = 50000, verbose = 0, useFastRead = TRUE, server = NULL, databaseName = NULL, user = NULL, password = NULL, writeFactorsAsIndexes = FALSE) `

## Arguments

The following table shows the arguments to RxSqlServerData in order and their default values.


|Parameter | Default |  Description|
| --------- | --------- | --------- |
|_table_ | NULL| Specify either a table name or view name.|
|_sqlQuery_| NULL| If you do not specify a table or view, provide a valid SQL statement.  |
|_connectionString_|NULL|A string or string variable that defines a valid connection string.|
|_rowBuffering_ |TRUE | Indicates whether buffering should be used during reads. If you are having problems with your ODBC driver, try setting this to ‘FALSE’.|
|_returnDataFrame_ |TRUE | Indicates whether the result should be converted from to a data frame. All results returned by SQL Server are data frames.|
|_stringsAsFactors_ |FALSE| Indicates whether strings should be converted to factors on import. By default, SQL Server treats strings as factors.|
|_colClasses_ |NULL|  A string specifying the data types for each of the columns returned from the SQL Server. For information about how to map SQL Server data types to R data types, see [Working with R Data Types](https://msdn.microsoft.com/en-us/library/mt590948.aspx). |
|_colInfo_ |NULL| A list containing the names of variables.|
|_rowsPerRead_ |50000| The number of rows to read in each batch.|
|_verbose_ |0| Indicates the level of ODBC error reporting. If 0, no progress is reported. A variety of options are available.|
|_useFastRead_| TRUE| Indicates whether to use the FastRead option.
|server |NULL| The name of the SQL Server instance, or the IP address. If you specify the server name as part of the connection string, you can omit this argument.
|_databaseName_| NULL| Specify the database name, if not already included in the connection string. |
|_user_ |NULL| Specify a SQL Server login or Windows user name.
|_password_| NULL| For SQL logins, provide the password associated with the login. Passwords should not be provided for Windows Integrated authentication; specify `Trusted_Connection=true`.|
|_writeFactorsAsIndexes_| FALSE| Indicates whether columns treated as factors should be indexed. |

>[!NOTE]
> 
> If values are provided using the _user_ or _password_ arguments, the values take precedence over equivalent information provided within the _connectionString_ argument. For example, if you specify a user name as part of the connection string, you can override it by providing a value in the _user_ argument.


## Remarks
This is the main generator for the class RxSqlServerData, which extends RxDataSource.

The object is prepared but not used until you use a command that begins to load the data or otherwise process it.

## Return Value
An object of class RxSqlServerData. 

## Example

The following example defines a SQL Server data source using a SQL login and password. The `RxSqlServerData` function instantiates the data source object but does not populate it until the `rxImport` function is called. Then the data is read in chunks from the database, using the predefined batch size, and written to a local XDF file.
~~~~
     
     # Create an RxSqlServerData data source
     
     sqlServerConnString <- "SERVER=RTest01;DATABASE=TestDatabase;UID=DBUser;PWD=DBUserPassword;"
     
     dsSqlServerData <- RxSqlServerData(sqlQuery = "SELECT * FROM MyData",
                                    connectionString = sqlServerConnString)
        
     # Create an xdf file name
     localXdfFileName <- file.path(tempdir(), "importedData.xdf")
     
     # Import the data into the xdf file
     rxImport(dsSqlServerData, localXdfFileName, overwrite = TRUE)
     
     # Read xdf file into a data frame
     MyDataIn <- rxDataStep(localXdfFileName)
     head(MyDataIn)
     
~~~~

The following example defines a connection string using Windows integrated authentication.

~~~~
# Create the connection string
instance_name <- "type instance name here";
database_name <- "type database name here";
myConnString <- paste("Driver= SQL Server;Server=",instance_name, ";Database=",database_name,";Trusted_Connection=true;",sep="");

# Set other variables used to define the compute context
sqlWait = TRUE;
sqlConsoleOutput = TRUE;

# Create the compute context
cc <-RxInSqlServer
~~~~

## See Also
[Comparison of rx Functions and CRAN R Functions](compare-base-r-scaler-functions.md)

[ScaleR Functions for Working with SQL Server Data](functions-for-sql-server-data.md)