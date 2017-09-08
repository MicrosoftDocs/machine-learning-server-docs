--- 
 
# required metadata 
title: "rx_get_jobs: Get Distributed Computing Jobs" 
description: "Returns a list of job objects associated with the given compute context and matching the specified parameters." 
keywords: "get, job" 
author: "Microsoft Corporation Microsoft Technical Support" 
manager: "jhubbard" 
ms.date: "08/31/2017" 
ms.topic: "reference" 
ms.prod: "microsoft-r" 
ms.service: "" 
ms.assetid: "" 
 
# optional metadata 
ROBOTS: "" 
audience: "" 
ms.devlang: "Python" 
ms.reviewer: "" 
ms.suite: "" 
ms.tgt_pltfrm: "" 
ms.technology: "r-server" 
ms.custom: "" 
 
---

# `rx_get_jobs`


**Applies to: SQL Server 2017**


## Usage



```
revoscalepy.rx_get_jobs(compute_context: revoscalepy.computecontext.RxRemoteComputeContext.RxRemoteComputeContext, exact_match: bool = False, start_time: <module 'datetime' from 'C:\\swarm\\workspace\\bigAnalytics-9.2.1\\runtime\\Python\\lib\\datetime.py'> = None, end_time: <module 'datetime' from 'C:\\swarm\\workspace\\bigAnalytics-9.2.1\\runtime\\Python\\lib\\datetime.py'> = None, states: list = None, verbose: bool = True) -> list
```




## Description

Returns a list of job objects associated with the given compute context and matching the specified parameters.


## Arguments


### compute_context

A compute context object.


### exact_match

Determines if jobs are matched using the full compute
context, or a simpler subset. If True, only jobs which use the same context
object are returned. If False, all jobs which have the same headNode (if
available) and ShareDir are returned.


### start_time

A time, specified as a POSIXct object. If specified, only
jobs created at or after start_time are returned.


### end_time

A time, specified as a POSIXct object. If specified, only
jobs created at or before end_time are returned.


### states

If specified (as a list of strings of states that can include
“none”, “finished”, “failed”, “canceled”, “undetermined” “queued”or
“running”), only jobs in those states are returned. Otherwise, no filtering
is performed on job state.


### verbose

If True (the default), a brief summary of each job is printed
as it is found. This includes the current job status as returned by
rx_get_job_status, the modification time of the job, and the current job ID
(this is used as the component name in the returned list of job information
objects). If no job status is returned, the job status shows none.


## Returns

Returns a list of job information objects based on the compute context.







## See also


## Example



```
from revoscalepy import RxInSqlServer
from revoscalepy import rx_exec
from revoscalepy import rx_get_jobs

connection_string = 'Driver=SQL Server;Server=.;Database=RevoTestDb;Trusted_Connection=True;'

# Setting wait to False allows the job to be run asynchronously
# Setting console_output to True allows us to get the console output of the distributed computing job
compute_context = RxInSqlServer(connection_string=connection_string,
                                num_tasks=1,
                                console_output=True,
                                wait=False)

def hello_from_sql():
    import time
    print('Hello from SQL server')
    time.sleep(3)
    return 'We just ran Python code asynchronously on a SQL server!'

job = rx_exec(function=hello_from_sql, compute_context=compute_context)

job_list = rx_get_jobs(compute_context)
print(job_list)
```

