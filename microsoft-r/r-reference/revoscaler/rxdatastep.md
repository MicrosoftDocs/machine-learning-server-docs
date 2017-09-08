--- 
 
# required metadata 
title: "rxDataStep function (RevoScaleR) | Microsoft Docs" 
description: "    Transform data from an input data set to an output data set " 
keywords: "(RevoScaleR), rxDataStep, manip, file" 
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
 
 
 #rxDataStep: Data Step for RevoScaleR data sources 
 ##Description
 
Transform data from an input data set to an output data set
 
 
 ##Usage

```   
  rxDataStep(inData = NULL, outFile = NULL, varsToKeep = NULL, varsToDrop = NULL,
                rowSelection = NULL, transforms = NULL, transformObjects = NULL,
                transformFunc = NULL, transformVars = NULL, 
                transformPackages = NULL, transformEnvir = NULL, 
                append = "none", overwrite = FALSE, rowVarName = NULL, 
                removeMissingsOnRead = FALSE, removeMissings = FALSE, 
                computeLowHigh = TRUE, maxRowsByCols = 3000000,
                rowsPerRead = -1, startRow = 1, numRows = -1, 
                returnTransformObjects = FALSE,
                blocksPerRead = rxGetOption("blocksPerRead"),
                reportProgress = rxGetOption("reportProgress"),
                xdfCompressionLevel = rxGetOption("xdfCompressionLevel"), ...)  
                
                          
 
```
 
 ##Arguments

   
    
 ### `inData`
 A data source object of these types: [RxTextData](RxTextData.md), [RxXdfData](RxXdfData.md),  [RxSasData](RxSasData.md), [RxSpssData](RxSpssData.md), [RxOdbcData](RxOdbcData.md),  [RxSqlServerData](RxSqlServerData.md), [RxTeradata](RxTeradata.md). If not using a distributed compute context such as [RxHadoopMR](RxHadoopMR.md), a data frame,  a character string specifying the input .xdf file, or `NULL` can also be used.  If `NULL`, a data set will be created automatically with a single variable, `.rxRowNums`, containing row numbers. It will have a total of `numRows` rows with `rowsPerRead` rows in each block. 
  
  
    
 ### `outFile`
 A character string specifying the output .xdf file or any of these data sources: [RxTextData](RxTextData.md), [RxXdfData](RxXdfData.md),  [RxSasData](RxSasData.md), [RxSpssData](RxSpssData.md), [RxOdbcData](RxOdbcData.md),  [RxSqlServerData](RxSqlServerData.md), [RxTeradata](RxTeradata.md), [RxHiveData](RxSparkData.md),  [RxParquetData](RxSparkData.md), [RxOrcData](RxSparkData.md).  If `NULL`, a data frame will be returned from `rxDataStep` unless `returnTransformObjects` is set to `TRUE`. Setting `outFile` to `NULL` and `returnTransformObjects=TRUE` allows chunkwise computations on the data without modifying the existing data or creating a new data set. `outFile` can also be a delimited [RxTextData](RxTextData.md) data source if using a native file system and not appending. 
  
  
    
 ### `varsToKeep`
 A character vector of variable names to include when reading from the input data file. If `NULL`, argument is ignored. Cannot be used with `varsToDrop` or when `outFile` is the same as the input data file. Variables used in transformations or row selection will be retained even if not specified in `varsToKeep`. If `newName` is used in `colInfo` in a non-xdf  data source, the `newName` should be used in `varsToKeep`. Not supported for [RxTeradata](RxTeradata.md), [RxOdbcData](RxOdbcData.md), or [RxSqlServerData](RxSqlServerData.md) data sources. 
  
  
    
 ### `varsToDrop`
 A character vector of variable names to exclude when reading from the input data file. If `NULL`, argument is ignored. Cannot be used with `varsToKeep` or when `outFile` is the same as the input data file. Variables used in transformations or row selection will be retained even if specified in `varsToDrop`. If `newName` is used in `colInfo` in a non-xdf  data source, the `newName` should be used in `varsToDrop`. Not supported for [RxTeradata](RxTeradata.md), [RxOdbcData](RxOdbcData.md), or  [RxSqlServerData](RxSqlServerData.md) data sources. 
  
  
    
 ### `rowSelection`
 The name of a logical variable in the data set or a logical expression using variables in the data set to specify row selection.  For example, `rowSelection = old` will use only observations in which the value of the variable `old` is `TRUE`.  `rowSelection = (age > 20) & (age < 65) & (log(income) > 10)` will use only observations in which the value of the `age` variable is between 20 and 65 and the value of the `log` of the `income` variable is greater than 10.  The row selection is performed after processing any data transformations  (see the arguments `transforms` or `transformFunc`). As with all expressions, `rowSelection` can be defined outside of the function  call using the expression function. 
  
  
    
 ### `transforms`
 An expression of the form `list(name = expression, ...)` representing the first round of variable transformations. As with all expressions, `transforms` (or `rowSelection`)  can be defined outside of the function call using the expression function. When using function call in the expression, `transformObject` should be used to pass  the function name to remote context. In addition, calling and enclosing environment of the function  are very important if the function uses undefined variable. 
  
  
    
 ### `transformObjects`
 A named list containing objects that can be referenced by `transforms`, `transformsFunc`, and `rowSelection`.  
  
  
    
 ### `transformFunc`
 A variable transformation function. The recommended way to do variable transformation. See [rxTransform](rxTransform.md) for details. 
  
  
    
 ### `transformVars`
 A character vector of input data set variables needed for the transformation function. See [rxTransform](rxTransform.md) for details. 
  
  
    
 ### `transformPackages`
 A character vector defining additional R packages (outside of those specified in `rxGetOption("transformPackages")`) to be made available and  preloaded for use in variable transformation functions, e.g., those explicitly defined in **RevoScaleR** functions via their `transforms` and `transformFunc` arguments or those  defined implicitly via their `formula` or `rowSelection` arguments.  The `transformPackages` argument may also be `NULL`,  indicating that no packages outside `rxGetOption("transformPackages")` will be preloaded. 
  
  
    
 ### `transformEnvir`
 A user-defined environment to serve as a parent to  all environments developed internally and used for variable data transformation. If `transformEnvir = NULL`, a new "hash" environment with parent `baseenv()` is used instead. 
  
  
    
 ### `append`
 Either `"none"` to create a new files, `"rows"` to append rows to an existing file, or `"cols"` to append columns to an existing file. If `outFile` exists and `append` is `"none"`,  the `overwrite` argument must be set to `TRUE`. Ignored for data frames. You cannot append to [RxTextData](RxTextData.md) or append columns to [RxTeradata](RxTeradata.md) data sources,  and appending is not supported for composite .xdf files or when using the [RxHadoopMR](RxHadoopMR.md) compute context. 
  
  
    
 ### `overwrite`
 A logical value. If `TRUE`, an existing `outFile` will be overwritten, or if appending columns existing columns with the same name will be overwritten. `overwrite` is ignored if appending rows. Ignored for data frames. 
  
  
    
 ### `rowVarName`
 A character string or `NULL`. If `inData` is a `data.frame`: If `NULL`, the data frame's row names will be dropped. If a character string, an additional variable of that name will be added to the data set containing the data frame's row names. If a `data.frame` is being returned, a variable with the name `rowVarName` will be removed as a column from the data frame and will be used as the row names.  
  
  
    
 ### `removeMissingsOnRead`
 A logical value. If `TRUE`, rows with missing values will be removed on read. 
  
  
    
 ### `removeMissings`
 A logical value. If `TRUE`, rows with missing values will not be included in the output data. 
  
  
    
 ### `computeLowHigh`
 A logical value. If `FALSE`, low and high values will not automatically be computed. This should only be set to `FALSE` in special circumstances, such as when `append` is being used repeatedly. Ignored for data frames. 
  
  
  
 ### `maxRowsByCols`
 The maximum size of a data frame that will be returned if `outFile` is set to `NULL` and `inData` is an .xdf file , measured by the number of rows times the number of columns. If the number of rows times the number of columns being created from the .xdf file exceeds this, a warning will be reported and the number of rows in the returned data frame will be truncated. If `maxRowsByCols` is set to be too large, you may experience problems  from loading a huge data frame into memory. 
  
  
    
 ### `rowsPerRead`
 The number of rows to read for each chunk of data read from the input data source. Use this argument for finer control of the number of rows per block in the output data source. If greater than 0, `blocksPerRead` is ignored. Cannot be used if `inFile` is the same as `outFile`. The default value of `-1` specifies that data should be read by blocks according to the `blocksPerRead` argument. 
  
  
    
 ### `startRow`
 The starting row to read from the input data source.   Cannot be used if `inFile` is the same as `outFile`. 
   
  
    
 ### `numRows`
 The number of rows to read from the input data source.  If `rowSelection` or `removeMissings` are used, the output data set may have fewer rows than specified by `numRows`. Cannot be used  if `inFile` is the same as `outFile`. 
   
  
    
 ### `returnTransformObjects`
 A logical value. If `TRUE`,  the list of `transformObjects` will be returned instead of  a data frame or data source object.  If the input `transformObjects` have been modified, by using `.rxSet` or `.rxModify` in the `transformFunc`, the updated values will be returned.  Any data returned from the `transformFunc` is ignored. If no `transformObjects` are used, `NULL` is returned. This argument allows for user-defined  computations within a `transformFunc` without creating new data. `returnTransformObjects` is not supported in distributed compute contexts  such as [RxHadoopMR](RxHadoopMR.md). 
   
  
    
 ### `blocksPerRead`
 The number of blocks to read for each chunk of data read from the data source. Ignored for data frames or if `rowsPerRead` is positive. 
   
  
    
 ### `reportProgress`
 An integer value with options:  
