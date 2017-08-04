--- 
 
# required metadata 
title: "Class RxComputeContext" 
description: "   Base class for all RevoScaleR Compute Contexts.   " 
keywords: "RevoScaleR, RxComputeContext-class, show,RxComputeContext-method, classes" 
author: "HeidiSteen"
ms.author: "heidist" 
manager: "jhubbard" 
ms.date: "04/18/2017" 
ms.topic: "reference" 
ms.prod: "microsoft-r" 
ms.service: "" 
ms.assetid: "" 
 
# optional metadata 
#ROBOTS: "" 
#audience: "" 
#ms.devlang: "" 
#ms.reviewer: "" 
#ms.suite: "" 
#ms.tgt_pltfrm: "" 
ms.technology: "r-server" 
#ms.custom: "" 
 
--- 
 
 
 
 
 #RxComputeContext-class: Class RxComputeContext

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
Base class for all RevoScaleR Compute Contexts.  
 
 
 ## Objects from the Class 

 
A virtual class: No objects may be created from it.
 
 ## Generator 

 
The generator for classes that extend RxComputeContext is
[RxComputeContext](rxcomputecontext.md).  
 
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[RxHadoopMR](rxhadoopmr.md),
[RxSpark](rxspark.md),
[RxInSqlServer](rxinsqlserver.md),
[RxInTeradata](rxinteradata.md),
[RxLocalSeq](rxlocalseq.md),
[RxLocalParallel](rxlocalparallel.md),
[RxForeachDoPar](rxforeachdopar.md).
   
 ##Examples

 ```
   
  myComputeContext <- RxComputeContext("local")
  is(myComputeContext, "RxComputeContext")
  # [1] TRUE
  is(myComputeContext, "RxLocalSeq")
  # [1] TRUE
 
```
 
 
