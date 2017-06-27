--- 
 
# required metadata 
title: "Logistic Regression" 
description: "Use rx_logit to fit logistic regression models for small or large data." 
keywords: "M, I, S, S, I, N, G,  , K, E, Y, W, O, R, D, S" 
author: "Microsoft Corporation Microsoft Technical Support" 
manager: "" 
<<<<<<< HEAD
ms.date: "06/26/2017" 
=======
ms.date: "06/27/2017" 
>>>>>>> heidist-revoscalepy
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

<<<<<<< HEAD
# rx_logit
=======
## rx_logit


### Usage
>>>>>>> heidist-revoscalepy



```
revoscalepy.functions.RxLogit.rx_logit(formula: str, data: typing.Union[revoscalepy.datasource.RxDataSource.RxDataSource, pandas.core.frame.DataFrame], pweights: str = None, fweights: str = None, cube: bool = False, cube_predictions: bool = False, variable_selection: list = None, row_selection: str = None, transforms: dict = None, transform_objects: dict = None, transform_function: str = None, transform_variables: list = None, transform_packages: list = None, transform_environment=None, drop_first: bool = False, drop_main: bool = None, cov_coef: bool = False, cov_data: bool = False, initial_values: typing.Union[NoneType, list, int] = None, coef_label_style: str = None, blocks_per_read: int = None, max_iterations: int = None, coefficent_tolerance: float = 1e-06, gradient_tolerance: float = 1e-06, objective_function_tolerance: float = 1e-08, report_progress: int = None, verbose: int = 0, compute_context: revoscalepy.computecontext.RxComputeContext.RxComputeContext = None, **kwargs) -> revoscalepy.functions.RxLogit.rx_logit_results
```




<<<<<<< HEAD
## Description
=======
### Description
>>>>>>> heidist-revoscalepy

Use rx_logit to fit logistic regression models for small or large data.


<<<<<<< HEAD
## Parameters


### formula
=======
### Arguments


##### formula
>>>>>>> heidist-revoscalepy

formula as described in rxFormula. Dependent variable must
be binary. It can be a logical variable, a factor with only two categories,
or a numeric variable with values in the range (0,1). In the latter case
it will be converted to a logical.


<<<<<<< HEAD
### data
=======
##### data
>>>>>>> heidist-revoscalepy

either a data source object, a character string specifying a
‘.xdf’ file, or a data frame object.


<<<<<<< HEAD
### pweights
=======
##### pweights
>>>>>>> heidist-revoscalepy

character string specifying the variable to use as probability
weights for the observations.


<<<<<<< HEAD
### fweights
=======
##### fweights
>>>>>>> heidist-revoscalepy

character string specifying the variable to use as frequency
weights for the observations.


<<<<<<< HEAD
### cube
=======
##### cube
>>>>>>> heidist-revoscalepy

logical flag. If True and the first term of the predictor variables
is categorical (a factor or an interaction of factors), the regression is
performed by applying the Frisch-Waugh-Lovell Theorem, which uses a partitioned
inverse to save on computation time and memory. See Details section below.


<<<<<<< HEAD
### cube_predictions
=======
##### cube_predictions
>>>>>>> heidist-revoscalepy

logical flag. If True and cube is True the estimated
model is evaluated (predicted) for each cell in the cube, fixing the non-cube
variables in the model at their mean values, and these predictions are included
in the countDF component of the returned value. This may be time and memory
intensive for large models. See Details section below.


<<<<<<< HEAD
### variable_selection
=======
##### variable_selection
>>>>>>> heidist-revoscalepy

a list specifying various parameters that control
aspects of stepwise regression. If it is an empty list (default), no stepwise
model selection will be performed. If not, stepwise regression will be
performed and cube must be False. See rxStepControl for details.


<<<<<<< HEAD
### row_selection
=======
##### row_selection
>>>>>>> heidist-revoscalepy

None. Not currently supported, reserved for future use.


<<<<<<< HEAD
### transforms
=======
##### transforms
>>>>>>> heidist-revoscalepy

None. Not currently supported, reserved for future use.


<<<<<<< HEAD
### transform_objects
=======
##### transform_objects
>>>>>>> heidist-revoscalepy

None. Not currently supported, reserved for future use.


<<<<<<< HEAD
### transform_function
=======
##### transform_function
>>>>>>> heidist-revoscalepy

variable transformation function.


<<<<<<< HEAD
### transform_variables
=======
##### transform_variables
>>>>>>> heidist-revoscalepy

