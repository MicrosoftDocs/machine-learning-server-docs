--- 
 
# required metadata 
title: "Parallel External Memory Algorithm for Stochastic Gradient Boosted Decision Trees" 
description: "     Fit stochastic gradient boosted decision trees on an .xdf file or data frame     for small or large data using parallel external memory algorithm. " 
keywords: "RevoScaleR, rxBTrees, plot.rxBTrees, print.rxBTrees, models, tree, classif, regression, classification" 
author: "richcalaway" 
manager: "jhubbard" 
ms.date: "04/03/2017" 
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
 
 
 
 
 #`rxBTrees`: Parallel External Memory Algorithm for Stochastic Gradient Boosted Decision Trees

 Applies to version 9.0.1 of package RevoScaleR.
 
 
 ##Description
 
Fit stochastic gradient boosted decision trees on an .xdf file or data frame
for small or large data using parallel external memory algorithm.
 
 
 ##Usage

```   
  rxBTrees(formula, data,
      outFile = NULL, writeModelVars = FALSE, overwrite = FALSE,    
      pweights = NULL, fweights = NULL, cost = NULL,    
      minSplit = NULL, minBucket = NULL, maxDepth = 1, cp = 0,
      maxCompete = 0, maxSurrogate = 0, useSurrogate = 2, surrogateStyle = 0,    
      nTree = 10, mTry = NULL, replace = FALSE,    
      strata = NULL, sampRate = NULL, importance = FALSE, seed = sample.int(.Machine$integer.max, 1),
      lossFunction = "bernoulli", learningRate = 0.1,    
      maxNumBins = NULL, maxUnorderedLevels = 32, removeMissings = FALSE, 
      useSparseCube = rxGetOption("useSparseCube"), findSplitsInParallel = TRUE,    
      scheduleOnce = FALSE,    
      rowSelection = NULL, transforms = NULL, transformObjects = NULL, transformFunc = NULL,
      transformVars = NULL, transformPackages = NULL, transformEnvir = NULL,
      blocksPerRead = rxGetOption("blocksPerRead"), reportProgress = rxGetOption("reportProgress"),
      verbose = 0, computeContext = rxGetOption("computeContext"), 
      xdfCompressionLevel = rxGetOption("xdfCompressionLevel"),
        ...  )
  
 ## S3 method for class `rxBTrees':
print  (x, by.class = FALSE,   ...  )
  
 ## S3 method for class `rxBTrees':
plot  (x, type = "l", lty = 1:5, lwd = 1, pch = NULL, col = 1:6, 
      main = deparse(substitute(x)), by.class = FALSE,   ...  )
 
