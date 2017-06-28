--- 
 
# required metadata 
title: "Covariance/Correlation Matrix" 
description: " Calculate the covariance, correlation, or sum of squares / cross-product matrix for a set of variables. " 
keywords: "RevoScaleR, rxCovCor, rxCov, rxCor, rxSSCP, print.rxCovCor, univar, multivariate" 
author: "heidisteen" 
manager: "jhubbard" 
ms.date: "04/18/2017" 
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
 
 
 
 
 
 
 
 #`rxCovCor`: Covariance/Correlation Matrix

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
Calculate the covariance, correlation, or sum of squares / cross-product matrix
for a set of variables.
 
 
 ##Usage

```   
  rxCovCor(formula, data, pweights = NULL, fweights = NULL, rowSelection = NULL,
           transforms = NULL, transformObjects = NULL,
           transformFunc = NULL, transformVars = NULL,
           transformPackages = NULL,transformEnvir = NULL, 
  		 keepAll = TRUE, varTol = 1e-12, type = "Cov",
           blocksPerRead = rxGetOption("blocksPerRead"),
           reportProgress = rxGetOption("reportProgress"), verbose = 0, 
           computeContext = rxGetOption("computeContext"), ...)
  
  rxCov(formula, data, pweights = NULL, fweights = NULL, rowSelection = NULL,
        transforms = NULL, transformObjects = NULL,
        transformFunc = NULL, transformVars = NULL,
        transformPackages = NULL, transformEnvir = NULL,
        keepAll = TRUE, varTol = 1e-12,
        blocksPerRead = rxGetOption("blocksPerRead"),
        reportProgress = rxGetOption("reportProgress"), verbose = 0,
        computeContext = rxGetOption("computeContext"), ...)
  
  rxCor(formula, data, pweights = NULL, fweights = NULL, rowSelection = NULL,
        transforms = NULL, transformObjects = NULL, 
        transformFunc = NULL, transformVars = NULL,
        transformPackages = NULL, transformEnvir = NULL, 
         keepAll = TRUE, varTol = 1e-12,
        blocksPerRead = rxGetOption("blocksPerRead"),
        reportProgress = rxGetOption("reportProgress"), verbose = 0,
        computeContext = rxGetOption("computeContext"), ...)
  
  rxSSCP(formula, data, pweights = NULL, fweights = NULL, rowSelection = NULL,
         transforms = NULL, transformObjects = NULL, 
         transformFunc = NULL, transformVars = NULL,
         transformPackages = NULL, transformEnvir = NULL, 
         keepAll = TRUE, varTol = 1e-12,
         blocksPerRead = rxGetOption("blocksPerRead"),
         reportProgress = rxGetOption("reportProgress"), verbose = 0, 
         computeContext = rxGetOption("computeContext"), ...)
  
 ## S3 method for class `rxCovCor':
print  (x, header = TRUE, ...)
 
