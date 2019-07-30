--- 

# required metadata 
title: "rxCancelJob function (revoAnalytics) | Microsoft Docs" 
description: " Causes R to cancel an existing distributed computing job. " 
keywords: "(revoAnalytics), rxCancelJob, rxCancelJob,list-method, rxCancelJob,RxDistributedJob-method, rxCancelJob,RxDistributedHadoopMRJob-method, rxCancelJob,RxDistributedSqlServerJob-method, rxCancelJob,RxDistributedTeradataJob-method, rxCancelJob,ANY-method, IO" 
author: "heidisteen" 
manager: "cgronlun" 
ms.date: 07/15/2019
ms.topic: "reference" 
ms.prod: "mlserver" 
ms.service: "" 
ms.assetid: "" 

# optional metadata 
ROBOTS: "" 
audience: "" 
ms.devlang: "" 
ms.reviewer: "" 
ms.suite: "" 
ms.tgt_pltfrm: "" 
#ms.technology: "" 
ms.custom: "" 

--- 









 # rxCancelJob:  Cancel Distributed Computing Job  
 ## Description

Causes R to cancel an existing distributed computing job.



 ## Usage

```   
  rxCancelJob( jobInfo, consoleOutput = NULL)

```


 ## Arguments



 ### `jobInfo`
 a jobInfo object, such as that returned by [rxExec](rxExec.md) or one of the  RevoScaleR analysis functions in a non-waiting compute context, or the current contents of the `rxgLastPendingJob` object. 


 ### `consoleOutput`
 If `NULL`, the `consoleOutput` value assigned to  the job by the compute context at launch is used.  If `TRUE`, any console output present at the time the job is canceled is displayed.  If `FALSE`, no console output  is displayed. 



 ## Details

This function does not attempt to retrieve any output objects; if the output is desired,
the `consoleOutput` flag can be used to display it. This function does, however,
remove all job-related artifacts from the distributed computing resources, including
any job results.

On Windows, if you press ESC to interrupt a job and then call `rxCancelJob`, you
may get an error message if the job was not completely submitted before you pressed ESC.


 ## Value

`TRUE` if the job is successfully canceled; `FALSE` otherwise.

 ## Author(s)

Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)



 ## See Also

[rxGetJobs](rxGetJobs.md),
RxHadoopMR.

 ## Examples

 ```

  ## Not run:

rxCancelJob( rxgLastPendingJob, consoleOutput = TRUE )
rxCancelJob( myNonWaitingJob, consoleOutput = FALSE )
 ## End(Not run) 
```


