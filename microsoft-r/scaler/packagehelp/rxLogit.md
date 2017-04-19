--- 
 
# required metadata 
title: "Logistic Regression" 
description: " Use `rxLogit` to fit logistic regression models for small or large data. " 
keywords: "RevoScaleR, rxLogit, models, regression" 
author: "heidisteen" 
manager: "jhubbard" 
ms.date: "04/17/2017" 
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
 
 
 #`rxLogit`: Logistic Regression

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
Use `rxLogit` to fit logistic regression models for small or large data.
 
 
 ##Usage

```   
  rxLogit(formula, data, pweights = NULL, fweights = NULL, cube = FALSE,
          cubePredictions = FALSE, variableSelection = list(), rowSelection = NULL, 
          transforms = NULL, transformObjects = NULL,
          transformFunc = NULL, transformVars = NULL, 
          transformPackages = NULL, transformEnvir = NULL,   
          dropFirst = FALSE, dropMain = rxGetOption("dropMain"),
          covCoef = FALSE, covData = FALSE, 
          initialValues = NULL, 
          coefLabelStyle = rxGetOption("coefLabelStyle"),
          blocksPerRead = rxGetOption("blocksPerRead"), 
          maxIterations = 25, coeffTolerance = 1e-06, 
          gradientTolerance = 1e-06, objectiveFunctionTolerance = 1e-08,
          reportProgress = rxGetOption("reportProgress"), verbose = 0,
          computeContext = rxGetOption("computeContext"),
          ...)
 
```
 
 ##Arguments

   
    
 ### `formula`
 formula as described in [rxFormula](rxFormula.md). Dependent variable  must be binary. It can be a logical variable, a factor with only two categories, or a numeric variable with values in   the range (0,1). In the latter case it will be converted to a logical. 
  
  
    
 ### `data`
 either a data source object, a character string specifying a .xdf file, or a data frame object. 
  
  
    
 ### `pweights`
 character string specifying the variable to use as probability weights for the observations. 
  
  
    
 ### `fweights`
 character string specifying the variable to use as frequency weights for the observations. 
  
  
    
 ### `cube`
 logical flag. If `TRUE` and the first term of the predictor variables is categorical (a factor or an interaction of factors), the regression is performed by applying the Frisch-Waugh-Lovell Theorem, which uses a partitioned inverse to save on computation time and memory. See Details section below. 
  
  
    
 ### `cubePredictions`
 logical flag. If `TRUE` and `cube` is `TRUE` the estimated model is evaluated (predicted) for each cell in the cube, fixing the non-cube variables in the model at their mean values, and these predictions  are included in the `countDF` component of the returned value. This may be time and  memory intensive for large models. See Details section below. 
  
  
    
 ### `variableSelection`
  a list specifying various parameters that control aspects of stepwise regression. If it is an empty list (default), no stepwise model selection will be performed. If not, stepwise regression will be performed and `cube` must be `FALSE`. See [rxStepControl](rxStepControl.md) for details. 
  
  
    
 ### `rowSelection`
 name of a logical variable in the data set (in quotes) or a logical expression using variables in the data set to specify row selection.  For example, `rowSelection = "old"` will use only observations in which the value of the variable `old` is `TRUE`.  `rowSelection = (age > 20) & (age < 65) & (log(income) > 10)` will use only observations in which the value of the `age` variable is between 20 and 65 and the value of the `log` of the `income` variable is greater than 10.  The row selection is performed after processing any data transformations  (see the arguments `transforms` or `transformFunc`). As with all expressions, `rowSelection` can be defined outside of the function  call using the expression function. 
  
  
    
 ### `transforms`
 an expression of the form `list(name = expression, ...)` representing the first round of variable transformations. As with all expressions, `transforms` (or `rowSelection`)  can be defined outside of the function call using the expression function. 
  
  
    
 ### `transformObjects`
 a named list containing objects that can be referenced by `transforms`, `transformsFunc`, and `rowSelection`. 
  
  
    
 ### `transformFunc`
 variable transformation function. See [rxTransform](rxTransform.md) for details. 
  
  
    
 ### `transformVars`
 character vector of input data set variables needed for the transformation function. See [rxTransform](rxTransform.md) for details. 
  
  
    
 ### `transformPackages`
 character vector defining additional R packages (outside of those specified in `rxGetOption("transformPackages")`) to be made available and  preloaded for use in variable transformation functions, e.g., those explicitly defined in **RevoScaleR** functions via their `transforms` and `transformFunc` arguments or those  defined implicitly via their `formula` or `rowSelection` arguments.  The `transformPackages` argument may also be `NULL`,  indicating that no packages outside `rxGetOption("transformPackages")` will be preloaded. 
  
  
    
 ### `transformEnvir`
 user-defined environment to serve as a parent to  all environments developed internally and used for variable data transformation. If `transformEnvir = NULL`, a new "hash" environment with parent `baseenv()` is used instead. 
  
  
    
 ### `dropFirst`
 logical flag. If `FALSE`, the last level is dropped in  all sets of factor levels in a model. If that level has no observations (in any of  the sets), or if the model as formed is otherwise determined to be singular, then  an attempt is made to estimate the model by dropping the first level in all sets  of factor levels. If `TRUE`, the starting position is to drop the first level.  Note that for cube regressions, the first set of factors is excluded from these rules  and the intercept is dropped.  
  
  
    
 ### `dropMain`
 logical value. If `TRUE`, main-effect terms are dropped before their interactions. 
  
  
    
 ### `covCoef`
 logical flag. If `TRUE` and if `cube` is `FALSE`,  the variance-covariance matrix of the regression coefficients is returned.  Use the [rxCovCoef](rxCovRegression.md) function to obtain these data. 
  
  
    
 ### `covData`
 logical flag. If `TRUE` and if `cube` is `FALSE` and if  constant term is included in the formula, then the variance-covariance matrix of the data is returned. Use the [rxCovData](rxCovRegression.md) function to obtain these data. 
  
  
    
 ### `initialValues`
 Starting values for the Iteratively Reweighted Least Squares algorithm  used to estimate the model coefficients. Supported values for this argument come in four forms:  
