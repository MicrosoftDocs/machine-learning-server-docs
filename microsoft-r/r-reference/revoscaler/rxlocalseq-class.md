--- 
 
# required metadata 
title: "Class RxLocalSeq" 
description: "   Class for the RevoScaleR Local Compute Context.   " 
keywords: "RevoScaleR, RxLocalSeq-class, show,RxLocalSeq-method, classes" 
author: "heidisteen" 
manager: "jhubbard" 
ms.date: "04/18/2017" 
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
 
 
 
 
 #`RxLocalSeq-class`: Class RxLocalSeq

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
Class for the RevoScaleR Local Compute Context.  
 
 
 ## Generators 

 
The targeted generator [RxLocalSeq](rxlocalseq.md) as well as the general generator
[RxComputeContext](rxcomputecontext.md).
 
 ## Extends 

 
Class RxComputeContext, directly.
 
 ## Methods 

 


###`show`
`signature(object = "RxLocalSeq")`: ...



 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[RxSpark](rxspark.md),
[RxHadoopMR](rxhadoopmr.md),
[RxInSqlServer](rxinsqlserver.md),
[RxInTeradata](rxinteradata.md),
[RxLocalParallel](rxlocalparallel.md),
[RxForeachDoPar](rxforeachdopar.md),
[RxComputeContext](rxcomputecontext.md),
[rxSetComputeContext](rxsetcomputecontext.md).
   
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
 
 
