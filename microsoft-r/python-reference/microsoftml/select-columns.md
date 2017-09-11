--- 
 
# required metadata 
title: "Selects a set of columns and drops all others" 
description: "Selects a set of columns to retrain, dropping all others." 
keywords: "transform, schema" 
author: "bradsev" 
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

# *microsoftml.select_columns*: Retains columns of a dataset


**Applies to: SQL Server 2017**


## Usage



```
microsoftml.select_columns(cols: [<class ‘list’>, <class ‘str’>], **kargs)
```




## Description

Selects a set of columns to retrain, dropping all others.


## Arguments


### cols

A character string or list of the names of the variables to keep.


### kargs

Additional arguments sent to compute engine.


## Returns

An object defining the transform.


## See also

[`concat`](concat.md),
[`drop_columns`](drop-columns.md).
