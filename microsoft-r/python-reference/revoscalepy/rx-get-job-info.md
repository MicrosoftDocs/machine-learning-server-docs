--- 
 
# required metadata 
title: "rx_get_job_info: Get Job Information from Distributed Computing Job" 
description: "The object returned from a non-waiting, distributed computation  contains job information together with other information.  The job information is used internally by such functions as rx_get_job_status, rx_get_job_output, and rx_get_job_results. It is sometimes useful to extract it for its own sake, as it contains complete information on the job’s compute context as well as other information needed by the distributed computing resources.For most users, the principal use of this function is to determine whether a given object actually contains job information. If the return value is not None, then the object contains job information. Note, however, that the structure of the job information is subject to change, so code that attempts to manipulate it directly is not guaranteed to be forward-compatible." 
keywords: "context" 
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

# rx_get_job_info


 


## Usage



```
revoscalepy.rx_get_job_info(job) -> revoscalepy.computecontext.RxRemoteJob.RxRemoteJob
```





## Description

The object returned from a non-waiting, distributed computation  contains job information together with other
information.  The job information is used internally by such functions as *rx_get_job_status*,
*rx_get_job_output*, and *rx_get_job_results*. It is sometimes useful to extract it for its own sake, as it
contains complete information on the job’s compute context as well as other information needed by the
distributed computing resources.

For most users, the principal use of this function is to determine whether a given object actually contains job
information. If the return value is not *None*, then the object contains job information. Note, however, that
the structure of the job information is subject to change, so code that attempts to manipulate it directly is
not guaranteed to be forward-compatible.


## Arguments


### job

An object containing jobInfo information, such as that returned from a non-waiting, distributed
computation.


## Returns

The job information, if present, or None.


## See also

`rx_get_job_status`


## Example



```
from revoscalepy import RxInSqlServer
from revoscalepy import rx_exec
from revoscalepy import rx_get_job_info

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

#Get the job information
info = rx_get_job_info(job)
print(info)
```

