--- 
 
# required metadata 
title: "rx_get_job_results: Obtain Distributed Computing Job Status and Results" 
description: "Obtain distributed computing results and processing status." 
keywords: "" 
author: "heidist" 
manager: "cgronlun" 
ms.date: "01/19/2018" 
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

# rx_get_job_results


 


## Usage



```
revoscalepy.rx_get_job_results(job_info: revoscalepy.computecontext.RxRemoteJob.RxRemoteJob,
    console_output: bool = None,
    auto_cleanup: bool = None) -> list
```





## Description

Obtain distributed computing results and processing status.


## Arguments


### job_info

A job object as returned by rx_exec or a revoscalepy
analysis function, if available.


### console_output

None or bool value. If True, the console output
from all of the processes is printed to the user console. If False, no
console output is displayed. Output can be retrieved with the function
rxGetJobOutput for a non-waiting job. If not None, this flag overrides the
value set in the compute context when the job was submitted. If None, the
setting in the compute context will be used.


### auto_cleanup

None or bool value. If True, the default behavior is
to clean up any artifacts created by the distributed computing job. If False,
then the artifacts are not deleted, and the results may be acquired using
rxGetJobResults, and the console output via rxGetJobOutput until the
rxCleanupJobs is used to delete the artifacts. If not None, this flag
overwrites the value set in the compute context when the job was submitted.
If you routinely set auto_cleanup=False, you will eventually fill your hard
disk with compute artifacts.If you set auto_cleanup=True and experience
performance degradation on a Windows XP client, consider setting
auto_cleanup=False.


## Returns

Either the results of the run (prepended with console output if the console_output argument is set to True) or
a message saying that the results are not available because the job has not finished, has failed, or was
deleted.


## See also

`rx_get_job_status`


## Example



```
from revoscalepy import RxInSqlServer
from revoscalepy import rx_exec
from revoscalepy import rx_wait_for_job
from revoscalepy import rx_get_job_results

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

# Wait for the job to finish
rx_wait_for_job(job)

# Print out what we returned from SQL
results = rx_get_job_results(job)
print(results)
```

