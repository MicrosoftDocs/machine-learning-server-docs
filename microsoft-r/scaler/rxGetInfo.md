---

# required metadata
title: "rxGetInfo ScaleR Function (RevoScaleR package)"
description: "rxGetInfo function in the RevoScaleR package in Microsoft R."
keywords: "RevoScaleR, ScaleR, rxGetInfo"
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "08/13/2016"
ms.topic: "article"
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

#rxGetInfo function (RevoScaleR)
Get information about an .xdf file or data frame.

## Usage
~~~~
rxGetInfo(data, getVarInfo = FALSE, getBlockSizes = FALSE,
          getValueLabels = NULL, varsToKeep = NULL, varsToDrop = NULL,
          startRow = 1, numRows = 0, computeInfo = FALSE,
          allNodes = TRUE, verbose = 0)

rxGetInfoXdf(file, getVarInfo = FALSE, getBlockSizes = FALSE,
             getValueLabels = NULL, varsToKeep = NULL, varsToDrop = NULL,
             startRow = 1, numRows = 0, computeInfo = FALSE, verbose = 0)
~~~~

## Argument

The following table shows the arguments to rxGetInfo in order and their default values.

|Parameter | Description|
| --------- | --------- |
|data |A data frame, a character string specifying an .xdf, or an [RxXdfData](RxXdfData.md) object. If a local compute context is being used, this argument may also be a list of data sources, in which case the output will be returned in a named list. See the Remarks for more information. |
|file |Either an [RxXdfData](RxXdfData.md) object or a character string specifying the .xdf file. If a local compute context is being used, this argument may also be a list of data sources, in which case the output will be returned in a named list. See the details section for more information.|
|getVarInfo |Logical value. If `TRUE`, variable information is returned.|
|getBlockSizes |Logical value. If `TRUE`, block sizes are returned in the output for an .xdf file, and when printed the first 10 block sizes are shown.|
|getValueLabels |Logical value. If `TRUE` and `getVarInfo` is `TRUE` or `NULL`, factor value labels are included in the output.|
|varsToKeep |character vector of variable names for which information is returned. If `NULL` or `getVarInfo` is `FALSE`, argument is ignored. Cannot be used with `varsToDrop`.|
|varsToDrop |Character vector of variable names for which information is not returned. If `NULL` or `getVarInfo` is `FALSE`, argument is ignored. Cannot be used with `varsToKeep`.|
|startRow |The starting row for retrieval of data if a data frame or .xdf file.|
|numRows |The number of rows of data to retrieve if a data frame or .xdf file.|
|computeInfo |Logical value. If `TRUE`, and `getVarInfo` is `TRUE`, variable information (e.g., high/low values) for non-xdf data sources will be computed by reading through the data set. If `TRUE`, and `getVarInfo` is FALSE, the number of variables will be gotten from non-xdf data sources (but not the number of rows).|
|allNodes |Logical value. Ignored if the active [RxComputeContext](RxComputeContext.md) compute context is local or RxForeachDoPar. Otherwise, if `TRUE`, a list containing the information for the data set on each node in the active compute context will be returned. If `FALSE`, only information on the data set on the master node will be returned. Note that the determination of the master node is not controlled by the end user. See [Distributed Computing](scaler-distributed-computing.md) for more information on master node computations. |
|verbose |Integer value. If 0, no additional output is printed. If 1, additional summary information is printed for an .xdf file.|

## Remarks
If a local compute context is being used, the data and file arguments may be a list of data source objects, e.g., `data = list(iris, airquality, attitude)`, in which case a named list of results are returned. For rxGetInfo, a mix of supported data sources is allowed. However, for rxGetInfoXdf, the file argument (specified as a list) must be comprised only of .xdf file paths and/or [RxXdfData](RxXdfData.md) source objects. Note that data frames should not be specified in quotes because, in that case, they will be interpreted as .xdf data paths.

If the RxComputeContext is distributed, rxGetInfo will request information from the compute context nodes. rxGetInfoXdf will only access local files.

## Return Value

A list containing the following possible elements:

|Element | Description|
|--------| ----------|
|fileName |Character string containing the file name and path (if an .xdf file).|
|objName |Character string containing object name (if not an .xdf file).|
|class |The class of the object if an R object.|
|length |The length of the object if it is not an .xdf file or data frame.|
|numCompositeFiles |The number of composite data files(if a composite .xdf file).|
|numRows |The number of rows in the data set.|
|numVars |The number of variables in the data set.|
|numBlocks |The number of blocks in the data set.|
|varInfo |A list of variable information where each element is a list describing a variable. (See return value of rxGetVarInfo for more information.)|
|rowsPerBlock |Integer vector containing number of rows in each block (if `getBlockSizes` is set to `TRUE`). Set to `NULL` if data is a data frame.|
|data |Data frame containing the data (if `numRows > 0`).|

If using a distributed compute context with the `allNodes` set to `TRUE`, a list of lists with information on the data from each node will be returned.

## Examples
~~~~
# Summary information about a data frame
fileInfo <- rxGetInfo(data = iris, numRows = 5)
fileInfo

# Summary information about an .xdf file
mortFile <- file.path(rxGetOption("sampleDataDir"), "CensusWorkers")
infoObj <- rxGetInfo(mortFile, getBlockSizes=TRUE)
numBlocks <- infoObj$numBlocks
aveRowsPerBlock <- infoObj$numRows/numBlocks
rowsPerBlock <- infoObj$rowsPerBlock
aveRowsPerBlock1 <- mean(rowsPerBlock)

# Obtain information on a variety of data sources
fourthGradersXDF <- file.path(rxGetOption("sampleDataDir"), "fourthgraders.xdf")
KyphosisDS <- RxXdfData(file.path(rxGetOption("sampleDataDir"), "Kyphosis.xdf"))
rxGetInfo(data = list(iris, fourthGradersXDF, KyphosisDS))
~~~~

## See Also

[ScaleR Functions (RevoScaleR package)](scaler.md)

[Comparison of rx Functions and CRAN R Functions](compare-base-r-scaler-functions.md)
