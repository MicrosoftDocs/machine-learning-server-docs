--- 
 
# required metadata 
title: "sgd_optimizer: sgd_optimizer" 
description: "Stochastic gradient descent optimizer." 
keywords: "optimizer, sgd" 
author: "dphansen" 
manager: "cgronlun" 
ms.date: 07/15/2019
ms.topic: "reference" 
ms.prod: "mlserver" 
ms.service: "" 
ms.assetid: "" 
 
# optional metadata 
ROBOTS: "" 
audience: "" 
ms.devlang: "Python" 
ms.reviewer: "" 
ms.suite: "" 
ms.tgt_pltfrm: "" 
#ms.technology: "" 
ms.custom: "" 
 
---

# *microsoftml.sgd_optimizer*: Stochastic gradient descent





## Usage



```
microsoftml.sgd_optimizer(learning_rate: numbers.Real = None,
    momentum: numbers.Real = None, nag: bool = None,
    weight_decay: numbers.Real = None,
    l_rate_red_ratio: numbers.Real = None,
    l_rate_red_freq: numbers.Real = None,
    l_rate_red_error_ratio: numbers.Real = None)
```





## Description

Stochastic gradient descent optimizer.


## Arguments


### learning_rate

Learning rate (settings).


### momentum

Momentum Term (settings).


### nag

Use Nesterov’s accelerated gradient (settings).


### weight_decay

Weight decay (settings).


### l_rate_red_ratio

Learning rate reduction ratio (settings).


### l_rate_red_freq

Learning rate reduction ratio (settings).


### l_rate_red_error_ratio

Relative error reduction criterion for learning rate reduction (settings).


## See also

[`adadelta_optimizer`](adadelta-optimizer.md)
