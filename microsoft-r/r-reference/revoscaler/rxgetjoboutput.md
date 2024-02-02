--- 

# required metadata 
title: "rxGetJobOutput function (revoAnalytics) | Microsoft Docs" 
description: " Gets the console output from the various nodes in a non-waiting distributed computing job. " 
keywords: "(revoAnalytics), rxGetJobOutput, IO" 
author: "chuckheinzelman"
ms.author: "charlhe" 
manager: "cgronlun" 
ms.date: 07/15/2019
ms.topic: "reference" 
ms.service: mlserver
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


 # rxGetJobOutput:  Get Console Output from Distributed Computing Job  
 ## Description

Gets the console output from the various nodes in a non-waiting distributed computing job.



 ## Usage

```   
  rxGetJobOutput(jobInfo)

```


 ## Arguments



 ### `jobInfo`
 a job information object, such as that returned from a non-waiting,  distributed computation, for example, the `rxgLastPendingJob` object, if available. 




 ## Details

During a job run, the state of the output is non-deterministic (that is, it may or 
may not be on disk, and what is on disk at any given point in time may not reflect the 
actual completion state of a job).

If `autoCleanup` has been set to `TRUE`, the console output will not persist after the 
job completes.

Unlike [rxGetJobResults](rxGetJobResults.md), this function does not remove any job information upon
retrieval.


 ## Value

This function is called for its side effect of printing console output; it does not have a
useful return value.

 ## See Also

[RxSpark](RxSpark.md),
RxHadoopMR,
[RxInSqlServer](RxInSqlServer.md),
[rxGetJobs](rxGetJobs.md),
[rxCleanupJobs](rxCleanup.md), 
[rxGetJobResults](rxGetJobResults.md),
[rxExec](rxExec.md).

 ## Examples

 ```

  ## Not run:

# set up a non-waiting HPC Server compute context: 
myCluster <- RxSpark(nameNode = "my-name-service-server", port = 8020, wait = FALSE) 
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


