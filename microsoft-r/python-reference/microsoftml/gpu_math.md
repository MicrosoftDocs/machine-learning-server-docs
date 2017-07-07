--- 
 
# required metadata 
title: "gpu_math" 
description: "NVidia CUDA implementation." 
keywords: "neural network, math, gpu" 
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

## ``gpu_math``


*Applies to:* SQL Server 2017, Machine Learning Services 9.3


### Usage



```
microsoftml.gpu_math(gpu_id: numbers.Real = -1, cu_dnn: bool = False, cu_dnn_algo: str = ‘ImplicitPrecompGemm’)
```




### Description

NVidia CUDA implementation.


### Arguments


##### gpu_id

GPU device id (settings).


##### cu_dnn

Use cuDNN on GPU (settings).


##### cu_dnn_algo

cuDNN optimization options (settings).


### See also

[``avx_math``](avx_math.md),
[``clr_math``](clr_math.md),
[``mkl_math``](mkl_math.md),
[``sse_math``](sse_math.md)
