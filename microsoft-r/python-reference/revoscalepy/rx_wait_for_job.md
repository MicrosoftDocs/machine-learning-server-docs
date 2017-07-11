--- 
 
# required metadata 
title: "Wait for Distributed Job to Complete" 
description: "Causes Python to block on an existing distributed job until completion." 
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

## `rx_wait_for_job`


*Applies to:* SQL Server 2017, Machine Learning Services 9.3


### Usage



```
revoscalepy.rx_wait_for_job(job_info: revoscalepy.computecontext.RxRemoteJob.RxRemoteJob)
```




### Description

Causes Python to block on an existing distributed job until completion.


### Arguments


##### job_info

A jobInfo object.


### See also


### Example
