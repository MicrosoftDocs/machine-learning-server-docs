--- 
 
# required metadata 
title: "rxCube function (RevoScaleR) | Microsoft Docs" 
description: " Use rxCube to create efficiently represented contingency tables from cross-classifying factors using a formula interface. It performs equivalent calculations to the rxCrossTabs function, but returns its results in a different way. " 
keywords: "(RevoScaleR), rxCube, print.rxCube, summary.rxCube, as.data.frame.rxCube, subset.rxCube, [.rxCube, category, models" 
author: "heidisteen" 
manager: "jhubbard" 
ms.date: "09/07/2017" 
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
 
 
 
 
 
 
 
 #rxCube: Cross Tabulation 
 ##Description
 
Use `rxCube` to create efficiently represented contingency tables from
cross-classifying factors using a formula interface. It performs equivalent
calculations to the `rxCrossTabs` function, but returns its results in a
different way.
 
 
 ##Usage

```   
  rxCube(formula, data, outFile = NULL, pweights = NULL, fweights = NULL, 
         means = TRUE, cube = TRUE, rowSelection = NULL, 
         transforms = NULL, transformObjects = NULL,
         transformFunc = NULL, transformVars = NULL, 
         transformPackages = NULL, transformEnvir = NULL,
         overwrite = FALSE, 
         useSparseCube = rxGetOption("useSparseCube"),
         removeZeroCounts = useSparseCube, returnDataFrame = FALSE, 
         blocksPerRead = rxGetOption("blocksPerRead"),
         rowsPerBlock = 100000,
         reportProgress = rxGetOption("reportProgress"), verbose = 0, 
         computeContext = rxGetOption("computeContext"), ...)
  
 ## S3 method for class `rxCube':
print  (x, header = TRUE, ...)
   
 ## S3 method for class `rxCube':
summary  (object, header = TRUE, ...)
  
 ## S3 method for class `rxCube':
as.data.frame  (x, row.names = NULL, optional = FALSE, ...)
  
 ## S3 method for class `rxCube':
subset  (x, ...)
  
 ## S3 method for class `rxCube':
