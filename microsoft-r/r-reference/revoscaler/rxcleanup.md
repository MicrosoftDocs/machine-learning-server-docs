--- 
 
# required metadata 
title: "rxCleanupJobs function (RevoScaleR) | Microsoft Docs" 
description: " Removes artifacts created while executing a distributed computing job. " 
keywords: "(RevoScaleR), rxCleanupJobs, IO" 
author: "heidisteen" 
manager: "jhubbard" 
ms.date: "09/07/2017" 
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
 
 
 #rxCleanupJobs:  Cleanup of a Distributed Computing Job or Jobs.  
 ##Description
 
Removes artifacts created while executing a distributed computing job.
 
 
 
 ##Usage

```   
  rxCleanupJobs(jobInfoList, force = FALSE, verbose = TRUE)
 
```
 
 
 ##Arguments

   
  
 ### `jobInfoList`
 `rxJobInfo` object or a list of job objects that can be obtained  from [rxGetJobs](rxGetJobs.md). 
  
  
  
 ### `force`
 logical scalar. If `TRUE`, forces removal of job directories even if  there are retrievable results or if the current job state is undetermined. 
  
  
  
 ### `verbose`
 logical scalar.  If `TRUE`, will print the directories/records being deleted. 
  
 
 
 
 ##Details
 
If `jobInfoList` is a `jobInfo` object, `rxCleanupJobs` attempts to remove the artifacts.
However, if the job has successfully completed and `force=FALSE`,
`rxCleanupJobs` issues a warning saying to either set `force=TRUE` or use 
[rxGetJobResults](rxGetJobResults.md) to get the results and delete the artifacts.  

If `jobInfoList` is a list of jobs, `rxCleanupJobs` attempts to apply the cleanup rules 
for a single job to each element in the list.
 
 
 
 ##Value
 
This function is called for its side effects (removing job artifacts); it does not have a useful return value.
 
 ##Author(s)
 
Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)

 
 
 ##See Also
 
[rxGetJobs](rxGetJobs.md), 
[rxGetJobOutput](rxGetJobOutput.md),
[RxSpark](RxSpark.md),
[RxHadoopMR](RxHadoopMR.md),
[rxGetJobResults](rxGetJobResults.md)
   
 ##Examples

 ```
   
  ## Not run:
 
rxCleanupJobs(jobInfoList = myJobs, force = TRUE)

rxCleanupJobs(rxGetJobs(rxGetOption("computeContext")))
 ## End(Not run) 
  
 
```
 
 
