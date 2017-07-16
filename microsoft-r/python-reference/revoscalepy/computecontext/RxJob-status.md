--- 
 
# required metadata 
title: "Obtain Distributed Computing Job Status" 
description: "Obtain distributed computing processing status for the specified job.rx_get_job_status(job_info)" 
keywords: "" 
author: "Microsoft Corporation Microsoft Technical Support" 
manager: "jhubbard" 
ms.date: "07/13/2017" 
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


**Applies to: SQL Server 2017 RC1**


## Usage



```
revoscalepy.computecontext.RxJob.rx_get_job_status(job_info: revoscalepy.computecontext.RxRemoteJob.RxRemoteJob) -> revoscalepy.computecontext.RxJob.RxRemoteJobStatus
```




## Description

Obtain distributed computing processing status for the specified job.

rx_get_job_status(job_info)


## Arguments


### job_info

a job object as returned by rx_exec or a RevoScalePy
analysis function, if available.


## Returns

a RxRemoteJobStatus enumeration value that designates the status of the remote job


## Author

Microsoft Corporation [Microsoft Technical Support](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)


## See also

`rx_get_job_results`
`RxRemoteJobStatus`


## Example



```
import time
from revoscalepy import RxInSqlServer
from revoscalepy import rx_exec
from revoscalepy import RxRemoteJobStatus
from revoscalepy import rx_get_job_status
from revoscalepy import rx_get_job_results
from revoscalepy import rx_get_job_output

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

# Poll until status is FINISHED
status = rx_get_job_status(job)
while status != RxRemoteJobStatus.FINISHED:
    time.sleep(1)
    status = rx_get_job_status(job)

# Print out what our code printed on sql
output = rx_get_job_output(job)
print(output)

# Print out what we returned from SQL
result = rx_get_job_results(job)
print(result)
```

