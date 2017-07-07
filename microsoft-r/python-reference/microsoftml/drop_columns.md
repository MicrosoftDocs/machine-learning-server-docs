--- 
 
# required metadata 
title: "Drops columns from the dataset" 
description: "Specified columns to drop from the dataset." 
keywords: "transform, schema" 
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

## ``drop_columns``: Select and drop a subset of columns


*Applies to:* SQL Server 2017, Machine Learning Services 9.3


### Usage



```
microsoftml.drop_columns(cols: [<class ‘list’>, <class ‘str’>], **kargs)
```




### Description

Specified columns to drop from the dataset.


### Arguments


##### cols

A character string or list of the names of the variables to drop.


##### kargs

Additional arguments sent to compute engine.


### Returns

An object defining the transform.


### See also

[``concat``](concat.md),
[``select_columns``](select_columns.md).
