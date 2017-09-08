--- 
 
# required metadata 
title: "RxSqlServerData function (RevoScaleR) | Microsoft Docs" 
description: " This is the main generator for S4 class RxSqlServerData, which extends RxDataSource. " 
keywords: "(RevoScaleR), RxSqlServerData, head.RxSqlServerData, tail.RxSqlServerData, database, connection" 
author: "heidisteen" 
manager: "jhubbard" 
ms.date: "09/07/2017" 
ms.topic: "reference" 
ms.prod: "microsoft-r" 
ms.service: "" 
ms.assetid: "" 
 
# optional metadata 
ROBOTS: "" 
audience: "" 
ms.devlang: "" 
ms.reviewer: "" 
ms.suite: "" 
ms.tgt_pltfrm: "" 
ms.technology: "r-server" 
ms.custom: "" 
 
--- 
 
 
 
 
 #RxSqlServerData: Generate SqlServer Data Source Object 
 ##Description
 
This is the main generator for S4 class RxSqlServerData, which extends RxDataSource.
 
 
 ##Usage

```   
  RxSqlServerData(table = NULL, sqlQuery = NULL, connectionString = NULL, 
             rowBuffering = TRUE, returnDataFrame = TRUE, stringsAsFactors = FALSE, 
             colClasses = NULL, colInfo = NULL, rowsPerRead = 50000, verbose = 0,
             useFastRead = TRUE, server = NULL, databaseName = NULL, 
             user = NULL, password = NULL, writeFactorsAsIndexes = FALSE) 
             
 ## S3 method for class `RxSqlServerData':
head  (x, n = 6L, reportProgress = 0L,   ...  )
 ## S3 method for class `RxSqlServerData':
tail  (x, n = 6L, addrownums = TRUE, reportProgress = 0L,   ...  )
 
