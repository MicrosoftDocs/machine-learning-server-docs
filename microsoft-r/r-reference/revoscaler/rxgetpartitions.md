--- 
 
# required metadata 
title: "rxGetPartitions function (revoAnalytics) | Microsoft Docs" 
description: " Get partitions enumeration of a partitioned Xdf data source. " 
keywords: "(revoAnalytics), rxGetPartitions, GetPartitions" 
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
 
 
 
 
 #rxGetPartitions:  Get Partitions of a Partitioned Xdf Data Source  
 
 ##Description
 
Get partitions enumeration of a partitioned Xdf data source.
 
 
 ##Usage

```   
  rxGetPartitions(data)
 
```
 
 
 ##Arguments

   
    
 ### `data`
 an existing partitioned data source object which was created by [RxXdfData](RxXdfData.md) with `createPartitionSet = TRUE` and constructed by [rxPartition](rxPartition.rd.md). 
  
 
 
 ##Details
 

 
 
 
 ##Value
 
Data frame with `(n+1)` columns, the first `n` columns are partitioning columns specified by `varsToPartition` in [rxPartition](rxPartition.rd.md) and the `(n+1)th` column is a data source column where each row contains an Xdf data source object of one partition.
 
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 
 ##See Also
 
RXdfData,
[rxPartition](rxPartition.rd.md),
[rxExecBy](rxExecBy.rd.md)
   
 
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
 
