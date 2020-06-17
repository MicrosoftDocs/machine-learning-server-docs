--- 
 
# required metadata 
title: "RxLocalSeq: Generate local compute context (revoscalepy)" 
description: "Creates a local compute context object. Computations using rx_exec will be processed sequentially. This is the default compute context." 
keywords: "context, local" 
author: "dphansen" 
manager: "cgronlun" 
ms.date: 07/15/2019
ms.topic: "reference" 
ms.prod: "mlserver" 
ms.service: "" 
ms.assetid: "" 
 
# optional metadata 
ROBOTS: "" 
audience: "" 
ms.devlang: "Python" 
ms.reviewer: "" 
ms.suite: "" 
ms.tgt_pltfrm: "" 
#ms.technology: "" 
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

