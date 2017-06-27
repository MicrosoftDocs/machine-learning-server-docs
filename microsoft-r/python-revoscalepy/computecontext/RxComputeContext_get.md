--- 
 
# required metadata 
title: "Get and Set the compute context" 
description: "Get or set the active compute context for RevoScalePy computationsrx_set_compute_context(computeContext: ‘RxComputeContext’)" 
keywords: "M, I, S, S, I, N, G,  , K, E, Y, W, O, R, D, S" 
author: "Microsoft Corporation Microsoft Technical Support" 
manager: "" 
ms.date: "06/27/2017" 
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

## rx_get_compute_context


### Usage



```
revoscalepy.computecontext.RxComputeContext.rx_get_compute_context() -> revoscalepy.computecontext.RxComputeContext.RxComputeContext
```




### Description

Get or set the active compute context for RevoScalePy computations

rx_set_compute_context(computeContext: ‘RxComputeContext’)
rx_get_compute_context()


### Arguments


##### compute_context

character string specifying class name or description
of the specific class to instantiate, or an existing RxComputeContext object.
Choices include: “RxLocalSeq” or “local”, “RxLocalParallel” or “localpar”.


### Returns

rx_set_compute_context returns the previously active compute context
invisibly. rx_get_compute_context returns the active compute context.


### Author

Microsoft Corporation [Microsoft Technical Support](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409.md)


### See also


### Example



```
localcc = RxLocalSeq()
inSqlServercc = RxInSqlServer('Driver=SQL Server;Server=.;Database=RevoTestDb;Trusted_Connection=True;')
rx_set_compute_context(localcc)
previouscc = rx_set_compute_context(inSqlServercc)
```

