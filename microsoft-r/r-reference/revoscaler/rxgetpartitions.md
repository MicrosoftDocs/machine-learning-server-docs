--- 
 
# required metadata 
title: "rxGetPartitions function (RevoScaleR) " 
description: " Get partitions enumeration of a partitioned Xdf data source. " 
keywords: "(RevoScaleR), rxGetPartitions, GetPartitions" 
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
 
 
 
 
 #rxGetPartitions:  Get Partitions of a Partitioned Xdf Data Source  
 
 ##Description
 
Get partitions enumeration of a partitioned Xdf data source.
 
 
 ##Usage

```   
  rxGetPartitions(data)
 
```
 
 
 ##Arguments

   
    
 ### `data`
 an existing partitioned data source object which was created by [RxXdfData](RxXdfData.md) with `createPartitionSet = TRUE` and constructed by rxPartition. 
  
 
 
 ##Details
 

 
 
 
 ##Value
 
Data frame with `(n+1)` columns, the first `n` columns are partitioning columns specified by `varsToPartition` in rxPartition and the `(n+1)th` column is a data source column where each row contains an Xdf data source object of one partition.
 
 

 
 
 
 
 ##See Also
 
RXdfData,
rxPartition,
rxExecBy
   
 
 ##Examples

 ```
   
    # create an input Xdf data source
    inFile <- "claims.xdf"
    inFile <- file.path(dataPath = rxGetOption(opt = "sampleDataDir"), inFile)
    inXdfDS <- RxXdfData(file = inFile)
  
    # create an output partitioned Xdf data source
    outFile <- file.path(tempdir(), "partitionedClaims.xdf")
    outPartXdfDataSource <- RxXdfData(file = outFile, createPartitionSet = TRUE)
  
    # construct and save the partitioned Xdf to disk
    partDF <- rxPartition(inData = inXdfDS, outData = outPartXdfDataSource, varsToPartition = c("car.age"))
    
    # enumerate partitions
    partDF <- rxGetPartitions(outPartXdfDataSource)
    rxGetInfo(partDF, getVarInfo = TRUE)
    partDF$DataSource
 
```
 
