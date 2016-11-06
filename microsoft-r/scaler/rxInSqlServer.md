---

# required metadata
title: "ScaleR Functions RxInSqlServer"
description: "ScaleR Functions: RxInSqlServer"
keywords: "RevoScaleR, ScaleR, RxInSqlServer"
author: "j-martens"
manager: "jhubbard"
ms.date: "11/13/2016"
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

#RxInSqlServer

Generates a SQL Server compute context using SQL Server R Services.

## Usage

` RxInSqlServer(object, connectionString = "",  numTasks = rxGetOption("numTasks"), autoCleanup = TRUE,  consoleOutput = FALSE, executionTimeoutSeconds = 0, wait = TRUE, packagesToLoad = NULL, shareDir = NULL, server = NULL, databaseName = NULL, user = NULL, password = NULL, ...)`

## Arguments

The following table shows the arguments in order and their default values.


|Parameter | Default |  Description|
| --------- | --------- | --------- |
|_object_ | NULL| An optional RxInSqlServer object.|
|_connectionString_ | NULL|  A connection string used to connect to the Microsoft SQL Server database.|
|_numTasks_ | 1|  An integer representing the number of tasks, or processes, to run for each computation. This is the maximum number of tasks that will be used; SQL Server may start fewer processes if there is not enough data, if too many resources are already being used by other jobs, or if the value of _numTasks_ exceeds the MAXDOP (maximum degree of parallelism) configuration option in SQL Server. |
|_wait_ | FALSE|  If TRUE, the job will be blocking and will not return until it has completed or has failed. If  FALSE, the job will be non-blocking and return immediately, allowing you to continue running other R code. The object `rxgLastPendingJob` is created with the job information. The client connection with SQL Server must be maintained while the job is running, even in non-blocking mode.|
|_consoleOutput_ | FALSE|  If TRUE, the standard output of the R process started by SQL Server will be printed to the user console. Functions such as `rxExec` and `rxGetJobResults` support over-riding this value, by passing a non-NULL logical value to the _consoleOutput_ argument.|
|_autoCleanup_| TRUE | If TRUE, all temporary computational artifacts are cleaned up and result objects deleted after data has been retrieved.  If FALSE, computational results are not deleted, and the results may be acquired by using the functions `rxGetJobResults`. The output remains until the `rxCleanupJobs` method is used to delete the results and other artifacts. See Remarks for more information.|
|_executionTimeoutSeconds_| 0 | The number of seconds to wait. The default value of 0 means infinite wait. |
|_packagesToLoad_| NULL | (optional) A character vector specifying additional packages to be loaded when jobs are run in this compute context.|
|_shareDir_| NULL| A string that defines a temporary directory on the client that is used to serialize the R objects back and forth. You can specify an absolute path or a path relative to the current directory. If a shared directory is not specified, a default is used. |
|_server_ | NULL|  The name or IP address of the SQL Server instance.  You can also specify the server name in the connection string by using the **Server=** keyword, but the value of the _server_ parameter, if supplied, will override the server name in the connection string.|
|_databaseName_ | NULL|  Database on the target SQL Server instance.  Can also be specified in the connection string by using the **Database=** keyword.|
|_user_ | NULL|  A string containing the SQL login used for making the connection to the SQL Server instance.  You can also specify the SQL login in the connection string by providing a value for **uid=** keyword, but the value of the _user_ parameter, if supplied, will override the user name in the connection string.|
|_password_ | NULL| Password associated with the SQL login. Can also be specified in the connection string with the ‘pwd’ keyword.
|_..._ | NULL|  Additional arguments required by the underlying class. For SQL Server compute contexts, consider using the arguments `traceEnabled=TRUE` and `traceLevel=7`. Combined, these arguments enable run-time tracing of computations that run in SQL Server.|

## Remarks
If a number is specified for _numTasks_ argument, each of the tasks is given data in parallel, and does computations in parallel. Therefore, it is possible that computation time might decrease as the value of _numTasks_ increases.

However, there are cases where increasing the value of _numTasks_ might not have the desired effect. Computation time might even increase if too many tasks are competing for machine resources.

To control how many threads are used in parallel within each process, you can use the `rxOptions` function and set the _numCoresToUse_ argument to an integer specifying the number of cores you want to use. However, there is a trade-off between the value in _numCoresToUse_ and _numTasks_. The best performance depends upon the specific algorithm, the type of data, the hardware, and other jobs that are running.

> [!IMPORTANT]
>
> The value of _autoCleanup_ should be set to the default value of TRUE except when absolutely necessary for debugging. If you leave this flag set to FALSE by mistake, it can result in accumulation of large computational artifacts that you will need to manage and eventually delete before they fill up your hard drive.

## Return Value
None

## Example
The following example defines a compute context using previously defined variables for the connection string and other required parameters.
~~~~
sqlCompute <- RxInSqlServer(
     connectionString = sqlConnString,     
     shareDir = sqlShareDir,    
     wait = sqlWait,
     consoleOutput = sqlConsoleOutput)
~~~~

## See Also
[Comparison of rx Functions and CRAN R Functions](compare-base-r-scaler-functions.md)

[ScaleR Functions for Working with SQL Server Data](https://msdn.microsoft.com/en-us/library/mt652103.aspx)
