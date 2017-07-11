--- 
 
# required metadata 
title: "" 
description: "" 
keywords: "" 
author: "bradsev" 
manager: "jhubbard" 
ms.date: "07/11/2017" 
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

## `RxRemoteJob`


*Applies to:* SQL Server 2017, Machine Learning Services 9.3


### Usage



```
class revoscalepy.RxRemoteJob(compute_context: revoscalepy.computecontext.RxComputeContext.RxComputeContext, job_id: str = None)
```




### Usage



```
close()
```



Closes the remote job, purging all the data associated with the job


### Usage



```
static deserialize_job(job_id: str) -> revoscalepy.computecontext.RxRemoteJob.RxRemoteJob
```



Deserializes a RxRemoteJob given the job id
:returns: The job that was deserialized


### Usage



```
static deserialize_jobs() -> list
```



Deserializes the existing jobs that have not been cleaned up by calling close().


## Arguments


## Returns

The deserialized jobs


### Usage



```
resolve_context() -> revoscalepy.computecontext.RxComputeContext.RxComputeContext
```



Resolves the `RxComputeContext` that is associated with the job
:returns: The `RxComputeContext` that is associated with the job or `None` if the compute context isn’t valid
