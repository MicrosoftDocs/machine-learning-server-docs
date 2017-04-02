--- 
 
# required metadata 
title: "Linear Models" 
description: " Fit linear models on small or large data. " 
keywords: "RevoScaleR, rxLinMod, models, regression" 
author: "richcalaway" 
manager: "jhubbard" 
ms.date: "03/23/2017" 
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
 
 
 #`rxLinMod`: Linear Models

 Applies to version {PRODUCT_VERSION} of package RevoScaleR.
 
 ##Description
 
Fit linear models on small or large data.
 
 
 ##Usage

```   
  rxLinMod(formula, data, pweights = NULL, fweights = NULL, cube = FALSE,
           cubePredictions = FALSE, variableSelection = list(),
           rowSelection = NULL, transforms = NULL, transformObjects = NULL,
           transformFunc = NULL, transformVars = NULL, 
           transformPackages = NULL, transformEnvir = NULL,
           dropFirst = FALSE, dropMain = rxGetOption("dropMain"),
           covCoef = FALSE, covData = FALSE, 
           coefLabelStyle = rxGetOption("coefLabelStyle"),
           blocksPerRead = rxGetOption("blocksPerRead"),
           reportProgress = rxGetOption("reportProgress"), verbose = 0,
           computeContext = rxGetOption("computeContext"),  
           ...)
 
```
 
 ##Arguments

   
    
 ### `formula`
 formula as described in [rxFormula](rxFormula.md). 
  
  
    
 ### `data`
 either a data source object, a character string specifying a .xdf file, or a data frame object. 
  
  
    
 ### `pweights`
 character string specifying the variable to use as probability weights for the observations. 
  
  
    
 ### `fweights`
 character string specifying the variable to use as frequency weights for the observations. 
  
  
    
 ### `cube`
 logical flag. If `TRUE` and the first term of the predictor variables is categorical (a factor or an interaction of factors), the regression is performed by applying the Frisch-Waugh-Lovell Theorem, which uses a partitioned inverse to save on computation time and memory. See Details section below. 
  
  
    
 ### `cubePredictions`
 logical flag. If `TRUE` and `cube` is `TRUE` the predicted values are computed and included in the `countDF`component of the returned value. This may be memory intensive. See Details section below. 
  
  
    
 ### `variableSelection`
  a list specifying various parameters that control aspects of stepwise regression. If it is an empty list (default), no stepwise model selection will be performed. If not, stepwise regression will be performed and `cube` must be `FALSE`. See [rxStepControl](rxStepControl.md) for details. 
  
  
    
 ### `rowSelection`
 name of a logical variable in the data set (in quotes) or a logical expression using variables in the data set to specify row selection.  For example, `rowSelection = "old"` will use only observations in which the value of the variable `old` is `TRUE`.  `rowSelection = (age > 20) & (age < 65) & (log(income) > 10)` will use only observations in which the value of the `age` variable is between 20 and 65 and the value of the `log` of the `income` variable is greater than 10.  The row selection is performed after processing any data transformations  (see the arguments `transforms` or `transformFunc`). As with all expressions, `rowSelection` can be defined outside of the function  call using the expression function. 
  
  
    
 ### `transforms`
 an expression of the form `list(name = expression, ...)` representing the first round of variable transformations. As with all expressions, `transforms` (or `rowSelection`)  can be defined outside of the function call using the expression function. 
  
  
    
 ### `transformObjects`
 a named list containing objects that can be referenced by `transforms`, `transformsFunc`, and `rowSelection`. 
  
  
    
 ### `transformFunc`
 variable transformation function. The variables used in the  transformation function must be specified in `transformVars` if they are not variables used in the model. See [rxTransform](rxTransform.md) for details. 
  
  
    
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
  
  
    
 ### `coefLabelStyle`
 character string specifying the coefficient label style.  The default is "Revo". If "R", R-compatible labels are created. 
  
  
    
 ### `blocksPerRead`
 number of blocks to read for each chunk of data read from the data source. 
  
  
    
 ### `reportProgress`
 integer value with options:  