```
 
 
 ##Arguments

   
    
 ### `table`
 `NULL` or character string specifying the table name. Cannot be used with `sqlQuery`. 
  
  
    
 ### `sqlQuery`
 `NULL` or character string specifying a valid SQL select query. Cannot contain hidden characters such as tabs or newlines.  Cannot be used with `table`. If you want to use `TABLESAMPLE` clause in `sqlQuery`, set `rowBuffering` argument to `FALSE`. 
  
  
    
 ### `connectionString`
 `NULL` or character string specifying the connection string. 
  
  
    
 ### `rowBuffering`
 logical specifying whether or not to buffer rows on read from the database. If you are having problems with your ODBC driver,  try setting this to `FALSE`. 
  
  
    
 ### `returnDataFrame`
 logical indicating whether or not to convert the result from a list to a data frame (for use in `rxReadNext` only). If `FALSE`,  a list is returned. 
  
  
    
 ### `stringsAsFactors`
 logical indicating whether or not to automatically convert strings to factors on import. This can be overridden by specifying `"character"` in `colClasses` and `colInfo`. If `TRUE`, the factor levels will be coded in the order encountered. Since this factor level ordering is row dependent, the preferred method for handling factor columns is to use `colInfo` with specified `"levels"`. 
  
  
    
 ### `colClasses`
 character vector specifying the column types to use when converting the data. The element names for the vector are used to identify which column should be converted to which type.   
*   Allowable column types are:  
   *   `"logical"` (stored as `uchar`), 
   *   `"integer"` (stored as `int32`), 
   *   `"float32"` (the default for floating point data for .xdf files), 
   *   `"numeric"` (stored as `float64` as in R), 
   *   `"character"` (stored as `string`), 
   *   `"factor"` (stored as `uint32`), 
   *   `"int16"` (alternative to integer for smaller storage space), 
   *   `"uint16"` (alternative to unsigned integer for smaller storage space), 
   *   `"Date"` (stored as Date, i.e. `float64`) 
 
*   Note for `"factor"` type, the levels will be coded in the order encountered. Since this factor level ordering is row dependent, the preferred method for handling factor columns is to use `colInfo` with specified `"levels"`. 
*   Note that equivalent types share the same bullet in the list above; for some types we allow both 'R-friendly' type names, as well as our own, more specific type names for .xdf data. 
*   Note also that specifying the column as a "factor" type is currently equivalent to "string" - for the moment, if you wish to import a column as factor data you must use the `colInfo` argument, documented below. 
  
  
  
    
 ### `colInfo`
 list of named variable information lists. Each variable information list contains one or more of the named elements given below. The information supplied for `colInfo` overrides that supplied for `colClasses`.  
*   Currently available properties for a column information list are:  
* `type` - character string specifying the data type for the column. See `colClasses` argument description for the available types. Specify `"factorIndex"` as the `type` for 0-based factor indexes. `levels` must also be specified.  
* `newName` - character string specifying a new name for the variable.   
* `description` - character string specifying a description  for the variable.   
* `levels` - character vector containing the levels when `type = "factor"`.  If the levels property is not provided, factor levels will be determined by the values in the source column. If levels are provided, any value that does not match a provided level will be converted to a missing value.  
* `newLevels` - new or replacement levels  specified for a column of type "factor". It must be used in conjunction with the `levels` argument.  After reading in the original data, the labels for each level will be replaced with the `newLevels`.  
* `low` - the minimum data value in the variable (used in computations using the `F()` function.  
* `high` - the maximum data value in the variable (used in computations using the `F()` function.  
 
  
  
  
    
 ### `rowsPerRead`
 number of rows to read at a time. 
  
  
    
 ### `verbose`
 integer value. If `0`, no additional output is printed.  If `1`, information on the odbc data source type (`odbc` or `odbcFast`) is printed. 
  
  
    
 ### ` ...`
 additional arguments to be passed directly to the underlying functions. 
   
   
    
 ### `x`
 an `RxSqlServerData` object 
  
   
     
 ### `n`
 positive integer. Number of rows of the data set to extract. 
  
   
     
 ### `addrownums`
 logical. If `TRUE`, row numbers will be created to match the original data set. 
  
   
     
 ### `reportProgress`
 integer value with options:  
   *   `0`: no progress is reported. 
   *   `1`: the number of processed rows is printed and updated. 
   *   `2`: rows processed and timings are reported. 
   *   `3`: rows processed and all timings are reported. 
  
  
   
    
 ### `useFastRead`
 logical specifying whether or not to use a direct ODBC connection. On Linux systems, this is the only ODBC connection available. 
  
  
    
 ### `server`
 Target SQL Server instance. 	 Can also be specified in the connection string with the `Server` keyword. 
  
  
    
 ### `databaseName`
 Database on the target SQL Server instance. 	 Can also be specified in the connection string with the `Database` keyword. 
  
  
    
 ### `user`
 SQL login to connect to the SQL Server instance.   SQL login can also be specified in the connection string with the `uid` keyword. 
  
  
    
 ### `password`
 Password associated with the SQL login. Can also be specified in the connection  string with the `pwd` keyword. 
  
  
     
 ### `writeFactorsAsIndexes`
 logical. If `TRUE`, when writing to an `RxOdbcData` data source, underlying factor indexes will be written instead of the string representations. 
  
  
 
 
 ##Details
 
The `tail` method is not functional for this data source type and will report an error.

The `user` and `password` arguments take precedence over
equivalent information provided within the `connectionString` argument. If, for
example, you provide the user name in both the `connectionString` and `user`
arguments, the one in `connectionString` is ignored. 
 
 
 ##Value
 
object of class RxSqlServerData.
 

 
 
 
 ##References
 
TODO: Update references.
 
 
 ##See Also
 
[RxSqlServerData-class](RxSqlServerData-class.md),
[rxNewDataSource](rxNew.md),
[rxImport](rxImport.md).
   
 ##Examples

 ```
   
  ## Not run:
 
# Create an RxSqlServerData data source

# Note: for improved security, read connection string from a file, such as
# sqlServerConnString <- readLines("sqlServerConnString.txt")

sqlServerConnString <- "SERVER=hostname;DATABASE=RevoTestDB;UID=DBUser;PWD=Password;"

sqlServerDataDS <- RxSqlServerData(sqlQuery = "SELECT * FROM claims",
                               connectionString = sqlServerConnString)
   
# Create an xdf file name
claimsXdfFileName <- file.path(tempdir(), "importedClaims.xdf")

# Import the data into the xdf file
rxImport(sqlServerDataDS, claimsXdfFileName, overwrite = TRUE)

# Read xdf file into a data frame
claimsIn <- rxDataStep(claimsXdfFileName)
head(claimsIn)
 ## End(Not run) 
  
 
```
 
 
 
