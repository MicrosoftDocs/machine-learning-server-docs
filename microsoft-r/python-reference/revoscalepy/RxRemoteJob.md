--- 
 
# required metadata 
title: "RxRemoteJob,close,deserialize_job,deserialize_jobs,resolve_context: " 
description: "" 
keywords: "" 
author: "bradsev" 
manager: "jhubbard" 
ms.date: "09/11/2017" 
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

# RxRemoteJob


 



```
revoscalepy.RxRemoteJob(compute_context: revoscalepy.computecontext.RxComputeContext.RxComputeContext,
    job_id: str = None)
```





```
close()
```




Closes the remote job, purging all the data associated with the job



```
deserialize_job(job_id: str) -> revoscalepy.computecontext.RxRemoteJob.RxRemoteJob
```




Deserializes a RxRemoteJob given the job id


# Returns

The job that was deserialized



```
deserialize_jobs() -> list
```




Deserializes the existing jobs that have not been cleaned up by calling close().


# Returns

The deserialized jobs



```
resolve_context() -> revoscalepy.computecontext.RxComputeContext.RxComputeContext
```




Resolves the `RxComputeContext` that is associated with the job


# Returns

The `RxComputeContext` that is associated with the job or `None` if the compute context isn’t valid
