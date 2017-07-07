--- 
 
# required metadata 
title: "Generate Local Compute Context" 
description: "Creates a local compute context object. Computations using rx_exec will be processed sequentially. This is the default compute context." 
keywords: "context, local" 
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

## ``RxLocalSeq``


*Applies to:* SQL Server 2017, Machine Learning Services 9.3


### Usage



```
class revoscalepy.RxLocalSeq
```




### Description

Creates a local compute context object. Computations using rx_exec will be processed sequentially. This is the default compute context.


### Arguments


### Returns

object of class ``RxLocalSeq``.


### Example



```
from revoscalepy import RxLocalSeq
localcc = RxLocalSeq()
```

