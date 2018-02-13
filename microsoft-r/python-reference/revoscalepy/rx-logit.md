--- 
 
# required metadata 
title: "rx_logit: Fit a logistic regression model (revoscalepy)" 
description: "Use rx_logit to fit logistic regression models for small or large data sets." 
keywords: "logic" 
author: "HeidiSteen" 
manager: "cgronlun" 
ms.date: "01/26/2018" 
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

# rx_logit


 


## Usage



```
revoscalepy.rx_logit()
```





## Description

Use rx_logit to fit logistic regression models for small or large data sets.


## Arguments


### formula

Statistical model using symbolic formulas. Dependent variable must
be binary. It can be a bool variable, a factor with only two categories,
or a numeric variable with values in the range (0,1). In the latter case
it will be converted to a bool.


### data

Either a data source object, a character string specifying a
‘.xdf’ file, or a data frame object.
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

Bool flag. If True and cube is True the estimated
model is evaluated (predicted) for each cell in the cube, fixing the non-cube
variables in the model at their mean values, and these predictions are included
in the countDF component of the returned value. This may be time and memory
intensive for large models.


### variable_selection

A list specifying various parameters that control
aspects of stepwise regression. If it is an empty list (default), no stepwise
model selection will be performed. If not, stepwise regression will be
performed and cube must be False.


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
for the transform function.


### transform_packages

None. Not currently supported, reserved for future use.


### drop_first

Bool flag. If False, the last level is dropped in all sets
of factor levels in a model. If that level has no observations (in any of the
sets), or if the model as formed is otherwise determined to be singular, then
an attempt is made to estimate the model by dropping the first level in all
sets of factor levels. If True, the starting position is to drop the first
level. Note that for cube regressions, the first set of factors is excluded
from these rules and the intercept is dropped.


### drop_main

Bool value. If True, main-effect terms are dropped before
their interactions.


### cov_coef

Bool flag. If True and if cube is False, the variance-covariance
matrix of the regression coefficients is returned.


### cov_data

Bool flag. If True and if cube is False and if constant term
is included in the formula, then the variance-covariance matrix of the data
is returned.


### initial_values

Starting values for the Iteratively Reweighted Least
Squares algorithm used to estimate the model coefficients.


### coef_label_style

Character string specifying the coefficient label style.
The default is “Revo”.


### blocks_per_read

Number of blocks to read for each chunk of data read
from the data source.


### max_iterations

Maximum number of iterations.


### coefficent_tolerance

Convergence tolerance for coefficients. If the
maximum absolute change in the coefficients (step), divided by the maximum
absolute coefficient value, is less than or equal to this tolerance at the
end of an iteration, the estimation is considered to have converged. To
disable this test, set this value to 0.


### gradient_tolerance

This argument is deprecated.


### objective_function_tolerance

Convergence tolerance for the objective function.


### report_progress

Integer value with options:
0: No progress is reported.
1: The number of processed rows is printed and updated.
2: Rows processed and timings are reported.
3: Rows processed and all timings are reported.


### verbose

Integer value.


### compute_context

A RxComputeContext object for prediction.


### kwargs

Additional parameters


## Returns

A RxLogitResults object of linear model.


## See also

[`rx_lin_mod`](rx-lin-mod.md).


## Example



```
import os
from revoscalepy import rx_logit, RxOptions, RxXdfData
sample_data_path = RxOptions.get_option("sampleDataDir")
kyphosis = RxXdfData(os.path.join(sample_data_path, "kyphosis.xdf"))

logit = rx_logit('Kyphosis ~ Age + Number + Start', data = kyphosis)
```

