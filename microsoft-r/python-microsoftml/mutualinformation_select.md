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

## mutualinformation_select


*Applies to:* SQL Server 2017, Machine Learning Services 9.3


### Usage



```
microsoftml.modules.select_features.mutualinformation_select(cols: (<class ‘list’>, <class ‘str’>), label, num_features_to_keep=1000, num_bins=256, **kargs)
```



Selects the top k slots across all specified columns ordered by their mutual information with the label column.
