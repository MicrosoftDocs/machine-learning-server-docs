--- 
 
# required metadata 
title: "rxPartition function (revoAnalytics) | Microsoft Docs" 
description: " Partition input data sources by key values and save the results to a partitioned Xdf on disk. " 
keywords: "(revoAnalytics), rxPartition, Partition" 
author: "heidisteen" 
manager: "cgronlun" 
ms.date: "01/24/2018" 
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
 
 
 
 #rxPartition: Partition Data by Key Values and Save the results to a Partitioned .Xdf 
 
 ##Description
 
Partition input data sources by key values and save the results to a partitioned Xdf on disk.
 
 
 
 ##Usage

```   
  rxPartition(inData, outData, varsToPartition, append = "rows", overwrite = FALSE, ...)
 
```
 
 
 ##Arguments

   
    
 ### `inData`
 either a data source object, a character string specifying a .xdf file, or a data frame object. 
  
  
    
 ### `outData`
 a partitioned data source object created by [RxXdfData](RxXdfData.md) with `createPartitionSet = TRUE`. 
  
  
    
 ### `varsToPartition`
 character vector of variable names to specify the values in those variables to be used for partitioning 
  
  
    
 ### `append`
 either "none" to create a new files or "rows" to append rows to an existing file. If outData exists and append is "none", the `overwrite` argument must be set to `TRUE`. 
  
  
    
 ### `overwrite`
 logical value. If `TRUE`, an existing `outData` will be overwritten. `overwrite` is ignored if `append = "rows"`. 
  
  
    
 ### ` ...`
 additional arguments to be passed directly to the Revolution Compute Engine. 
  
 
 
 
 ##Value
 
a data frame of partitioning values and data sources, each row in the data frame represents one partition and the data source in the last variable holds the data of a specifict partition.
 
 
 ##Note
 
In the current version, this function is single threaded.
 
 
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 
 ##See Also
 
[rxExecBy](rxExecBy.rd.md),
[RxXdfData](RxXdfData.md)
   
 ##Examples

 ```
   
  
  ##############################################################################
  # Construct a partitioned Xdf
  ##############################################################################
  
    # create an input Xdf data source
    inFile <- "claims.xdf"
    inFile <- file.path(dataPath = rxGetOption(opt = "sampleDataDir"), inFile)
    inXdfDS <- RxXdfData(file = inFile)
  
    # create an output partitioned Xdf data source
    outFile <- file.path(tempdir(), "partitionedClaims.xdf")
    outPartXdfDataSource <- RxXdfData(file = outFile, createPartitionSet = TRUE)
  
    # construct and save the partitioned Xdf to disk
    partDF <- rxPartition(inData = inXdfDS, outData = outPartXdfDataSource, varsToPartition = c("car.age"))
  
  ##############################################################################
  # Append new data to an existing partitioned Xdf
  ##############################################################################
  
    # create two sets of data frames from Xdf data source
    inFile <- "claims.xdf"
    inFile <- file.path(dataPath = rxGetOption(opt = "sampleDataDir"), inFile)
    inXdfDS <- RxXdfData(file = inFile)
    inDF <- rxImport(inData = inXdfDS)
  
    df1 <- inDF[1:50,]
    df2 <- inDF[51:nrow(inDF),]
  
    # create an output partitioned Xdf data source
    outFile <- file.path(tempdir(), "partitionedClaims.xdf")
    outPartXdfDataSource <- RxXdfData(file = outFile, createPartitionSet = TRUE)
  
    # construct the partitioned Xdf from the first data set df1 and save to disk
    partDF1 <- rxPartition(inData = df1, outData = outPartXdfDataSource, varsToPartition = c("car.age", "type"), append = "none", overwrite = TRUE)
  
    # append data from the second data set to the existing partitioned Xdf
    partDF2 <- rxPartition(inData = df2, outData = outPartXdfDataSource, varsToPartition = c("car.age", "type"))
  
    # overwrite an existing partitioned Xdf
    partDF2 <- rxPartition(inData = inXdfDS, outData = outPartXdfDataSource, varsToPartition = c("car.age"), append = "none", overwrite = TRUE)
 
```
 
