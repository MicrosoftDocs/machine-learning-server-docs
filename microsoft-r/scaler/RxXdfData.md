---

# required metadata
title: "RxXdfData ScaleR Function (RevoScaleR package)"
description: "RxXdfData function in the RevoScaleR package in Microsoft R."
keywords: "RevoScaleR, ScaleR, RxXdfData"
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

#RxXdfData function (RevoScaleR)
This is the main generator for S4 class RxXdfData, which extends [RxDataSource](RxDataSource.md).

## Usage
~~~~
RxXdfData(file, varsToKeep = NULL, varsToDrop = NULL, returnDataFrame = TRUE,
        stringsAsFactors = FALSE, blocksPerRead = rxGetOption("blocksPerRead"),
        fileSystem = NULL, createCompositeSet = NULL,
        blocksPerCompositeFile = 3)

## S3 method for class 'RxXdfData'
head(x, n = 6L, reportProgress = 0L, ...)

## S3 method for class 'RxXdfData'
summary(object, ...)

## S3 method for class 'RxXdfData'
tail(x, n = 6L, addrownums = TRUE, reportProgress = 0L, ...)
~~~~

## Argument

The following table shows the arguments to RxXdfData in order and their default values.

|Parameter | Description|
| --------- | --------- |
|file|Character string specifying the .xdf file.|
|varsToKeep|Character vector of variable names to keep around during operations. If `NULL`, argument is ignored. Cannot be used with `varsToDrop`.|
|varsToDrop|Character vector of variable names to drop from operations. If `NULL`, argument is ignored. Cannot be used with `varsToKeep`.|
|returnDataFrame|Logical indicating whether or not to convert the result to a data frame when reading with [rxReadNext](rxReadNext.md). If `FALSE`, a list is returned when reading with rxReadNext.|
|stringsAsFactors|Logical indicating whether or not to convert strings into factors in R (for reader mode only).|
|blocksPerRead|Number of blocks to read for each chunk of data read from the data source.|
|fileSystem|Character string or [RxFileSystem](RxFileSystem.md) object indicating type of file system; "native" or [RxNativeFileSystem](RxNativeFileSystem.md) object can be used for the local operating system, or an [RxHdfsFileSystem](RxHdfsFileSystem.md) object for the Hadoop file system. If `NULL`, the file system will be set to that in the current compute context, if available, otherwise the `fileSystem` option.|
|createCompositeSet|Logical value or NULL. Used only when writing. If `TRUE`, a composite set of files will be created instead of a single .xdf file. A directory will be created whose name is the same as the .xdf file that would otherwise be created, but with no extension. Subdirectories ‘data’ and ‘metadata’ will be created. In the ‘data’ subdirectory, the data will be split across a set of .xdfd files (see `blocksPerCompositeFile` below for determining how many blocks of data will be in each file). In the ‘metadata’ subdirectory there is a single .xdfm file, which contains the meta data for all of the .xdfd files in the ‘data’ subdirectory. When the compute context is [RxHadoopMR](RxHadoopMR.md) a composite set of files is always created.|
|blocksPerCompositeFile|Integer value. If `createCompositeSet=TRUE`, and if the compute context is not RxHadoopMR, this will be the number of blocks put into each .xdfd file in the composite set. When importing is being done on Hadoop using MapReduce, the number of rows per .xdfd file is determined by the rows assigned to each MapReduce task, and the number of blocks per .xdfd file is therefore determined by `rowsPerRead`.|
|x|An RxXdfData object.|
|object|An RxXdfData object.|
|n|Positive integer. Number of rows of the data set to extract.|
|addrownums|Logical. If `TRUE`, row numbers will be created to match the original data set.|
|reportProgress|Integer value with options:<br /><br />- 0: no progress is reported.<br />- 1: the number of processed rows is printed and updated.<br />- 2: rows processed and timings are reported.<br />- 3: rows processed and all timings are reported. |
|...| Arguments to be passed to underlying functions.|

## Return Value
Object of class RxXdfData.

## Examples
~~~~
myDataSource <- RxXdfData(file.path(rxGetOption("sampleDataDir"), "claims"))
# both of these should return TRUE
is(myDataSource, "RxXdfData")
is(myDataSource, "RxDataSource")

names(myDataSource)

modelFormula <- formula(myDataSource, depVars = "cost", varsToDrop = "RowNum")
~~~~

## See Also

[ScaleR Functions (RevoScaleR package)](scaler.md)

[Comparison of rx Functions and CRAN R Functions](compare-base-r-scaler-functions.md)