*   `0`: no progress is reported. 
*   `1`: the number of processed rows is printed and updated. 
*   `2`: rows processed and timings are reported. 
*   `3`: rows processed and all timings are reported. 
  
  
  
    
 ### `verbose`
 integer value. If `0`, no additional output is printed.  If `1`, additional summary information is printed. 
  
  
  
 ### `computeContext`
 a valid [RxComputeContext](RxComputeContext.md).  The `RxHpcServer`,  `RxHadoopMR`, and `RxInTeradata` compute  contexts distribute the computation among the nodes specified by the  compute context; for other compute contexts, the  computation is distributed if possible on the local computer. 
  
  
  
    
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
`countDF`. This may be memory intensive. If there are no conditional
independent variables (outside of the cube), the predicted values are
equivalent to the coefficients and will be included in `countDF` whenever
`cube` is `TRUE`.  Regardless of the setting for 
`cube`, the null model for the F-test of global significance is always
the intercept-only model.

The `dropFirst` and `dropMain` arguments are provided primarily as a 
convenience to users comparing `rxLinMod` results to those of `lm`.
While `rxLinMod` will sometimes drop main effects while retaining interactions
involving those terms, `lm` will not. Setting `dropMain=FALSE` will give
results more akin to those of `lm`. Similarly, `lm` defaults to using
treatment contrasts, which essentially drop the first level of each factor from 
the finished model. On the other hand, `rxLinMod` by default uses a set of
contrasts that drop the last level of each factor. Setting `dropFirst=TRUE`
will give results more akin to those of `lm`.
 
 
 
 
 
 
 
 
 
 
 
 
 
 ##Value
 
Let P be the number of regression coefficients returned for each dependent
variable, `Y(n)` for `n=1,...,N`, specified in
the regression model. Let **X** be the linear regression design matrix. The
`rxLinMod` function returns an object of class rxLinMod, which is a list
containing the following elements:


###`coefficients`
P x N numeric matrix containing the regression coefficients.


###`covCoef`
variance-covariance matrix for the regression coefficient estimates.


###`covData`
variance-covariance matrix for the explanatory variables in the regression model.


###`residual.squares`
the sum of the squares of the residuals.


###`condition.number`
numeric scalar representing the estimated reciprocal condition number of X'X (moment or crossprod) matrix.


###`rank`
integer scalar denoting the numeric rank of the fitted linear model.


###`aliased`
logical vector specifying whether columns were dropped or not due to collinearity.


###`coef.std.error`
P x N numeric matrix containing the standard errors of the regression coefficients.


###`coef.t.value`
P x N numeric matrix containing the t-statistics for the regression coefficients.


###`coef.p.value`
P x N numeric matrix containing the p-values for the t-stats (Pr(>|t|)) 


###`total.squares`
N element numeric vector whose nth element is defined by `Y'(n)Y(n)` for `n=1,...,N`.


###`y.var`
N element numeric vector whose nth element is defined by `(Y(n) - E{Y(n)})'(Y(n) - E{Y(n)}`, i.e., the mean deviation of each dependent variable.


###`sigma`
N element numeric vector of standard error of residuals.


###`residual.variance`
the variance of the residuals.
 

###`r.squared`
N element numeric vector containing r-squared, the fraction of variance explained by the model.


###`f.pvalue`
the p-value resulting from an F-test on the fitted model.


###`df`
degrees of freedom, a 3-element vector (p, n-p, p*), the last being the number of non-aliased coefficients.


###`y.names`
N element character vector containing the names of the dependent variables in the specified model.


###`partitions`
when cube is `TRUE`, partitioned results will also be returned.


###`fstatistics`
(for models including non-intercept terms) a list containing the named elements: **value:** an N element numeric vector of F-statistic values, **numdf:** corresponding numerator degrees of freedom and **dendf:** corresponding denominator degrees of freedom.


###`adj.r.squared`
R-squared statistic 'adjusted', penalizing for higher p.


###`params`
parameters sent to Microsoft R Services Compute Engine.


###`formula`
the model formula. For stepwise regression, this is the final model selected.


###`call`
the matched call.


###`countDF`
when `cube` is `TRUE`, a data frame containing counts information for each cube. If `cubePredictions` is also `TRUE`, predicted values for each group in the cube are included.


###`nValidObs`
number of valid observations.


###`nMissingObs`
number of missing observations.


###`deviance`
minus twice the maximized log-likelihood (up to a constant)


###`anova`
for stepwise regression,  a data frame corresponding to the steps taken in the search.


