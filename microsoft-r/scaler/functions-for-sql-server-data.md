---

# required metadata
title: "ScaleR Functions"
description: "ScaleR Functions"
keywords: "RevoScaleR, ScaleR"
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

#ScaleR Functions for Working with SQL Server Data

This topic provides an overview of the main ScaleR functions for use with SQL Server, along with comments on their syntax.

For a complete list of ScaleR functions and how to use them, see the [Microsoft R Server](https://msdn.microsoft.com/microsoft-r/index#) reference in the MSDN library. 

## Functions for working with SQL Server Data Sources
The following functions let you define a SQL Server data source. A data source object is a container that specifies a connection string together with the set of data that you want, defined either as a table, view, or query. Stored procedure calls are not supported.  

In addition to defining a data source, you can execute DDL statements from R, if you have the necessary permissions on the instance and database. 
+ [RxSqlServerData](RxSqlServerData.md) - Define a SQL Server  data source object
+ [rxSqlServerDropTable](rxSqlServerDropTable.md) - Drop a SQL Server  table
+ [rxSqlServerTableExists](rxSqlServerTableExists.md) - Check for the existence of a database table or object
+ [rxExecuteSQLDDL](rxExecuteSQLDDL.md) - Execute a command to define, manipulate, or control SQL data, but not return data  

## Functions for Defining or Managing a Compute Context
The following functions let you define a new compute context, switch compute contexts, or identify the current compute context.
+ [RxComputeContext](rxComputeContext.md) - Create a compute context. 
+ [rxInSqlServer](rxInSqlServer.md) - Generate a SQL Server compute context that lets **ScaleR** functions run in SQL Server R Services.
+ [rxGetComputeContext](rxGetComputeContext.md) - Get the current compute context. 
+ [rxSetComputeContext](rxSetComputeContext.md) - Specify which compute context to use. The local compute context is available by default, or you can specify the keyword **local**.

## Functions for Using a Data Source
After you have created a data source object, you can open it to get data, or write new data to it. Depending on the size of the data in the source, you can also define the batch size as part of the data source and move data in chunks. 
+ [rxIsOpen](rxIsOpen.md) - Check whether a data source is available
+ [rxOpen](rxOpen.md) - Open a data source for reading
+ [rxReadNext](rxReadNext.md) - Read data from a source
+ [rxWriteNext](rxWriteNext.md) - Write data to the target
+ [rxClose](rxClose.md) - Close a data source

For more information about working with these ScaleR functions, which can work with data sources other than SQL Server, see [ Microsoft R Server - Getting Started](../microsoft-r-getting-started.md).

## Functions that work with XDF Files
The following functions can be used to create a local data cache in the XDF format. This file can be useful when working with more data than can be transferred from the database in one batch, or more data than can fit in memory.

If you regularly move large amounts of data from a database to a local workstation, rather than query the database repeatedly for each R operation, you can use the XDF file to save the data locally and then work with it in your R workspace, using the XDF file as the cache.

+ `rxImport` - Move data from an ODBC source to the XDF file
+ `RxXdfData` - Create an XDF data object
+ `RxDataStep` - Read data from XDF int a data frame
+ `rxReadXdf` - Reads data from XDF into a data frame

For an example of how XDF files are used, see this tutorial:  [Data Science Deep Dive - Using the ScaleR Functions](https://msdn.microsoft.com/en-us/library/mt637368.aspx)

For more information about these ScaleR functions, which can be used to transfer data from many different sources, see[ Microsoft R Server - Getting Started](../microsoft-r-getting-started.md).

## See Also
[Comparison of ScaleR and CRAN R Functions](compare-base-r-scaler-functions.md)
