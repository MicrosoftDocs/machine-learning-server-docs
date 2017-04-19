--- 
 
# required metadata 
title: "RevoScaleR Compute Contexts: Class Generator" 
description: " This is the main generator for RxComputeContext S4 classes. " 
keywords: "RevoScaleR, RxComputeContext, file, connection" 
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
 
 
 #`RxComputeContext`: RevoScaleR Compute Contexts: Class Generator

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
This is the main generator for RxComputeContext S4 classes.
 
 
 ##Usage

```   
  	RxComputeContext( computeContext, ...)
 
```
 
 ##Arguments

   
    
 ### `computeContext`
 character string specifying class name or description of the specific  class to instantiate, or an existing `RxComputeContext` object.  Choices include: "RxLocalSeq" or "local", "RxLocalParallel" or "localpar", "RxSpark" or "spark",  "RxHadoopMR" or "hadoopmr", "RxInSqlServer" or "sqlserver", "RxInTeradata" or "teradata",  and "RxForeachDoPar" or "dopar". 
  
    
 ### ` ...`
 any other arguments are passed to the class generator determined from `context`. 
  
 
 
 ##Details
 
This is a wrapper to specific class generator functions for the
RevoScaleR compute context classes. For example, the RxInSqlServer class uses function
[RxInSqlServer](RxInSqlServer.md) as a generator. Therefore either `RxInSqlServer(...)`
or `RxComputeContext("RxInSqlServer", ...)` will create an RxInSqlServer instance.
 
 
 ##Value
 
A type of RxComputeContext compute context object. This object may be used to in
[rxSetComputeContext](rxSetComputeContext.md) or [rxOptions](rxOptions.md) to set the compute context.
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[RxComputeContext-class](RxComputeContext-class.md),
[RxHadoopMR](RxHadoopMR.md),
[RxSpark](RxSpark.md),
[RxInSqlServer](RxInSqlServer.md),
[RxInTeradata](RxInTeradata.md),
[RxLocalSeq](RxLocalSeq.md),
[RxLocalParallel](RxLocalParallel.md),
[RxForeachDoPar](RxForeachDoPar.md),
[rxSetComputeContext](rxSetComputeContext.md),
[rxOptions](rxOptions.md),
[rxExec](rxExec.md).
   
 ##Examples

 ```
   
  # Setup to run analyses on a SQL Server
  ## Not run:
 

# Note: for improved security, read connection string from a file, such as
# connectionString <- readLines("connectionString.txt")

connectionString <- "Server=MyServer;Database=MyDatabase;UID=MyUser;PWD=MyPassword"
sqlQuery <- "WITH nb AS (SELECT 0 AS n UNION ALL SELECT n+1 FROM nb where n < 9) SELECT n1.n+10*n2.n+100*n3.n+1 AS n, ABS(CHECKSUM(NewId())) 

myServer <- RxComputeContext("RxInSqlServer",
		sqlQuery = sqlQuery, connectionString = connectionString)                 
rxSetComputeContext(computeContext = myServer )
 ## End(Not run) 
  
 
```
 
 
 