###`formulaBase`
for stepwise regression,  the base model from which the search is started.

 
 
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
 
lm,
[rxLogit](rxLogit.md),
[rxTransform](rxTransform.md).
   
 ##Examples

 ```
   
  # Compare rxLinMod and lm, which produce similar results when contr.SAS is used
  form <- Sepal.Length ~ Sepal.Width + Petal.Length + Petal.Width + Species
  irisLinMod <- rxLinMod(form, data = iris)
  irisLinMod
  summary(irisLinMod)
  irisLM <- lm(form, data = iris, contrasts = list(Species = contr.SAS))
  summary(irisLM)
  
  # Instead of using the equivalent of contr.SAS, estimate the parameters
  # for the categorical levels without contrasting against an intercept term.
  # The null model for the global F-test remains the intercept-only model.
  irisCubeLinMod <-
      rxLinMod(Sepal.Length ~ Species + Sepal.Width + Petal.Length + Petal.Width,
               data = iris, cube = TRUE)
  summary(irisCubeLinMod)
  
  # Use the Sample Census Data
  censusWorkers <- file.path(rxGetOption("sampleDataDir"), "CensusWorkers")
  censusLinMod <- rxLinMod(wkswork1 ~ age:sex, data = censusWorkers,
                           pweights = "perwt")
  censusLinMod
  censusSubsetLinMod <- rxLinMod(wkswork1 ~ age:sex, data = censusWorkers,
                                 pweights = "perwt", rowSelection = age > 39)
  censusSubsetLinMod
  
  # Use the Sample Airline Data and report progress during computations
  sampleDataDir <- rxGetOption("sampleDataDir")
  airlineDemoSmall <- file.path(sampleDataDir, "AirlineDemoSmall.xdf")
  airlineLinMod <- rxLinMod(ArrDelay ~ CRSDepTime, data = airlineDemoSmall,
                            reportProgress = 1)
  airlineLinMod <- rxLinMod(ArrDelay ~ CRSDepTime, data = airlineDemoSmall,
                            reportProgress = 2)
  airlineLinMod <- rxLinMod(ArrDelay ~ CRSDepTime, data = airlineDemoSmall,
                            reportProgress = 2, blocksPerRead = 3)
  summary(airlineLinMod)
  
  # Create a local data.frame and define a transformation  
  # function to be applied to the data prior to processing.
  myDF <- data.frame(sex = c("Male", "Male", "Female", "Male"),
                     age = c(20, 20, 12, 15), score = 1.1:4.1, sport=c(1:3,2))
  
  # define variable transformation list. Wrap with expression()
  # so that it is not evaluated upon assignment.
  transforms <- expression(list(
      revage = rev(age),
      division = factor(c("A","B","B","A")),
      sport = factor(sport, labels=c("tennis", "golf", "football"))))
      
  # Both user-defined transform and arithmetic expressions in formula.
  rxLinMod(score ~ sin(revage) + sex, data = myDF, transforms = transforms)
  
  # User-defined transform only.
  rxLinMod(revage ~ division + sport, data = myDF, transforms = transforms)
  
  # Arithmetic formula expression only.
  rxLinMod(log(score) ~ sin(age) + sex, data = myDF)
  
  # No variable transformations.
  rxLinMod(score ~ age + sex, data = myDF)
  
  # use multiple dependent variables in model formula
  # print and summarize results for comparison
  sampleDataDir <- rxGetOption("sampleDataDir")
  airlineDemoSmall <- file.path(sampleDataDir, "AirlineDemoSmall")
  airlineLinMod1 <- rxLinMod(ArrDelay ~ DayOfWeek, data = airlineDemoSmall)
  airlineLinMod2 <- rxLinMod(cbind(ArrDelay, CRSDepTime) ~ DayOfWeek,
                             data = airlineDemoSmall)
  airlineLinMod3 <-
      rxLinMod(cbind(pow(ArrDelay, 2), ArrDelay, CRSDepTime) ~ DayOfWeek,
               data = airlineDemoSmall)
  airlineLinMod4 <- rxLinMod(pow(ArrDelay, 2) ~ DayOfWeek,
                             data = airlineDemoSmall)
  airlineLinMod1
  airlineLinMod2
  airlineLinMod3
  airlineLinMod4
  summary(airlineLinMod2)
 
```
 
 
 
 
