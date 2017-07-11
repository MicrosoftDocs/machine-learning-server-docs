--- 
 
# required metadata 
title: "Obtain Distributed Computing Job Status and Results" 
description: "Obtain distributed computing results and processing status.rx_get_job_status(job_info) rx_get_job_results(job_info, console_output, auto_cleanup)" 
keywords: "context, job" 
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

## `rx_get_job_status`


*Applies to:* SQL Server 2017, Machine Learning Services 9.3


### Usage



```
revoscalepy.rx_get_job_status(job_info: revoscalepy.computecontext.RxRemoteJob.RxRemoteJob) -> revoscalepy.computecontext.RxJob.RxRemoteJobStatus
```




### Description

Obtain distributed computing results and processing status.

rx_get_job_status(job_info)
rx_get_job_results(job_info, console_output, auto_cleanup)


### Arguments


##### job_info

a job object as returned by rx_exec or a RevoScalePy
analysis function, if available.


##### console_output

None or bool value. If True, the console output
from all of the processes is printed to the user console. If False, no
console output is displayed. Output can be retrieved with the function
rxGetJobOutput for a non-waiting job. If not None, this flag overrides the
value set in the compute context when the job was submitted. If None, the
setting in the compute context will be used.


##### auto_cleanup

None or bool value. If True, the default behavior is
to clean up any artifacts created by the distributed computing job. If False,
then the artifacts are not deleted, and the results may be acquired using
rxGetJobResults, and the console output via rxGetJobOutput until the
rxCleanupJobs is used to delete the artifacts. If not None, this flag
overwrites the value set in the compute context when the job was submitted.
If you routinely set auto_cleanup=False, you will eventually fill your hard
disk with compute artifacts.If you set auto_cleanup=True and experience
performance degradation on a Windows XP client, consider setting
auto_cleanup=False.


### Returns

rx_get_job_results: either the results of the run (prepended with console
output if the console_output argument is set to True) or a message saying
that the results are not available because the job has not finished, has
failed, or was deleted.

rx_get_job_status: a character string denoting the job status.


### See also


### Example
