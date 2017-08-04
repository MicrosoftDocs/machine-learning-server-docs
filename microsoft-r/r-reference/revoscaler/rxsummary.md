--- 
 
# required metadata 
title: "Object Summaries" 
description: " Produce univariate summaries of objects in **RevoScaleR**. " 
keywords: "RevoScaleR, rxSummary, univar" 
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
 
 
 #rxSummary: Object Summaries

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
Produce univariate summaries of objects in **RevoScaleR**.
 
 
 ##Usage

```   
  rxSummary(formula, data, byGroupOutFile = NULL, 
            summaryStats = c("Mean", "StdDev", "Min", "Max", "ValidObs", "MissingObs"),
            byTerm = TRUE, pweights = NULL, fweights = NULL, rowSelection = NULL,
            transforms = NULL, transformObjects = NULL,
            transformFunc = NULL, transformVars = NULL,
            transformPackages = NULL, transformEnvir = NULL, 
            overwrite = FALSE,
            useSparseCube = rxGetOption("useSparseCube"),
            removeZeroCounts = useSparseCube, 		  
            blocksPerRead = rxGetOption("blocksPerRead"),
            rowsPerBlock = 100000,
            reportProgress = rxGetOption("reportProgress"), verbose = 0,
            computeContext = rxGetOption("computeContext"), ...)
 
```
 
 
 ##Arguments

   
    
 ### formula
 formula, as described in [rxFormula](rxformula.md). The formula typically does not contain a response variable, i.e. it should be of the form `~ terms`. If `~.` is used as the formula, summary statistics will be computed for all  non-character variables. If a numeric variable is interacted with a factor variable, summary statistics will be computed for each category of the factor. 
  
  
    
 ### data
 either a data source object, a character string specifying a .xdf file, or a data frame object to summarize. 
  
  
    
 ### byGroupOutFile
 NULL, a character string or vector of character strings  specifying .xdf file names(s), or an RxXdfData object or list of RxXdfData objects.  If not NULL, and the formula includes computations by factor, the by-group summary results will be  written out to one or more .xdf files.  If more than one `.xdf` file is created and a single character string is specified, an integer will be appended to the base byGroupOutFile name  for additional file names. The resulting RxXdfData objects will be listed in  the `categorical` component of the output object.  `byGroupOutFile` is not supported when using distributed compute contexts such as [RxHadoopMR](rxhadoopmr.md) and [RxInTeradata](rxinteradata.md). 
  
  
    
 ### summaryStats
 a character vector containing one or more of the following  values:  "Mean", "StdDev", "Min", "Max", "ValidObs", "MissingObs", "Sum". 
   
  
    
 ### byTerm
 logical variable. If `TRUE`, missings will be removed  by term (by variable or by interaction expression) before computing  summary statistics.  If `FALSE`, observations with missings in any  term will be removed before computations. 
  
  
    
 ### pweights
 character string specifying the variable to use as probability weights for the observations. 
  
  
    
 ### fweights
 character string specifying the variable to use as frequency weights for the observations. 
  
  
    
 ### rowSelection
 name of a logical variable in the data set (in quotes) or a logical expression using variables in the data set to specify row selection.  For example, `rowSelection = "old"` will use only observations in which the value of the variable `old` is `TRUE`.  `rowSelection = (age > 20) & (age < 65) & (log(income) > 10)` will use only observations in which the value of the `age` variable is between 20 and 65 and the value of the `log` of the `income` variable is greater than 10.  The row selection is performed after processing any data transformations  (see the arguments `transforms` or `transformFunc`). As with all expressions, `rowSelection` can be defined outside of the function  call using the expression function. 
  
  
    
 ### transforms
 an expression of the form `list(name = expression, ...)` representing the first round of variable transformations. As with all expressions, `transforms` (or `rowSelection`)  can be defined outside of the function call using the expression function. 
  
  
    
 ### transformObjects
 a named list containing objects that can be referenced by `transforms`, `transformsFunc`, and `rowSelection`. 
  
  
    
 ### transformFunc
 variable transformation function. See [rxTransform](rxtransform.md) for details. 
  
  
    
 ### transformVars
 character vector of input data set variables needed for the transformation function. See [rxTransform](rxtransform.md) for details. 
  
  
    
 ### transformPackages
 character vector defining additional R packages (outside of those specified in `rxGetOption("transformPackages")`) to be made available and  preloaded for use in variable transformation functions, e.g., those explicitly defined in **RevoScaleR** functions via their `transforms` and `transformFunc` arguments or those  defined implicitly via their `formula` or `rowSelection` arguments.  The `transformPackages` argument may also be `NULL`,  indicating that no packages outside `rxGetOption("transformPackages")` will be preloaded. 
  
  
    
 ### transformEnvir
 user-defined environment to serve as a parent to  all environments developed internally and used for variable data transformation. If `transformEnvir = NULL`, a new "hash" environment with parent `baseenv()` is used instead. 
  
  
    
 ### overwrite
 logical value. If `TRUE`, an existing `byGroupOutFile` will be overwritten. `overwrite` is ignored `byGroupOutFile` is `NULL`. 
  
  
    
 ### useSparseCube
 logical value. If `TRUE`, sparse cube is used. 
  
  
    
 ### removeZeroCounts
 logical flag. If `TRUE`, rows with no observations will be removed from the output for counts of categorical data. By default, it has the same value as `useSparseCube`. For large summary computation, this should be set to `TRUE`, otherwise R may run out of memory even if the internal C++ computation succeeds. 
  
  
    
 ### blocksPerRead
 number of blocks to read for each chunk of data read from the data source. 
  
  
    
 ### rowsPerBlock
 maximum number of rows to write to each block in the  `byGroupOutFile` (if it is not `NULL`). 
  
  
    
 ### reportProgress
 integer value with options:  
