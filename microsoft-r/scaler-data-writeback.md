---

# required metadata
title: "Write back to an ODBC data source using rxDataStep (Microsoft R)"
description: "Post variables or observations from an R data frame back to an RxOdbcData source in Microsoft R."
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "05/25/2017"
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

# Write back to an ODBC data source using rxDataStep (Microsoft R)

If you generate new variables or observations in an R data frame, you can write back to the data source using the RevoScaleR **rxDataStep** function, given sufficient permissions and an **RxOdbcData** data source, such as a SQL Server table. 

An **RxOdbcData** object supports local compute context only, which means that when you create the object, any read or write operations are executed by R Server on the local machine.

Requirements for writing back to the data source include:

+ Database is local 
+ RevoScaleR is local 
+ Write permissions on the database 
+ Write-back data is loaded as an **RxOdbcData** object 
+ Write-back function is **rxDataStep** 

Depending on the script, you can add a new table, overwrite an existing table, add a new column (variable), or add new observations (rows). 

Instructions for uploading the sample data used in the following demonstration exercises can be found in [Sample data](scaler-user-guide-sample-data.md#demo-sql-data).

## Add a new table

## Overwrite an existing table

## Add a variable

## Add observations


## See Also

 [Import ODBC data in Microsoft R](scaler-data-odbc.md)	
 [Import SQL Server data in Microsoft R](scaler-data-sql.md)