*   `0`: no progress is reported. 
*   `1`: the number of processed rows is printed and updated. 
*   `2`: rows processed and timings are reported. 
*   `3`: rows processed and all timings are reported. 
  
  
  
     
 ### `xdfCompressionLevel`
 An integer in the range of -1 to 9.  The higher the value, the greater the  amount of compression - resulting in smaller files but a longer time to create them. If  `xdfCompressionLevel` is set to 0, there will be no compression and files will be compatible  with the 6.0 release of Revolution R Enterprise.  If set to -1, a default level of compression  will be used. 
   
  
    
 ### ` ...`
 Additional arguments to be passed to the input data source. If  `stringsAsFactors`is specified and the input data source is `RxXdfData`,  strings with be converted to factors when returning a data frame to R. 
    
 
 
 ##Value
 
For `rxDataStep`, if `returnTransformObjects` is `FALSE`)and an `outFile` 
is specified, an [RxXdfData](RxXdfData.md) data source is returned invisibly. If no
`outFile` is specified, a data frame is
returned invisibly.  Either can be used in subsequent RevoScaleR analysis.  
If `returnTransformObjects` 
is `TRUE`, the `transformObjects` list as modified by the transformation
function is returned invisibly.
When working with an [RxInSqlServer](RxInSqlServer.md) compute context, both the input
and output data sources must be [RxSqlServerData](RxSqlServerData.md).
 
 

 
 
 
 ##See Also
 
