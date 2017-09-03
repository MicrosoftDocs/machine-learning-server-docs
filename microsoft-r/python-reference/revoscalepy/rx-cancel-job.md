--- 
 
# required metadata 
title: "rx_cancel_job: Causes Python to cancel an existing distributed computing job" 
description: "This function does not attempt to retrieve any output objects; if the output is desired the console_output flag can be used to display it.  This function does, however, remove all job-related artifacts from the distributed computing resources including any job results." 
keywords: "cancel" 
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

# `rx_cancel_job`


**Applies to: SQL Server 2017 RC2**


## Usage



```
revoscalepy.rx_cancel_job(job_info: revoscalepy.computecontext.RxRemoteJob.RxRemoteJob, console_output: bool = False) -> bool
```




## Description

This function does not attempt to retrieve any output objects; if the output is desired the *console_output*
flag can be used to display it.  This function does, however, remove all job-related artifacts from the
distributed computing resources including any job results.


## Arguments


### job_info

A job to cancel


### console_output

If *None* use the console option that was present when the job was created, if *True*
output any console output that was present when the the job was cancelled will be displayed.  If *False* then
all console output will not be displayed.


## Returns

*True* if the job is successfully cancelled; *False* otherwise.z


## Author

Microsoft Corporation [Microsoft Technical Support](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)


## See also

`rx_get_job_status`


## Example



```
import time
from revoscalepy import RxInSqlServer
from revoscalepy import RxSqlServerData
from revoscalepy import rx_dforest
from revoscalepy import rx_cancel_job
from revoscalepy import RxRemoteJobStatus
from revoscalepy import rx_get_job_status
from revoscalepy import rx_cleanup_jobs

connection_string = 'Driver=SQL Server;Server=.;Database=RevoTestDb;Trusted_Connection=True;'
airline_data_source = RxSqlServerData(sql_query='select * from airlinedemosmall',
                                      connection_string=connection_string,
                                      column_info={
                                          'ArrDelay': {'type': 'integer'},
                                          'CRSDepTime': {'type': 'float'},
                                          'DayOfWeek': {
                                              'type': 'factor',
                                              'levels': ['Monday', 'Tuesday', 'Wednesday', 'Thursday',
                                                         'Friday',
                                                         'Saturday', 'Sunday']
                                          }
                                      })

# Setting wait to False allows the job to be run asynchronously
compute_context = RxInSqlServer(connection_string=connection_string,
                                num_tasks=1,
                                auto_cleanup=False,
                                wait=False)

job = rx_dforest(formula='ArrDelay ~ DayOfWeek',
                 seed=1,
                 data=airline_data_source,
                 compute_context=compute_context)

# Poll until status is RUNNING
status = rx_get_job_status(job)
while status == RxRemoteJobStatus.QUEUED:
    time.sleep(1)
    status = rx_get_job_status(job)

# Only running or queued jobs can be canceled
if status == RxRemoteJobStatus.RUNNING :
    # Cancel the job
    rx_cancel_job(job)

# Only canceled or failed jobs can be cleaned up
if status == RxRemoteJobStatus.CANCELED or status == RxRemoteJobStatus.FAILED :
    # Cleanup after the job
    rx_cleanup_jobs(job)
```

