--- 
 
# required metadata 
title: "Generate SQL Server In-Database Compute Context" 
description: "Creates a compute context for running RevoScalePy analyses inside Microsoft SQL Server." 
keywords: "sql" 
author: "HeidiSteen" 
manager: "" 
ms.date: "" 
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

## ``RxInSqlServer``


*Applies to:* SQL Server 2017, Machine Learning Services 9.3


### Usage



```
class revoscalepy.RxInSqlServer(connection_string: str, num_tasks: int = None, auto_cleanup: bool = None, console_output: bool = None, execution_timeout_seconds: int = None, wait: bool = True, packages_to_load: list = None)
```




### Description

Creates a compute context for running RevoScalePy analyses inside Microsoft SQL Server.
Currently only supported in Windows.


### Arguments


##### connection_string

An ODBC connection string used to connect to the
Microsoft SQL Server database.


##### num_tasks

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


##### wait

logical value. If True, the job will be blocking and will not
return until it has completed or has failed. If False, the job will be
non-blocking and return immediately, allowing you to continue running other
Python code. The object rxgLastPendingJob is created with the job information.
The client connection with SQL Server must be maintained while the job is
running, even in non-blocking mode.


##### console_output

logical scalar.If True, causes the standard output
of the Python process started by SQL Server to be printed to the user console.
This value may be overwritten by passing a non-None logical value to the
consoleOutput argument provided in rxExec and rxGetJobResults.


##### auto_cleanup

logical scalar. If True, the default behavior is to
clean up the temporary computational artifacts and delete the result
objects upon retrieval. If False, then the computational results are not
deleted, and the results may be acquired using rxGetJobResults, and the
output via rxGetJobOutput until the rxCleanupJobs is used to delete the
results and other artifacts. Leaving this flag set to False can result in
accumulation of compute artifacts which you may eventually need to delete
before they fill up your hard drive.


##### execution_timeout_seconds

numeric scalar. Defaults to 0 which means
infinite wait.


##### packages_to_load

optional character vector specifying additional
packages to be loaded on the nodes when jobs are run in this compute context.


### Example



```
from revoscalepy import RxSqlServerData, RxInSqlServer, rx_lin_mod
formula = "ArrDelay ~ CRSDepTime + DayOfWeek"
connection_string="Driver=SQL Server;Server=.;Database=RevoTestDB;Trusted_Connection=True"
query="select top 100 [ArrDelay],[CRSDepTime],[DayOfWeek] FROM airlinedemosmall"

ds = RxSqlServerData(sql_query = query, connection_string = connection_string)

cc = RxInSqlServer(
    connection_string = connection_string,
    num_tasks = 1,
    auto_cleanup = False,
    console_output = True,
    execution_timeout_seconds = 0,
    wait = True
    )

linmod = rx_lin_mod(formula, data = ds, compute_context = cc)
```

