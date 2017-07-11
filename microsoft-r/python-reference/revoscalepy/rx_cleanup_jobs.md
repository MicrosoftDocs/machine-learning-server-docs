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

## `rx_cleanup_jobs`


*Applies to:* SQL Server 2017, Machine Learning Services 9.3


### Usage



```
revoscalepy.rx_cleanup_jobs(job_info_list: typing.Union[revoscalepy.computecontext.RxRemoteJob.RxRemoteJob, list], force: bool = False, verbose: bool = True)
```



Removes the artifacts for the specified jobs


## Arguments


#### job_info_list

The jobs for which to remove the artifacts, this can be a list of jobs or a single job


#### force

True indicates the cleanup should happen regardless of whether or not the job status can be determined
false indicates that the job must be completed before it can be cleaned up.


#### verbose

True indicates that verbose output should


## Returns

This function does not return a value
