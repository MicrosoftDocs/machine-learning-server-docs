--- 
 
# required metadata 
title: "rxLaunchClusterJobManager function (RevoScaleR) | Microsoft Docs" 
description: " Launches the job management UI (if available) for a compute context. Currently this is only available for RxHadoopMR and RxSpark compute contexts. " 
keywords: "(RevoScaleR), rxLaunchClusterJobManager, rxLaunchClusterJobManager,character-method, rxLaunchClusterJobManager,RxHadoopMR-method, IO" 
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
 
 
 
 
 
 #rxLaunchClusterJobManager:  Launches the job management UI for a compute context.  
 ##Description
 
Launches the job management UI (if available) for a compute context. Currently this is only
available for RxHadoopMR and RxSpark compute contexts.
 
 
 
 ##Usage

```   
  rxLaunchClusterJobManager(context=rxGetOption("computeContext"))
 
```
 
 
 ##Arguments

   
  
 ### `context`
 The compute context used to determine which UI to launch. 
  
 
 
 
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
 
myCluster <- RxSparkConnect(nameNode = "my-name-service-server", port = 8020)
rxOptions(computeContext=myCluster)
rxLaunchClusterJobManager(myCluster)
 ## End(Not run) 
  
 
```
 
 
