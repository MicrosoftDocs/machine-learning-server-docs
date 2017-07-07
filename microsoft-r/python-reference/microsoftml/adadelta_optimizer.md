--- 
 
# required metadata 
title: "adadelta_optimizer" 
description: "Adaptive learing rate method." 
keywords: "optimizer, adadelta" 
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

## ``adadelta_optimizer``: Adaptive learing rate method


*Applies to:* SQL Server 2017, Machine Learning Services 9.3


### Usage



```
microsoftml.adadelta_optimizer(decay: numbers.Real = 0.95, cond: numbers.Real = 1e-06)
```




### Description

Adaptive learing rate method.


### Arguments


##### decay

Decay rate (settings).


##### cond

Condition constant (settings).


### See also

[``sgd_optimizer``](sgd_optimizer.md)
