---

# required metadata
title: "rxExecuteSQLDDL - ScaleR Functions"
description: "ScaleR Functions: rxExecuteSQLDDL"
keywords: "RevoScaleR, ScaleR, rxExecuteSQLDDL"
author: "j-martens"
manager: "Paulette.McKay"
ms.date: "06/13/2016"
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

#rxExecuteSQLDDL

Executes a DDL command to define, manipulate, or control SQL data, but not return data.

## Usage

`rxExecuteSQLDDL(src, ...)`

## Arguments

_src_: An RxOdbcData data source object 

Other additional arguments are typically of the type `sSQLString=`, after which you would supply a  string containing a well-formed T-SQL DDL statement. 

For example, you could add create a table, add a column to a table, or truncate a table before inserting new data.

## Return Value
Returns NULL. 
If you need to verify that the DDL statement was executed successfully, you can use a try-catch statement with an ODBC call to check for table names, columns, etc.

## Example
The following example demonstrates how to load data from a text file into a new database table. 

~~~~
# Define the database where DDL statements will be executed
     
conString <- "Driver=SQL Server;Server=localhost;Database=RTest;Uid=tester;pwd=pwd;"
outOdbcDS <- RxOdbcData(table = "NewData", connectionString = conString, useFastRead=TRUE)         

# Open the ODBC connection and execute the DDL statement
rxOpen(outOdbcDS, "w")                       
rxExecuteSQLDDL(outOdbcDS, sSQLString = paste("CREATE TABLE [NewData]([Col1] [int] NULL, [Col2] [char](25) NULL, [Col3] [float] NULL);", sep=""))

# Get the new data from a text file
inTextData <- RxTextData(file = file.path("C:\\Temp"), "newdata.txt"), stringsAsFactors = TRUE, useFastRead = TRUE)
outOdbcDS <- RxOdbcData(table = "NewData",  connectionString = conString, useFastRead=TRUE) 

# Move the data from one data source to another
rxDataStep(inData = inTextData, outFile = outOdbcDS)   

~~~~

## See Also
[Comparison of rx Functions and CRAN R Functions](compare-base-r-scaler-functions.md)

[ScaleR Functions for Working with SQL Server Data](functions-for-sql-server-data.md)