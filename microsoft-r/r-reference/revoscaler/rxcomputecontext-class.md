--- 

# required metadata 
title: "RxComputeContext-class class (revoAnalytics) | Microsoft Docs" 
description: "   Base class for all RevoScaleR Compute Contexts.   " 
keywords: "(revoAnalytics), RxComputeContext-class, show,RxComputeContext-method, classes" 
author: "DaniBunny"
ms.author: "dacoelho" 
manager: "cgronlun" 
ms.date: 07/15/2019
ms.topic: "reference" 
ms.prod: "mlserver" 
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

--- 




 # RxComputeContext-class: Class RxComputeContext 
 ## Description

Base class for all RevoScaleR Compute Contexts.  


 ## Objects from the Class 


A virtual class: No objects may be created from it.

 ## Generator 


The generator for classes that extend RxComputeContext is
[RxComputeContext](RxComputeContext.md).  


 ## Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[RxHadoopMR](RevoScaleR-deprecated.md),
[RxSpark](RxSpark.md),
[RxInSqlServer](RxInSqlServer.md),
[RxLocalSeq](RxLocalSeq.md),
[RxLocalParallel](RxLocalParallel.md),
[RxForeachDoPar](RxForeachDoPar.md).

 ## Examples

 ```

  myComputeContext <- RxComputeContext("local")
  is(myComputeContext, "RxComputeContext")
  # [1] TRUE
  is(myComputeContext, "RxLocalSeq")
  # [1] TRUE
```


