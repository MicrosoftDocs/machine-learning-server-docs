--- 
 
# required metadata 
title: "rxGetJobs function (RevoScaleR) | Microsoft Docs" 
description: " Returns a list of job objects associated with the given compute context  and matching the specified parameters. " 
keywords: "(RevoScaleR), rxGetJobs, rxGetJobs,RxDistributedHpa-method, rxGetJobs,RxForeachDoPar-method, rxGetJobs,RxHadoopMR-method, rxGetJobs,RxInSqlServer-method, rxGetJobs,RxLocalParallel-method, rxGetJobs,RxLocalSeq-method, IO" 
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
 
 
 
 
 
 
 
 
 #rxGetJobs:  Get Distributed Computing Jobs  
 ##Description
 
Returns a list of job objects associated with the given compute context 
and matching the specified parameters.
 
 
 
 ##Usage

```   
  rxGetJobs(computeContext, exactMatch = FALSE, startTime = NULL, endTime = NULL, 
            states = NULL, verbose = TRUE)
 
```
 
 
 ##Arguments

   
  
 ### `computeContext`
 A compute context object. 
  
  
 ### `exactMatch`
 Determines if jobs are matched using the full compute  context, or a simpler subset.  If `TRUE`, only jobs which use the same  context object are returned. If `FALSE`, all jobs which have the same `headNode` (if available) and `ShareDir` are returned. 
  
  
 ### `startTime`
 A time, specified as a `POSIXct` object. If specified, only jobs created at  or after `startTime` are returned.  For non-RxHadoopMR contexts, this time should be specified in the user's local time; for RxHadoopMR contexts, the time should specified in GMT. See below for more details. 
  
  
 ### `endTime`
 A time, specified as a `POSIXct` object. If specified, only jobs created at  or before `endTime` are returned.  For non-RxHadoopMR contexts, this time should be specified in the user's local time; for RxHadoopMR contexts, the time should specified in GMT. See below for more details. 
  
  
 ### `states`
 If specified (as a character vector of states that can include `"none"`,  `"finished"`, `"failed"`, `"canceled"`, `"undetermined"` `"queued"`or  `"running"`), only jobs in those states are returned.   Otherwise, no filtering is performed on job state. 
  
  
 ### `verbose`
 If `TRUE` (the default), a brief summary of each job is printed as it is found. This includes the current job status as returned by [rxGetJobStatus](rxGetJobResults.md), the modification time of the job, and the current job ID (this is used as the component name in the returned list of job information objects). If no job status is returned, the job status shows `none`. 
  
 
 
 
 ##Details
 
One common use of `rxGetJobs` is as input to the [rxCleanupJobs](rxCleanup.md) function, which
is used to clean up completed non-waiting jobs when `autoCleanup` is not specified.

If `exactMatch=FALSE`, only the shared directory `shareDir` and the cluster 
head name `headNode` (if available) are compared.  Otherwise, all slots are compared. However, if the
`nodes` slot in either compute context is `NULL`, that slot is also
omitted from the comparison.

On LSF clusters, job information by default is held for only one hour (although this is configurable using
the LSF parameter CLEAN_PERIOD); jobs older than the CLEAN_PERIOD setting will have status `"undetermined"`. 

For non-RxHadoopMR cluster types, all time values are specified and displayed in the user's computer's local time settings, regardless
of the time zone settings and differences between the user's computer and the cluster.  Thus, start and end times
for job filtering should be provided in local time, with the expectation that cluster time values will also be converted
for the user into system local time.  For RxHadoopMR, the job time and comparison times are stored and performed based on a GMT time.

Note also that when there are a large number of jobs on the cluster, you can improve performance by
using the `startTime` and `endTime` parameters to narrow your search.
 
 
 ##Value
 
Returns a `rxJobInfoList`, list of job information objects based on the compute context.
 

 


 
 
 ##See Also
 
[rxCleanupJobs](rxCleanup.md),
[rxGetJobOutput](rxGetJobOutput.md),
[rxGetJobResults](rxGetJobResults.md),
[rxGetJobStatus](rxGetJobResults.md),
[rxExec](rxExec.md), 
[RxSpark](RxSpark.md),
[RxHadoopMR](RxHadoopMR.md),
[RxInSqlServer](RxInSqlServer.md),
[RxComputeContext](RxComputeContext.md)
   
 ##Examples

 ```
   
  ## Not run:
 
myCluster <- RxComputeContext("RxSpark",
    # Location of Revo64 on each node
    revoPath = file.path(defaultRNodePath, "bin", "x64"),  											
    nameNode = "cluster-head2", 
    # User directory for read/write                                      	
    shareDir = "\\AllShare\\myName"                            
    )

rxSetComputeContext(computeContext = myCluster )

# Get all jobs older than a week and newer than two weeks that are finished or canceled.
rxGetJobs(myCluster, startTime = Sys.time() - 3600 * 24 * 14, endTime = Sys.time() - 3600 * 24 * 7, 
          exactMatch = FALSE, states = c( "finished", "canceled") )
		  
# Get all jobs associated with myCluster compute context and then get job output and results
myJobs <- rxGetJobs(myCluster)
print(myJobs)
# returns
# rxJob_1461  
myJobs$rxJob_1461
rxGetJobOutput(myJobs$rxJob_1461)
rxGetJobResults(myJobs$rxJob_1461)
 ## End(Not run) 
  
 
```
 
 
