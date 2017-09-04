--- 
 
# required metadata 
title: "rx_get_job_output: Gets the console output from the various nodes in a non-waiting distributed computing job." 
description: "During a job run, the state of the output is non-deterministic (that is, it may or may not be on disk, and what is on disk at any given point in time may not reflect the actual completion state of a job).If auto_cleanup has been set to True on the distributed computing job’s compute context, the console output will not persist after the job completes.Unlike rx_get_job_results, this function does not remove any job information upon retrieval." 
keywords: "job, output" 
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

# `rx_get_job_output`


**Applies to: SQL Server 2017**


## Usage



```
revoscalepy.rx_get_job_output(job_info: revoscalepy.computecontext.RxRemoteJob.RxRemoteJob) -> str
```




## Description

During a job run, the state of the output is non-deterministic (that is, it may or may not be on disk, and what
is on disk at any given point in time may not reflect the actual completion state of a job).

If *auto_cleanup* has been set to *True* on the distributed computing job’s compute context, the console output
will not persist after the job completes.

Unlike *rx_get_job_results*, this function does not remove any job information upon retrieval.


## Arguments


### job_info

The distributed computing job for which to retrieve the job output


## Returns

*str* that contains the console output for the nodes participating in the distributed computing job


## Author

Microsoft Corporation [Microsoft Technical Support](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)


## See also

`rx_get_job_status`


## Example



```
from revoscalepy import RxInSqlServer
from revoscalepy import rx_exec
from revoscalepy import rx_get_job_output
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

# Wait for the job to finish or fail whatever the case may be
rx_wait_for_job(job)

# Print out what our code printed on sql
output = rx_get_job_output(job)
print(output)
```

