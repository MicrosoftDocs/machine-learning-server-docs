--- 
 
# required metadata 
title: "Remove or list cached RxParquetData or RxHiveData" 
description: " Use these functions to remove or list all cached `RxParquetData` and `RxHiveData` objects cached in the Spark memory. " 
keywords: "RevoScaleR, rxSparkRemoveData, rxSparkListData" 
author: "richcalaway" 
manager: "jhubbard" 
ms.date: "03/23/2017" 
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
 
 
 
 #`rxSparkRemoveData`: Remove or list cached RxParquetData or RxHiveData

 Applies to version {PRODUCT_VERSION} of package RevoScaleR.
 
 ##Description
 
Use these functions to remove or list all cached `RxParquetData`
and `RxHiveData` objects cached in the Spark memory.
 
 
 ##Usage

```   
  rxSparkRemoveData(list,
      computeContext = rxGetOption("computeContext"))
  
  rxSparkListData(showDescription=TRUE,
      computeContext = rxGetOption("computeContext"))
 
```
 
 
 ##Arguments

   
    
 ### `list`
 list of RxSparkData objs need to be deleted. 
  
    
 ### `computeContext`
 an `RxSpark` compute context object 
  
    
 ### `showDescription`
 logical indicating whether or not to print out the detail to console. 
  
 
 
 
 ##Value
 
List of all cached RxParquetData and RxHiveData.
 
 
 ##Examples

 ```
   
  ## Not run:
 
### 
colInfo = list( DayOfWeek = list(type = "factor"))
myHadoopCluster <- rxSparkConnect(consoleOutput = TRUE, executorCores = 2, executorMem = "1g", driverMem = "1g", executorOverheadMem = "1g", numExecutors = 2)
df <- RxParquetData(parquetPath = "/tmp/AirlineDemoSmall.parquet", colInfo = colInfo, cache = TRUE)
df2 <- RxParquetData(parquetPath = "/tmp/AirlineDemoSmall.parquet", colInfo = colInfo, cache = TRUE)

###example for rxSparkListData

## No object in list, because no algorithm has been run 
rxSparkListData()

rxLogit((ArrDelay>0) ~ CRSDepTime + DayOfWeek, data = df)

## After the first run, now a Spark data object is added into the list
rxSparkListData()


### example for rxSparkRemoveData

## remove one dataframe obj
rxSparkRemoveData(df2)

## remove a list of dataframe obj
rxSparkRemoveData(rxSparkListData())

rxSparkDisconnect(myHadoopCluster)
 ## End(Not run) 
  
 
```
 