[  (x, ...)
 
```
 
 ##Arguments

   
    
 ### `formula`
 formula as described in [rxFormula](rxFormula.md) with the cross-classifying variables (separated by `:`) on the right hand side. Independent variables must be factors. If present, the dependent variable must be numeric. 
  
  
    
 ### `data`
 either a data source object, a character string specifying a .xdf file, or a data frame object containing the cross-classifying variables. 
  
  
    
 ### `outFile`
 `NULL`, a character string specifying a .xdf file, or an [RxXdfData](RxXdfData.md) object.  If not NULL, the cube results will be written out to an .xdf file and an `RxXdfData` object will be returned. `outFile` is not supported when using distributed compute contexts. 
  
  
    
 ### `pweights`
 character string specifying the variable to use as probability weights for the observations. 
  
  
    
 ### `fweights`
 character string specifying the variable to use as frequency weights for the observations. 
  
  
    
 ### `means`
 logical flag. If `TRUE` (default), the mean values of the dependent variable are returned. Otherwise, the variable summations are returned. 
  
  
    
 ### `cube`
 logical flag. If `TRUE`, the C++ cube functionality is called. 
  
  
    
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
  
  
    
 ### `overwrite`
 logical value. If `TRUE`, an existing `outFile` will be overwritten. `overwrite` is ignored `outFile` is `NULL`. 
  
  
    
 ### `useSparseCube`
 logical value. If `TRUE`, sparse cube is used. 
  
  
    
 ### `removeZeroCounts`
 logical flag. If `TRUE`, rows with no observations will be removed from the output. By default, it has the same value as `useSparseCube`. For large cube computation, this should be set to `TRUE`, otherwise R may run out of memory even if the internal C++ computation succeeds. 
  
  
    
 ### `returnDataFrame`
 logical flag. If `TRUE`, a data frame is returned, otherwise a list is returned. Ignored if `outFile` is specified and is not `NULL`. See the Details section for more information. 
  
  
    
 ### `blocksPerRead`
 number of blocks to read for each chunk of data read from the data source. 
  
  
    
 ### `rowsPerBlock`
 maximum number of rows to write to each block in the  `outFile` (if it is not `NULL`). 
  
  
    
 ### `reportProgress`
 integer value with options:  
*   `0`: no progress is reported. 
*   `1`: the number of processed rows is printed and updated. 
*   `2`: rows processed and timings are reported. 
*   `3`: rows processed and all timings are reported. 
  
  
  
    
 ### `verbose`
 integer value. If `0`, no additional output is printed.  If `1`, additional summary information is printed. 
  
  
    
 ### `computeContext`
 a valid [RxComputeContext](RxComputeContext.md).  The `RxSpark` and `RxHadoopMR` compute  contexts distribute the computation among the nodes specified by the  compute context; for other compute contexts, the  computation is distributed if possible on the local computer. 
  
  
    
 ### ` ...`
 additional arguments to be passed directly to the Revolution Compute Engine. 
  
  
    
 ### `x, object`
 output objects from rxCube function. 
  
  
    
 ### `header`
 logical value. If `TRUE`, header information is printed. 
  
  
    
 ### `row.names`
 the `row.names` argument passed unaltered to the underlying `as.data.frame.list` function. 
  
  
    
 ### `optional`
 the `optional` argument passed unaltered to the underlying `as.data.frame.list` function. 
  
 
 
 
 ##Details
 
The output of the `rxCube` function is essentially the same as that
produced by [rxCrossTabs](rxCrossTabs.md) except that it is presented in a
different format. While the `rxCrossTabs` function produces lists of
contingency tables (where each table is a matrix), the `rxCube` function
outputs a single list (or data frame, or .xdf file) containing one column for each variable
specified in the formula, plus a `"Counts"` column. The columns
corresponding to *independent* variables contain the factor levels of
that variable, replicated as necessary. If a dependent variable is specified
in the formula, an output column of the same name is produced and contains the
mean values of the categories defined by the interaction of the
independent/categorical variables. 
The `"Counts"` column contains the counts of the interactions used to
form the corresponding means.
 
 
 ##Value
 


* 
 `outFile` is not NULL: an `RxXdfData` object representing the output .xdf file.
In this case, the value for `returnDataFrame` is ignored.      

* 
 `returnDataFrame = FALSE`: an object of class rxCube that is also of class `"list"`. 
This is the default.

* 
 `returnDataFrame = TRUE`: an object of class `"data.frame"`.


In all cases, the names of the output columns are those of the
variables defined in the formula plus a `"Counts"` column.
See the Details section for more information regarding the content of these
columns.
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
xtabs,
[rxCrossTabs](rxCrossTabs.md),
[as.xtabs](as.xtabs.md),
[rxTransform](rxTransform.md).
   
 ##Examples

 ```
   
  # Basic data.frame source example
  admissions <- as.data.frame(UCBAdmissions)
  admissCube <- rxCube(Freq ~ Gender : Admit, data = admissions)
  admissCube
  
  # XDF example: small subset of census data
  censusWorkers <- file.path(rxGetOption("sampleDataDir"), "CensusWorkers.xdf")
  censusCube <- rxCube(wkswork1 ~ sex : F(age), data = censusWorkers,
  	pweights = "perwt", blocksPerRead = 3, returnDataFrame = TRUE)
  censusCube
  censusCube$age <- as.integer(as.character(censusCube$F_age))
  rxLinePlot(wkswork1 ~ age, groups=sex, data = censusCube)
  
  # perform a census cube, limiting the analysis to ages
  # on the interval [20, 65]. Verify the age range from the output.
  censusCubeAge.20.65 <- rxCube(wkswork1 ~ sex : F(age), data = censusWorkers,
      rowSelection = age >= 20 & age <= 65)
  ageRange <- range(as.numeric(as.character(censusCubeAge.20.65$F_age)))
  (ageRange[1] >= 20 & ageRange[2] <=65)
  
  # Create a local data.frame and define a transformation  
  # function to be applied to the data prior to processing.
  myDF <- data.frame(sex = c("Male", "Male", "Female", "Male"),
  	age = factor(c(20,20,12,15)), score = 1.1:4.1, sport=c(1:3,2))
                     
  # Use the 'transforms' argument to dynamically transform the
  # variables of the data source. Here, we form a named list of 
  # transformation expressions. To avoid evaluation when assigning
  # to a local variable, we wrap the transformation list with expression().
  transforms <- expression(list(
  	scoreDoubled = score * 2,
  	sport = factor(sport, labels=c("tennis", "golf", "football"))))
  
  rxCube(scoreDoubled ~ sport : sex, data = myDF, transforms = transforms,
  	removeZeroCounts=TRUE)
  
  # Arithmetic formula expression only (no transformFunc specification).
  rxCube(log(score) ~ age : sex, data = myDF)
  
  # No transformFunc or arithmetic expressions in formula.
  rxCube(score ~ age : sex, data = myDF)
  
  # Transform a categorical variable to a continuous one and use it
  # as a response variable in the formula for cross-tabulation.
  # The transformation is equvalent to doing the following, which
  # is reflected in the cross-tabulation results.
  #
  #   > as.numeric(as.factor(c(20,20,12,15))) - 1
  #   [1] 2 2 0 1
  myDF <- data.frame(sex = c("Male", "Male", "Female", "Male"),
  	age = factor(c(20, 20, 12, 15)), score = 1.1:4.1)
  rxCube(N(age) ~ sex : F(score), data = myDF)
  
  # this should break because 'age' is a categorical variable
  ## Not run:
 
try(rxCube(age ~ sex : score, data = myDF))
 ## End(Not run) 
  
  
  # frequency weighting
  fwts <- 1:4
  sex <- c("Male", "Male", "Female", "Male")
  age <- c(20, 20, 12, 15)
  score <- 1.1:4.1
  
  myDF1 <- data.frame(sex = sex, age = age, score = factor(score), fwts = fwts)    
  myDF2 <- data.frame(sex = rep(sex, fwts), age = rep(age, fwts),
  	score = factor(rep(score, fwts)))
  
  myCube1 <- rxCube(age ~ sex : score, data = myDF1, fweights = "fwts")
  myCube2 <- rxCube(age ~ sex : score, data = myDF2)
  all.equal(myCube1, myCube2, check.attributes = FALSE)
 
```
 
 
 
