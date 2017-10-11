---

# required metadata
title: "SQL Server RevoScaleR functions (Machine Learning Server and Microsoft R) | Microsoft Docs"
description: "RevoScaleR functions for a SQL Server compute context"
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "jhubbard"
ms.date: "10/11/2017"
ms.topic: "reference"
ms.prod: "microsoft-r

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
ms.technology: "r-server"
#ms.custom: ""

---

# RevoScaleR Functions for a SQL Server compute context

This topic provides an overview of the main RevoScaleR functions for use with SQL Server, along with comments on their syntax.


## Functions for working with SQL Server Data Sources
The following functions let you define a SQL Server data source. A data source object is a container that specifies a connection string together with the set of data that you want, defined either as a table, view, or query. Stored procedure calls are not supported.  

In addition to defining a data source, you can execute DDL statements from R, if you have the necessary permissions on the instance and database.
+ [RxSqlServerData](rxsqlserverdata.md) - Define a SQL Server  data source object
+ [rxSqlServerDropTable](rxsqlserverdroptable.md) - Drop a SQL Server  table
+ [rxSqlServerTableExists](rxsqlserverdroptable.md) - Check for the existence of a database table or object
+ [rxExecuteSQLDDL](rxexecutesqlddl.md) - Execute a command to define, manipulate, or control SQL data, but not return data  

## Functions for Defining or Managing a Compute Context
The following functions let you define a new compute context, switch compute contexts, or identify the current compute context.
+ [RxComputeContext](rxcomputecontext.md) - Create a compute context.
+ [rxInSqlServer](rxinsqlserver.md) - Generate a SQL Server compute context that lets **RevoScaleR** functions run in SQL Server R Services.
+ [rxGetComputeContext](rxsetcomputecontext.md) - Get the current compute context.
+ [rxSetComputeContext](rxsetcomputecontext.md) - Specify which compute context to use. The local compute context is available by default, or you can specify the keyword **local**.

## Functions for Using a Data Source
After you have created a data source object, you can open it to get data, or write new data to it. Depending on the size of the data in the source, you can also define the batch size as part of the data source and move data in chunks.
+ [rxIsOpen](rxopen-methods.md) - Check whether a data source is available
+ [rxOpen](rxopen-methods.md) - Open a data source for reading
+ [rxReadNext](rxopen-methods.md) - Read data from a source
+ [rxWriteNext](rxopen-methods.md) - Write data to the target
+ [rxClose](rxopen-methods.md) - Close a data source


## Functions that work with XDF Files
The following functions can be used to create a local data cache in the XDF format. This file can be useful when working with more data than can be transferred from the database in one batch, or more data than can fit in memory.

If you regularly move large amounts of data from a database to a local workstation, rather than query the database repeatedly for each R operation, you can use the XDF file to save the data locally and then work with it in your R workspace, using the XDF file as the cache.

+ `rxImport` - Move data from an ODBC source to the XDF file
+ `RxXdfData` - Create an XDF data object
+ `RxDataStep` - Read data from XDF int a data frame

## See Also
[Comparison of RevoScaleR and CRAN R Functions](revoscaler-compared-to-base-r.md)
