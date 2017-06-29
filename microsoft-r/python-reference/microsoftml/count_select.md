--- 
 
# required metadata 
title: "" 
description: "" 
keywords: "" 
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

## count_select


*Applies to:* SQL Server 2017, Machine Learning Services 9.3


### Usage



```
microsoftml.modules.select_features.count_select(cols: (<class ‘list’>, <class ‘str’>), count=1, **kargs)
```



Selects the slots for which the count of non-default values is greater than or equal to a threshold.
