--- 
 
# required metadata 
title: "rx_cleanup_jobs: " 
description: "If job_info_list is a RxRemoteJob object, rx_cleanup_jobs attempts to remove the artifacts. However, if the job has successfully completed and force is False, rx_cleanup_jobs issues a warning saying to either set force=True or use rx_get_job_results to get the results and delete the artifacts.If job_info_list is a list of jobs, rx_cleanup_jobs attempts to apply the cleanup rules for a single job to each element in the list." 
keywords: "cleanup, job" 
author: "bradsev" 
manager: "jhubbard" 
ms.date: "09/06/2017" 
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

# rx_cleanup_jobs


**Applies to: SQL Server 2017**


## Usage



```
revoscalepy.rx_cleanup_jobs(job_info_list: typing.Union[revoscalepy.computecontext.RxRemoteJob.RxRemoteJob,
    list], force: bool = False, verbose: bool = True)
```




Removes the artifacts for the specified jobs


## Description

If *job_info_list* is a *RxRemoteJob* object, *rx_cleanup_jobs* attempts to remove the artifacts. However, if
the job has successfully completed and *force* is *False*, *rx_cleanup_jobs* issues a warning saying to either
set *force=True* or use *rx_get_job_results* to get the results and delete the artifacts.

If *job_info_list* is a list of jobs, *rx_cleanup_jobs* attempts to apply the cleanup rules for a single job to
each element in the list.


## Arguments


### job_info_list

The jobs for which to remove the artifacts, this can be a list of jobs or a single job


### force

True indicates the cleanup should happen regardless of whether or not the job status can be determined
and job results have already been retrieved false indicates that the should be cleaned up by calling
*rx_get_job_results* or the job should have completed unsuccessfully (such as by using rx_cancel_job).


### verbose

If *True*, will print the directories/records being deleted.


## Returns

This function does not return a value


## See also


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

if status == RxRemoteJobStatus.RUNNING :
    # Cancel the job
    rx_cancel_job(job)

# Only canceled or failed jobs can be cleaned up
if status == RxRemoteJobStatus.CANCELED or status == RxRemoteJobStatus.FAILED :
    # Cleanup after the job
    rx_cleanup_jobs(job)
```

