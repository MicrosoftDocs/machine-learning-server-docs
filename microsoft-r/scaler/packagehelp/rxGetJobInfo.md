--- 
 
# required metadata 
title: " Get Job Information from Distributed Computing Job " 
description: " Gets job information for a given distributed computing job. " 
keywords: "RevoScaleR, rxGetJobInfo, IO" 
author: "richcalaway" 
manager: "jhubbard" 
ms.date: "03/23/2017" 
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
 
 
 #`rxGetJobInfo`:  Get Job Information from Distributed Computing Job 

 Applies to version {PRODUCT_VERSION} of package RevoScaleR.
 
 ##Description
 
Gets job information for a given distributed computing job.
 
 
 
 ##Usage

```   
  rxGetJobInfo(object)
 
```
 
 
 ##Arguments

   
  
 ### `object`
 an object containing `jobInfo` information, such as that returned from a non-waiting, distributed computation. 
  
 
 
 
 ##Details
 
The object returned from a non-waiting, distributed computation contains job information together with other 
information.  The job information is used internally by such functions as `rxGetJobStatus`,
`rxGetJobOutput`, and `rxGetJobResults`. It is sometimes useful to extract it for its own sake, as
it contains complete information on the job's compute context as well as other information needed by the
distributed computing resources.

For most users, the principal use of this function is to determine whether a given object actually contains job 
information. If the return value is not `NULL`, then the object contains job information. Note, however,
that the structure of the job information is subject to change, so code that attempts to manipulate it directly
is not guaranteed to be forward-compatible.
 
 
 ##Value
 
the job information, if present, or `NULL`.
 
 ##Author(s)
 
Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)

 
 
 ##See Also
 
[RxSpark](RxSpark.md),
[RxHadoopMR](RxHadoopMR.md),
[RxInSqlServer](RxInSqlServer.md),
[RxInTeradata](RxInTeradata.md), 
[rxGetJobStatus](rxGetJobResults.md), 
[rxGetJobOutput](rxGetJobOutput.md), 
[rxGetJobResults](rxGetJobResults.md)
   
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
# The job information consists of the job's compute context
# and other information needed to prepare and complete the job
rxGetJobInfo(myJob)
# The results object will be a list containing the value 1 for each node
rxGetJobResults(myJob)
# Get job results will remove the job object (by default)
# Another call to rxGetJobInfo(myJob) would return no output

 ## End(Not run) 
  
 
```
 
 
