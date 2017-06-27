--- 
 
# required metadata 
title: "Generate Local Compute Context" 
description: "Creates a local compute context object. Computations using rx_exec will be processed sequentially. This is the default compute context." 
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

## RxLocalSeq


### Usage



```
revoscalepy.computecontext.RxLocalSeq.RxLocalSeq()
```




### Description

Creates a local compute context object. Computations using rx_exec will be processed sequentially. This is the default compute context.


### Arguments


### Returns

object of class RxLocalSeq.


### Author

Microsoft Corporation [Microsoft Technical Support](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409.md)


### See also


### Example



```
from revoscalepy import RxLocalSeq
localcc = RxLocalSeq()
```

