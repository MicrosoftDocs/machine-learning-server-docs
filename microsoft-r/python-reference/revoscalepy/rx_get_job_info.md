--- 
 
# required metadata 
title: "Get Job Information from Distributed Computing Job" 
description: "Gets job information for a given distributed computing job." 
keywords: "context" 
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

## `rx_get_job_info`


*Applies to:* SQL Server 2017, Machine Learning Services 9.3


### Usage



```
revoscalepy.rx_get_job_info(job) -> revoscalepy.computecontext.RxRemoteJob.RxRemoteJob
```




### Description

Gets job information for a given distributed computing job.


### Arguments


##### job

an object containing jobInfo information, such as that returned from a non-waiting, distributed computation.


### Returns

the job information, if present, or None.


### See also


### Example
