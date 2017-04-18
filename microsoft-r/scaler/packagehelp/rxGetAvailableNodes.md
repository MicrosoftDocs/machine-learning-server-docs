--- 
 
# required metadata 
title: " Gets a list of operational nodes on a cluster. " 
description: " Gets a list of operational nodes on a cluster. Note that this function will attempt to connect to the cluster when executed. " 
keywords: "RevoScaleR, rxGetAvailableNodes, IO" 
author: "heidisteen" 
manager: "jhubbard" 
ms.date: "04/17/2017" 
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
 
 
 #`rxGetAvailableNodes`:  Gets a list of operational nodes on a cluster. 

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
Gets a list of operational nodes on a cluster.
Note that this function will attempt to connect to the cluster when executed.
 
 
 
 ##Usage

```   
  rxGetAvailableNodes(computeContext,  makeRNodeNames = FALSE)
 
```
 
 
 ##Arguments

   
  
 ### `computeContext`
 A distributed compute context (preferred, see [RxComputeContext](RxComputeContext.md))  or a `jobInfo` object 
  
  
  
 ### `makeRNodeNames`
 logical. If `TRUE`, names of the nodes will be normalized for use  as R variables.  See [rxMakeRNodeNames](rxMakeRNodeNames.md) for details on name mangling. 
  
  
 
 
 ##Value
 
a character vector of node names, or `NULL`.
 
 ##Author(s)
 
Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)

 
 
 ##See Also
 
[rxGetNodeInfo](rxGetNodeInfo.md)
[RxComputeContext](RxComputeContext.md)
[rxGetJobs](rxGetJobs.md)
[rxMakeRNodeNames](rxMakeRNodeNames.md)
   
 ##Examples

 ```
   
  ## Not run:
 
rxGetAvailableNodes( myCluster )
 ## End(Not run) 
  
 
```
 
 
