--- 
 
# required metadata 
title: "rx_lin_mod: Linear Models" 
description: "Fit linear models on small or large data." 
keywords: "linear" 
author: "bradsev" 
manager: "jhubbard" 
ms.date: "08/31/2017" 
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

# `rx_lin_mod`


**Applies to: SQL Server 2017 RC2**


## Usage



```
revoscalepy.rx_lin_mod(formula, data, pweights=None, fweights=None, cube: bool = False, cube_predictions: bool = False, row_selection: str = None, transforms: dict = None, transform_objects: dict = None, transform_function: typing.Union[str, <built-in function callable>] = None, transform_variables: list = None, transform_packages: list = None, transform_environment=None, drop_first: bool = False, drop_main: bool = True, cov_coef: bool = False, cov_data: bool = False, blocks_per_read: int = 1, report_progress: int = None, verbose: int = 0, compute_context=None, **kwargs)
```




## Description

Fit linear models on small or large data.


## Arguments


### formula

statistical model using symbolic formulas.


### data

either a data source object, a character string specifying a
.xdf file, or a data frame object.


### pweights

character string specifying the variable to use as probability
weights for the observations.


### fweights

character string specifying the variable to use as frequency
weights for the observations.


### cube

bool flag. If True and the first term of the predictor variables
is categorical (a factor or an interaction of factors), the regression is
performed by applying the Frisch-Waugh-Lovell Theorem, which uses a partitioned
inverse to save on computation time and memory.


### cube_predictions

bool flag. If True and cube is True the predicted
values are computed and included in the countDF component of the returned
value. This may be memory intensive.


### row_selection

None. Not currently supported, reserved for future use.


### transforms

None. Not currently supported, reserved for future use.


### transform_objects

None. Not currently supported, reserved for future use.


### transform_function

variable transformation function. The variables used
in the transformation function must be specified in transformVars if they
are not variables used in the model.


### transform_variables

list of strings of input data set variables needed
for the transformation function.


### transform_packages

None. Not currently supported, reserved for future use.


### transform_environment

None. Not currently supported, reserved for future use.


### drop_first

bool flag. If False, the last level is dropped in all sets
of factor levels in a model. If that level has no observations (in any of the
sets), or if the model as formed is otherwise determined to be singular, then
an attempt is made to estimate the model by dropping the first level in all sets
of factor levels. If True, the starting position is to drop the first level. Note
that for cube regressions, the first set of factors is excluded from these rules
and the intercept is dropped.


### drop_main

bool value. If True, main-effect terms are dropped before their
interactions.


### cov_coef

bool flag. If True and if cube is False, the variance-covariance
matrix of the regression coefficients is returned.


### cov_data

bool flag. If True and if cube is False and if constant term is
included in the formula, then the variance-covariance matrix of the data is
returned.


### blocks_per_read

number of blocks to read for each chunk of data read from
the data source.


### report_progress

integer value with options:
0: no progress is reported.
1: the number of processed rows is printed and updated.
2: rows processed and timings are reported.
3: rows processed and all timings are reported.


### verbose

integer value. If 0, no additional output is printed. If 1,
additional summary information is printed.


### compute_context

a RxComputeContext object for prediction.


### kwargs

additional parameters


## Returns

a RxLinModResults object of linear model.


## See also

[`rx_logit`](rx-logit.md).


## Example



```
import os
import tempfile
from revoscalepy import RxOptions, RxXdfData, rx_lin_mod

sample_data_path = RxOptions.get_option("sampleDataDir")
in_mort_ds = RxXdfData(os.path.join(sample_data_path, "mortDefaultSmall.xdf"))

lin_mod = rx_lin_mod("creditScore ~ yearsEmploy", in_mort_ds)
```

