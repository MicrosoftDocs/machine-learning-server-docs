--- 

# required metadata 
title: "rxSqlServerDropTable function (revoAnalytics) | Microsoft Docs" 
description: " Execute a SQL statement that drops a table or checks for existence. " 
keywords: "(revoAnalytics), rxSqlServerDropTable, rxSqlServerTableExists, file" 
author: "chuckheinzelman"
ms.author: "charlhe" 
manager: "cgronlun" 
ms.date: 07/15/2019
ms.topic: "reference" 
ms.service: mlserver
ms.assetid: "" 

# optional metadata 
ROBOTS: "" 
audience: "" 
ms.devlang: "" 
ms.reviewer: "" 
ms.suite: "" 
ms.tgt_pltfrm: "" 
#ms.technology: "" 
ms.custom: "" 

--- 



 # rxSqlServerDropTable:  rxSqlServerDropTable  
 ## Description

Execute a SQL statement that drops a table or checks for existence.


 ## Usage

```   
  rxSqlServerTableExists(table, connectionString = NULL)
  rxSqlServerDropTable(table, connectionString = NULL)

```


 ## Arguments



 ### `table`
  character string specifying a table name or an [RxSqlServerData](RxSqlServerData.md)data source that has the `table` specified.  



 ### `connectionString`
 `NULL` or character string specifying the connection string.  If `NULL`, the connection string from the currently  active compute context will be used if available.  



 ### ` ...`
  Additional arguments to be passed through.  




 ## Details

An SQL query is passed to the ODBC driver.


 ## Value

`rxSqlServerTableExists` returns `TRUE` is the table exists, `FALSE` otherwise.
`rxSqlServerDropTable` returns `TRUE` is the table is successfully dropped, 
`FALSE` otherwise (for example, if the table did not exist).


 ## Author(s)

Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)





 ## See Also

[RxInSqlServer](RxInSqlServer.md),
[RxSqlServerData](RxSqlServerData.md).

 ## Examples

 ```

  ## Not run:

# With an RxInSqlServer active compute context
tempTable <- "rxTempTest"
if (rxSqlServerTableExists(tempTable)) rxSqlServerDropTable(tempTable)
 ## End(Not run) 
```




