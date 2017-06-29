--- 
 
# required metadata 
title: " Wait for Distributed Job to Complete " 
description: " Causes R to block on an existing distributed job until completion. " 
keywords: "RevoScaleR, rxWaitForJob, rxWaitForJob,RxDistributedJob-method, rxWaitForJob,RxDistributedSqlServerJob-method, rxWaitForJob,RxDistributedTeradataJob-method, rxWaitForJob,RxDistributedHadoopMRJob-method, rxWaitForJob,ANY-method, IO" 
author: "HeidiSteen"
ms.author: "heidist" 
manager: "jhubbard" 
ms.date: "04/18/2017" 
ms.topic: "reference" 
ms.prod: "microsoft-r" 
ms.service: "" 
ms.assetid: "" 
 
# optional metadata 
#ROBOTS: "" 
#audience: "" 
#ms.devlang: "" 
#ms.reviewer: "" 
#ms.suite: "" 
#ms.tgt_pltfrm: "" 
ms.technology: "r-server" 
#ms.custom: "" 
 
--- 
 
 
 
 
 
 
 
 #rxWaitForJob:  Wait for Distributed Job to Complete 

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
Causes R to block on an existing distributed job until completion.
 
 
 
 ##Usage

```   
  rxWaitForJob(jobInfo)
 
```
 
 
 ##Arguments

   
  
 ### jobInfo
 A `jobInfo` object. 
  
 
 
 
 ##Details
 
This function essentially changes a non-blocking distributed computing job to 
a blocking job. You can change a blocking job to non-blocking on Windows by
pressing the Esc key, then using `rxGetJobStatus` and `rxGetJobResults`
on the object `rxgLastPendingJob`. This function does not, however, modify
the compute context.
 
 
 ##Author(s)
 
Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)

 
 
 ##Examples

 ```
   
  ## Not run:
 
rxWaitForJob( rxgLastPendingJob )
rxWaitForJob( myNonWaitingJob )
 ## End(Not run) 
  
 
```
 
 
