--- 
 
# required metadata 
title: "Drops columns from the dataset" 
description: "Specified columns to drop from the dataset." 
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

# *microsoftml.drop_columns*: Drops columns from a dataset


**Applies to: SQL Server 2017 RC1**


## Usage



```
microsoftml.drop_columns(cols: [<class ‘list’>, <class ‘str’>], **kargs)
```




## Description

Specified columns to drop from the dataset.


## Arguments


### cols

A character string or list of the names of the variables to drop.


### kargs

Additional arguments sent to compute engine.


## Returns

An object defining the transform.


## See also

[`concat`](concat.md),
[`select_columns`](select-columns.md).
