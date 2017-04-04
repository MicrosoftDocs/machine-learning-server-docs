--- 
 
# required metadata 
title: ".xdf File to data frame" 
description: " Extract a data frame into memory from an .xdf file. " 
keywords: "RevoScaleR, ScaleR, KEYWORDS" 
author: "richcalaway" 
manager: "jhubbard" 
ms.date: "02/17/2017" 
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
 
 
 #.xdf File to data frame 
 ##Description
 
Extract a data frame into memory from an .xdf file.
 
 
 ##Usage

```   
  rxXdfToDataFrame(file, varsToKeep = NULL, varsToDrop = NULL, rowVarName = NULL,
                   rowSelection = NULL, transforms = NULL, transformObjects = NULL,
                   transformFunc = NULL, transformVars = NULL, 
                   transformPackages = NULL, transformEnvir = NULL,  
                   removeMissings = FALSE, stringsAsFactors = FALSE,
                   blocksPerRead = rxGetOption("blocksPerRead"),
                   maxRowsByCols = 3000000,
                   reportProgress = rxGetOption("reportProgress"),
                   cppInterp = NULL) 
 
```
 
 ##Arguments

   
    
 >  `file` -  either an RxXdfData object or a character string specifying the .xdf file.  
  
    
 >  `varsToKeep` -  character vector of variable names to include when reading from the input data file. If `NULL`, argument is ignored. Cannot be used with `varsToDrop`.  
  
    
 >  `varsToDrop` -  character vector of variable names to exclude when reading from the input data file. If `NULL`, argument is ignored. Cannot be used with `varsToKeep`.  
  
    
 >  `rowVarName` -  optional character string specifying the variable in the data file to use as row names for the output data frame.  
  
    
 >  `rowSelection` -  name of a logical variable in the data set (in quotes) or a logical expression using variables in the data set to specify row selection.  For example, `rowSelection = "old"` will use only observations in which the value of the variable `old` is `TRUE`.  `rowSelection = (age > 20) & (age < 65) & (log(income) > 10)` will use only observations in which the value of the `age` variable is between 20 and 65 and the value of the `log` of the `income` variable is greater than 10.  The row selection is performed after processing any data transformations  (see the arguments `transforms` or `transformFunc`). As with all expressions, `rowSelection` can be defined outside of the function  call using the expression function.  
  
    
 >  `transforms` -  an expression of the form `list(name = expression, ...)` representing the variable transformations. As with all expressions, `transforms` (or `rowSelection`)  can be defined outside of the function call using the expression function.  
  
    
 >  `transformObjects` -  a named list containing objects that can be referenced by `transforms`, `transformsFunc`, and `rowSelection`.  
  
    
 >  `transformFunc` -  variable transformation function. See [rxTransform](rxTransform.md) for details.  
  
    
 >  `transformVars` -  character vector of input data set variables needed for the transformation function. See [rxTransform](rxTransform.md) for details.  
  
    
 >  `transformPackages` -  character vector defining additional R packages (outside of those specified in `rxGetOption("transformPackages")`) to be made available and  preloaded for use in variable transformation functions, e.g., those explicitly defined in **RevoScaleR** functions via their `transforms` and `transformFunc` arguments or those  defined implicitly via their `formula` or `rowSelection` arguments.  The `transformPackages` argument may also be `NULL`,  indicating that no packages outside `rxGetOption("transformPackages")` will be preloaded.  
  
    
 >  `transformEnvir` -  user-defined environment to serve as a parent to  all environments developed internally and used for variable data transformation. If `transformEnvir = NULL`, a new "hash" environment with parent `baseenv()` is used instead.  
  
    
 >  `removeMissings` -  logical value. If `TRUE`, rows with missing values will not be included in the output file.  
  
    
 >  `stringsAsFactors` -  logical indicating whether or not to convert strings into factors in R.  
  
    
 >  `blocksPerRead` -  number of blocks to read for each chunk of data read from the data source.  
  
    
 >  `maxRowsByCols` -  the maximum size of a data frame that will be read in, measured by the number of rows times the number of columns. If the numer of rows times the number of columns being extracted from the .xdf file exceeds this, a warning will be reported and a smaller number of rows will be read in than requested. If `maxRowsByCols` is set to be too large, you may experience problems  from loading a huge data frame into memory.  
  
    
 >  `reportProgress` -  integer value with options:  
