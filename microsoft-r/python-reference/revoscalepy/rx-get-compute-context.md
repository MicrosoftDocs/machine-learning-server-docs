--- 
 
# required metadata 
title: "rx_get_compute_context: Gets the current compute context (revoscalepy)" 
description: "Gets the active compute context for revoscalepy computations" 
keywords: "context" 
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

# rx_get_compute_context


 


## Usage



```
revoscalepy.rx_get_compute_context() -> revoscalepy.computecontext.RxComputeContext.RxComputeContext
```





## Description

Gets the active compute context for revoscalepy computations


## Arguments


### compute_context

Character string specifying class name or description
of the specific class to instantiate, or an existing RxComputeContext object.
Choices include: “RxLocalSeq” or “local”, “RxInSqlServer”.


## Returns

rx_set_compute_context returns the previously active compute context
invisibly. rx_get_compute_context returns the active compute context.


## See also

`RxComputeContext`,
[`RxLocalSeq`](RxLocalSeq.md),
[`RxInSqlServer`](RxInSqlServer.md),
[`rx_set_compute_context`](rx-set-compute-context.md).


## Example



```
from revoscalepy import RxLocalSeq, RxInSqlServer, rx_get_compute_context, rx_set_compute_context

local_cc = RxLocalSeq()
sql_server_cc = RxInSqlServer('Driver=SQL Server;Server=.;Database=RevoTestDb;Trusted_Connection=True;')
previous_cc = rx_set_compute_context(sql_server_cc)
rx_get_compute_context()
```