* `NA` - Initial values of the parameters are computed by a weighted least squares step in which,  if there is no user-specified weight, the expected value (mu) of the dependent variable is set to 0.75 if  the binary dependent variable is 1, and to 0.25 if it is 0, and to a correspondingly weighted value otherwise.  This is equivalent to the default option for the `glm` function's logistic regression initial values,  and can sometimes lead to convergence when the default `NULL` option does not, though it may result in  more iterations in other cases. If convergence fails with the default `NULL` option, estimation is  restarted with `NA`, and vice versa.   
* `NULL` - (Default) The initial values will be estimated based on a linear regression. This can speed up  convergence significantly in many cases. If the model fails to converge using these values, estimation is automatically re-started using the `NA` option for the initial values.  
* `Scalar` - A numeric scalar that will be replicated once for each of the coefficients in the model to  form the initial values. Zeros are often good starting values.   
* `Vector` - A numeric vector of length `K`, where `K` is the number of model coefficients, including the intercept if any and including any that might be dropped from sets of dummies. This argument is  most useful when a set of estimated coefficients is available from a similar set of data.  Note that `K` can be found by running the model on a small number of observations and obtaining  the number of coefficients from the output, say `z`, via `length(z$coefficients)`.    
 
  
  
    
 ### `coefLabelStyle`
 character string specifying the coefficient label style.  The default is "Revo". If "R", R-compatible labels are created. 
  
  
    
 ### `blocksPerRead`
 number of blocks to read for each chunk of data read from the data source. 
  
  
    
 ### `maxIterations`
 maximum number of iterations. 
  
  
    
 ### `coeffTolerance`
 convergence tolerance for coefficients. If the maximum absolute  change in the coefficients (step), divided by the maximum absolute coefficient value, is less than or equal to this tolerance at the end of an iteration, the estimation is considered to have converged. To disable this test, set this value to 0. 
  
  
    
 ### `gradientTolerance`
 This argument is deprecated. Use `objectiveFunctionTolerance` and  `coeffTolerance` to control convergence. 
  
  
    
 ### `objectiveFunctionTolerance`
 convergence tolerance for the objective function. If the absolute  relative change in the deviance (-2.0 times log likelihood) is less than or equal to this tolerance at the end of an iteration, the estimation is considered to have converged. To disable this test, set this value to 0.  
  
  
    
 ### `reportProgress`
 integer value with options:  
   *   `0`: no progress is reported. 
   *   `1`: the number of processed rows is printed and updated. 
   *   `2`: rows processed and timings are reported. 
   *   `3`: rows processed and all timings are reported. 
  
  
  
    
 ### `verbose`
 integer value. If `0`, no verbose output is printed during calculations. Integer values from `1` to `4` provide increasing amounts of information are provided. 
  
  
  
 ### `computeContext`
 a valid [RxComputeContext](RxComputeContext.md).  The `RxSpark`, `RxHadoopMR`, and `RxInTeradata` compute  contexts distribute the computation among the nodes specified by the  compute context; for other compute contexts, the  computation is distributed if possible on the local computer. 
  
  
  
    
 ### ` ...`
 additional arguments to be passed directly to the Revolution Compute Engine. 
  
 
 
 ##Details
 
The special function `F()` can be used in `formula` to force a
variable to be interpreted as a factor.

