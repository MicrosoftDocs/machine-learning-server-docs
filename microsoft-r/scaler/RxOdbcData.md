---

# required metadata
title: "RxOdbcData ScaleR Function (RevoScaleR package)"
description: "RxOdbcData function in the RevoScaleR package in Microsoft R."
keywords: "RevoScaleR, ScaleR, RxOdbcData"
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "09/14/2016"
ms.topic: "article"
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

#RxOdbcData function (RevoScaleR)
This is the main generator for S4 class **RxOdbcData**, which extends **RxDataSource**.

## Usage
~~~~
RxOdbcData(table = NULL, sqlQuery = NULL, dbmsName = NULL,  databaseName = NULL,
           useFastRead = NULL, connectionString = NULL,
           rowBuffering = TRUE, returnDataFrame = TRUE, stringsAsFactors = FALSE,
           colClasses = NULL, colInfo = NULL, rowsPerRead = 500000,
           verbose = 0, writeFactorsAsIndexes, ...)

## S3 method for class 'RxOdbcData'
head(x, n = 6L, reportProgress = 0L, ...)
## S3 method for class 'RxOdbcData'
tail(x, n = 6L, addrownums = TRUE, reportProgress = 0L, ...)  
~~~~

## Argument

The following table shows the arguments to **rxImport** in order and their default values.

|Parameter | Description|
| --------- | --------- |
|table |NULL or character string specifying the table name. Cannot be used with `sqlQuery`.|
|sqlQuery |NULL or character string specifying a valid SQL select query. Cannot be used with `table`.|
|dbmsName |NULL or character string specifying the Database Management System (DBMS) name.|
|databaseName |NULL or character string specifying the name of the database.|
|useFastRead |Logical specifying whether or not to use a direct ODBC connection. On Linux systems, this is the only ODBC connection available.|
|connectionString |NULL or character string specifying the connection string.|
|rowBuffering |Logical specifying whether or not to buffer rows on read from the database. If you are having problems with your ODBC driver, try setting this to FALSE.|
|returnDataFrame |Logical indicating whether or not to convert the result from a list to a data frame (for use in rxReadNext only). If FALSE, a list is returned.|
|stringsAsFactors |Logical indicating whether or not to automatically convert strings to factors on import. This can be overridden by specifying "character" in `colClasses` and `colInfo`. If TRUE, the factor levels will be coded in the order encountered. Since this factor level ordering is row dependent, the preferred method for handling factor columns is to use `colInfo` with specified "levels".|
|colClasses |Character vector specifying the column types to use when converting the data. The element names for the vector are used to identify which column should be converted to which type. <br /> <br /> Allowable column types are: <br /> - "logical" (stored as uchar)<br /> - "integer" (stored as int32)<br />  - "float32" (the default for floating point data for ‘.xdf’ files)<br />  - "numeric" (stored as float64 as in R)<br />  - "character" (stored as string)<br /> - "factor" (stored as uint32)<br />  - "int16" (alternative to integer for smaller storage space)<br />  - "uint16" (alternative to unsigned integer for smaller storage space)<br />  - "Date" (stored as Date, i.e. float64) <br /> <br /> Note for "factor" type, the levels will be coded in the order encountered. Since this factor level ordering is row dependent, the preferred method for handling factor columns is to use colInfo with specified "levels". <br /> <br /> Note that equivalent types share the same bullet in the list above; for some types we allow both ‘R-friendly’ type names, as well as our own, more specific type names for ‘.xdf’ data. <br /> <br /> Note also that specifying the column as a “factor” type is currently equivalent to “string” - for the moment, if you wish to import a column as factor data you must use the colInfo argument, documented below. |
|colInfo| List of named variable information lists. Each variable information list contains one or more of the named elements given below. The information supplied for `colInfo` overrides that supplied for `colClasses`.<br /><br />Currently available properties for a column information list are:<br />`type` Character string specifying the data type for the column. See `colClasses` argument description for the available types.<br />`newName` character string specifying a new name for the variable.<br />`description` Character string specifying a description for the variable.<br />`levels` Character vector containing the levels when type = "factor". If the levels property is not provided, factor levels will be determined by the values in the source column. If levels are provided, any value that does not match a provided level will be converted to a missing value.<br />`newLevels` New or replacement levels specified for a column of type “factor”. It must be used in conjunction with the levels argument. After reading in the original data, the labels for each level will be replaced with the `newLevels`.<br />`low` The minimum data value in the variable (used in computations using the F() function.<br />`high` The maximum data value in the variable (used in computations using the F() function.|
|rowsPerRead |Number of rows to read at a time.|
|verbose |Integer value. If 0, no additional output is printed. If 1, information on the odbc data source type (`odbc` or `odbcFast`) is printed.|
|writeFactorsAsIndexes |Logical. If TRUE, when writing to an **RxOdbcData** data source, underlying factor indexes will be written instead of the string representations.|
|... |Additional arguments to be passed directly to the underlying functions.|
|x |An **RxOdbcData** object.|
|n |Positive integer. Number of rows of the data set to extract.|
|addrownums |Logical. If TRUE, row numbers will be created to match the original data set.|
|reportProgress |Integer value with options: <br />- 0: no progress is reported. <br />- 1: the number of processed rows is printed and updated. <br />- 2: rows processed and timings are reported. <br />- 3: rows processed and all timings are reported. |

## Remarks
The `tail` method is not functional for this data source type and will report an error.

If you are using **RxOdbcData** with a Teradata database and experience difficulty, try the [RxTeradata](rxTeradata.md) data source instead. The **RxTeradata** data source permits finer control of the underlying Teradata options.

## Return Value
Object of class **RxOdbcData**.

## Examples
~~~~
## Not run:
# Create an ODBC data source for a SQLite database
claimsSQLiteFileName <- file.path(rxGetOption("sampleDataDir"), "claims.sqlite")
connectionString <-
  paste("Driver={SQLite3 ODBC Driver};Database=", claimsSQLiteFileName,
        sep = "")
claimsOdbcSource <-
  RxOdbcData(sqlQuery = "SELECT * FROM claims",
             connectionString = connectionString)

# Create an xdf file name
claimsXdfFileName <- file.path(tempdir(), "importedClaims.xdf")

# Import the data into the xdf file
rxImport(claimsOdbcSource, claimsXdfFileName, overwrite = TRUE)

# Read xdf file into a data frame
claimsIn <- rxDataStep(inData = claimsXdfFileName)
head(claimsIn)

# Clean-up: delete the new file
file.remove( claimsXdfFileName)

## End(Not run)
~~~~

## See Also
[Comparison of rx Functions and CRAN R Functions](compare-base-r-scaler-functions.md)

[ScaleR Functions for Working with SQL Server Data](functions-for-sql-server-data.md)
