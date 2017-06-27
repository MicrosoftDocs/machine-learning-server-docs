--- 
 
# required metadata 
title: "RevoScalePy Compute Contexts: Class Generator" 
description: "Main generator for RxComputeContext classes." 
keywords: "M, I, S, S, I, N, G,  , K, E, Y, W, O, R, D, S" 
author: "Microsoft Corporation Microsoft Technical Support" 
manager: "" 
ms.date: "06/26/2017" 
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

# RxComputeContext



```
revoscalepy.computecontext.RxComputeContext.RxComputeContext(description: str, version: str, id: str = None)
```




## Description

Main generator for RxComputeContext classes.


## Parameters


## Returns

This is a wrapper to specific class generator functions for the
RevoScalePy compute context classes. For example, the RxInSqlServer class
uses function RxInSqlServer as a generator. Therefore either
RxInSqlServer(…) or RxComputeContext(“RxInSqlServer”, …) will create an
RxInSqlServer instance.


## Author

Microsoft Corporation [Microsoft Technical Support](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409.md)


## See also


## Example