*  `0`: no progress is reported. 
*  `1`: the number of processed rows is printed and updated. 
*  `2`: rows processed and timings are reported. 
*  `3`: rows processed and all timings are reported. 
   
  
  
  
 >  `cppInterp` -  NOT SUPPORTED information to be sent to the C++ interpreter.  
 
 
 ##Value
 
data frame containing the specified data from the .xdf file
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](http://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 
 ##See Also
 
[rxDataStep](rxDataStep.md),
[rxImport](rxImport.md),
[rxSplit](rxSplitXdf.md).
   
 ##Examples

 ```
   
  # Extract a subsample of CensusWorkers.xdf into a data frame in memory
  inFile <- file.path(rxGetOption("sampleDataDir"), "CensusWorkers.xdf")
  myCensusDF <- rxXdfToDataFrame(file=inFile, 
    rowSelection = state == "Washington" & age > 40,
    varsToKeep = c("age", "perwt", "sex"))
  head(myCensusDF)
  
  # Extract a random 10% subsample of CensusWorkers.xdf into a data frame 
  myCensusSample <- rxXdfToDataFrame(file=inFile, 
  	rowSelection = as.logical( rbinom(length(age),1,.10)) == TRUE ) 
  head(myCensusSample)
  nrow(myCensusSample)
  	
  # Perform dynamic data transformation
  myCensusDF2 <- rxXdfToDataFrame(file=inFile, 
    varsToKeep = c("age", "perwt", "sex"),
    transforms = list(ageGroup = cut(age, seq(1,100,20))))
  head(myCensusDF2)
  
  # What are the Counts of Males/Females in the ageGroup categories?
  rxCrossTabs(~ sex : ageGroup, myCensusDF2)
  
  ###
  # Develop a data transform function generator to both alter
  # an existing .xdf data source variable as well as return data 
  # summary variables through a user-defined environment. The 
  # transform function generator technique follows a distributed
  # computing data processing paradigm.
  ###
  myTransformGenerator <- function(varName, weightedVarName, weight, env)
  {
      # Initialize closure variables
      count <- 0L
      sumVar <- sumVarSquared <- 0
      
      # Primary function definition
      function(dataList)
      {
          # Obtain the specified variable from the data list
          myVar <- dataList[[varName]]
          
          # Transform an existing variable and store as new name
          dataList[[weightedVarName]] <- myVar * weight
          
          # Update the closure variables if not a test chunk
          # using the '<<-' assignment operator. Assign the updated
          # results to the user-specified environment.
          # Note that the variable '.rxIsTestChunk' is reserved and read-only,
          # and indicates whether the data source is being probed for information
          # rather than processing. We must take care not to accumulate the
          # results for the closure variables if .rxIsTestChunk is TRUE.
          if (!.rxIsTestChunk)
          {
              # handle missing values
              myVar <- myVar[!is.na(myVar)]
  
              # update closure variables with data processed by current block
              count <<- count + length(myVar)
              sumVar <<- sumVar + sum(myVar)
              sumVarSquared <<- sumVarSquared + sum(myVar^2)
              env[["closureVars"]] <- 
                  list(count=count, sum=sumVar, sumSquared=sumVarSquared,
                      varName=varName, weightedVarName=weightedVarName)
          }
         
          # Return the data list
          dataList
      }
  }
  
  # Define XDF data source and create new environment to hold
  # transform function closure variables. Perform the transformation.
  XDF <- file.path(rxGetOption("unitTestDataDir"), "test100r5b.xdf")
  tempEnv <- new.env(hash = TRUE, parent = baseenv())
  DF <- rxXdfToDataFrame(XDF, 
      transformFunc = myTransformGenerator(varName = "y", 
          weightedVarName = "yWeighted", weight = 5, env = tempEnv),
      varsToKeep="y", 
      blocksPerRead = 1)
  head(DF)
  
  # Obtain transform closure variables
  closureVars <- get("closureVars", envir = tempEnv)
  print(closureVars)
  
  # Compare naive biased sample variance estimate to that calculated
  # directly by the stats::var function. Note that there
  # are more robust sample variance estimators but this technique
  # suffices in illustrating the transform function generator technique.
  
  stdVar <- rxDataStep(XDF, varsToKeep = "y")$y 
  stdVar <- stdVar[!is.na(stdVar)]
  N <- length(stdVar)
  stdVarBiased <- stats::var(stdVar) * (N - 1) / N
  calcVarBiased <- closureVars[["sumSquared"]] / closureVars[["count"]] - 
      (closureVars[["sum"]] / closureVars[["count"]])^2
  all.equal(stdVarBiased, calcVarBiased)
 
```
 
 
 