```
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 ##Arguments

   
    
 ### `formula`
  formula as described in [rxFormula](rxFormula.md).   Currently, formula functions are not supported. 
  
    
 ### `data`
  either a data source object, a character string  specifying a .xdf file, or a data frame object. 
  
    
 ### `outFile`
  either an RxXdfData data source object or a character string  specifying the .xdf file for storing the resulting OOB predictions.  If `NULL` or the input data is a data frame, then no OOB predictions are stored to disk.    If `rowSelection` is specified and not `NULL`,  then `outFile` cannot be the same as the `data`since the resulting set of OOB predictions will generally not  have the same number of rows as the original data source. 
  
   
   
   
   
   
    
 ### `writeModelVars`
  logical value. If `TRUE`, and the output file is different from the input file,  variables in the model will be written to the output file in addition to the OOB predictions.  If variables from the input data set are transformed in the model,  the transformed variables will also be written out. 
  
    
 ### `overwrite`
  logical value. If `TRUE`, an existing `outFile`with an existing column named `outColName` will be overwritten. 
  
    
 ### `pweights`
  character string specifying the variable of numeric values to use as probability weights for the observations. 
  
    
 ### `fweights`
  character string specifying the variable of integer values to use as frequency weights for the observations. 
  
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
    
 ### `cost`
  a vector of non-negative costs, containing one element for each variable in the model. Defaults to one for all variables. When deciding which split	to choose, the improvement on splitting on a variable is divided by its cost. 
  
    
 ### `minSplit`
  the minimum number of observations that must exist in a node before a split is attempted. By default, this is `sqrt(num of obs)`. For non-XDF data sources, as `(num of obs)` is unknown in advance, it is wisest to specify this argument directly. 
  
    
 ### `minBucket`
  the minimum number of observations in a terminal node (or leaf). By default, this is `minSplit`/3. 
  
    
 ### `maxDepth`
  the maximum depth of any tree node. The computations take much longer at greater depth, so lowering `maxDepth` can greatly speed up computation time. 
  
    
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
  
    
 ### `surrogateStyle`
  an integer controlling selection of a best surrogate. The default, `0`, instructs the program to use the total number of correct classifications for a potential surrogate, while `1` instructs the program to use the percentage of correct classification over  the non-missing values of the surrogate. Thus, `0` penalizes potential surrogates with a large number of missing values. 
  
   
   
   
  
    
 ### `nTree`
  a positive integer specifying the number of boosting iterations,  which is generally the number of trees to grow except for `multinomial` loss function, where the number of trees to grow for each boosting iteration  is equal to the number of levels of the categorical response.  
  
    
 ### `mTry`
  a positive integer specifying the number of variables to sample as split candidates at each tree node. The default values is `sqrt(num of vars)` for classification and `(num of vars)/3` for regression.     
  
    
 ### `replace`
  a logical value specifying if the sampling of observations should be done with or without replacement.  
  
   
   
   
   
   
    
 ### `strata`
  a character string specifying the (factor) variable to use for stratified sampling.  
  
    
 ### `sampRate`
  a scalar or a vector of positive values specifying the percentage(s) of observations to sample for each tree:  
   * for unstratified sampling:  a scalar of positive value specifying the percentage of observations to sample for each tree. The default is 1.0 for sampling with replacement (i.e., `replace=TRUE`) and  0.632 for sampling without replacement (i.e., `replace=FALSE`).    
   * for stratified sampling:  a vector of positive values of length equal to the number of strata specifying  the percentages of observations to sample from the strata for each tree.   
  
  
    
 ### `importance`
  a logical value specifying if the importance of predictors should be assessed.  
  
    
 ### `seed`
  an integer that will be used to initialize the random number generator. The default is random. For reproducibility, you can specify the random seed either using set.seed or by setting this `seed` argument as part of your call.  
  
   
   
   
   
   
   
   
   
    
 ### `lossFunction`
  character string specifying the name of the loss function to use. The following options are currently supported:  
* `"gaussian"` - regression: for numeric responses.  
* `"bernoulli"` - regression: for 0-1 responses.  
* `"multinomial"` - classification: for categorical responses with two or more levels.  
  
  
    
 ### `learningRate`
  numeric scalar specifying the learning rate of the boosting procedure.  
  
  
    
 ### `maxNumBins`
  the maximum number of bins to use to cut numeric data.  The default is `min(1001, max(101, sqrt(num of obs)))`. For non-XDF data sources, as `(num of obs)` is unknown in advance, it is wisest to specify this argument directly. If set to `0`, unit binning will be used instead of cutting. See the 'Details' section for more information. 
  
    
 ### `maxUnorderedLevels`
  the maximum number of levels allowed for an unordered factor predictor for multiclass (>2) classification. 
  
    
 ### `removeMissings`
  logical value.  If `TRUE`, rows with missing values are removed and  will not be included in the output data. 
  
    
 ### `useSparseCube`
 logical value. If `TRUE`, sparse cube is used. 
  
    
 ### `findSplitsInParallel`
  logical value.  If `TRUE`, optimal splits for each node are determined using parallelization methods;  this will typically speed up computation as the number of nodes on the same level is increased. 
  
   
   
   
   
   
  
    
 ### `scheduleOnce`
 EXPERIMENTAL. logical value. If `TRUE`, rxBTrees will be run with [rxExec](rxExec.md), which submits only one job to the scheduler and thus can speed up computation on small data sets particularly in the [RxHadoopMR](RxHadoopMR.md) compute context. 
  
  
    
 ### `rowSelection`
 name of a logical variable in the data set (in quotes) or a logical expression using variables in the data set to specify row selection.  For example, `rowSelection = "old"` will use only observations in which the value of the variable `old` is `TRUE`.  `rowSelection = (age > 20) & (age < 65) & (log(income) > 10)` will use only observations in which the value of the `age` variable is between 20 and 65 and the value of the `log` of the `income` variable is greater than 10.  The row selection is performed after processing any data transformations  (see the arguments `transforms` or `transformFunc`). As with all expressions, `rowSelection` can be defined outside of the function  call using the expression function. 
  
  
    
 ### `transforms`
  an expression of the form `list(name = expression, ...)`representing the first round of variable transformations.  As with all expressions, `transforms` (or `rowSelection`)  can be defined outside of the function call  using the expression function. 
  
    
 ### `transformObjects`
  a named list containing objects that can be referenced by  `transforms`, `transformsFunc`, and `rowSelection`. 
  
    
 ### `transformFunc`
  variable transformation function.  The ".rxSetLowHigh" attribute must be set for transformed variables if they are to be used in `formula`. See [rxTransform](rxTransform.md) for details. 
  
    
 ### `transformVars`
  character vector of input data set variables  needed for the transformation function.  See [rxTransform](rxTransform.md) for details. 
  
    
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
 a valid [RxComputeContext](RxComputeContext.md).  The `RxHpcServer`, `RxHadoopMR`, and `RxInTeradata` compute contexts distribute the computation among the nodes specified by the compute context; for other compute contexts, the computation is distributed if possible on the local computer. 
  
  
    
 ### `xdfCompressionLevel`
  integer in the range of -1 to 9 indicating the compression level for the output data  if written to an `.xdf` file.   The higher the value, the greater the amount of compression -  resulting in smaller files but a longer time to create them.  If `xdfCompressionLevel` is set to 0, there will be no compression and  files will be compatible with the 6.0 release of Revolution R Enterprise.   If set to -1, a default level of compression will be used. 
  
    
 ### ` ...`
  additional arguments to be passed directly to the Microsoft R Services Compute Engine and to [rxExec](rxExec.md) when `scheduleOnce` is set to `TRUE`. 
  
  
    
 ### `x`
  an object of class `rxBTrees`. 
  
    
 ### `type, lty, lwd, pch, col, main`
  see plot.default and matplot for details. 
  
    
 ### `by.class`
  (classification with multinomial loss function only) logical value.  If `TRUE`, the out-of-bag error estimate will be broken down by classes. 
  
 
 
 ##Details
 
`rxBTrees` is a parallel external memory algorithm for stochastic gradient boosted decision trees
targeted for very large data sets.  It is based on the gradient boosting machine of 
Jerome Friedman and Trevor Hastie and Robert Tibshirani and 
modeled after the gbm package of Greg Ridgeway with contributions from others, 
using the tree-fitting algorithm introduced in [rxDTree](rxDTree.md).

In a decision forest, a number of decision trees are fit to bootstrap samples of the
original data. Observations omitted from a given bootstrap sample are termed 
"out-of-bag" observations. For a given observation, the decision forest prediction
is determined by the result of sending the observation through all the trees for which it
is out-of-bag. For classification, the prediction is the class to which a majority assigned 
the observation, and for regression, the prediction is the mean of the predictions. 

For each tree, the out-of-bag observations are
fed through the tree to estimate out-of-bag error estimates. The reported out-of-bag
error estimates are cumulative (that is, the *i*th element represents the 
out-of-bag error estimate for all trees through the *i*th).
 
 
 ##Value
 
an object of class `"rxBTrees"` inherited from class `"rxDForest"`.
It is a list with the following components, similar to those of class `"rxDForest"`:

###`ntree`
 The number of trees.


###`mtry`
 The number of variables tried at each split.


###`type`
 One of `"class"` (for classification) or `"anova"` (for regression).


###`forest`
a list containing the entire forest.


###`oob.err`
a data frame containing the out-of-bag error estimate. For classification forests, this includes the OOB error estimate, which represents the proportion of times the predicted class is  not equal to the true class, and the cumulative number of out-of-bag observations for the forest. For   regression forests, this includes the OOB error estimate, which here represents the sum of squared  residuals of the out-of-bag observations divided by the number of out-of-bag observations, the number  of out-of-bag observations, the out-of-bag variance, and the "pseudo-R-Squared", which is 1 minus the quotient of the `oob.err` and `oob.var`.








###`init.pred`
 The initial prediction value(s).


###`params`
 The input parameters passed to the underlying code.


###`formula`
 The input formula.


###`call`
 The original call to `rxBTrees`.

 
 ##Note
 
Like [rxDTree](rxDTree.md), `rxBTrees` requires multiple passes over the data set and 
the maximum number of passes can be computed as follows for loss functions other than `multinomial`:


* quantile computation:  `1` pass for computing the quantiles for all continuous variables,


* recursive partition:  `maxDepth + 1` passes per tree for building the tree on the entire dataset,


* leaf prediction estimation:  `1` pass per tree for estimating the optimal terminal node predictions,


* out-of-bag prediction:  `1` pass per tree for computing the out-of-bag error estimates.


 

For `multinomial` loss function, the number of passes except for the quantile computation
needs to be multiplied by the number of levels of the categorical response.

`rxBTrees` uses random streams and RNGs in parallel computation for sampling. 
Different threads on different nodes will be using different random streams so that
different but equivalent results might be obtained for different number of threads.
 
 
 ##Author(s)
 
Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)

 
 
 ##References
 
Y. Freund and R.E. Schapire (1997)
A decision-theoretic generalization of on-line learning and an application to boosting.
*Journal of Computer and System Sciences*, **55(1)**, 119--139.

G. Ridgeway (1999).
The state of boosting.
*Computing Science and Statistics* **31**, 172--181.

J.H. Friedman, T. Hastie, R. Tibshirani (2000).
Additive Logistic Regression: a Statistical View of Boosting.
*Annals of Statistics* **28(2)**, 337--374.

J.H. Friedman (2001).
Greedy Function Approximation: A Gradient Boosting Machine.
*Annals of Statistics* **29(5)**, 1189--1232.

J.H. Friedman (2002).
Stochastic Gradient Boosting.
*Computational Statistics and Data Analysis* **38(4)**, 367--378.

Greg Ridgeway with contributions from others, 
gbm: Generalized Boosted Regression Models (R package),
[`http://cran.r-project.org/web/packages/gbm/index.html`](http://cran.r-project.org/web/packages/gbm/index.html)

 
 
 ##See Also
 
[rxDForest](rxDForest.md), [rxDForestUtils](rxDForestUtils.md), [rxPredict.rxDForest](rxPredict.rxDForest.md).
   
 ##Examples

 ```
   
  library(RevoScaleR)
  set.seed(1234)
  
  # multi-class classification
  iris.sub <- c(sample(1:50, 25), sample(51:100, 25), sample(101:150, 25))
  iris.form <- Species ~ Sepal.Length + Sepal.Width + Petal.Length + Petal.Width
  iris.btrees <- rxBTrees(iris.form, data = iris[iris.sub, ], nTree = 50,
      importance = TRUE, lossFunction = "multinomial", learningRate = 0.1)
      
  iris.btrees
  plot(iris.btrees, by.class = TRUE)
  rxVarImpPlot(iris.btrees)
  
  iris.pred <- rxPredict(iris.btrees, iris[-iris.sub, ], type = "class")
  table(iris.pred[["Species_Pred"]], iris[-iris.sub, "Species"])
  
  # binary response
  require(rpart)
  kyphosis.nrow <- nrow(kyphosis)
  kyphosis.sub <- sample(kyphosis.nrow, kyphosis.nrow / 2)
  kyphosis.form <- Kyphosis ~ Age + Number + Start
  kyphosis.btrees <- rxBTrees(kyphosis.form, data = kyphosis[kyphosis.sub, ], 
      maxDepth = 6, minSplit = 2, nTree = 50,
      lossFunction = "bernoulli", learningRate = 0.1)
      
  kyphosis.btrees
  plot(kyphosis.btrees)
  
  kyphosis.prob <- rxPredict(kyphosis.btrees, kyphosis[-kyphosis.sub, ], type = "response")
  table(kyphosis.prob[["Kyphosis_prob"]] > 0.5, kyphosis[-kyphosis.sub, "Kyphosis"])
  
  # regression with .xdf file
  claims.xdf <- file.path(rxGetOption("sampleDataDir"), "claims.xdf")
  claims.form <- cost ~ age + car.age + type
  claims.btrees <- rxBTrees(claims.form, data = claims.xdf, 
      maxDepth = 6, minSplit = 2, nTree = 50,
      lossFunction = "gaussian", learningRate = 0.1)
      
  claims.btrees
  plot(claims.btrees)
 
```
 
 
 
 
 
 
