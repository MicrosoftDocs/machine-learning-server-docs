--- 
 
# required metadata 
title: " DEPRECATED: Gets a list of operational nodes on a cluster. " 
description: " DEPRECATED: Gets a list of operational nodes on a cluster that are also (optionally) in a  set of groups. Use rxGetAvailableNodes. Note that this function will attempt to connect to the cluster when executed. " 
keywords: "RevoScaleR, ScaleR, KEYWORDS" 
author: "richcalaway" 
manager: "jhubbard" 
ms.date: "02/17/2017" 
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
 
 
 # DEPRECATED: Gets a list of operational nodes on a cluster.  
 ##Description
 
DEPRECATED: Gets a list of operational nodes on a cluster that are also (optionally) in a 
set of groups. Use rxGetAvailableNodes.
Note that this function will attempt to connect to the cluster when executed.
 
 
 
 ##Usage

```   
  rxGetNodes(headNode, includeHeadNode = FALSE, makeRNodeNames = FALSE, getWorkersOnly=TRUE)
 
```
 
 
 ##Arguments

   
  
 >  `headNode` -  A compute context (preferred), a `jobInfo` object, or (deprecated) a character scalar containing the name of the head node of a Microsoft HPC cluster.  
  
  
 >  `includeHeadNode` -  logical scalar.  Indicates whether to include the name of the head node in the list of returned nodes.  
  
  
 >  `makeRNodeNames` -  Determines if the names of the nodes should be normalized for use as R variables.   See also [rxMakeRNodeNames](rxMakeRNodeNames.md) for details on name mangeling.  
  
  
 >  `getWorkersOnly` -  logical.  If `TRUE`, returns only those nodes within the cluster that are configured to actually execute jobs (where applicable; currently LSF only.).  
  
 
 
 ##See Also
 
[rxGetAvailableNodes](rxGetAvailableNodes.md),
[rxMakeRNodeNames](rxMakeRNodeNames.md).
   
 ##Examples

 ```
   
  ## Not run:
 
rxGetNodes( myCluster )
 ## End(Not run) 
  
 
```
 
 
