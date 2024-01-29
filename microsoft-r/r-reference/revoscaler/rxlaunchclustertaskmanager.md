--- 

# required metadata 
title: "rxLaunchClusterJobManager function (revoAnalytics) | Microsoft Docs" 
description: " Launches the job management UI (if available) for a compute context. Currently this is only available for RxHadoopMR and RxSpark compute contexts. " 
keywords: "(revoAnalytics), rxLaunchClusterJobManager, rxLaunchClusterJobManager,character-method, rxLaunchClusterJobManager,RxHadoopMR-method, IO" 
author: "chuckheinzelman"
ms.author: "charlhe" 
manager: "cgronlun" 
ms.date: 07/15/2019
ms.topic: "reference" 
ms.prod: "sql-non-specified"
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
ms.technology: machine-learning-server

--- 





 # rxLaunchClusterJobManager:  Launches the job management UI for a compute context.  
 ## Description

Launches the job management UI (if available) for a compute context. Currently this is only
available for RxHadoopMR and RxSpark compute contexts.



 ## Usage

```   
  rxLaunchClusterJobManager(context=rxGetOption("computeContext"))

```


 ## Arguments



 ### `context`
 The compute context used to determine which UI to launch. 




 ## Details

For RxHadoopMR, the job default tracker web page and dfs information page are launched for the cluster.  Note that
the dfs information page is generated from the `nameNode` slot of the `RxHadoopMR` compute context, 
and the `jobTrackerURL` slot of the `RxHadoopMR` is used as the URL for the job tracker.  If one or both of these
are not provied, a best attempt will be made based on the hostname provided in the `sshHostname` slot.

For other compute contexts, if compute context is not supplied, current compute context will be used.  If no job management 
UI is available for the context, this function will report an error.


 ## Author(s)

Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)



 ## Examples

 ```

  ## Not run:

myCluster <- RxSpark(nameNode = "my-name-service-server", port = 8020)
rxOptions(computeContext=myCluster)
rxLaunchClusterJobManager(myCluster)
 ## End(Not run) 
```


