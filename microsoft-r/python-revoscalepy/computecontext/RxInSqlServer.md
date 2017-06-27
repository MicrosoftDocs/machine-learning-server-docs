--- 
 
# required metadata 
title: "Generate SQL Server In-Database Compute Context" 
description: "Creates a compute context for running RevoScalePy analyses inside Microsoft SQL Server." 
keywords: "M, I, S, S, I, N, G,  , K, E, Y, W, O, R, D, S" 
author: "Microsoft Corporation Microsoft Technical Support" 
manager: "" 
<<<<<<< HEAD
ms.date: "06/26/2017" 
=======
ms.date: "06/27/2017" 
>>>>>>> heidist-revoscalepy
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

<<<<<<< HEAD
# RxInSqlServer
=======
## RxInSqlServer


### Usage
>>>>>>> heidist-revoscalepy



```
revoscalepy.computecontext.RxInSqlServer.RxInSqlServer(connection_string: str, num_tasks: int = None, auto_cleanup: bool = None, console_output: bool = None, execution_timeout_seconds: int = None, wait: bool = True, packages_to_load: list = None)
```




<<<<<<< HEAD
## Description
=======
### Description
>>>>>>> heidist-revoscalepy

Creates a compute context for running RevoScalePy analyses inside Microsoft SQL Server.
Currently only supported in Windows.


<<<<<<< HEAD
## Parameters


### connection_string
=======
### Arguments


##### connection_string
>>>>>>> heidist-revoscalepy

An ODBC connection string used to connect to the
Microsoft SQL Server database.


<<<<<<< HEAD
### num_tasks
=======
##### num_tasks
>>>>>>> heidist-revoscalepy

Number of tasks (processes) to run for each computation.
This is the maximum number of tasks that will be used; SQL Server may start
fewer processes if there is not enough data, if too many resources are
already being used by other jobs, or if numTasks exceeds the MAXDOP
(maximum degree of parallelism) configuration option in SQL Server. Each of
the tasks is given data in parallel, and does computations in parallel, and
so computation time may decrease as numTasks increases. However, that may
not always be the case, and computation time may even increase if too many
tasks are competing for machine resources. Note that
rxOptions(numCoresToUse=n) controls how many cores (actually, threads) are
used in parallel within each process, and there is a trade-off between
numCoresToUse and numTasks that depends upon the specific algorithm, the
type of data, the hardware, and the other jobs that are running.


<<<<<<< HEAD
### wait
=======
##### wait
>>>>>>> heidist-revoscalepy

logical value. If True, the job will be blocking and will not
return until it has completed or has failed. If False, the job will be
non-blocking and return immediately, allowing you to continue running other
Python code. The object rxgLastPendingJob is created with the job information.
The client connection with SQL Server must be maintained while the job is
running, even in non-blocking mode.


<<<<<<< HEAD
### console_output
=======
##### console_output
>>>>>>> heidist-revoscalepy

logical scalar.If True, causes the standard output
of the Python process started by SQL Server to be printed to the user console.
This value may be overwritten by passing a non-None logical value to the
consoleOutput argument provided in rxExec and rxGetJobResults.


<<<<<<< HEAD
### auto_cleanup
=======
##### auto_cleanup
>>>>>>> heidist-revoscalepy

logical scalar. If True, the default behavior is to
clean up the temporary computational artifacts and delete the result
objects upon retrieval. If False, then the computational results are not
deleted, and the results may be acquired using rxGetJobResults, and the
output via rxGetJobOutput until the rxCleanupJobs is used to delete the
results and other artifacts. Leaving this flag set to False can result in
accumulation of compute artifacts which you may eventually need to delete
before they fill up your hard drive.


<<<<<<< HEAD
### execution_timeout_seconds
=======
##### execution_timeout_seconds
>>>>>>> heidist-revoscalepy

numeric scalar. Defaults to 0 which means
infinite wait.


<<<<<<< HEAD
### packages_to_load
=======
##### packages_to_load
>>>>>>> heidist-revoscalepy

optional character vector specifying additional
packages to be loaded on the nodes when jobs are run in this compute context.


<<<<<<< HEAD
## Author
=======
### Author
>>>>>>> heidist-revoscalepy

Microsoft Corporation [Microsoft Technical Support](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409.md)


<<<<<<< HEAD
## See also


## Example
=======
### See also


### Example
>>>>>>> heidist-revoscalepy



```
from revoscalepy import RxSqlServerData, RxInSqlServer, rx_lin_mod
formula = "ArrDelay ~ CRSDepTime + DayOfWeek"
connection_string="Driver=SQL Server;Server=.;Database=RevoTestDB;Trusted_Connection=True"
query="select top 100 [ArrDelay],[CRSDepTime],[DayOfWeek] FROM airlinedemosmall"

ds = RxSqlServerData(sql_query = query, connection_string = connection_string)

cc = RxInSqlServer(
    connectionString = connection_string,
    numTasks = 1,
    autoCleanup = False,
    consoleOutput = True,
    executionTimeoutSeconds = 0,
    wait = True
    )

linmod = rx_lin_mod(formula, data = ds, compute_context = cc)
```

