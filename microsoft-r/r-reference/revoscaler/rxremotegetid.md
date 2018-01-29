--- 
 
# required metadata 
title: "rxRemoteGetId function (revoAnalytics) | Microsoft Docs" 
description: " Provides a unique identifier for the process in the range [1:N] where N is the number of processes. " 
keywords: "(revoAnalytics), rxRemoteGetId, IO" 
author: "heidisteen" 
manager: "cgronlun" 
ms.date: "01/24/2018" 
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
 
 
 #rxRemoteGetId:  Provides a unique identifier for the process.  
 ##Description
 
Provides a unique identifier for the process in the range [1:N] where N is the number of processes.
 
 
 
 ##Usage

```   
  rxRemoteGetId()
 
```
 
 ##Details
 
This function is intended for use in calls to [rxExec](rxExec.md) to identify the process performing a given computation.  

You can also use `rxRemoteGetId` to obtain separate random number seeds.
See the examples.
 
 
 ##Value
 
In a distributed compute context,
this returns the parametric ID for each node or core that executes the function.
If the compute context is local, this always returns `-1`.
 
 
 ##Examples

 ```
   
  ## Not run:
 
rxExec(rxRemoteGetId)

# set a seed on each node before generating random numbers
myNorm <- function(x)
{
    set.seed(rxRemoteGetId())
	rnorm(x)
}
rxExec(myNorm, ARGS=as.list(1:5))
 ## End(Not run) 
  
 
```
 
 
