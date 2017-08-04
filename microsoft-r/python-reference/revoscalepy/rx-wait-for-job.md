--- 
 
# required metadata 
title: "Wait for Distributed Job to Complete" 
description: "Causes Python to block on an existing distributed job until completion.  This effectively turns a non-blocking job into a blocking job." 
keywords: "wait, job" 
author: "bradsev" 
manager: "jhubbard" 
ms.date: "07/19/2017" 
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

# `rx_wait_for_job`


**Applies to: SQL Server 2017 RC1**


## Usage



```
revoscalepy.rx_wait_for_job(job_info: revoscalepy.computecontext.RxRemoteJob.RxRemoteJob)
```




## Description

Causes Python to block on an existing distributed job until completion.  This effectively turns a non-blocking
job into a blocking job.


## Arguments


### job_info

A jobInfo object.


## See also


## Example



```
from revoscalepy import RxInSqlServer
from revoscalepy import RxSqlServerData
from revoscalepy import rx_logit
from revoscalepy import rx_wait_for_job
from revoscalepy import rx_get_job_results

connection_string = 'Driver=SQL Server;Server=.;Database=RevoTestDb;Trusted_Connection=True;'
airline_data_source = RxSqlServerData(sql_query='select top 1000 * from airlinedemosmall',
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
compute_context = RxInSqlServer(connection_string=connection_string, num_tasks=1, wait=False)

def transform_late(dataset, context):
    dataset['Late'] = dataset['ArrDelay'] > 15
    return dataset

# Since wait is set to False on the compute_context rx_logit will return a job rather than the model
job = rx_logit(formula='Late ~ DayOfWeek',
               data=airline_data_source,
               compute_context=compute_context,
               transform_function=transform_late,
               transform_variables=['ArrDelay'])

# Wait for the job to finish
rx_wait_for_job(job)
model = rx_get_job_results(job)
print(model)
```

