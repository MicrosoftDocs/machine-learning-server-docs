--- 
 
# required metadata 
title: "rx_lin_mod: Linear Models" 
description: "Fit linear models on small or large data." 
keywords: "linear" 
author: "heidist" 
manager: "cgronlun" 
ms.date: "01/19/2018" 
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

# rx_lin_mod


 


## Usage



```
revoscalepy.rx_lin_mod(formula, data, pweights=None, fweights=None,
    cube: bool = False, cube_predictions: bool = False,
    row_selection: str = None, transforms: dict = None,
    transform_objects: dict = None,
    transform_function: typing.Union[str, <built-
    in function callable>] = None,
    transform_variables: list = None,
    transform_packages: list = None, drop_first: bool = False,
    drop_main: bool = True, cov_coef: bool = False,
    cov_data: bool = False, blocks_per_read: int = 1,
    report_progress: int = None, verbose: int = 0,
    compute_context=None, **kwargs)
```





## Description

Fit linear models on small or large data.


## Arguments


### formula

Statistical model using symbolic formulas.


### data

Either a data source object, a character string specifying a
.xdf file, or a data frame object.
If a Spark compute context is being used, this argument may also be an RxHiveData,
RxOrcData, RxParquetData or RxSparkDataFrame object or a Spark data frame object from pyspark.sql.DataFrame.


### pweights

Character string specifying the variable to use as probability
weights for the observations.


### fweights

Character string specifying the variable to use as frequency
weights for the observations.


### cube

Bool flag. If True and the first term of the predictor variables
is categorical (a factor or an interaction of factors), the regression is
performed by applying the Frisch-Waugh-Lovell Theorem, which uses a partitioned
inverse to save on computation time and memory.


### cube_predictions

Bool flag. If True and cube is True the predicted
values are computed and included in the countDF component of the returned
value. This may be memory intensive.


### row_selection

None. Not currently supported, reserved for future use.


### transforms

None. Not currently supported, reserved for future use.


### transform_objects

A dictionary of variables besides the data that are used in the transform function.
See rx_data_step for examples.


### transform_function

Name of the function that will be used to modify the data before the model is built.
The variables used in the transform function must be specified in transform_objects.
See rx_data_step for examples.


### transform_variables

List of strings of the column names needed
for the transform function and model building.


### transform_packages

None. Not currently supported, reserved for future use.


### drop_first

Bool flag. If False, the last level is dropped in all sets
of factor levels in a model. If that level has no observations (in any of the
sets), or if the model as formed is otherwise determined to be singular, then
an attempt is made to estimate the model by dropping the first level in all sets
of factor levels. If True, the starting position is to drop the first level. Note
that for cube regressions, the first set of factors is excluded from these rules
and the intercept is dropped.


### drop_main

Bool value. If True, main-effect terms are dropped before their
interactions.


### cov_coef

Bool flag. If True and if cube is False, the variance-covariance
matrix of the regression coefficients is returned.


### cov_data

Bool flag. If True and if cube is False and if constant term is
included in the formula, then the variance-covariance matrix of the data is
returned.


### blocks_per_read

Number of blocks to read for each chunk of data read from
the data source.


### report_progress

Integer value with options:
0: No progress is reported.
1: The number of processed rows is printed and updated.
2: Rows processed and timings are reported.
3: Rows processed and all timings are reported.


### verbose

Integer value. If 0, no additional output is printed. If 1,
additional summary information is printed.


### compute_context

A RxComputeContext object for prediction.


### kwargs

Additional parameters


## Returns

A RxLinModResults object of linear model.


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

