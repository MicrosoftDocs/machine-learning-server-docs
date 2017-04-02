--- 
 
# required metadata 
title: " Launches the job management UI for a compute context. " 
description: " Launches the job management UI (if available) for a compute context. Currently this is only available for RxHadoopMR and RxHpcServer compute contexts. " 
keywords: "RevoScaleR, rxLaunchClusterJobManager, rxLaunchClusterJobManager,character-method, rxLaunchClusterJobManager,RxHadoopMR-method, rxLaunchClusterJobManager,RxHpcServer-method, IO" 
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
 
 
 
 
 
 
 #`rxLaunchClusterJobManager`:  Launches the job management UI for a compute context. 

 Applies to version {PRODUCT_VERSION} of package RevoScaleR.
 
 ##Description
 
Launches the job management UI (if available) for a compute context. Currently this is only
available for RxHadoopMR and RxHpcServer compute contexts.
 
 
 
 ##Usage

```   
  rxLaunchClusterJobManager(context=rxGetOption("computeContext"))
 
```
 
 
 ##Arguments

   
  
 ### `context`
 The compute context used to determine which UI to launch, or a  character string with the name of the head node, or the character string "none" to launch a defult session. Note that use of a character string is deprecated, and only used for MS HPC clusters. 
  
 
 
 
 ##Details
 
For RxHadoopMR, the job default tracker web page and dfs information page are launched for the cluster.  Note that
the dfs information page is generated from the `nameNode` slot of the `RxHadoopMR` compute context, 
and the `jobTrackerURL` slot of the `RxHadoopMR` is used as the URL for the job tracker.  If one or both of these
are not provied, a best attempt will be made based on the hostname provided in the `sshHostname` slot.

For other compute contexts, if compute context is not supplied, current compute context will be used.  If no job management 
UI is available for the context, this function will report an error.
 
 
 ##Author(s)
 
Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)

 
 
 ##Examples

 ```
   
  ## Not run:
 
baseHPC <- RxHpcServer(headNode = "cluster-head")
rxOptions(computeContext=baseHPC)
rxLaunchClusterJobManager(baseHPC)
 ## End(Not run) 
  
 
```
 
 