```
 
 
 ##Arguments

   
    
 ### `formula`
 formula, as described in [rxFormula](rxformula.md), with all the terms on the right-hand side of the `~` separated by `+` operators. Each term may be a single variable, a transformed variable, or the interaction of (transformed) variables separated by the `:` operator. e.g. `~ x1 + log(x2) + x3 : x4` 
  
  
    
 ### `data`
 either a data source object, a character string specifying a .xdf file, or a data frame object. 
  
  
    
 ### `pweights`
 character string specifying the variable to use as probability weights for the observations. Only one of `pweights` and `fweights` may be specified at a time. 
  
  
    
 ### `fweights`
 character string specifying the variable to use as frequency weights for the observations. Only one of `pweights` and `fweights` may be specified at a time. 
  
  
    
 ### `rowSelection`
 name of a logical variable in the data set (in quotes) or a logical expression using variables in the data set to specify row selection.  For example, `rowSelection = "old"` will use only observations in which the value of the variable `old` is `TRUE`.  `rowSelection = (age > 20) & (age < 65) & (log(income) > 10)` will use only observations in which the value of the `age` variable is between 20 and 65 and the value of the `log` of the `income` variable is greater than 10.  The row selection is performed after processing any data transformations  (see the arguments `transforms` or `transformFunc`). As with all expressions, `rowSelection` can be defined outside of the function  call using the expression function. 
  
  
    
 ### `transforms`
 an expression of the form `list(name = expression, ...)` representing the first round of variable transformations. As with all expressions, `transforms` (or `rowSelection`) can be defined outside of the function call using the expression function. 
  
  
    
 ### `transformObjects`
 a named list containing objects that can be referenced by `transforms`, `transformsFunc`, and `rowSelection`. 
  
  
    
 ### `transformFunc`
 variable transformation function. See [rxTransform](rxtransform.md) for details. 
  
  
    
 ### `transformVars`
 character vector of input data set variables needed for the transformation function. See [rxTransform](rxtransform.md) for details. 
  
  
    
 ### `transformPackages`
 character vector defining additional R packages (outside of those specified in `rxGetOption("transformPackages")`) to be made available and  preloaded for use in variable transformation functions, e.g., those explicitly defined in **RevoScaleR** functions via their `transforms` and `transformFunc` arguments or those  defined implicitly via their `formula` or `rowSelection` arguments.  The `transformPackages` argument may also be `NULL`,  indicating that no packages outside `rxGetOption("transformPackages")` will be preloaded. 
  
  
    
 ### `transformEnvir`
 user-defined environment to serve as a parent to  all environments developed internally and used for variable data transformation. If `transformEnvir = NULL`, a new "hash" environment with parent `baseenv()` is used instead. 
  
  
    
 ### `keepAll`
 logical value. If `TRUE`, all of the columns are kept in the returned matrix. If `FALSE`, columns (and corresponding rows in the returned matrix) that are symbolic linear combinations of other columns, see alias, are dropped.   
  
  
    
 ### `varTol`
 numeric tolerance used to identify columns in the data matrix that have near zero variance. If the variance of a column is less than or equal to `varTol` and `keepAll=TRUE`, that column is dropped from the data matrix. 
  
  
    
 ### `type`
 character string specifying the type of matrix to return. The supported types are:   
*   `"SSCP"`: **S**ums of **S**quares / **C**ross **P**roducts matrix. 
*   `"Cov"`: covariance matrix. 
*   `"Cor"`: correlation matrix.  
 The `type` argument is case insensitive, e.g. `"SSCP"` and `"sscp"` are equivalent. 
  
  
    
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
 a valid [RxComputeContext](rxcomputecontext.md).  The `RxSpark`, `RxHadoopMR`, and `RxInTeradata` compute  contexts distribute the computation among the nodes specified by the  compute context; for other compute contexts, the  computation is distributed if possible on the local computer. 
  
  
    
 ### ` ...`
 additional arguments to be passed directly to the Revolution Compute Engine. 
  
  
    
 ### `x`
 an object of class "rxCovCor". 
  
  
    
 ### `header`
 logical value. If `TRUE`, header information is printed. 
  
 
 
 
 ##Details
 
The `rxCovCor`, and the appropriate convenience functions `rxCov`,
`rxCor` and `rxSSCP`, calculates either the covariance, Pearson's
correlation, or a sums of squares/cross-product matrix, which may or may not
use probability or frequency weights.

The sums of squares/cross-product matrix differs from the other two output types
in that an initial column of `1`s or square root of the weights, if specified,
is added to the data matrix prior to multiplication so the first row and first
column must be dropped from the output to obtain the cross-product of just the
specified data matrix.
 
 
  
 ##Value
 
For `rxCovCor`, an object of class "rxCovCor" with the following list
elements:


###`CovCor`
numeric matrix representing either the (weighted) covariance, correlation, or sum of squares/cross-product.



###`StdDevs`
For type = "Cor" and "Cov", numeric vector of (weighted)  standard deviations of the columns. For type = "SSCP", the standard deviations are not calculated and the return value is `numeric(0)`.



###`Means`
numeric vector containing the (weighted) column means.



###`valid.obs`
number of valid observations.



###`missing.obs`
number of missing observations.



###`SumOfWeights`
either the sum of the weights or `NA` if no weights are specified.



###`DroppedVars`
character vector containing the names of the data columns that were dropped during the calculations.



###`DroppedVarIndexes`
integer vector containing the indices of the data columns that were dropped during the calculations.



###`params`
parameters sent to Microsoft R Services Compute Engine.



###`call`
the matched call.



###`formula`
formula as described in [rxFormula](rxformula.md).


For `rxCov`, a covariance matrix.

For `rxCor`, a correlation matrix.

For `rxSSCP`, a sum of squares/cross-product matrix.
 
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
cov,
cor,
[rxCovData](rxcovregression.md),
[rxCorData](rxcovregression.md).
   
 
 ##Examples

 ```
   
  # Obtain all components from rxCovCor
  form <- ~ Sepal.Length + Sepal.Width + Petal.Length + Petal.Width
  allCov <- rxCovCor(form, data = iris, type = "Cov")
  allCov
  
  # Direct access to covariance or correlation matrix
  rxCov(form, data = iris, reportProgress = 0)
  cov(iris[,1:4])
  rxCor(form, data = iris, reportProgress = 0)
  cor(iris[,1:4])
  
  # Cross-product of data matrix (need to drop first row and column of output)
  rxSSCP(form, data = iris, reportProgress = 0)[-1, -1]
  crossprod(as.matrix(iris[,1:4]))
 
```
 
 
 
 
