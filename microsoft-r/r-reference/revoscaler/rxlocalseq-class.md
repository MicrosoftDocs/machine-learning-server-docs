--- 
 
# required metadata 
title: "RxLocalSeq-class class (revoAnalytics) | Microsoft Docs" 
description: "   Class for the RevoScaleR Local Compute Context.   " 
keywords: "(revoAnalytics), RxLocalSeq-class, show,RxLocalSeq-method, classes" 
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
 
 
 
 
 #RxLocalSeq-class: Class RxLocalSeq 
 ##Description
 
Class for the RevoScaleR Local Compute Context.  
 
 
 ## Generators 

 
The targeted generator [RxLocalSeq](RxLocalSeq.md) as well as the general generator
[RxComputeContext](RxComputeContext.md).
 
 ## Extends 

 
Class RxComputeContext, directly.
 
 ## Methods 

 


###`show`
`signature(object = "RxLocalSeq")`: ...



 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[RxSpark](RxSpark.md),
RxHadoopMR,
[RxInSqlServer](RxInSqlServer.md),
[RxLocalParallel](RxLocalParallel.md),
[RxForeachDoPar](RxForeachDoPar.md),
[RxComputeContext](RxComputeContext.md),
[rxSetComputeContext](rxSetComputeContext.md).
   
 ##Examples

 ```
   
  ## Not run:
 
myComputeContext <- RxComputeContext("RxLocalSeq")
is(myComputeContext, "RxComputeContext")
# [1] TRUE
is(myComputeContext, "RxLocalSeq")
# [1] TRUE
 ## End(Not run) 
  
 
```
 
 
