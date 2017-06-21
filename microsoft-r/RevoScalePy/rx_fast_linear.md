--- 
 
# required metadata 
title: "Fast Linear" 
description: "Creates a list containing the function name and arguments to train a" 
keywords: "M, I, S, S, I, N, G,  , K, E, Y, W, O, R, D, S" 
author: "HeidiSteen" 
manager: "" 
ms.date: "06/21/2017" 
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

# rx_fast_linear

microsoftml.modules.fast_linear.rx_fast_linear(formula, data, method: [‘binary’, ’regression’] = ‘binary’, loss_function=None, l2_weight=None, l1_weight=None, train_threads=None, convergence_tolerance=0.1, max_iterations=None, shuffle=True, check_frequency=None, normalize: [‘No’, ’Warn’, ’Auto’, ’Yes’] = ‘Auto’, ml_transforms=None, ml_transform_vars=None, row_selection=None, transforms=None, transform_objects=None, transform_func=None, transform_vars=None, transform_packages=None, transform_envir=None, blocks_per_read=1, report_progress=2, verbose=1, compute_context=<revoscalepy.computecontext.RxLocalSeq.RxLocalSeq object>)



## Description

Creates a list containing the function name and arguments to train a
Fast Linear model with ``rx_ensemble()``.
:param loss_function: Specifies the empirical loss function to optimize.
For binary classification, the following choices are available:

* ``log_loss()``: The log-loss. This is the default. 

* ``hinge_loss()``: The SVM hinge loss. Its parameter represents the margin size. 

* ``smooth_hinge_loss()``: The smoothed hinge loss. Its parameter represents the smoothing constant. 

For linear regression, squared loss ``squared_loss()`` is
currently supported. When this parameter is set to *None*, its
default value depends on the type of learning:

* ``log_loss()`` for binary classification. 

* ``squared_loss()`` for linear regression. 


## Parameters


### l1_weight

Specifies the L1 regularization weight. The value must be
either non-negative or *None*. If *None* is specified, the
actual value is automatically computed based on data set. *None*
is the default value.


### l2_weight

Specifies the L2 regularization weight. The value must be
either non-negative or *None*. If *None* is specified, the
actual value is automatically computed based on data set. *None*
is the default value.


### train_threads

Specifies how many concurrent threads can be used to run
the algorithm. When this parameter is set to *None*, the number of
threads used is determined based on the number of logical processors
available to the process as well as the sparsity of data. Set it to ``1``
to run the algorithm in a single thread.


### convergence_tolerance

Specifies the tolerance threshold used as a
convergence criterion. It must be between 0 and 1. The default value is
``0.1``. The algorithm is considered to have converged if the relative
duality gap, which is the ratio between the duality gap and the primal loss,
falls below the specified convergence tolerance.


### max_iterations

Specifies an upper bound on the number of training
iterations. This parameter must be positive or *None*. If *None*
is specified, the actual value is automatically computed based on data set.
Each iteration requires a complete pass over the training data. Training
terminates after the total number of iterations reaches the specified
upper bound or when the loss function converges, whichever happens earlier.


### shuffle

Specifies whether to shuffle the training data. Set ``TRUE``
to shuffle the data; ``FALSE`` not to shuffle. The default
value is ``TRUE``. SDCA is a stochastic optimization algorithm.  If
shuffling is turned on, the training data is shuffled on each iteration.


### check_frequency

The number of iterations after which the loss function
is computed and checked to determine whether it has converged. The value
specified must be a positive integer or *None*. If *None*,
the actual value is automatically computed based on data set. Otherwise,
for example, if ``checkFrequency = 5`` is specified, then the loss
function is computed and convergence is checked every 5 iterations. The
computation of the loss function requires a separate complete pass over
the training data.
