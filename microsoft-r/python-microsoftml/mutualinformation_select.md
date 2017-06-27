--- 
 
# required metadata 
title: "MISSING TITLE" 
description: "" 
keywords: "M, I, S, S, I, N, G,  , K, E, Y, W, O, R, D, S" 
author: "HeidiSteen" 
manager: "" 
ms.date: "06/27/2017" 
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


### Usage



```
microsoftml.modules.select_features.mutualinformation_select(cols: (<class ‘list’>, <class ‘str’>), label, num_features_to_keep=1000, num_bins=256, **kargs)
```



Selects the top k slots across all specified columns ordered by their mutual information with the label column.
