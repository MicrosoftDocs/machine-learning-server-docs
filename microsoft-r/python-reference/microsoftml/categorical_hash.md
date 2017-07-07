--- 
 
# required metadata 
title: "Machine Learning Categorical HashData TransformDescriptionCategorical hash transform that can be performed on data before" 
description: "Categorical hash transform that can be performed on data before" 
keywords: "transform, catagory, hash" 
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

## ``categorical_hash``: Hash and convert text column into categories


*Applies to:* SQL Server 2017, Machine Learning Services 9.3


### Usage



```
microsoftml.categorical_hash(cols: [<class ‘str’>, <class ‘dict’>, <class ‘list’>], hash_bits: int = 16, seed: int = 314489979, ordered: bool = True, invert_hash: int = 0, output_kind: [‘Bag’, ‘Ind’, ‘Key’, ‘Bin’] = ‘Bag’, **kargs)
```


