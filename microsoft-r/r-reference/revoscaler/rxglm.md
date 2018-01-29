--- 
 
# required metadata 
title: "rxGlm function (revoAnalytics) | Microsoft Docs" 
description: " Use rxGlm to fit generalized linear regression models for small or large data. Any valid glm family object can be used. " 
keywords: "(revoAnalytics), rxGlm, models, regression" 
author: "heidisteen" 
manager: "cgronlun" 
ms.date: "01/24/2018" 
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
 
 
 #rxGlm: Generalized Linear Models 
 ##Description
 
Use `rxGlm` to fit generalized linear regression models for small or large data. Any valid glm family object can be used.
 
 
 ##Usage

```   
  rxGlm(formula, data, family = gaussian(),
        pweights = NULL, fweights = NULL, offset = NULL,
        cube = FALSE, variableSelection = list(), rowSelection = NULL, 
        transforms = NULL, transformObjects = NULL,
        transformFunc = NULL, transformVars = NULL, 
        transformPackages = NULL, transformEnvir = NULL,   
        dropFirst = FALSE, dropMain = rxGetOption("dropMain"),
        covCoef = FALSE, computeAIC = FALSE, initialValues = NA, 
        coefLabelStyle = rxGetOption("coefLabelStyle"), 
        blocksPerRead = rxGetOption("blocksPerRead"), 
        maxIterations = 25, coeffTolerance = 1e-06, 
        objectiveFunctionTolerance = 1e-08,
        reportProgress = rxGetOption("reportProgress"), verbose = 0,
        computeContext = rxGetOption("computeContext"),
        ...)
 
```
 
 ##Arguments

   
    
 ### `formula`
 formula as described in [rxFormula](rxFormula.md). 
  
  
    
 ### `data`
 either a data source object, a character string specifying a .xdf file, or a data frame object. 
  
  
    
 ### `family`
 either an object of class family or a character string specifying the family. A family is a description of the error distribution and is associated with a link function to be used in the model.  Any valid family object may be used. The following family/link combinations are implemented in C++: binomial/logit, gamma/log, poisson/log,  and Tweedie. Other family/link combinations use a combination of C++ and R code. For the Tweedie distribution, use  `family=rxTweedie(var.power, link.power)`. It is also possible to use `family=tweedie(var.power, link.power)` from the  R package **tweedie**.  
  
  
    
 ### `pweights`
 character string specifying the variable to use as probability weights for the observations. 
  
  
    
 ### `fweights`
 character string specifying the variable to use as frequency weights for the observations. 
  
  
    
 ### `offset`
 character string specifying a variable to use as an offset for the model (`offset="x"`). An offset may also be specified as a term in the formula (`offset(x)`). An offset is a variable to be included as part of the linear predictor that requires no coefficient. 
  
  
    
 ### `cube`
 logical flag. If `TRUE` and the first term of the predictor variables is categorical (a factor or an interaction of factors), the regression is performed by applying the Frisch-Waugh-Lovell Theorem, which uses a partitioned inverse to save on computation time and memory. See Details section below. 
  
  
    
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
  
  
    
 ### `computeAIC`
 logical flag. If `TRUE`, the AIC of the fitted model is returned.  The default is `FALSE`. 
  
  
    
 ### `initialValues`
 Starting values for the Iteratively Reweighted Least Squares algorithm  used to estimate the model coefficients. Supported values for this argument come in three forms:  
* `NA` - The initial values are based upon the 'mustart' values generated by the family$initialize expression if an R family object is used, or the equivalent for a family coded in C++.  
* `Scalar` - A numeric scalar that will be replicated once for each of the coefficients in the model to  form the initial values. Zeros may be good starting values, depending upon the model.   
* `Vector` - A numeric vector of length `K`, where `K` is the number of model coefficients, including the intercept if any and including any that might be dropped from sets of dummies. This argument is  most useful when a set of estimated coefficients is available from a similar set of data.  Note that `K` can be found by running the model on a small number of observations and obtaining  the number of coefficients from the output, say `z`, via `length(z$coefficients)`.    
 
  
  
    
 ### `coefLabelStyle`
 character string specifying the coefficient label style.  The default is "Revo". If "R", R-compatible labels are created. 
  
  
    
 ### `blocksPerRead`
 number of blocks to read for each chunk of data read from the data source. 
  
  
    
 ### `maxIterations`
 maximum number of iterations. 
  
  
    
 ### `coeffTolerance`
 convergence tolerance for coefficients. If the maximum absolute  change in the coefficients (step), divided by the maximum absolute coefficient value, is less than or equal to this tolerance at the end of an iteration, the estimation is considered to have converged. To disable this test, set this value to 0. 
  
  
    
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
 a valid [RxComputeContext](RxComputeContext.md).  The `RxSpark`  and `RxHadoopMR` compute  contexts distribute the computation among the nodes specified by the  compute context; for other compute contexts, the  computation is distributed if possible on the local computer. 
  
  
  
    
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
category. 
 
 
 
 ##Value
 
