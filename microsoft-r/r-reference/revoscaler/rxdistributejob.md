--- 
 
# required metadata 
title: "rxDistributeJob function (RevoScaleR) | Microsoft Docs" 
description: " Allows distributed execution of a function in parallel across nodes (computers)  of a 'compute context' such as a cluster. A helper functions checks to see if the 'compute context' is appropriate. " 
keywords: "(RevoScaleR), rxDistributeJob, rxIsDistributedContext, IO" 
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
 
 
 
 #rxDistributeJob:  Distribute job across nodes of a cluster  
 ##Description
 
Allows distributed execution of a function in parallel across nodes (computers) 
of a 'compute context' such as a cluster. A helper functions checks to see
if the 'compute context' is appropriate.
 
 
 
 ##Usage

```   
  rxDistributeJob(matchCallList, matchCall)
  rxIsDistributedContext( computeContext = NULL, data = NULL)
 
```
 
 
 ##Arguments

   
  
 ### `matchCallList`
  a list containing the function name and arguments, adjusting environments as needed 
  
  
  
 ### `matchCall`
  a call in which all of the specified arguments are specified by their full names;  typically the result of match.call 
  
  
  
 ### `computeContext`
 `NULL` or an [RxComputeContext](RxComputeContext.md) object. 
  
  
  
 ### `data`
 `NULL` or an [RxDataSource](RxDataSource.md) object.  If specified, compatibility of the data source with the 'dataDistType' in the compute context will be checked. 
  
  
 
 
 
 ##Details
 
An example of usage can be found in the **RevoPemaR** package.
 
 
 
 ##Value
 
The result of the distributed computation.

 
 
 
 ##See Also
 
[rxExec](rxExec.md),
[RxComputeContext](RxComputeContext.md),
[rxSetComputeContext](rxSetComputeContext.md),
[rxGetComputeContext](rxSetComputeContext.md)
   
 ##Examples

 ```
   
  ## Not run:
 

# Example is provided in the RevoPemaR package

 ## End(Not run) 
  
 
```
 
 