*   `0`: no progress is reported. 
*   `1`: the number of processed rows is printed and updated. 
*   `2`: rows processed and timings are reported. 
*   `3`: rows processed and all timings are reported. 
  
  
  
    
 ### verbose
 integer value. If `0`, no additional output is printed.  If `1`, additional summary information is printed. 
  
  
  
 ### computeContext
 a valid [RxComputeContext](rxcomputecontext.md).  The `RxHpcServer`,  `RxHadoopMR`, and `RxInTeradata` compute  contexts distribute the computation among the nodes specified by the  compute context; for other compute contexts, the  computation is distributed if possible on the local computer. 
  
  
  
    
 ###  ...
 additional arguments to be passed directly to the Revolution Compute Engine. 
  
 
 
 
 ##Details
 
Special function `F()` can be used in formula to force a variable to be
interpreted as factors.

If the formula contains a single dependent or response variable, summary statistics are
computed for the interaction between that variable and the first term
of the independent variables. (Multiple response variables are not permitted.)
For example, using the formula `y ~ xfac` 
will give the same results as using the formula `~y:xfac`, where `y` 
is a continuous variable and `xfac` is a factor. Summary statistics 
for `y` are computed for each factor level of `x`. This facilitates using the
same formula in `rxSummary` as in, for example, `rxCube` or `rxLinMod`.
 
 
 ##Value
 
an rxSummary object containing the following elements:

###nobs.valid
number of valid observations.


###nobs.missing
number of missing observations.


###sDataFrame
data frame containing summaries for continuous variables.


###categorical
list of summaries for categorical variables.


###categorical.type
types of categorical summaries: can be "counts", or "cube" (for crosstab counts) or "none" (if there is no categorical summaries).


###formula
formula used to obtain the summary.

 
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 
 ##See Also
 
[rxTransform](rxtransform.md)
   
 ##Examples

 ```
   
  # Create a local data frame
  DF <- data.frame(sex = c("Male", "Male", "Female", "Male"),
                   age = c(20, 20, 12, 10), score = 1.1:4.1)
  
  # get summary of sex variable
  rxSummary(~ sex, DF)
  
  # obtain within sex-category statistics of the score variable
  rxSummary(score ~ sex, DF)
  
  # use transforms to create a factor variable and compute
  # summary statistics by each factor level
  rxSummary(~score:ageGroup, data=DF, 
            transforms = list(ageGroup = cut(age, seq(0, 30, 10))))
  
  # the following will give the same results
  rxSummary(score~ageGroup, data=DF, 
            transforms = list(ageGroup = cut(age, seq(0, 30, 10))))
  
  # the same formula can be used in rxCube          
  rxCube(score~ageGroup, data=DF, transforms=list(ageGroup = cut(age, seq(0,30,10))))
  
  # Write summary statistics by group to an .xdf file.  Here the groups 
  # are defined as year of age by sex by state (3 states in CensusWorkers file), 
  # so summary statistics for 46 x 2 x 3 groups are computed. The first term
  # will just compute the Counts for each group, while the second two will
  # compute by-group Means and ValidObs for incwage and wkswork1
  
  censusWorkers <- file.path(rxGetOption("sampleDataDir"), "CensusWorkers.xdf")
  sumOutFile <-  tempfile(pattern = ".rxTempSumOut", fileext = ".xdf")
  	
  sumOut <- rxSummary(~F(age):sex:state + incwage:F(age):sex:state + wkswork1:F(age):sex:state, 
  		data = censusWorkers, blocksPerRead = 3,
  		byGroupOutFile = sumOutFile, rowsPerBlock = 10, summaryStats = c("Mean", "ValidObs"))
  		
  rxGetVarInfo(sumOutFile)
  
  file.remove(sumOutFile)
  
 
```
 
 
