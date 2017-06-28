--- 
 
# required metadata 
title: "Generate Teradata In-Database Compute Context" 
description: "Creates a compute context for running RevoScaleR analyses inside a Teradata database." 
keywords: "RevoScaleR, RxInTeradata, IO" 
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
 
 
 #`RxInTeradata`: Generate Teradata In-Database Compute Context

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 Creates a compute context for running RevoScaleR analyses inside a Teradata database. 
 
 ##Usage

```   
  RxInTeradata(object, remoteShareDir = "/tmp", connectionString = "", shareDir = "/tmp", 
     revoPath = NULL, controlDB = "revoAnalytics_Zqht2", wait = TRUE, consoleOutput = FALSE, autoCleanup = TRUE, 
     packagesToLoad = NULL,   ...  )
 
```
 
 
 ##Arguments

   
    
 ### `object`
  An optional RxInTeradata object.    
  
    
 ### `remoteShareDir`
  The share directory in Teradata.  
  
    
 ### `connectionString`
  An ODBC connection string used to connect to the Teradata database.  
  
    
 ### `shareDir`
  The share directory. This directory must exist on the client with write permission.  
  
    
 ### `revoPath`
  The path where Microsoft R Server is installed on Teradata.  
  
    
 ### `controlDB`
  The control database name a user can specify for supporting user isolation to enhance security. This requires that stored procedures, stored functions and tables are properly created and initialized in the control database, and access permissions are granted to users.  
  
    
 ### `wait`
  logical value. If `TRUE`, the job will be blocking and will not return until   it has completed or has failed. If `FALSE`, the job will be non-blocking and return immediately,  allowing you to continue running other R code. The object `rxgLastPendingJob` is created with the job information. The client connection with Teradata must be maintained while the job is running, even in non-blocking mode.  
  
    
 ### `consoleOutput`
  logical scalar.If `TRUE`, causes the standard output  of the R processes on the AMPS to be printed to the user console. This value may be  overwritten by passing a non-`NULL` logical value to the `consoleOutput` argument  provided in [rxExec](rxExec.md) and [rxGetJobResults](rxGetJobResults.md).  
  
    
 ### `autoCleanup`
  logical scalar. If `TRUE`, the default behavior is to clean up the  temporary computational artifacts and delete the result objects upon retrieval.  If `FALSE`,  then the computational results are not deleted, and the results may be acquired using  [rxGetJobResults](rxGetJobResults.md), and the output via [rxGetJobOutput](rxGetJobOutput.md) until the  [rxCleanupJobs](../../r-reference/revoscaler/rxcleanup.md) is used to delete the results and other artifacts. Leaving this flag set to `FALSE` can result in accumulation of compute artifacts which you may eventually need to delete before they fill up your hard drive.  
  
  
    
 ### `packagesToLoad`
 NOT YET IMPLEMENTED. Optional character vector specifying additional packages to be  loaded on the nodes when jobs are run in this compute context.  
  
  
    
 ### ` ...`
 additional arguments to be passed to the underlying function. Two useful additional arguments are `traceEnabled=TRUE` and `traceLevel=7`, which taken together enable run-time tracing of your in-Teradata computations. `traceEnabled` and `traceLevel` are deprecated as of MRS 9.0.2 and will be removed from this Compute Context in the next major release. Please use `rxOptions(traceLevel=7)` to enable run-time tracing in-Teradata 
  
   
 
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[RxComputeContext](RxComputeContext.md),
[RxInTeradata-class](RxInTeradata-class.md),
[RxTeradata](RxTeradata.md),
[RxInSqlServer](RxInSqlServer.md),
[rxOptions](rxOptions.md).
   
 
 ##Examples

 ```
   
  ## Not run:
 
baseTD <- RxInTeradata(
	connectionString = "DSN=TeradataVM64",
    shareDir = "C:/AllShare/myName",
	remoteShareDir = "/tmp/revoJobs",
	revoPath = "/usr/lib64/RRO-8.0.3/R-3.1.3/lib64/R",
    consoleOutput = TRUE,
    wait = TRUE,
	autoCleanup = TRUE)
   ## End(Not run) 
  
 
```
 
 
