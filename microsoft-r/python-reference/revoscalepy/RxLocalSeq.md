--- 
 
# required metadata 
title: "Generate Local Compute Context" 
description: "Creates a local compute context object. Computations using rx_exec will be processed sequentially. This is the default compute context." 
keywords: "context, local" 
author: "bradsev" 
manager: "jhubbard" 
ms.date: "07/28/2017" 
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

# `RxLocalSeq`


**Applies to: SQL Server 2017 RC1**


## Usage



```
class revoscalepy.RxLocalSeq
```




## Description

Creates a local compute context object. Computations using rx_exec will be processed sequentially. This is the default compute context.


## Arguments


## Returns

object of class `RxLocalSeq`.


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

