--- 
 
# required metadata 
title: "Manage Cached Data in Spark" 
description: " Use these functions to manage the objects cached in the Spark memory system. These functions are only applicable when using [RxSpark](rxspark.md) compute context. " 
keywords: "RevoScaleR, rxSparkListData, rxSparkRemoveData {RevoScaleR}, rxSparkListData, rxSparkRemoveData" 
author: "heidisteen" 
manager: "jhubbard" 
ms.date: "04/18/2017" 
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
 
 
 
 #`rxSparkListData, rxSparkRemoveData {RevoScaleR}`: Manage Cached Data in Spark

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
Use these functions to manage the objects cached in the Spark memory system. These functions are only applicable when using [RxSpark](rxspark.md) compute context.
 
 
 ##Usage

```   
  rxSparkListData(showDescription=TRUE,
      computeContext = rxGetOption("computeContext"))
      
  rxSparkRemoveData(list,
      computeContext = rxGetOption("computeContext"))    
 
```
 
 
 ##Arguments

   
    
 ### `list`
 list of cached objects need to be deleted. 
  
    
 ### `showDescription`
 logical indicating whether or not to print out the detail to console. 
  
    
 ### `computeContext`
 `RxSpark` compute context object. 
  
 
 
 
 ##Value
 
List of all objects cached Spark memory system for `rxSparkListData`.

No return values for `rxSparkRemoveData`.
 
 
 ##Examples

 ```
   
  ## Not run:
 
cc <- rxSparkConnect()

colInfo = list( DayOfWeek = list(type = "factor"))
df <- RxParquetData(file = "/tmp/AirlineDemoSmall.parquet", colInfo = colInfo)

### example for rxSparkListData

## No object in list, because no algorithm has been run 
rxSparkListData()

rxLogit((ArrDelay>0) ~ CRSDepTime + DayOfWeek, data = df)

## After the first run, a Spark data object is added into the list
rxSparkListData()

### example for rxSparkRemoveData

## remove an object
rxSparkRemoveData(df)

## remove all cached objs
rxSparkRemoveData(rxSparkListData())

rxSparkDisconnect(cc)
 ## End(Not run) 
  
 
```
 
