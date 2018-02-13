--- 
 
# required metadata 
title: "rx_get_job_status: Obtain Distributed Computing Job Status (revoscalepy)" 
description: "Obtain distributed computing processing status for the specified job." 
keywords: "job, status" 
author: "HeidiSteen" 
manager: "cgronlun" 
ms.date: "01/26/2018" 
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

# rx_get_job_status


 


## Usage



```
revoscalepy.rx_get_job_status(job_info: revoscalepy.computecontext.RxRemoteJob.RxRemoteJob) -> revoscalepy.computecontext.RxJob.RxRemoteJobStatus
```





## Description

Obtain distributed computing processing status for the specified job.


## Arguments


### job_info

A job object as returned by rx_exec or a revoscalepy
analysis function, if available.


## Returns

A RxRemoteJobStatus enumeration value that designates the status of the remote job


## See also

`rx_get_job_results`
`RxRemoteJobStatus`


## Example



```
from revoscalepy import RxInSqlServer
from revoscalepy import rx_exec
from revoscalepy import rx_get_job_status
from revoscalepy import rx_wait_for_job

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

# Poll initial status
status = rx_get_job_status(job)

print(status)

# Wait for the job to finish or fail whatever the case may be
rx_wait_for_job(job)

# Poll final status
status = rx_get_job_status(job)

print(status)
```

