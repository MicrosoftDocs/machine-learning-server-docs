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

## rx_featurize


*Applies to:* SQL Server 2017, Machine Learning Services 9.3


### Usage



```
microsoftml.modules.featurize.rx_featurize(data, out_data=None, overwrite=False, data_threads=None, random_seed=None, max_slots=5000, ml_transforms=None, ml_transform_vars=None, row_selection=None, transforms=None, transform_objects=None, transform_func=None, transform_vars=None, transform_packages=None, transform_envir=None, blocks_per_read=1, report_progress=2, verbose=1, compute_context=<revoscalepy.computecontext.RxLocalSeq.RxLocalSeq object>)
```



Transforms data from an input data set to an output data set
