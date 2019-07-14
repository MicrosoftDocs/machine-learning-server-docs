--- 
 
# required metadata 
title: "oneClassSvm function (MicrosoftML) " 
description: " Creates a list containing the function name and arguments to train a  OneClassSvm model with [rxEnsemble](rxEnsemble.md). " 
keywords: "(MicrosoftML), oneClassSvm" 
author: "heidisteen" 
manager: "cgronlun" 
ms.date: 07/15/2019
ms.topic: "reference" 
ms.prod: "mlserver" 
ms.service: "" 
ms.assetid: "" 
 
# optional metadata 
ROBOTS: "" 
audience: "" 
ms.devlang: "" 
ms.reviewer: "" 
ms.suite: "" 
ms.tgt_pltfrm: "" 
#ms.technology: "" 
ms.custom: "" 
 
--- 
 
 
 
 
 # oneClassSvm: oneClassSvm 
 ## Description
 
Creates a list containing the function name and arguments to train a
 OneClassSvm model with [rxEnsemble](rxEnsemble.md).
 
 
 ## Usage

```   
  oneClassSvm(cacheSize = 100, kernel = rbfKernel(), epsilon = 0.001,
    nu = 0.1, shrink = TRUE, ...)
 
```
 
 ## Arguments

   
  
 ### `cacheSize`
 The maximal size in MB of the cache that stores the training data. Increase this for large training sets. The default value is 100 MB. 
  
  
  
 ### `kernel`
 A character string representing the kernel used for computing inner products. For more information, see [maKernel](kernel.md). The following choices are available:   
*   `rbfKernel()`: Radial basis function kernel. Its parameter  represents`gamma` in the term `exp(-gamma|x-y|^2`. If not  specified, it defaults to `1` divided by the number of features used. For example, `rbfKernel(gamma = .1)`. This is the default value. 
*   `linearKernel()`: Linear kernel.   
*   `polynomialKernel()`: Polynomial kernel with parameter names `a`,  `bias`, and `deg` in the term `(a*<x,y> + bias)^deg`. The  `bias`, defaults to `0`. The degree, `deg`, defaults to  `3`. If `a` is not specified, it is set to `1` divided by the number of features. For example, `maKernelPoynomial(bias = 0, deg = ``  3)`.   
*   `sigmoidKernel()`: Sigmoid kernel with parameter names  `gamma` and `coef0` in the term `tanh(gamma*<x,y> + coef0)`.  `gamma`, defaults to `1` divided by the number of features. The  parameter `coef0` defaults to `0`.  For example,  `sigmoidKernel(gamma = .1, coef0 = 0)`.   
 
  
  
  
 ### `epsilon`
 The threshold for optimizer convergence. If the  improvement between iterations is less than the threshold, the algorithm  stops and returns the current model. The value must be greater than or equal to `.Machine$double.eps`. The default value is 0.001. 
  
  
  
 ### `nu`
 The trade-off between the fraction of outliers and the number of support vectors (represented by the Greek letter nu). Must be between 0 and 1, typically between 0.1 and 0.5. The default value is 0.1. 
  
  
  
 ### `shrink`
 Uses the shrinking heuristic if `TRUE`. In this case, some samples will be "shrunk" during the training procedure, which may speed up training. The default value is `TRUE`. 
  
  
  
 ### ` ...`
 Additional arguments to be passed directly to the Microsoft Compute Engine. 
  
 
 
 
