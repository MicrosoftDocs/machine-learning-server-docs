--- 
 
# required metadata 
title: "oneClassSvm" 
description: "Creates a list containing the function name and arguments to train a" 
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

## rx_oneclass_svm


*Applies to:* SQL Server 2017, Machine Learning Services 9.3


### Usage



```
microsoftml.modules.oneclass_svm.rx_oneclass_svm(formula, data, cache_size=100, kernel: {‘AnomalyKernel’: [‘LinearKernel’, ’PolynomialKernel’, ’RbfKernel’, ’SigmoidKernel’]} = {‘settings’: {}, ’name’: ‘RbfKernel’}, epsilon=0.001, nu=0.1, shrink=True, normalize: [‘No’, ’Warn’, ’Auto’, ’Yes’] = ‘Auto’, ml_transforms=None, ml_transform_vars=None, row_selection=None, transforms=None, transform_objects=None, transform_func=None, transform_vars=None, transform_packages=None, transform_envir=None, blocks_per_read=1, report_progress=2, verbose=1, compute_context=<revoscalepy.computecontext.RxLocalSeq.RxLocalSeq object>)
```




### Description

Creates a list containing the function name and arguments to train a
OneClassSvm model with ``rx_ensemble()``.


### Arguments


##### cache_size

The maximal size in MB of the cache that stores the training
data. Increase this for large training sets. The default value is 100 MB.


##### kernel

A character string representing the kernel used for computing
inner products. For more information, see ``ma_kernel()``. The
following choices are available:

* ``rbfKernel()``: Radial basis function kernel. It’s parameter represents“gamma“ in the term ``exp(-gamma|x-y|^2``. If not specified, it defaults to ``1`` divided by the number of features used. For example, ``rbfKernel(gamma = .1)``. This is the default value. 

* ``linearKernel()``: Linear kernel. 

* ``polynomialKernel()``: Polynomial kernel with parameter names ``a``, ``bias``, and ``deg`` in the term ``(a*<x,y> + bias)^deg``. The ``bias``, defaults to ``0``. The degree, ``deg``, defaults to ``3``. If ``a`` is not specified, it is set to ``1`` divided by the number of features. 

* ``sigmoidKernel()``: Sigmoid kernel with parameter names ``gamma`` and ``coef0`` in the term ``tanh(gamma*<x,y> + coef0)``. ``gamma``, defaults to to ``1`` divided by the number of features. The parameter ``coef0`` defaults to ``0``.  For example, ``sigmoidKernel(gamma = .1, coef0 = 0)``. 


##### epsilon

The threshold for optimizer convergence. If the
improvement between iterations is less than the threshold, the algorithm
stops and returns the current model. The value must be greater than or equal
to ``.Machine$double.eps``. The default value is 0.001.


##### nu

The trade-off between the fraction of outliers and the number of
support vectors (represented by the Greek letter nu). Must be between 0 and
1, typically between 0.1 and 0.5. The default value is 0.1.


##### shrink

Uses the shrinking heuristic if ``TRUE``. In this case,
some samples will be “shrunk” during the training procedure, which may speed
up training. The default value is ``TRUE``.
