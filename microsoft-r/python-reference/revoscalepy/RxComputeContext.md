--- 
 
# required metadata 
title: "RevoScalePy Compute Contexts: Class Generator" 
description: "Main generator for RxComputeContext classes." 
keywords: "context" 
author: "HeidiSteen" 
manager: "" 
ms.date: "" 
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

## ``RxComputeContext``


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


### Usage



```
static lookup(to_lookup: str) -> revoscalepy.computecontext.RxComputeContext.RxComputeContext
```



Looks up a ``RxComputeContext`` instance by the specified id
:param to_lookup: The id for which to retrieve the compute context
:return: The retrieved compute context
:raises: ValueError
