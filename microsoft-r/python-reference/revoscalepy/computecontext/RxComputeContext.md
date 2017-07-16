--- 
 
# required metadata 
title: "RevoScalePy Compute Contexts: Class Generator" 
description: "Main generator for RxComputeContext classes." 
keywords: "" 
author: "Microsoft Corporation Microsoft Technical Support" 
manager: "jhubbard" 
ms.date: "07/13/2017" 
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

# RxComputeContext


**Applies to: SQL Server 2017 RC1**


## Usage



```
revoscalepy.computecontext.RxComputeContext.RxComputeContext(description: str, version: str, id: str = None)
```




## Description

Main generator for RxComputeContext classes.


## Arguments


## Returns

This is a wrapper to specific class generator functions for the
RevoScalePy compute context classes. For example, the RxInSqlServer class
uses function RxInSqlServer as a generator. Therefore either
RxInSqlServer(…) or RxComputeContext(“RxInSqlServer”, …) will create an
RxInSqlServer instance.


## Author

Microsoft Corporation [Microsoft Technical Support](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)


## See also

[`RxLocalSeq`](RxLocalSeq.md),
[`RxInSqlServer`](RxInSqlServer.md),
[`rx_get_compute_context`](RxComputeContext-get.md),
[`rx_set_compute_context`](RxComputeContext-set.md).


## Example



```
from revoscalepy import RxComputeContext, rx_get_compute_context, rx_set_compute_context

sql_server_cc = RxComputeContext("RxInSqlServer", "1.0")
rx_set_compute_context(sql_server_cc)
rx_get_compute_context()
```


Output:
