--- 
 
# required metadata 
title: "Parallel External Memory Algorithm for Classification and Regression Trees" 
description: "     Fit classification and regression trees on an .xdf file or data frame     for small or large data using parallel external memory algorithm. " 
keywords: "RevoScaleR, rxDTree, print.rxDTree, models, tree, classif, regression, classification" 
author: "HeidiSteen"
ms.author: "heidist" 
manager: "jhubbard" 
ms.date: "04/18/2017" 
ms.topic: "reference" 
ms.prod: "microsoft-r" 
ms.service: "" 
ms.assetid: "" 
 
# optional metadata 
#ROBOTS: "" 
#audience: "" 
#ms.devlang: "" 
#ms.reviewer: "" 
#ms.suite: "" 
#ms.tgt_pltfrm: "" 
ms.technology: "r-server" 
#ms.custom: "" 
 
--- 
 
 
 
 #`rxDTree`: Parallel External Memory Algorithm for Classification and Regression Trees

 Applies to version 9.1.0 of package RevoScaleR.
 
 
 ##Description
 
Fit classification and regression trees on an .xdf file or data frame
for small or large data using parallel external memory algorithm.
 
 
 ##Usage

```   
  rxDTree(formula, data,
      outFile = NULL, outColName = ".rxNode", writeModelVars = FALSE, extraVarsToWrite = NULL, overwrite = FALSE,
      pweights = NULL, fweights = NULL, method = NULL, parms = NULL, cost = NULL,    
      minSplit = max(20, sqrt(numObs)), minBucket = round(minSplit/3), maxDepth = 10, cp = 0,
      maxCompete = 0, maxSurrogate = 0, useSurrogate = 2, surrogateStyle = 0, xVal = 2,
      maxNumBins = NULL, maxUnorderedLevels = 32, removeMissings = FALSE, 
      computeObsNodeId = NULL, useSparseCube = rxGetOption("useSparseCube"), findSplitsInParallel = TRUE, pruneCp = 0,
      rowSelection = NULL, transforms = NULL, transformObjects = NULL, transformFunc = NULL,
      transformVars = NULL, transformPackages = NULL, transformEnvir = NULL,
      blocksPerRead = rxGetOption("blocksPerRead"), reportProgress = rxGetOption("reportProgress"),
      verbose = 0, computeContext = rxGetOption("computeContext"), 
      xdfCompressionLevel = rxGetOption("xdfCompressionLevel"),
        ...  )
      
  
 
```
 
 
 
 ##Arguments

   
    
 ### `formula`
  formula as described in [rxFormula](rxformula.md).   Currently, formula functions are not supported. 
  
    
 ### `data`
  either a data source object, a character string  specifying a .xdf file, or a data frame object. 
  
    
 ### `outFile`
  either an RxXdfData data source object or a character string  specifying the .xdf file for storing the resulting node indices.  If `NULL`, then no node indices are stored to disk.  If the input data is a data frame,  the node indices are returned automatically.  If `rowSelection` is specified and not `NULL`,  then `outFile` cannot be the same as the `data`since the resulting set of node indices will generally not  have the same number of rows as the original data source. 
  
    
 ### `outColName`
  character string to be used as a column name  for the resulting node indices if `outFile` is not `NULL`.  Note that make.names is used on `outColName`to ensure that the column name is valid. If the `outFile` is an `RxOdbcData` source, dots are first converted to underscores. Thus, the default `outColName` becomes `"X_rxNode"`. 
  
    
 ### `writeModelVars`
  logical value. If `TRUE`, and the output file is different from the input file,  variables in the model will be written to the output file in addition to the node numbers.  If variables from the input data set are transformed in the model,  the transformed variables will also be written out. 
  
    
 ### `extraVarsToWrite`
  `NULL` or character vector of additional variables names from the input data to include in the `outFile`.  If `writeModelVars` is `TRUE`, model variables will be included as well. 
  
    
 ### `overwrite`
  logical value. If `TRUE`, an existing `outFile`with an existing column named `outColName` will be overwritten. 
  
    
 ### `pweights`
  character string specifying the variable of numeric values to use as probability weights for the observations. 
  
    
 ### `fweights`
  character string specifying the variable of integer values to use as frequency weights for the observations. 
  
    
 ### `method`
  character string specifying the splitting method. Currently, only `"class"` or `"anova"` are supported. The default is `"class"` if the response is a factor, otherwise `"anova"`. 
  
    
 ### `parms`
  optional list with components specifying additional parameters for the `"class"` splitting method, as follows:  
* `prior` - a vector of prior probabilities. The priors must be positive and sum to 1. The default  priors are proportional to the data counts.  
* `loss` - a loss matrix, which must have zeros on the diagonal and positive off-diagonal elements. By default, the off-diagonal elements are set to 1.  
* `split` - the splitting index, either `gini` (the default) or `information`.  
 If `parms` is specified, any of the components can be specified or omitted. The defaults will be used for missing components. 
  
    
 ### `cost`
  a vector of non-negative costs, containing one element for each variable in the model. Defaults to one for all variables. When deciding which split	to choose, the improvement on splitting on a variable is divided by its cost. 
  
    
 ### `minSplit`
  the minimum number of observations that must exist in a node before a split is attempted. By default, this is `sqrt(num of obs)`. For non-XDF data sources, as `(num of obs)` is unknown in advance, it is wisest to specify this argument directly. 
  
    
 ### `minBucket`
  the minimum number of observations in a terminal node (or leaf). By default, this is `minSplit`/3. 
  
    
 ### `cp`
  numeric scalar specifying the complexity parameter. Any split that does not decrease overall lack-of-fit by at  least `cp` is not attempted. 
  
    
 ### `maxCompete`
  the maximum number of competitor splits retained in the output. These are useful model diagnostics, as they allow you to compare splits in the output with the alternatives. 
      
    
 ### `maxSurrogate`
  the maximum number of surrogate splits retained in the output. See the Details for a description of how surrogate splits are used in the model fitting. Setting this to 0 can greatly improve the performance of the algorithm; in some cases almost half the computation time is spent in computing surrogate splits. 
  
    
 ### `useSurrogate`
  an integer specifying how surrogates are to be used in the splitting process:  
* `0` - display-only; observations with a missing value for the primary split variable are not sent further down the tree.  
* `1` - use surrogates,in order, to split observations missing the primary split variable. If all surrogates are missing, the  observation is not split.  
* `2` - use surrogates, in order, to split observations missing the primary split variable. If all surrogates are missing or `maxSurrogate=0`, send the observation in the majority direction.   
 The `0` value corresponds to the behavior of the `tree` function, and `2` (the default) corresponds to the recommendations of Breiman et al. 
  
    
 ### `xVal`
  the number of cross-validations to be performed along with the model building.  Currently, `1:xVal` is repeated and used to identify the folds. If not zero, the `cptable` component of the resulting model will contain both the mean (xerror) and standard deviation (xstd) of the cross-validation errors,  which can be used to select the optimal cost-complexity pruning of the fitted tree. Set it to zero if external cross-validation will be used to evaluate the fitted model because a value of k increases the compute time to (k+1)-fold over a value of zero. 
  
    
 ### `surrogateStyle`
  an integer controlling selection of a best surrogate. The default, `0`, instructs the program to use the total number of correct classifications for a potential surrogate, while `1` instructs the program to use the percentage of correct classification over  the non-missing values of the surrogate. Thus, `0` penalizes potential surrogates with a large number of missing values. 
  
    
 ### `maxDepth`
  the maximum depth of any tree node. The computations take much longer at greater depth, so lowering `maxDepth` can greatly speed up computation time. 
  
    
 ### `maxNumBins`
  the maximum number of bins to use to cut numeric data.  The default is `min(1001, max(101, sqrt(num of obs)))`. For non-XDF data sources, as `(num of obs)` is unknown in advance, it is wisest to specify this argument directly. If set to `0`, unit binning will be used instead of cutting. See the 'Details' section for more information. 
  
    
 ### `maxUnorderedLevels`
  the maximum number of levels allowed for an unordered factor predictor for multiclass (>2) classification. 
  
    
 ### `removeMissings`
  logical value.  If `TRUE`, rows with missing values are removed and  will not be included in the output data. 
  
    
 ### `computeObsNodeId`
 logical value or NULL.  If `TRUE`, the tree node IDs for all the observations are computed and returned. If `NULL`, the IDs are computed for data.frame with less than 1000 observations  and are returned as the `where` component in the fitted `rxDTree` object. 
  
    
 ### `useSparseCube`
 logical value. If `TRUE`, sparse cube is used. 
  
    
 ### `findSplitsInParallel`
  logical value.  If `TRUE`, optimal splits for each node are determined using parallelization methods;  this will typically speed up computation as the number of nodes on the same level is increased. 
  
    
 ### `pruneCp`
 Optional complexity parameter for pruning. If `pruneCp > 0`, `prune.rxDTree` is  called on the completed tree with the specified `pruneCp` and the pruned tree is returned. This contrasts with the `cp` parameter that determines which splits are considered in *growing* the tree. The option `pruneCp="auto"` causes `rxDTree` to call the function `rxDTreeBestCp` on the completed tree, then use its return value as the cp value for `prune.rxDTree`. 
  
  
    
 ### `rowSelection`
 name of a logical variable in the data set (in quotes) or a logical expression using variables in the data set to specify row selection.  For example, `rowSelection = "old"` will use only observations in which the value of the variable `old` is `TRUE`.  `rowSelection = (age > 20) & (age < 65) & (log(income) > 10)` will use only observations in which the value of the `age` variable is between 20 and 65 and the value of the `log` of the `income` variable is greater than 10.  The row selection is performed after processing any data transformations  (see the arguments `transforms` or `transformFunc`). As with all expressions, `rowSelection` can be defined outside of the function  call using the expression function. 
  
  
    
 ### `transforms`
  an expression of the form `list(name = expression, ...)`representing the first round of variable transformations.  As with all expressions, `transforms` (or `rowSelection`)  can be defined outside of the function call  using the expression function. 
  
    
 ### `transformObjects`
  a named list containing objects that can be referenced by  `transforms`, `transformsFunc`, and `rowSelection`. 
  
    
 ### `transformFunc`
  variable transformation function.  The ".rxSetLowHigh" attribute must be set for transformed variables if they are to be used in `formula`. See [rxTransform](rxtransform.md) for details. 
  
    
 ### `transformVars`
  character vector of input data set variables  needed for the transformation function.  See [rxTransform](rxtransform.md) for details. 
  
    
 ### `transformPackages`
  character vector defining additional R packages  (outside of those specified in `rxGetOption("transformPackages")`)  to be made available and preloaded for use in variable transformation functions,  e.g., those explicitly defined in **RevoScaleR** functions  via their `transforms` and `transformFunc` arguments  or those defined implicitly via their `formula` or `rowSelection` arguments.  The `transformPackages` argument may also be `NULL`,  indicating that no packages outside `rxGetOption("transformPackages")` will be preloaded. 
  
    
 ### `transformEnvir`
  user-defined environment to serve as a parent to all environments  developed internally and used for variable data transformation. If `transformEnvir = NULL`,  a new "hash" environment with parent `baseenv()` is used instead. 
  
    
 ### `blocksPerRead`
  number of blocks to read for each chunk of data  read from the data source. 
  
    
 ### `reportProgress`
  integer value with options:  
   *   `0`: no progress is reported. 
   *   `1`: the number of processed rows is printed and updated. 
   *   `2`: rows processed and timings are reported. 
   *   `3`: rows processed and all timings are reported. 
 
  
    
 ### `verbose`
  integer value.  If `0`, no verbose output is printed during calculations.  Integer values from `1` to `2` provide increasing amounts of information are provided. 
  
    
 ### `computeContext`
 a valid [RxComputeContext](rxcomputecontext.md).  The `RxSpark`, `RxHadoopMR`, and `RxInTeradata` compute contexts distribute the computation among the nodes specified by the compute context; for other compute contexts, the computation is distributed if possible on the local computer. 
  
  
    
 ### `xdfCompressionLevel`
  integer in the range of -1 to 9 indicating the compression level for the output data  if written to an `.xdf` file.   The higher the value, the greater the amount of compression -  resulting in smaller files but a longer time to create them.  If `xdfCompressionLevel` is set to 0, there will be no compression and  files will be compatible with the 6.0 release of Revolution R Enterprise.   If set to -1, a default level of compression will be used. 
  
    
 ### ` ...`
  additional arguments to be passed directly to the Microsoft R Services Compute Engine. 
  
 
 
 ##Details
 
`rxDTree` is a parallel external memory decision tree algorithm 
targeted for very large data sets.

It is modeled after rpart (Version 4.1-0) and 
inspired by the algorithm proposed by Yael Ben-Haim and Elad Tom-Tov (2010).

It uses a histogram as the approximate compressed representation of the data and
builds the tree in a breadth-first fashion using horizontal parallelism.  

`maxNumBins` specifies the maximum number of bins for the histogram
of each continuous independent variable and thus controls the accuracy of the algorithm.
Also, rxDTree builds histograms with roughly equal number of observations in each bin 
and checks only the boundaries of the bins as candidate splits to find the best split. 
So it is possible that a suboptimal split is chosen if `maxNumBins` is too small.
This may cause the tree to be different from one constructed by a standard algorithm.
Increasing `maxNumBins` allows more accurate results but with increased time and memory usage..

Surrogate splits may be used to assign observations for which the primary split variable 
is missing. Surrogate splits compare the groups produced by the remaining predictor variables
to the groups produced by the primary split variable, and the predictors are ranked by how
well their groups match the primary predictor. The best match is used as the surrogate split. 
 
 
 ##Value
 
an object of class `"rxDTree"` representing the fitted tree. 
It is a list with components similar to those of class `"rpart"` with the following distinctions:


* `where` -  A vector of integers indicating the node to which each point is allocated.  This information is always returned if the data source is a data frame.  If the data source is not a data frame and `outFile` is specified.  i.e., not `NULL`, the node indices are written/appended to  the specified file with a column name as defined by `outColName`.



For other components, see rpart.object for details.
 
 ##Note
 
rxDTree requires multiple passes over the data set and 
the maximum number of passes can be computed as follows:


* quantile computation:  `1` pass for computing the quantiles for all continuous variables,


* recursive partition:  `maxDepth + 1` passes for building the tree on the entire dataset,


* tree node assignment:  `1` pass for computing the id of the leaf node that each observation falls into.




If cross validation is specified (i.e., `xVal>0`), additional passes will be needed for each fold:


* recursive partition:  `maxDepth + 1` passes for building the tree on the other folds,


* tree node assignment:  `1` pass for predicting on the current fold.



resulting in `xVal * ((maxDepth + 1) + 1)` additional passes for cross validation.


 
 
 ##Author(s)
 
Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)

 
 
 ##References
 
Breiman, L., Friedman, J. H., Olshen, R. A. and Stone, C. J. (1984)
*Classification and Regression Trees*.
Wadsworth.

Therneau, T. M. and Atkinson, E. J. (2011)
*An Introduction to Recursive Partitioning Using the RPART Routines*.
[`http://r.789695.n4.nabble.com/attachment/3209029/0/zed.pdf`](http://r.789695.n4.nabble.com/attachment/3209029/0/zed.pdf)
.

Yael Ben-Haim and Elad Tom-Tov (2010)
A streaming parallel decision tree algorithm.
*Journal of Machine Learning Research* **11**, 849--872. 
 
 
 ##See Also
 
rpart, rpart.control, rpart.object,
[rxPredict.rxDTree](rxdtree.md), [rxAddInheritance](rxaddinheritance.md), [rxDForestUtils](rxdforestutils.md).
   
 ##Examples

 ```
   
  set.seed(1234)
  
  # classification
  iris.sub <- c(sample(1:50, 25), sample(51:100, 25), sample(101:150, 25))
  iris.dtree <- rxDTree(Species ~ Sepal.Length + Sepal.Width + Petal.Length + Petal.Width, 
      data = iris[iris.sub, ])
  iris.dtree
  
  table(rxPredict(iris.dtree, iris[-iris.sub, ], type = "class")[[1]], 
      iris[-iris.sub, "Species"])
  
  # regression
  infert.nrow <- nrow(infert)
  infert.sub <- sample(infert.nrow, infert.nrow / 2)
  infert.dtree <- rxDTree(case ~ age + parity + education + spontaneous + induced, 
      data = infert[infert.sub, ], cp = 0.01)
  infert.dtree
         
  hist(rxPredict(infert.dtree, infert[-infert.sub, ])[[1]] - 
      infert[-infert.sub, "case"])
  
  # use transformations with rxDTree
  range(log(iris$Sepal.Width)) ## find out the overall low/high values of the transformed variable
  #[1] 0.6931472 1.4816045 
  
  myTransform <- function(dataList) 
  { 
      dataList$log.Sepal.Width <-log(dataList$Sepal.Width) 
      attr(dataList$log.Sepal.Width, ".rxSetLowHigh") <- c(0.6931472, 1.4816045) # set the overall low/high values
      return(dataList) 
  } 
  
  frm <- Sepal.Length ~ log.Sepal.Width + Petal.Length + Petal.Width + Species 
  model <- rxDTree(frm, iris, transformFunc = myTransform, transformVars = c("Sepal.Width")) 
  stopifnot("log.Sepal.Width" %in% names(model$variable.importance)) 
  
  # .xdf file
  claimsXdf <- file.path(rxGetOption("sampleDataDir"),"claims.xdf")
  claims.dtree <- rxDTree(cost ~ age + car.age + type,
  	data = claimsXdf)
  claimsCp <- rxDTreeBestCp(claims.dtree)
  claims.dtree1 <- prune.rxDTree(claims.dtree, cp=claimsCp)
  claims.dtree2 <- rxDTree(cost ~ age + car.age + type, 
      data = claimsXdf, pruneCp="auto")
  ## claims.dtree1 and claims.dtree2 should differ only in their 
  ## "call" component
  claims.dtree1[[3]] <- claims.dtree2[[3]] <- NULL
  all.equal(claims.dtree1, claims.dtree2)
 
```
 
 
 
 
 
 
