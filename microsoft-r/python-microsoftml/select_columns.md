--- 
 
# required metadata 
title: "Selects a set of columns, dropping all others" 
description: "Selects a set of columns to retrain, dropping all others." 
keywords: "transform" 
author: "Microsoft Corporation Microsoft Technical Support" 
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

## select_columns


*Applies to:* SQL Server 2017, Machine Learning Services 9.3


### Usage



```
microsoftml.modules.schema_manipulation.select_columns(cols: (<class ‘list’>, <class ‘str’>), **kargs)
```




### Description

Selects a set of columns to retrain, dropping all others.


### Arguments


##### cols

Specifiies character vector or list of the names of the variables to keep.


### Returns

A ``maml`` object defining the transform.


### Author

Microsoft Corporation [Microsoft Technical Support](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409.md)
