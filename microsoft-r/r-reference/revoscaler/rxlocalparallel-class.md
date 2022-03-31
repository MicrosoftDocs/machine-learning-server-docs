--- 

# required metadata 
title: "RxLocalParallel-class class (revoAnalytics) | Microsoft Docs" 
description: "   Class for the RevoScaleR Local Parallel Compute Context.   " 
keywords: "(revoAnalytics), RxLocalParallel-class, show,RxLocalParallel-method, classes" 
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




 # RxLocalParallel-class: Class RxLocalParallel 
 ## Description

Class for the RevoScaleR Local Parallel Compute Context.  


 ## Generators 


The targeted generator [RxLocalParallel](RxLocalParallel.md) as well as the general generator
[RxComputeContext](RxComputeContext.md).

 ## Extends 


Class RxComputeContext, directly.

 ## Methods 




### `show`
`signature(object = "RxLocalParallel")`: ...




 ## Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)


 ## See Also

RxHadoopMR,
[RxSpark](RxSpark.md),
[RxInSqlServer](RxInSqlServer.md),
[RxLocalSeq](RxLocalSeq.md).

 ## Examples

 ```

  ## Not run:

myComputeContext <- RxComputeContext("RxLocalParallel")
is(myComputeContext, "RxComputeContext")
# [1] TRUE
is(myComputeContext, "RxLocalParallel")
# [1] TRUE
 ## End(Not run) 
```


