--- 
 
# required metadata 
title: " rxSqlServerDropTable " 
description: " Execute a SQL statement that drops a table or checks for existence. " 
keywords: "RevoScaleR, rxSqlServerDropTable, rxSqlServerTableExists, file" 
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
 
 
 
 #`rxSqlServerDropTable`:  rxSqlServerDropTable 

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
Execute a SQL statement that drops a table or checks for existence.
 
 
 ##Usage

```   
  rxSqlServerTableExists(table, connectionString = NULL)
  rxSqlServerDropTable(table, connectionString = NULL)
 
```
 
 
 ##Arguments

   
    
 ### `table`
  character string specifying a table name or an [RxSqlServerData](rxsqlserverdata.md)data source that has the `table` specified.  
  
  
    
 ### `connectionString`
 `NULL` or character string specifying the connection string.  If `NULL`, the connection string from the currently  active compute context will be used if available.  
  
  
    
 ### ` ...`
  Additional arguments to be passed through.  
  
  
 
 
 ##Details
 
An SQL query is passed to the ODBC driver.
 
 
 ##Value
 
`rxSqlServerTableExists` returns `TRUE` is the table exists, `FALSE` otherwise.
`rxSqlServerDropTable` returns `TRUE` is the table is successfully dropped, 
`FALSE` otherwise (for example, if the table did not exist).
 
 
 ##Author(s)
 
Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)

 
 
 
 
 ##See Also
 
[RxInSqlServer](rxinsqlserver.md),
[RxSqlServerData](rxsqlserverdata.md).
   
 ##Examples

 ```
   
  ## Not run:
 
# With an RxInSqlServer active compute context
tempTable <- "rxTempTest"
if (rxSqlServerTableExists(tempTable)) rxSqlServerDropTable(tempTable)
 ## End(Not run) 
  
 
```
 
 
 
 
