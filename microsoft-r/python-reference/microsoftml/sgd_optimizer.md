--- 
 
# required metadata 
title: "sgd_optimizer" 
description: "Stochastic gradient descent optimizer." 
keywords: "optimizer, sgd" 
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

## ``sgd_optimizer``: Stochastic Gradient Descent


*Applies to:* SQL Server 2017, Machine Learning Services 9.3


### Usage



```
microsoftml.sgd_optimizer(learning_rate: numbers.Real = None, momentum: numbers.Real = None, nag: bool = None, weight_decay: numbers.Real = None, l_rate_red_ratio: numbers.Real = None, l_rate_red_freq: numbers.Real = None, l_rate_red_error_ratio: numbers.Real = None)
```




### Description

Stochastic gradient descent optimizer.


### Arguments


##### learning_rate

Learning rate (settings).


##### momentum

Momentum Term (settings).


##### nag

Use Nesterov’s accelerated gradient (settings).


##### weight_decay

Weight decay (settings).


##### l_rate_red_ratio

Learning rate reduction ratio (settings).


##### l_rate_red_freq

Learning rate reduction ratio (settings).


##### l_rate_red_error_ratio

Relative error reduction criterion for learning rate reduction (settings).


### See also

[``adadelta_optimizer``](adadelta_optimizer.md)