When `cube` is `TRUE`, the Frisch-Waugh-Lovell (FWL) Theorem is
applied to the model. The FWL approach parameterizes the model to include
one coefficient for each category (a single factor level or combination of
factor levels) instead of using an intercept in the model with contrasts for
each of the factor combinations. Additionally when `cube` is `TRUE`,
the output contains a `countDF` element representing the counts for each
category. If `cubePredictions` is also `TRUE`, predicted values using
means of conditional independent continuous variables and weighted coefficients
of conditional independent categorical variables are also computed and included in 
`countDF`. This may be time and memory intensive for large models. 
If there are no conditional independent variables (outside of the cube), the predicted values are
equivalent to the coefficients and will be included in `countDF` whenever
`cube` is `TRUE`. 
 
 
 
 ##Value
 
an rxLogit object containing the following elements:

###`coefficients`
named vector of coefficients.


###`covCoef`
variance-covariance matrix for the regression coefficient estimates.


###`covData`
variance-covariance matrix for the explanatory variables in the regression model.


###`residual.squares`
sum of squares of residuals.


###`condition.number`
estimated reciprocal condition number of final weighted cross-product (X'WX) matrix.


###`rank`
numeric rank of the fitted linear model.


###`aliased`
logical vector specifying whether columns were dropped or not due to collinearity.


###`coef.std.error`
standard errors of the coefficients.


###`coef.t.value`
coefficients divided by their standard errors. 


###`coef.p.value`
p-values for coef.t.values, using the normal distribution (Pr(>|z|)) 


###`total.squares`
Y'Y of raw Y's


###`y.var`
(Y-Ybar)'(Y-ybar) (mean deviations)


###`sigma`
standard error of residuals.


###`residual.variance`
variance of residuals.


###`f.pvalue`
the p-value resulting from an F-test on the fitted model.


###`df`
degrees of freedom, a 3-vector (p, n-p, p*), the last being the number of non-aliased coefficients.


###`y.names`
names for dependent variables.


###`partitions`
when `cube` is `TRUE`, partitioned results will also be returned.


###`fstatistics`
(for models including non-intercept terms) a 3-vector with the value of the F-statistic with its numerator and denominator degrees of freedom.


###`params`
parameters sent to Microsoft R Services Compute Engine.


###`formula`
the model formula


###`call`
the matched call.


###`countDF`
when cube is `TRUE`, a data frame containing category counts for the cube portion will also be returned. If `cubePredictions` is also `TRUE`, predicted values for each group in the cube are included.


###`nValidObs`
number of valid observations.


###`nMissingObs`
number of missing observations.


###`deviance`
minus twice the maximized log-likelihood (up to a constant)


###`dispersion`
for `rxLogit`, which estimates a generalized linear model with family `binomial`, the dispersion is always 1.


 
 ##Note
 
Logistic regression is computed using the Iteratively Reweighted Least Squares
(IRLS) algorithm, which is equivalent to full maximum likelihood.
 
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##References
 
Frisch, Ragnar; Waugh, Frederick V., Partial Time Regressions as Compared with
Individual Trends, *Econometrica*, **1** (4) (Oct., 1933), pp. 387-401.

Lovell, M., 1963, Seasonal adjustment of economic time series,
*Journal of the American Statistical Association*, **58**, pp. 993-1010.

Lovell, M., 2008, A Simple Proof of the FWL (Frisch,Waugh,Lovell) Theorem,
*Journal of Economic Education*.
 
 
 ##See Also
 
[rxLinMod](rxLinMod.md),
[rxTransform](rxTransform.md).
   
 ##Examples

 ```
   
  # Compare rxLogit and glm, which produce similar results when contr.SAS is used
  infertLogit <- rxLogit(case ~ age + parity + education + spontaneous + induced,
                         data = infert)
  infertLogit
  summary(infertLogit)
  
  infertGLM <- glm(case ~ age + parity + education + spontaneous + induced,
                   data = infert, family = binomial(), contrasts = list(education = contr.SAS)) 
  summary(infertGLM)
  
  # Instead of using the equivalent of contr.SAS, estimate the parameters
  # for the categorical levels without contrasting against an intercept term.
  infertCubeLogit <-
      rxLogit(case ~ education + age + parity + spontaneous + induced,
              data = infert, cube = TRUE)
          
  # Define an ageGroup variable through a variable transformation list. 
  # Partition the age groups between 1 and 100 by 20 years. 
  rxLogit(case ~ education + ageGroup + parity + spontaneous + induced,
          transforms=list(ageGroup = cut(age, seq(1, 100, 20))),
          data = infert, cube = TRUE)
  
  summary(infertCubeLogit)
 
```
 
 
 
 
