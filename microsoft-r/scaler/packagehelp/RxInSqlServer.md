--- 
 
# required metadata 
title: "Generate SQL Server In-Database Compute Context" 
description: "Creates a compute context for running RevoScaleR analyses inside Microsoft SQL Server.  Currently only supported in Windows." 
keywords: "RevoScaleR, RxInSqlServer, IO" 
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
 
 
 #`RxInSqlServer`: Generate SQL Server In-Database Compute Context

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 Creates a compute context for running RevoScaleR analyses inside Microsoft SQL Server.

Currently only supported in Windows. 
 
 ##Usage

```   
  RxInSqlServer(object, connectionString = "",  numTasks = rxGetOption("numTasks"), autoCleanup = TRUE, 
     consoleOutput = FALSE, executionTimeoutSeconds = 0, wait = TRUE, packagesToLoad = NULL, 
     shareDir = NULL, server = NULL, databaseName = NULL, user = NULL, password = NULL,   ...  )
 
```
 
 
 ##Arguments

   
    
 ### `object`
  An optional RxInSqlServer object.  
  
    
 ### `connectionString`
  An ODBC connection string used to connect to the Microsoft SQL Server database.  
  
  
    
 ### `numTasks`
  Number of tasks (processes) to run for each computation. This is the maximum number of tasks that will be used; SQL Server  may start fewer processes if there is not enough data, if too many resources are already being used by other jobs, or if  numTasks exceeds the MAXDOP (maximum degree of parallelism) configuration option in SQL Server. Each of the tasks is given data in parallel, and does computations in parallel, and so computation time may decrease as `numTasks` increases. However, that may not always be the case, and computation time may even increase if too many tasks are competing for machine resources. Note that `rxOptions(numCoresToUse=n)` controls how many cores (actually, threads) are used in parallel within each process,  and there is a trade-off between `numCoresToUse` and `numTasks` that depends upon the specific algorithm,  the type of data, the hardware, and the other jobs that are running.  
  
  
    
 ### `wait`
  logical value. If `TRUE`, the job will be blocking and will not return until   it has completed or has failed. If `FALSE`, the job will be non-blocking and return immediately,  allowing you to continue running other R code. The object `rxgLastPendingJob` is created with the job information. The client connection with SQL Server must be maintained while the job is running, even in non-blocking mode.  
  
    
 ### `consoleOutput`
  logical scalar.If `TRUE`, causes the standard output  of the R process started by SQL Server to be printed to the user console. This value may be  overwritten by passing a non-`NULL` logical value to the `consoleOutput` argument  provided in [rxExec](../../r-reference/revoscaler/rxexec.md) and [rxGetJobResults](../../r-reference/revoscaler/rxgetjobresults.md).  
  
    
 ### `autoCleanup`
  logical scalar. If `TRUE`, the default behavior is to clean up the  temporary computational artifacts and delete the result objects upon retrieval.  If `FALSE`,  then the computational results are not deleted, and the results may be acquired using  [rxGetJobResults](../../r-reference/revoscaler/rxgetjobresults.md), and the output via [rxGetJobOutput](../../r-reference/revoscaler/rxgetjoboutput.md) until the  [rxCleanupJobs](../../r-reference/revoscaler/rxcleanup.md) is used to delete the results and other artifacts. Leaving this flag set to `FALSE` can result in accumulation of compute artifacts which you may eventually need to delete before they fill up your hard drive.  
  
  
    
 ### `executionTimeoutSeconds`
  numeric scalar. Defaults to 0 which means infinite wait.  
  
  
    
 ### `packagesToLoad`
 optional character vector specifying additional packages to be  loaded on the nodes when jobs are run in this compute context.  
  
  
    
 ### `shareDir`
 character string specifying the temporary directory on the client that is  used to serialize the R objects back and forth. If not specified, a subdirectory under  Absolute or paths relative to current directory can be specified.  
  
  
    
 ### `server`
 Target SQL Server instance. 	 Can also be specified in the connection string with the `Server` keyword. 
  
  
    
 ### `databaseName`
 Database on the target SQL Server instance. 	 Can also be specified in the connection string with the `Database` keyword. 
  
  
    
 ### `user`
 SQL login to connect to the SQL Server instance.   SQL login can also be specified in the connection string with the `uid` keyword. 
  
  
    
 ### `password`
 Password associated with the SQL login. Can also be specified in the connection  string with the `pwd` keyword. 
  
  
    
 ### ` ...`
 additional arguments to be passed to the underlying function. Two useful additional arguments are `traceEnabled=TRUE` and `traceLevel=7`, which taken together enable run-time tracing of your in-SQL Server computations. `traceEnabled` and `traceLevel` are deprecated as of MRS 9.0.2 and will be removed from this compute context in the next major release. Please use `rxOptions(traceLevel=7)` to enable run-time tracing in-SQL Server. 
  
   
 
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[RxComputeContext](../../r-reference/revoscaler/rxcomputecontext.md),
[RxInSqlServer-class](RxInSqlServer-class.md),
[RxSqlServerData](RxSqlServerData.md),
[RxInTeradata](RxInTeradata.md),
[rxOptions](rxOptions.md).
   
 
 ##Examples

 ```
   
  ## Not run:
 
# In this example we connect to the database MyDatabase on the SQL Server instance MyServer using MyUser and MyPassword as credentials.
# The query generates a rowset with 1000 rows and 2 columns:
# - The first column is all integers going from 1 to 1000.
# - The second column consists of random integers between 1 and 1000.

# Note: for improved security, read connection string from a file, such as
# connectionString <- readLines("connectionString.txt")

    connectionString <- "Server=MyServer;Database=MyDatabase;UID=MyUser;PWD=MyPassword"
    sqlQuery <- "WITH nb AS (SELECT 0 AS n UNION ALL SELECT n+1 FROM nb where n < 9) SELECT n1.n+10*n2.n+100*n3.n+1 AS n, ABS(CHECKSUM(NewId())) 
    rxSummary(
        formula = ~ .,
        data = RxSqlServerData(sqlQuery = sqlQuery, connectionString = connectionString),
        computeContext = RxInSqlServer(connectionString = connectionString)
        )
        
# Sample output:
# Number of valid observations: 1000 
# 
#  Name  Mean    StdDev   Min Max  ValidObs MissingObs
#  n     500.500 288.8194 1   1000 1000     0         
#  value 504.582 295.5115 1   1000 1000     0      
   ## End(Not run) 
  
 
```
 
 