an rxGlm object containing the following elements:

###`coefficients`
named vector of coefficients.


###`covCoef`
variance-covariance matrix for the regression coefficient estimates.


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
p-values for coef.t.value; for the poisson and binomial families, the dispersion is taken to be 1, and the normal distribution is used (Pr(>|z|)); for other families the dispersion is estimated,  and the t distribution is used (Pr(>|t|)).


###`f.pvalue`
the p-value resulting from an F-test on the fitted model.


###`df`
degrees of freedom, a 3-vector (p, n-p, p*), the last being the number of non-aliased coefficients.


###`fstatistics`
(for models including non-intercept terms) a 3-vector with the value of the F-statistic with its numerator and denominator degrees of freedom.


###`params`
parameters sent to Microsoft R Services Compute Engine.


###`formula`
the model formula


###`call`
the matched call.


###`nValidObs`
number of valid observations.


###`nMissingObs`
number of missing observations.


###`deviance`
minus twice the maximized log-likelihood (up to a constant)


###`dispersion`
for the poisson and binomial families, the dispersion is 1;  for other families the dispersion is estimated, and is the Pearson chi-squared statistic divided by the residual degrees of freedom.

 
 ##Note
 
The generalized linear models are computed using the Iteratively Reweighted Least Squares
(IRLS) algorithm.
 
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 
 ##See Also
 
glm,
family,
[rxLinMod](rxLinMod.md),
[rxLogit](rxLogit.md),
[rxTweedie](rxTweedie.md),
[rxTransform](rxTransform.md).
   
 ##Examples

 ```
   
  # Compare rxGlm and glm, which both can be used on small data sets
  infertRxGlm <- rxGlm(case ~ age + parity + education + spontaneous + induced,
                         family = binomial(), dropFirst = TRUE, data = infert)
  summary(infertRxGlm)
  
  infertGlm <- glm(case ~ age + parity + education + spontaneous + induced,
                   data = infert, family = binomial()) 
  summary(infertGlm)
  
  # For the case of the binomial family with the 'logit' link family,
  # the optimized rxLogit function can be used
  infertRxLogit <- rxLogit(case ~ age + parity + education + spontaneous + induced,
                   data = infert, dropFirst = TRUE) 
  summary(infertRxLogit)
          
  # Estimate a Gamma family model using sample data
  claimsXdf <- file.path(rxGetOption("sampleDataDir"),"claims.xdf")
  claimsGlm <- rxGlm(cost ~ age + car.age + type, family = Gamma,
  dropFirst = TRUE, data = claimsXdf)
  summary(claimsGlm)
  
  # In the claims data, the cost is set to NA if no claim was made
  # Convert NA to 0 for the cost, to prepare data for using
  # the Tweedie family - which is appropriate for positive data 
  # that also contains exact zeros
  # Read the transformed data into a data frame
  claims <- rxDataStep(inData = claimsXdf, 
             transforms = list(cost = ifelse(is.na(cost), 0, cost)))
    
  # Estimate using a Tweedie family                           
  claimsTweedie <- rxGlm(cost ~ age + car.age + type , 
      data=claims, family = rxTweedie(var.power =1.15)) 
  summary(claimsTweedie)
  
  # Illustrate the use of a Tweedie family with offset
  TestData <- data.frame(
      Factor1 = as.factor(c(1,1,1,1,2,2,2,2)), 
      Factor2 = as.factor(c(1,1,2,2,1,1,2,2)), 
      Discount = c(1,2,1,2,1,2,1,2), 
      Exposure = c(24000,40000,7000,14000,7500,15000,2000,5600), 
      PurePrem = c(46,32,73,58,48,25,220,30)
      )
      
  rxGlmTweedieOffset <- rxGlm(PurePrem ~ Factor1 * Factor2 - 1 + offset(log(Discount)), 
      family = rxTweedie(var.power = 1.5, link.power = 0), 
      data = TestData, fweights = "Exposure", dropFirst = TRUE, dropMain = FALSE
      )
      
  ## Not run:
 
require(statmod)
glmTweedieOffset <- glm(PurePrem ~ Factor1 * Factor2 - 1, 
    family = tweedie(var.power = 1.5, link.power = 0), 
    data = TestData, weights = Exposure, offset = log(Discount)
    )
 ## End(Not run) 
  
 
```
 
 
 
 
