--- 
 
# required metadata 
title: "Global Options for RevoScalePy" 
description: "Allows distributed execution of a function in parallel across nodes" 
keywords: "M, I, S, S, I, N, G,  , K, E, Y, W, O, R, D, S" 
author: "Microsoft Corporation Microsoft Technical Support" 
manager: "" 
ms.date: "06/26/2017" 
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

# rx_exec



```
revoscalepy.computecontext.RxInSqlServer.rx_exec(function: <built-in function callable>, args: dict = {}, localstate: dict = {}, computeContext=None)
```




## Description

Allows distributed execution of a function in parallel across nodes
(computers) or cores of a “compute context” such as a cluster.


## Parameters


### function

the function to be executed; the nodes or cores on which it
is run are determined by the currently-active compute context and by the
other arguments of rx_exec.


### args

arguments passed to the function FUN each time it is executed.
Separate argument values can be sent for each computation by wrapping a
vector or list of argument values in rxElemArg.


### compute_context

a RxComputeContext object


## Returns

If a waiting compute context is active, a list with an element for
each job, where each element contains the value(s) returned by that job’s
function call(s). If a non-waiting compute context is active, a jobInfo
object. See rxGetJobResults.


## Author

Microsoft Corporation [Microsoft Technical Support](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409.md)


## See also


## Example



```
from revoscalepy import RxSqlServerData, RxInSqlServer, rx_exec
formula = "ArrDelay ~ CRSDepTime + DayOfWeek"
connection_string="Driver=SQL Server;Server=.;Database=RevoTestDB;Trusted_Connection=TRUE"
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

def remote_call(dataset):
    from revoscalepy import rx_data_step
    df = rx_data_step(dataset)
    return len(df)

result = rx_exec(function = remote_call, args={'dataset': ds}, computeContext = cc)
```