[rxImport](rxImport.md),
[RxXdfData](RxXdfData.md),
[RxTextData](RxTextData.md),
[RxSqlServerData](RxSqlServerData.md),
[RxTeradata](RxTeradata.md),
[rxGetInfo](rxGetInfoXdf.md),
[rxGetVarInfo](rxGetVarInfoXdf.md),
[rxTransform](rxTransform.md)
   
 ##Examples

 ```
   
  
  # Create a data frame
  set.seed(100)
  myData <- data.frame(x = 1:100, y = rep(c("a", "b", "c", "d"), 25),
      z = rnorm(100), w = runif(100))
      
  # Get a subset rows and columns from the data frame    
  myDataSubset <- rxDataStep(inData = myData,
      varsToKeep = c("x", "w", "z"), rowSelection = z > 0) 
  rxGetInfo(myDataSubset, getVarInfo = TRUE) 
  
  # Create a simple .xdf file from the data frame (for use below)
  inputFile <- file.path(tempdir(), "testInput.xdf")
  rxDataStep(inData = myData, outFile = inputFile, overwrite = TRUE)
  
  # Redo, creating a multi-block .xdf file from the data frame
  rxDataStep(inData = myData, outFile = inputFile, rowsPerRead = 50, 
      overwrite = TRUE)
  rxGetInfo( inputFile )
  
  # Subset rows and columns, creating a new .xdf file
  outputFile <- file.path(tempdir(), "testOutput.xdf")
  rxDataStep(inData = inputFile, outFile = outputFile,
      varsToKeep = c("x", "w", "z"), rowSelection = z > 0,
      overwrite = TRUE)
  rxGetInfo(outputFile)
  
  # Use transforms list with data frame input and output data
  # Add new columns
  myNewData <- rxDataStep(inData = myData, 
      transforms = list(a = w > 0, b = 100 * z))
  names(myNewData)
  
  # Use transformFunc to add new columns
  myXformFunc <- function(dataList) {
    dataList$b <- 100 * dataList$z
    return (dataList)
  }
  myNewData <- rxDataStep(inData = myData,
      transformFunc = myXformFunc)
  names(myNewData)
  
  # Use transforms to remove columns
  rxDataStep(inData = inputFile, outFile = outputFile,
      transforms = list(w = NULL), overwrite = TRUE)
  rxGetInfo(outputFile)
  
  # use transformFunc to remove columns
  xform <- function(dataList) {
    dataList$w <- NULL
    return (dataList)
  }
  rxDataStep(inData = inputFile, outFile = outputFile,
      transformFunc = xform, overwrite = TRUE)
  rxGetInfo(outputFile)
  
  # use transform to change data type
  rxDataStep(inData = inputFile, outFile = outputFile, transformVars = c("x", "y"),
      transforms = list(x = as.numeric(x), y = as.numeric(y)), overwrite = TRUE)
  rxGetVarInfo(outputFile)
  
  # use transformFunc to change data type
  myXform <- function(dataList) {
    dataList <- lapply(dataList, as.numeric)
    return (dataList)
  }
  rxDataStep(inData = inputFile, outFile = outputFile,
      transformFunc = myXform, overwrite = TRUE)
  rxGetVarInfo(outputFile)
  
  # use transform to create new data
  rxDataStep(inData = inputFile, outFile = outputFile,
      transforms = list(maxZ = max(z), minW = min(w), z = NULL, w = NULL),
      varsToDrop = c("x", "y"), overwrite = TRUE)
  rxGetVarInfo(outputFile)
  
  # use transformFunc to create new data
  xform <- function(dataList){
    outList <- list()
    outList$maxZ <- max(dataList$z)
    outList$minW <- min(dataList$w)
    return (outList)
  }
  rxDataStep(inData = inputFile, outFile = outputFile,
      transformFunc = xform, overwrite = TRUE)
  rxGetVarInfo(outputFile)
  
  # add new column by calling function in expression for "transform"
  # "transform" validate the expression at server, so function name should be passed to 
  # remote context. R looking up undefined variable in calling environment, dynamic scoping 
  # should be used to find "const" used in function "myTransform"
  const <- 10
  myTransform <- function(x){
    const <- get("const", parent.frame())
    x * const
  }
  rxDataStep(inData = inputFile, outFile = outputFile,
             transforms = list(x10 = myTransform(x)), transformObjects = list(myTransform = myTransform, const = const),
             overwrite = TRUE)
  rxGetVarInfo(outputFile)
  
  # using "transform" and "transformEnvir" to add new columns
  env <- new.env()
  env$constant <- 10
  env$myTransform <- function(x){
    x * constant
  }
  environment(env$myTransform) <- env
  data <- rxDataStep(inData=inputFile, outFile = outputFile,
      transforms = list(b = myTransform(x)), transformEnvir = env, overwrite = TRUE)
  rxGetVarInfo(outputFile)
  
  # Specify which variables to keep in a new data file
  varsToKeep <- c("x", "w", "z")
  rxDataStep(inData = inputFile, outFile = outputFile, varsToKeep = varsToKeep,
      transforms = list(a = w > 0, b = 100 * z), overwrite = TRUE)
  rxGetInfo(outputFile)
  
  # Alternatively specify which variables to drop
  varsToDrop <- "y"
  rxDataStep(inData = inputFile, outFile = outputFile, varsToDrop = varsToDrop, 
      transforms = list(a = w > 0, b = 100 * z), overwrite = TRUE)
  rxGetInfo(outputFile)
  
  # Read a specific number of rows of data into a data frame
  myData <- rxDataStep(inData = inputFile, startRow = 5, numRows = 10)
  rxGetInfo(myData)
  
  # Create a selection variable to take a roughly 25% random sample
  # of each block of data read (in this case 1).
  rxDataStep(inData = inputFile, outFile = outputFile, rowSelection = selVar,
      transforms = list(
          selVar = as.logical(rbinom(.rxNumRows, 1, .25))
      ), overwrite = TRUE)
  rxGetInfo(outputFile)
  
  # Create a new variable containing row numbers for a multi-block data file
  # Note that .rxStartRow and .rxNumRows are not supported in 
  # a distributed compute context such as RxHadoopMR
  myDataSource <- rxDataStep(inData = inputFile, outFile = outputFile,  
    transforms = list(
              rowNum = .rxStartRow : (.rxStartRow + .rxNumRows - 1)),
    overwrite = TRUE )
    
  # Use the returned data source object in another call to rxDataStep
  myMiddleData <- rxDataStep(inData = myDataSource, startRow = 55,
    numRows = 5)
  myMiddleData  
   
  # Create a new factor variable from a continuous variable using the cut function
  rxDataStep(inData = inputFile, outFile = outputFile,
    transforms = list( 
      wFactor = cut(w, breaks = c(0, .33, .66, 1), labels = c("Low", "Med", "High"))
      ), overwrite=TRUE)
  rxGetInfo( outputFile, numRows = 10 )
  
  # Create a new data set with simulated data.  A row number variable will
  # be automatically created.  It will have 20 rows in each block,
  # with a total of 100 rows
  #(This functionality not supported in distributed compute contexts.)
  outFile <- tempfile(pattern = ".rxTest1", fileext = ".xdf")
  newDS <- rxDataStep( outFile = outFile, numRows = 100, rowsPerRead = 20,
      transforms = list(
          x = (.rxRowNums - 50)/25,
          pnormx = pnorm(x),
          dnormx = dnorm(x))) 
  # Compute summary statistics on the new data file        
  rxSummary(~., newDS)
  file.remove(outFile)
  
  # Use the data step to chunk through the data and compute
  # the sum of a squared deviation
  inFile <- file.path(rxGetOption("unitTestDataDir"), "test100r5b.xdf")
  myFun <- function( dataList )
  {
      chunkSumDev <- sum((dataList$y - toMyCenter)^2,  na.rm = TRUE)
      toTotalSumDev <<- toTotalSumDev + chunkSumDev
      return(NULL)
  }
  myCenter <- 40
  newTransformVals <- rxDataStep(inData = inFile, 
      transformFunc = myFun,
      transformObjects = list( 
          toMyCenter = myCenter,
          toTotalSumDev = 0),
      transformVars = "y", returnTransformObjects = TRUE)
  
  newTransformVals[["toTotalSumDev"]]
  
  ## Not run:
 
# These examples need to be modified to substitute appropriate paths.
# Convert an xdf file to csv on HDFS
myData <- data.frame(textVar = c("a", "b", "c", "d"), intVar = as.integer(c(1:2, NA, 4)))
# write data frame to an xdf file in local file system
# test1.xdf will be written out to local file system in the current working directory
xdfFile <- "test1.xdf"
xdfDS <- RxXdfData(xdfFile)
rxDataStep(inData = myData, outFile = xdfDS, overwrite=TRUE)
# use RxTextData to write a csv file to HDFS
# /user/RevoShare/myuser is HDFS path that must exist for testCsv.csv to be written successfully.
csvFile <- "/user/RevoShare/myuser/testCsv.csv"
# We are writing a csv with no header (column names), no quotes and empty string for missing values (NA)
hdfsFS <- RxHdfsFileSystem()
myTextDS <- RxTextData(csvFile, missingValueString = "NA", firstRowIsColNames = FALSE, quoteMark = "", fileSystem=hdfsFS)
# This will write out a csv file to HDFS
rxDataStep(inData = xdfDS, outFile = myTextDS, overwrite = TRUE)

# Convert a composite xdf file to csv writing to HDFS using RxTextData.
# Create a test xdf file.
myData <- data.frame(textVar = c("a", "b", "c", "d"), intVar = as.integer(c(1:2, NA, 4)))
hdfsFS <- RxHdfsFileSystem()
# composite xdf file path in HDFS. Path "/user/RevoShare/myuser/test1" must exist.
xdfFile <- "/user/RevoShare/myuser/test1"
xdfDS <- RxXdfData(xdfFile, fileSystem = hdfsFS)
rxDataStep(inData = myData, outFile = xdfDS, overwrite=TRUE)
# use RxTextData to write a csv file to HDFS
# /user/RevoShare/myuser is HDFS path that must exist for testCsv.csv to be written successfully.
csvFile <- "/user/RevoShare/myuser/testCsv.csv"
# We are writing a csv with no header, no quotes and empty string for missing values (NA)
myTextDS <- RxTextData(csvFile, missingValueString = "", firstRowIsColNames = FALSE, quoteMark = "", fileSystem = hdfsFS)
# This will write out a csv file to HDFS
rxDataStep(inData = xdfDS, outFile = myTextDS, overwrite = TRUE)
 ## End(Not run) 
  
 
```
 
 
 
