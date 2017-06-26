--- 
 
# required metadata 
title: "MISSING TITLE" 
description: "" 
keywords: "M, I, S, S, I, N, G,  , K, E, Y, W, O, R, D, S" 
author: "HeidiSteen" 
manager: "" 
ms.date: "06/26/2017" 
ms.topic: "reference" 
ms.prod: "microsoft-r" 
ms.service: "" 
ms.assetid: "" 
 
# optional metadata 
ROBOTS: "" 
audience: "" 
ms.devlang: "" 
ms.reviewer: "" 
ms.suite: "" 
ms.tgt_pltfrm: "" 
ms.technology: "r-server" 
ms.custom: "" 
 
---

# rx_cleanup_jobs



```
revoscalepy.computecontext.RxJob.rx_cleanup_jobs(job_info_list: list, force: bool = False, verbose: bool = True)
```



Removes the artifacts for the specified jobs


# Parameters


## job_info_list

The jobs for which to remove the artifacts, this can be a list of jobs or a single job


## force

True indicates the cleanup should happen regardless of whether or not the job status can be determined
false indicates that the job must be completed before it can be cleaned up.


## verbose

True indicates that verbose output should


# Returns

This function does not return a value
