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

## BaseLearner


*Applies to:* SQL Server 2017, Machine Learning Services 9.3


### Usage



```
class microsoftml.modules.base_learner.BaseLearner(**kwargs)
```



base class for all the learners


### Usage



```
coef_
```



get model coefficients


### Usage



```
fit(formula, data, ml_transforms=None, ml_transform_vars=None, row_selection=None, transforms=None, transform_objects=None, transform_func=None, transform_vars=None, transform_packages=None, transform_envir=None, blocks_per_read=1, report_progress=2, verbose=1, compute_context=<revoscalepy.computecontext.RxLocalSeq.RxLocalSeq object>, **kargs)
```



fit the model


### Usage



```
get_algo_args()
```



get algorithm arguments


### Usage



```
get_model_summary()
```



get model summary


### Usage



```
get_train_node(**kargs)
```



get train node


### Usage



```
predict(data, out_data=None, write_model_vars=False, extra_vars_to_write=None, suffix=None, overwrite=False, data_threads=None, blocks_per_read=1, report_progress=2, verbose=1, compute_context=<revoscalepy.computecontext.RxLocalSeq.RxLocalSeq object>)
```


