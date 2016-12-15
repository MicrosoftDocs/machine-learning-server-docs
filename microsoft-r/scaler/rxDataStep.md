---

# required metadata
title: "rxDataStep ScaleR Function (RevoScaleR package)"
description: "rxDataStep function in the RevoScaleR package in Microsoft R."
keywords: "RevoScaleR, ScaleR, rxDataStep"
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "11/03/2016"
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

#rxDataStep function (RevoScaleR)

Transform data from an input data set to an output data set.

## Usage

~~~~
rxDataStep(inData = NULL, outFile = NULL, varsToKeep = NULL, varsToDrop = NULL,
              rowSelection = NULL, transforms = NULL, transformObjects = NULL,
              transformFunc = NULL, transformVars = NULL,
              transformPackages = NULL, transformEnvir = NULL,
              append = "none", overwrite = FALSE, rowVarName = NULL,
              removeMissingsOnRead = FALSE, removeMissings = FALSE,
              computeLowHigh = TRUE, maxRowsByCols = 3000000,
              rowsPerRead = -1, startRow = 1, numRows = -1, returnTransformObjects = FALSE,
              blocksPerRead = rxGetOption("blocksPerRead"),
              reportProgress = rxGetOption("reportProgress"),
              xdfCompressionLevel = rxGetOption("xdfCompressionLevel"), ...)
~~~~

~~~~
rxDataStepXdf(inFile, outFile, varsToKeep = NULL, varsToDrop = NULL,
              rowSelection = NULL, transforms = NULL, transformObjects = NULL,
              transformFunc = NULL, transformVars = NULL,
              transformPackages = NULL, transformEnvir = NULL,
              append = "none", overwrite = FALSE, removeMissingsOnRead = FALSE, removeMissings = FALSE,
              computeLowHigh = TRUE, maxRowsByCols = 3000000,
              rowsPerRead = -1, startRow = 1, numRows = -1,
              startBlock = 1, numBlocks = -1, returnTransformObjects = FALSE,
              inSourceArgs = NULL, blocksPerRead = rxGetOption("blocksPerRead"),
              reportProgress = rxGetOption("reportProgress"),
              xdfCompressionLevel = rxGetOption("xdfCompressionLevel"),
              checkVarsToKeep = TRUE, cppInterp = NULL)
~~~~       

## Arguments

The following table shows the arguments to RxDataStep in order and their default values.

