--- 
 
# required metadata 
title: " Cleanup of a Distributed Computing Job or Jobs. " 
description: " Removes artifacts created while executing a distributed computing job. " 
keywords: "RevoScaleR, rxCleanupJobs, IO" 
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
 
 
 #`rxCleanupJobs`:  Cleanup of a Distributed Computing Job or Jobs. 

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
Removes artifacts created while executing a distributed computing job.
 
 
 
 ##Usage

```   
  rxCleanupJobs(jobInfoList, force = FALSE, verbose = TRUE)
 
```
 
 
 ##Arguments

   
  
 ### `jobInfoList`
 `rxJobInfo` object or a list of job objects that can be obtained  from [rxGetJobs](rxgetjobs.md). 
  
  
  
 ### `force`
 logical scalar. If `TRUE`, forces removal of job directories even if  there are retrievable results or if the current job state is undetermined. 
  
  
  
 ### `verbose`
 logical scalar.  If `TRUE`, will print the directories/records being deleted. 
  
 
 
 
 ##Details
 
If `jobInfoList` is a `jobInfo` object, `rxCleanupJobs` attempts to remove the artifacts.
However, if the job has successfully completed and `force=FALSE`,
`rxCleanupJobs` issues a warning saying to either set `force=TRUE` or use 
[rxGetJobResults](rxgetjobresults.md) to get the results and delete the artifacts.  

If `jobInfoList` is a list of jobs, `rxCleanupJobs` attempts to apply the cleanup rules 
for a single job to each element in the list.
 
 
 
 ##Value
 
This function is called for its side effects (removing job artifacts); it does not have a useful return value.
 
 ##Author(s)
 
Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)

 
 
 ##See Also
 
[rxGetJobs](rxgetjobs.md), 
[rxGetJobOutput](rxgetjoboutput.md),
[RxSpark](rxspark.md),
[RxHadoopMR](rxhadoopmr.md),
[RxInTeradata](rxinteradata.md),  
[rxGetJobResults](rxgetjobresults.md)
   
 ##Examples

 ```
   
  ## Not run:
 
rxCleanupJobs(jobInfoList = myJobs, force = TRUE)

rxCleanupJobs(rxGetJobs(rxGetOption("computeContext")))
 ## End(Not run) 
  
 
```
 
 
