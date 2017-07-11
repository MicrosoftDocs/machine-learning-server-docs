--- 
 
# required metadata 
title: "RevoScalePy Compute Contexts: Class Generator" 
description: "Main generator for RxComputeContext classes." 
keywords: "context" 
author: "bradsev" 
manager: "jhubbard" 
ms.date: "07/11/2017" 
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

## `RxComputeContext`


*Applies to:* SQL Server 2017, Machine Learning Services 9.3


### Usage



```
class revoscalepy.RxComputeContext(description: str, version: str, id: str = None)
```




### Description

Main generator for RxComputeContext classes.


### Arguments


### Returns

This is a wrapper to specific class generator functions for the
RevoScalePy compute context classes. For example, the RxInSqlServer class
uses function RxInSqlServer as a generator. Therefore either
RxInSqlServer(…) or RxComputeContext(“RxInSqlServer”, …) will create an
RxInSqlServer instance.


### See also

[`RxLocalSeq`](RxLocalSeq.md),
[`RxInSqlServer`](RxInSqlServer.md),
`rx_get_compute_context`,
[`rx_set_compute_context`](rx_set_compute_context.md).


### Example



```
from revoscalepy import RxComputeContext, rx_get_compute_context, rx_set_compute_context

sql_server_cc = RxComputeContext("RxInSqlServer", "1.0")
rx_set_compute_context(sql_server_cc)
rx_get_compute_context()
```


Output:


### Usage



```
static lookup(to_lookup: str) -> revoscalepy.computecontext.RxComputeContext.RxComputeContext
```



Looks up a `RxComputeContext` instance by the specified id
:param to_lookup: The id for which to retrieve the compute context
:returns: The retrieved compute context
:raises: ValueError
