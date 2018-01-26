--- 
 
# required metadata 
title: "RxLocalSeq: Generate local compute context (revoscalepy)" 
description: "Creates a local compute context object. Computations using rx_exec will be processed sequentially. This is the default compute context." 
keywords: "context, local" 
author: "HeidiSteen" 
manager: "cgronlun" 
ms.date: "01/26/2018" 
ms.topic: "reference" 
ms.prod: "microsoft-r" 
ms.service: "" 
ms.assetid: "" 
 
# optional metadata 
ROBOTS: "" 
audience: "" 
ms.devlang: "Python" 
ms.reviewer: "" 
ms.suite: "" 
ms.tgt_pltfrm: "" 
ms.technology: "r-server" 
ms.custom: "" 
 
---

# RxLocalSeq


 



```
revoscalepy.RxLocalSeq
```





## Description

Creates a local compute context object. Computations using rx_exec will be processed sequentially. This is the default compute context.


## Returns

Object of class `RxLocalSeq`.


## See also

`RxComputeContext`,
[`RxInSqlServer`](RxInSqlServer.md),
[`rx_get_compute_context`](rx-get-compute-context.md),
[`rx_set_compute_context`](rx-set-compute-context.md).


## Example



```
from revoscalepy import RxLocalSeq
localcc = RxLocalSeq()
```

