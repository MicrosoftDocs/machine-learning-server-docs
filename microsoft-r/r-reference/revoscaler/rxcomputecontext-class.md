--- 
 
# required metadata 
title: "RxComputeContext-class function (RevoScaleR) " 
description: "   Base class for all RevoScaleR Compute Contexts.   " 
keywords: "(RevoScaleR), RxComputeContext-class, show,RxComputeContext-method, classes" 
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
 
 
 
 
 #RxComputeContext-class: Class RxComputeContext 
 ##Description
 
Base class for all RevoScaleR Compute Contexts.  
 
 
 ## Objects from the Class 

 
A virtual class: No objects may be created from it.
 
 ## Generator 

 
The generator for classes that extend RxComputeContext is
[RxComputeContext](RxComputeContext.md).  
 
 

 
 
 
 ##See Also
 
[RxHadoopMR](RxHadoopMR.md),
[RxSpark](RxSpark.md),
[RxInSqlServer](RxInSqlServer.md),
[RxLocalSeq](RxLocalSeq.md),
[RxLocalParallel](RxLocalParallel.md),
[RxForeachDoPar](RxForeachDoPar.md).
   
 ##Examples

 ```
   
  myComputeContext <- RxComputeContext("local")
  is(myComputeContext, "RxComputeContext")
  # [1] TRUE
  is(myComputeContext, "RxLocalSeq")
  # [1] TRUE
 
```
 
 
