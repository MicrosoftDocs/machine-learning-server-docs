--- 
 
# required metadata 
title: "RxForeachDoPar-class function (RevoScaleR) " 
description: "   Class for the RevoScaleR Compute Context using one of the foreach dopar back ends.   " 
keywords: "(RevoScaleR), RxForeachDoPar-class, show,RxForeachDoPar-method, classes" 
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
 
 
 
 
 #RxForeachDoPar-class: Class RxForeachDoPar 
 ##Description
 
Class for the RevoScaleR Compute Context using one of the foreach dopar back ends.  
 
 
 ## Generators 

 
The targeted generator [RxForeachDoPar](RxForeachDoPar.md) as well as the general generator
[RxComputeContext](RxComputeContext.md).
 
 ## Extends 

 
Class RxComputeContext, directly.
 
 ## Methods 

 


###`show`
`signature(object = "RxForeachDoPar")`: ...



 

 
 
 
 ##See Also
 
[RxComputeContext](RxComputeContext.md),
[RxLocalSeq](RxLocalSeq.md),
[RxLocalParallel](RxLocalParallel.md),
[RxSpark](RxSpark.md),
[RxHadoopMR](RxHadoopMR.md).

   
 ##Examples

 ```
   
  ## Not run:
 
myComputeContext <- RxComputeContext("RxForeachDoPar")
is(myComputeContext, "RxComputeContext")
# [1] TRUE
is(myComputeContext, "RxForeachDoPar")
# [1] TRUE
 ## End(Not run) 
  
 
```
 
 
