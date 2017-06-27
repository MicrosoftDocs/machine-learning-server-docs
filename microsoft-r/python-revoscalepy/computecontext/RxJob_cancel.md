--- 
 
# required metadata 
title: "MISSING TITLE" 
description: "" 
keywords: "M, I, S, S, I, N, G,  , K, E, Y, W, O, R, D, S" 
author: "HeidiSteen" 
manager: "" 
<<<<<<< HEAD
ms.date: "06/26/2017" 
=======
ms.date: "06/27/2017" 
>>>>>>> heidist-revoscalepy
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

<<<<<<< HEAD
# rx_cancel_job
=======
## rx_cancel_job


### Usage
>>>>>>> heidist-revoscalepy



```
revoscalepy.computecontext.RxJob.rx_cancel_job(job_info: revoscalepy.computecontext.RxRemoteJob.RxRemoteJob, console_output: bool = False) -> bool
```



Causes Python to cancel an existing distributed computing job

This function does not attempt to retrieve any output objects; if the output is desired the ``console_output``
flag can be used to display it.  This function does, however, remove all job-related artifacts from the distributed
computing resources including any job results.


<<<<<<< HEAD
# Parameters


## job_info
=======
## Arguments


#### job_info
>>>>>>> heidist-revoscalepy

A job to cancel


<<<<<<< HEAD
## console_output
=======
#### console_output
>>>>>>> heidist-revoscalepy

If ``None`` use the console option that was present when the job was created, if ``True``

output any console output that was present when the the job was cancelled will be displayed.  If ``False`` then
all console output will not be displayed.
:return: ``True`` if the job is successfully cancelled; ``False`` otherwise.
