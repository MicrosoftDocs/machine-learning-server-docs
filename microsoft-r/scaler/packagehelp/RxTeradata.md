--- 
 
# required metadata 
title: "Generate Teradata Data Source Object" 
description: " This is the main generator for S4 class RxTeradata, which extends RxDataSource. " 
keywords: "RevoScaleR, RxTeradata, head.RxTeradata, tail.RxTeradata, database, connection" 
author: "heidisteen" 
manager: "jhubbard" 
ms.date: "04/18/2017" 
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
 
 
 
 
 #`RxTeradata`: Generate Teradata Data Source Object

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
This is the main generator for S4 class RxTeradata, which extends RxDataSource.
 
 
 ##Usage

```   
  RxTeradata(table = NULL, sqlQuery = NULL, dbmsName = NULL, databaseName = NULL, 
             connectionString = NULL, user = NULL,  password = NULL, teradataId = NULL,
             rowBuffering = TRUE, trimSpace = NULL, returnDataFrame = TRUE, stringsAsFactors = FALSE, 
             colClasses = NULL, colInfo = NULL, rowsPerRead = 500000, verbose = 0,
             tableOpClause = NULL, writeFactorsAsIndexes = FALSE,
               ...  ) 
             
 ## S3 method for class `RxTeradata':
head  (x, n = 6L, reportProgress = 0L,   ...  )
 ## S3 method for class `RxTeradata':
tail  (x, n = 6L, addrownums = TRUE, reportProgress = 0L,   ...  )             
 
```
 
 
 ##Arguments

   
    
 ### `table`
 `NULL` or character string specifying the table name. Cannot be used with `sqlQuery`. 
  
  
    
 ### `sqlQuery`
 `NULL` or character string specifying a valid SQL select query. Cannot contain hidden characters such as tabs or newlines.  Cannot be used with `table`.  When using the data source in the `RxInTeradata` compute context, this query must be a valid SQL subquery, and in particular may not contain `ORDER BY` clauses. 
  
  
    
 ### `dbmsName`
 `NULL` or character string specifying the Database Management System (DBMS) name. 
  
  
    
 ### `databaseName`
 `NULL` or character string specifying the name of the database. 
  
  
    
 ### `connectionString`
 `NULL` or character string specifying the connection string. 
  
  
    
 ### `user`
 `NULL` or character string specifying the user name. 
  
  
    
 ### `password`
 `NULL` or character string specifying the password. 
  
  
    
 ### `teradataId`
 `NULL` or character string specifying the teradataId. 
  
  
    
 ### `rowBuffering`
 logical specifying whether or not to buffer rows on read from the database. If you are having problems with your ODBC driver,  try setting this to `FALSE`. 
  
  
    
 ### `trimSpace`
 logical specifying whether or not to trim the white character of string data for reading.  
  
  
    
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
* `type` - character string specifying the data type for the column. See `colClasses` argument description for the available types.  
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
   
  
     
 ### `writeFactorsAsIndexes`
 logical. If `TRUE`, when writing to an `RxOdbcData` data source, underlying factor indexes will be written instead of the string representations. 
  
  
    
 ### ` ...`
 additional arguments to be passed directly to the underlying functions, including the Teradata Export driver. One important attribute that can be passed is the `TD_SPOOLMODE` attribute, which RevoScaleR sets to `"NoSpool"` for efficiency. If you encounter difficulties while extracting data, you might want to specify `TD_SPOOLMODE="Spool"`. To tune performance, use the `TD_MIN_SESSIONS` and `TD_MAX_SESSIONS` arguments to specify the minimum and maximum number of sessions.  If these are not specified, the default values of 1 and 4, respectively, are used. `TD_TRACE_LEVEL` specifies the types of diagnostic messages written by each instance of the driver to an external log file. Setting it to `1` turns off tracing  (the default), `2` activates the tracing function for driver specific activities, `3` activates the  tracing function for interaction with the Teradata Database, `4` activates the  tracing function for activities related to the Notify feature, `5` activates  the tracing function for activities involving the opcommon library, and `7` activates tracing for all of the above activities.  `TD_TRACE_OUTPUT` specifies the  name of the external file used for tracing messages. If a file with the specified  name already exists, that file will be overwritten. `TD_OUTLIMIT` limits the number of rows that the Export driver exports. `TD_MAX_DECIMAL_DIGITS` specifies the maximum number of decimal digits (a value for the maximum precision) to be returned by the database. The default value is 18, and values above that are not supported. See the *Teradata Parallel Transporter Application Programming Interface Programmer Guide* for details on allowable attributes. 
   
   
    
 ### `x`
 an `RxTeradata` object 
  
   
     
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
  
  
  
     
 ### `tableOpClause`
 `NULL` or character string specifying optional  table operator clause(s) to use when running [rxDataStep](../../r-reference/revoscaler/rxdatastep.md) in  [RxInTeradata](../../r-reference/revoscaler/rxinteradata.md) compute context. 
  
 
 
 ##Details
 
The `tail` method is not functional for this data source type and will report an error.

The `RxTeradata` data source behaves differently depending upon the
compute context currently in effect. In a local compute context,
`RxTeradata` uses the Teradata Parallel Transporter's Export driver 
to read large quantities of data quickly from Teradata. In the
`RxInTeradata` compute context, data analysis is performed in-database, so
the data is not moved out of the database but simply queried in-place.

To use the Teradata connector, you need Teradata ODBC drivers and the Teradata 
Parallel Transporter installed on your client using the Teradata 14.10 or
15.00 client installer, 
and you need a high-speed connection (100 Mbps or above) to a Teradata 
appliance running version 14.0 or later. 

The `user`, `password`, and `teradataId` arguments take precedence over
equivalent information provided within the `connectionString` argument. If, for
example, you provide the user name in both the `connectionString` and `user`
arguments, the one in `connectionString` is ignored. 
 
 
 ##Value
 
object of class RxTeradata.
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##References
 
Teradata Corporation (2012). *SQL Fundamentals*.

Teradata Corporation (2012). *Teradata Parallel Transporter Application Programming
Interface Programmer Guide*
 
 
 ##See Also
 
[RxTeradata-class](RxTeradata-class.md),
[RxInTeradata](../../r-reference/revoscaler/rxinteradata.md),
[rxNewDataSource](rxNew.md),
[rxImport](../../r-reference/revoscaler/rximport.md).
   
 ##Examples

 ```
   
  ## Not run:
 
# Create a Teradata data source
# Note: for improved security, read connection string from a file, such as
# teradataConnString <- readLines("tdConnString.txt")

teradataConnString <- "DRIVER=Teradata;DBCNAME=hostname;DATABASE=RevoTestDB;UID=DBUser;PWD=Password;"

rxTeradataDS <- RxTeradata(sqlQuery = "SELECT * FROM RevoTestDB.claims",
                               connectionString = teradataConnString)
   
# Create an xdf file name
claimsXdfFileName <- file.path(tempdir(), "importedClaims.xdf")

# Import the data into the xdf file
rxImport(rxTeradataDS, claimsXdfFileName, overwrite = TRUE)

# Read xdf file into a data frame
claimsIn <- rxDataStep(inData = claimsXdfFileName)
head(claimsIn)
 ## End(Not run) 
  
 
```
 
 
 
