--- 
 
# required metadata 
title: "Learners Python Objects" 
description: "An instance of the following objects is returned by every training function. They all inherit from the class BaseLearner and implement common methods.get_algo_args returns the training parameters,  coef_ retrieves the coefficients,  summary_ returns training information.The content changes based on the trained learner." 
keywords: "models, learners" 
author: "bradsev" 
manager: "jhubbard" 
ms.date: "07/13/2017" 
ms.topic: "reference" 
ms.prod: "microsoft-r" 
ms.service: "" 
ms.assetid: "" 
 
# optional metadata 
ROBOTS: "" 
audience: "" 
ms.devlang: "Python" 
ms.reviewer: "" 
ms.suite: "" 
ms.tgt_pltfrm: "" 
ms.technology: "r-server" 
ms.custom: "" 
 
---

# MicrosoftML Learners Objects


**Applies to: SQL Server 2017**


## Description

An instance of the following objects is returned by every
training function. They all inherit from the class *BaseLearner* and implement common methods.

* `get_algo_args` returns the training parameters, 

* `coef_`retrieves the coefficients, 

* `summary_` returns training information. 

The content changes based on the trained learner.


## Base Learner


## Usage



```
class microsoftml.modules.base_learner.BaseLearner(**kwargs)
```



Base class for all the learners.


## Usage



```
coef_
```



Get model coefficients.


## Usage



```
fit(formula: str, data: [<class ‘revoscalepy.datasource.RxDataSource.RxDataSource’>, <class ‘pandas.core.frame.DataFrame’>], ml_transforms: list = None, ml_transform_vars: list = None, row_selection: str = None, transforms: dict = None, transform_objects: dict = None, transform_function: str = None, transform_variables: list = None, transform_packages: list = None, transform_environment: dict = None, blocks_per_read: int = None, report_progress: int = None, verbose: int = 1, compute_context: revoscalepy.computecontext.RxComputeContext.RxComputeContext = None, **kargs)
```



Fit the model.


## Usage



```
get_algo_args()
```



Get algorithm arguments.


## Usage



```
predict(data: typing.Union[revoscalepy.datasource.RxDataSource.RxDataSource, pandas.core.frame.DataFrame], output_data: typing.Union[revoscalepy.datasource.RxDataSource.RxDataSource, str] = None, write_model_vars: bool = False, extra_vars_to_write: list = None, suffix: str = None, overwrite: bool = False, data_threads: int = None, blocks_per_read: int = None, report_progress: int = None, verbose: int = 1, compute_context: revoscalepy.computecontext.RxComputeContext.RxComputeContext = None)
```




## Usage



```
summary_
```



Get model summary.


## Specific Learners


## Usage



```
class microsoftml.FastTrees(method: [‘binary’, ’regression’] = ‘binary’, num_trees: int = 100, num_leaves: int = 20, learning_rate: float = 0.2, min_split: int = 10, example_fraction: float = 0.7, feature_fraction: float = 1, split_fraction: float = 1, num_bins: int = 255, first_use_penalty: float = 0, gain_conf_level: float = 0, unbalanced_sets: bool = False, train_threads: int = 8, random_seed: int = None, **kargs)
```



FastTree binary or regression model


## Usage



```
get_train_node(**all_args)
```



get train node


## Usage



```
class microsoftml.OneClassSvm(cache_size: float = 100, kernel: [<function linear_kernel at 0x000001FF51091F28>, <function polynomial_kernel at 0x000001FF510A30D0>, <function rbf_kernel at 0x000001FF51091D90>, <function sigmoid_kernel at 0x000001FF510A3158>] = {‘settings’: {}, ’name’: ‘RbfKernel’}, epsilon: float = 0.001, nu: float = 0.1, shrink: bool = True, normalize: [‘No’, ’Warn’, ’Auto’, ’Yes’] = ‘Auto’, **kargs)
```



one-class svm


## Usage



```
get_train_node(**all_args)
```



get train node


## Usage



```
class microsoftml.FastForest(method: [‘binary’, ’regression’] = ‘binary’, num_trees: int = 100, num_leaves: int = 20, min_split: int = 10, example_fraction: float = 0.7, feature_fraction: float = 0.7, split_fraction: float = 0.7, num_bins: int = 255, first_use_penalty: float = 0, gain_conf_level: float = 0, train_threads: int = 8, random_seed: int = None, **kargs)
```



FastForest binary or regression model


## Usage



```
get_train_node(**all_args)
```



get train node


## Usage



```
class microsoftml.FastLinear
```



SDCA binary or regression model


## Usage



```
get_train_node(**all_args)
```



get train node


## Usage



```
class microsoftml.LogisticRegression(method: [‘binary’, ’multiClass’] = ‘binary’, l2_weight: float = 1, l1_weight: float = 1, opt_tol: float = 1e-07, memory_size: int = 20, init_wts_diameter: float = 0, max_iterations: int = 2147483647, show_training_stats: bool = False, sgd_init_tol: float = 0, train_threads: int = None, dense_optimizer: bool = False, normalize: [‘No’, ’Warn’, ’Auto’, ’Yes’] = ‘Auto’, **kargs)
```



logistic regression


## Usage



```
aic(k=2)
```



get model aic


## Usage



```
coef_
```



get model coefficients


## Usage



```
deviance_
```



get residual deviance


## Usage



```
get_algo_args()
```



get algorithm arguments


## Usage



```
get_train_node(**all_args)
```



get train node


## Usage



```
class microsoftml.NeuralNetwork(method: [‘binary’, ’multiClass’, ’regression’] = ‘binary’, num_hidden_nodes: int = 100, num_iterations: int = 100, optimizer: [‘adadelta_optimizer’, ’sgd_optimizer’] = {‘settings’: {}, ’name’: ‘SgdOptimizer’}, net_definition: str = None, init_wts_diameter: float = 0.1, max_norm: float = 0, acceleration: [‘avx_math’, ’clr_math’, ’gpu_math’, ’mkl_math’, ’sse_math’] = {‘settings’: {}, ’name’: ‘AvxMath’}, mini_batch_size: int = 1, normalize: [‘No’, ’Warn’, ’Auto’, ’Yes’] = ‘Auto’, **kargs)
```



neural network


## Usage



```
get_train_node(**all_args)
```



get train node


## Usage



```
class microsoftml.OneClassSvm(cache_size: float = 100, kernel: [<function linear_kernel at 0x000001FF51091F28>, <function polynomial_kernel at 0x000001FF510A30D0>, <function rbf_kernel at 0x000001FF51091D90>, <function sigmoid_kernel at 0x000001FF510A3158>] = {‘settings’: {}, ’name’: ‘RbfKernel’}, epsilon: float = 0.001, nu: float = 0.1, shrink: bool = True, normalize: [‘No’, ’Warn’, ’Auto’, ’Yes’] = ‘Auto’, **kargs)
```



one-class svm


## Usage



```
get_train_node(**all_args)
```



get train node


### See also

[`rx_fast_forest`](rx-fast-forest.md),
[`rx_fast_trees`](rx-fast-trees.md),
[`rx_fast_linear`](rx-fast-linear.md),
[`rx_logistic_regression`](rx-logistic-regression.md),
[`rx_neural_network`](rx-neural-network.md),
[`rx_oneclass_svm`](rx-oneclass-svm.md),
[`rx_predict`](rx-predict.md)
