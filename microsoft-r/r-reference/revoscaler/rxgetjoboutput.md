--- 
 
# required metadata 
title: " Get Console Output from Distributed Computing Job " 
description: " Gets the console output from the various nodes in a non-waiting distributed computing job. " 
keywords: "RevoScaleR, rxGetJobOutput, IO" 
author: "heidisteen" 
manager: "jhubbard" 
ms.date: "04/18/2017" 
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
 
 
 #`rxGetJobOutput`:  Get Console Output from Distributed Computing Job 

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
Gets the console output from the various nodes in a non-waiting distributed computing job.
 
 
 
 ##Usage

```   
  rxGetJobOutput(jobInfo)
 
```
 
 
 ##Arguments

   
  
 ### `jobInfo`
 a job information object, such as that returned from a non-waiting,  distributed computation, for example, the `rxgLastPendingJob` object, if available. 
  
 
 
 
 ##Details
 
During a job run, the state of the output is non-deterministic (that is, it may or 
may not be on disk, and what is on disk at any given point in time may not reflect the 
actual completion state of a job).

If `autoCleanup` has been set to `TRUE`, the console output will not persist after the 
job completes.
 
Unlike [rxGetJobResults](../../scaler/packagehelp/rxgetjobresults.md), this function does not remove any job information upon
retrieval.
 
 
 ##Value
 
This function is called for its side effect of printing console output; it does not have a
useful return value.
 
 ##See Also
 
[RxSpark](../../scaler/packagehelp/rxspark.md),
[RxHadoopMR](../../scaler/packagehelp/rxhadoopmr.md),
[RxInSqlServer](../../scaler/packagehelp/rxinsqlserver.md),
[RxInTeradata](../../scaler/packagehelp/rxinteradata.md), 
[rxGetJobs](../../scaler/packagehelp/rxgetjobs.md),
[rxCleanupJobs](rxcleanup.md), 
[rxGetJobResults](../../scaler/packagehelp/rxgetjobresults.md),
[rxExec](rxexec.md).
   
 ##Examples

 ```
   
  ## Not run:
 
# set up a non-waiting HPC Server compute context: 
myCluster <- RxHpcServer( 
headNode = "cluster-head2", 
shareDir = "\\AllShare\\username", 
revoPath = file.path(defaultRNodePath, "bin", "x64"), 
workingDir = "C:\\Users\\username", 
dataPath ="C:\\data", 
wait = FALSE
) 
rxOptions(computeContext=myCluster) 

myJob <- rxExec(function(){ print( "Hello World"); return ( 1 ) })
# The job output will contain the printed text "Hello World" from each node
rxGetJobOutput(myJob)
# The results object will be a list containing the value 1 for each node
rxGetJobResults(myJob)
# Get job results will remove the job object (by default)
# Another call to rxGetJobOutput(myJob) would return no output

 ## End(Not run) 
  
 
```
 
 