|Parameter | Description|
| --------- | --------- |
|inData |An [RxXdfData](RxXdfData.md) or [RxTeradata](RxTeradata.md) data source object. If not using a distributed compute context such as [RxHadoopMR](RxHadoopMR.md), a data frame, a character string specifying the input ‘.xdf’ file, or `NULL` can also be used. Other data sources are supported as experimental. If `NULL`, a data set will be created automatically with a single variable, `.rxRowNums`, containing row numbers. It will have a total of `numRows` rows with `rowsPerRead` rows in each block.|
|inFile |Either an [RxXdfData](RxXdfData.md) object or a character string specifying the input ‘.xdf’ file.|
|outFile |A character string specifying the output ‘.xdf’ file, an [RxXdfData](RxXdfData.md) object, a [RxOdbcData](RxOdbcData.md) data source, or a [RxTeradata](RxTeradata.md) data source. If `NULL`, a data frame will be returned from rxDataStep unless `returnTransformObjects` is set to `TRUE`. Setting `outFile` to `NULL` and `returnTransformObjects=TRUE` allows chunkwise computations on the data without modifying the existing data or creating a new data set. `outFile` can also be a delimited [RxTextData](RxTextData.md) data source if using a native file system and not appending.|
|varsToKeep |Character vector of variable names to include when reading from the input data file. If `NULL`, argument is ignored. Cannot be used with `varsToDrop` or when `outFile` is the same as the input data file. Variables used in transformations or row selection will be retained even if not specified in `varsToKeep`. If `newName` is used in `colInfo` in a non-xdf data source, the `newName` should be used in `varsToKeep`. Not supported for [RxTeradata](RxTeradata.md), [RxOdbcData](RxOdbcData.md) , or [RxSqlServerData](RxSqlServerData.md) data sources.|
|varsToDrop |Character vector of variable names to exclude when reading from the input data file. If `NULL`, argument is ignored. Cannot be used with `varsToKeep` or when `outFile` is the same as the input data file. Variables used in transformations or row selection will be retained even if specified in `varsToDrop`. If `newName` is used in `colInfo` in a non-xdf data source, the `newName` should be used in `varsToDrop`. Not supported for [RxTeradata](RxTeradata.md), [RxOdbcData](RxOdbcData.md) , or [RxSqlServerData](RxSqlServerData.md)  data sources.|
|rowSelection |Name of a logical variable in the data set or a logical expression using variables in the data set to specify row selection. For example, `rowSelection = old` will use only observations in which the value of the variable old is `TRUE`. `rowSelection = (age > 20) & (age < 65) & (log(income) > 10)` will use only observations in which the value of the age variable is between 20 and 65 and the value of the log of the income variable is greater than 10. The row selection is performed after processing any data transformations (see the arguments transforms or `transformFunc`). As with all expressions, `rowSelection` can be defined outside of the function call using the expression function.|
|transforms |An expression of the form `list(name = expression, ...)` representing the first round of variable transformations. As with all expressions, transforms (or `rowSelection`) can be defined outside of the function call using the expression function.|
|transformObjects |A named list containing objects that can be referenced by `transforms`, `transformsFunc`, and `rowSelection`. |
|transformFunc |Variable transformation function. See [rxTransform function](rxTransform.md) for details.|
|transformVars |Character vector of input data set variables needed for the transformation function. See [rxTransform function](rxTransform.md) for details.|
|transformPackages |Character vector defining additional R packages (outside of those specified in [rxGetOption](rxGetOption.md)("transformPackages")) to be made available and preloaded for use in variable transformation functions, e.g., those explicitly defined in RevoScaleR functions via their transforms and `transformFunc` arguments or those defined implicitly via their formula or `rowSelection` arguments. The `transformPackages` argument may also be `NULL`, indicating that no packages outside [rxGetOption](rxGetOption.md)("transformPackages") will be preloaded.|
|transformEnvir |User-defined environment to serve as a parent to all environments developed internally and used for variable data transformation. If `transformEnvir = NULL`, a new "hash" environment with parent `baseenv()` is used instead.|
|append |Either "none" to create a new files, "rows" to append rows to an existing file, or "cols" to append columns to an existing file. If `outFile` exists and append is "none", the overwrite argument must be set to `TRUE`. Ignored for data frames. You cannot append to [RxTextData](RxTextData.md) or [RxTeradata](RxTeradata.md) data sources, and appending is not supported for composite .xdf files or when using the [RxHadoopMR](RxHadoopMR.md) compute context.|
|overwrite |Logical value. If `TRUE`, an existing outFile will be overwritten, or if appending columns existing columns with the same name will be overwritten. overwrite is ignored if appending rows. Ignored for data frames.|
|rowVarName |Character string or `NULL`. Only used if `inData` is a `data.frame`. If `NULL`, the data frame's row names will be dropped. If a character string, an additional variable of that name will be added to the data set containing the data frame's row names. |
|removeMissingsOnRead |Logical value. If `TRUE`, rows with missing values will be removed on read.|
|removeMissings |Logical value. If `TRUE`, rows with missing values will not be included in the output data.|
|computeLowHigh |Logical value. If F`ALSE`, low and high values will not automatically be computed. This should only be set to `FALSE` in special circumstances, such as when append is being used repeatedly. Ignored for data frames.|
|maxRowsByCols |The maximum size of a data frame that will be returned if `outFile` is set to `NULL` and `inData` is an ‘.xdf’ file , measured by the number of rows times the number of columns. If the number of rows times the number of columns being created from the ‘.xdf’ file exceeds this, a warning will be reported and the number of rows in the returned data frame will be truncated. If `maxRowsByCols` is set to be too large, you may experience problems from loading a huge data frame into memory.|
|rowsPerRead |Number of rows to read for each chunk of data read from the input data source. Use this argument for finer control of the number of rows per block in the output data source. If greater than 0, `blocksPerRead` is ignored. Cannot be used if `inFile` is the same as `outFile`. The default value of -1 specifies that data should be read by blocks according to the `blocksPerRead` argument.|
|startRow |The starting row to read from the input data source. Cannot be used if `inFile` is the same as `outFile`.|
|numRows |Number of rows to read from the input data source. If `rowSelection` or `removeMissings` are used, the output data set may have fewer rows than specified by `numRows`. Cannot be used if `inFile` is the same as `outFile`.|
|returnTransformObjects |Logical value. If `TRUE`, the list of `transformObjects` will be returned instead of a data frame or data source object. If the input `transformObjects` have been modified, by using `.rxSet` or `.rxModify` in the `transformFunc`, the updated values will be returned. Any data returned from the `transformFunc` is ignored. If no `transformObjects` are used, `NULL` is returned. This argument allows for user-defined computations within a `transformFunc` without creating new data. `returnTransformObjects` is not supported in distributed compute contexts such as [RxHadoopMR](RxHadoopMR.md) or RxInTeradata.|
|startBlock |Starting block to read. Ignored if `startRow` is set to greater than 1. |
|numBlocks |Number of blocks to read; all are read if set to -1. Ignored if `numRows` is not set to -1. |
|blocksPerRead |Number of blocks to read for each chunk of data read from the data source. Ignored for data frames or if `rowsPerRead` is positive.|
|inSourceArgs |An optional list of arguments to be applied to the input data source.|
|reportProgress |Integer value with options: <br />- 0: no progress is reported. <br />- 1: the number of processed rows is printed and updated. <br />- 2: rows processed and timings are reported. <br />- 3: rows processed and all timings are reported. |
|xdfCompressionLevel |Integer in the range of -1 to 9. The higher the value, the greater the amount of compression - resulting in smaller files but a longer time to create them. If `xdfCompressionLevel` is set to 0, there will be no compression and files will be compatible with the 6.0 release of Revolution R Enterprise. If set to -1, a default level of compression will be used.|
|checkVarsToKeep |Logical value. If `TRUE` variable names specified in `varsToKeep` will be checked against variables in the data set to make sure they exist. An error will be reported if not found. Ignored if more than 500 variables in the data set.|
|cppInterp |NOT SUPPORTED information to be sent to the C++ interpreter.|
|...| Additional arguments to be passed to the input data source.|


## Return Value

For rxDataStep, if `returnTransformObjects` is `FALSE` and an outFile is specified, an [RxXdfData](RxXdfData.md) data source is returned invisibly. If no `outFile` is specified, a data frame is returned invisibly. Either can be used in subsequent RevoScaleR analysis. For both rxDataStep and rxDataStepXdf, if `returnTransformObjects` is `TRUE`, the `transformObjects` list as modified by the transformation function is returned invisibly. When working with an RxInTeradata compute context, both the input and output data sources must be RxTeradata.

## Examples

~~~~
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
myNewData <- rxDataStep(inData = myData,
    transforms = list(a = w > 0, b = 100 * z))
names(myNewData)

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
~~~~

## See Also

[ScaleR Functions (RevoScaleR package)](scaler.md)

[Comparison of rx Functions and CRAN R Functions](compare-base-r-scaler-functions.md)

[rxImport function](rxImport.md)

[RxXdfData function](RxXdfData.md)

[RxTextData function](RxTextData.md)

[RxTeradata function](RxTeradata.md)

[rxGetInfo function](rxGetInfo.md)

[rxTransform function](rxTransform.md)