character vector of input data set variables needed
for the transformation function. See rxTransform for details.


<<<<<<< HEAD
### transform_packages
=======
##### transform_packages
>>>>>>> heidist-revoscalepy

None. Not currently supported, reserved for future use.


<<<<<<< HEAD
### transform_environment
=======
##### transform_environment
>>>>>>> heidist-revoscalepy

None. Not currently supported, reserved for future use.


<<<<<<< HEAD
### drop_first
=======
##### drop_first
>>>>>>> heidist-revoscalepy

logical flag. If False, the last level is dropped in all sets
of factor levels in a model. If that level has no observations (in any of the
sets), or if the model as formed is otherwise determined to be singular, then
an attempt is made to estimate the model by dropping the first level in all
sets of factor levels. If True, the starting position is to drop the first
level. Note that for cube regressions, the first set of factors is excluded
from these rules and the intercept is dropped.


<<<<<<< HEAD
### drop_main
=======
##### drop_main
>>>>>>> heidist-revoscalepy

logical value. If True, main-effect terms are dropped before
their interactions.


<<<<<<< HEAD
### cov_coef
=======
##### cov_coef
>>>>>>> heidist-revoscalepy

logical flag. If True and if cube is False, the variance-covariance
matrix of the regression coefficients is returned. Use the rxCovCoef function
to obtain these data.


<<<<<<< HEAD
### cov_data
=======
##### cov_data
>>>>>>> heidist-revoscalepy

logical flag. If True and if cube is False and if constant term
is included in the formula, then the variance-covariance matrix of the data
is returned. Use the rxCovData function to obtain these data.


<<<<<<< HEAD
### initial_values
=======
##### initial_values
>>>>>>> heidist-revoscalepy

Starting values for the Iteratively Reweighted Least
Squares algorithm used to estimate the model coefficients.


<<<<<<< HEAD
### coef_label_styl
=======
##### coef_label_styl
>>>>>>> heidist-revoscalepy

character string specifying the coefficient label style.
The default is “Revo”.


<<<<<<< HEAD
### blocks_per_read
=======
##### blocks_per_read
>>>>>>> heidist-revoscalepy

number of blocks to read for each chunk of data read
from the data source.


<<<<<<< HEAD
### max_iterations
=======
##### max_iterations
>>>>>>> heidist-revoscalepy

maximum number of iterations.


<<<<<<< HEAD
### coefficent_tolerance
=======
##### coefficent_tolerance
>>>>>>> heidist-revoscalepy

convergence tolerance for coefficients. If the
maximum absolute change in the coefficients (step), divided by the maximum
absolute coefficient value, is less than or equal to this tolerance at the
end of an iteration, the estimation is considered to have converged. To
disable this test, set this value to 0.


<<<<<<< HEAD
### gradient_tolerance
=======
##### gradient_tolerance
>>>>>>> heidist-revoscalepy

This argument is deprecated.


<<<<<<< HEAD
### objective_function_tolerance
=======
##### objective_function_tolerance
>>>>>>> heidist-revoscalepy

convergence tolerance for the objective function.


<<<<<<< HEAD
### report_progress
=======
##### report_progress
>>>>>>> heidist-revoscalepy

integer value with options:
0: no progress is reported.
1: the number of processed rows is printed and updated.
2: rows processed and timings are reported.
3: rows processed and all timings are reported.


<<<<<<< HEAD
### verbose
=======
##### verbose
>>>>>>> heidist-revoscalepy

integer value.


<<<<<<< HEAD
### compute_context
=======
##### compute_context
>>>>>>> heidist-revoscalepy

a RxComputeContext object for prediction.


<<<<<<< HEAD
### kwargs
=======
##### kwargs
>>>>>>> heidist-revoscalepy

additional parameters


<<<<<<< HEAD
## Returns
=======
### Returns
>>>>>>> heidist-revoscalepy

a rx_logit_results object of linear model.


<<<<<<< HEAD
## Author
=======
### Author
>>>>>>> heidist-revoscalepy

Microsoft Corporation [Microsoft Technical Support](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409.md)


<<<<<<< HEAD
## See also


## Example
=======
### See also


### Example
>>>>>>> heidist-revoscalepy



```
import os
from revoscalepy import rx_logit, RxOptions, RxXdfData
sample_data_path = RxOptions.get_option("sampleDataDir")
kyphosis = RxXdfData(os.path.join(sample_data_path, "kyphosis.xdf"))

logit = rx_logit('Kyphosis ~ Age + Number + Start', data = kyphosis)
```

