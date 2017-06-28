--- 
 
# required metadata 
title: " Set the Cache Flag in Spark Compute Context " 
description: " Use this function to set the cache flag to control whether data objects should be cached in Spark memory system, applicable for `RxXdfData`, `RxHiveData`, `RxParquetData`, and `RxOrcData`.  " 
keywords: "RevoScaleR, rxSparkCacheData {RevoScaleR}, rxSparkCacheData" 
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
 
 
 #`rxSparkCacheData {RevoScaleR}`:  Set the Cache Flag in Spark Compute Context 

 Applies to version 9.1.0 of package RevoScaleR.
 
 
 ##Description
 
Use this function to set the cache flag to control whether data objects should be cached in Spark memory system, applicable for `RxXdfData`, `RxHiveData`, `RxParquetData`, and `RxOrcData`. 
 
 
 
 ##Usage

```   
  rxSparkCacheData(data, cache = TRUE)
 
```
 
 
 ##Arguments

   
    
 ### `data`
 a data source that can be RxXdfData, RxHiveData, RxParquetData, or RxOrcData. RxTextData is currently not supported. 
  
    
 ### `cache`
 logical value controlling whether the data should be cached or not. If `TRUE`the data will be cached in the Spark memory system after the first use for performance enhancement. 
  
 
 
 
 ##Details
 
Note that `RxHiveData` with `saveAsTempTable=TRUE` is always cached in memory regardless of the `cache` parameter setting in this function.
 
 
 ##Value
 
data object with new cache flag. 
 
 
 ##See Also
 
[rxSparkListData](../../scaler/packagehelp/rxsparkdataops.md), [rxSparkRemoveData](../../scaler/packagehelp/rxsparkdataops.md), [RxXdfData](../../scaler/packagehelp/rxxdfdata.md), [RxHiveData](../../scaler/packagehelp/rxsparkdata.md), [RxParquetData](../../scaler/packagehelp/rxsparkdata.md), [RxOrcData](../../scaler/packagehelp/rxsparkdata.md).
   
 ##Examples

 ```
   
  ## Not run:
 
  hiveData <- RxHiveData(table = "mytable")
  
  ## this does not work. The cache flag of the input hiveData is not changed, so it is still FALSE.
  rxSparkCacheData(hiveData)
  rxImport(hiveData) # data is not cached
  
  ## this works. The cache flag of hiveData is now set to TRUE.
  hiveData <- rxSparkCacheData(hiveData)  
  rxImport(hiveData) # data is now cached
  
  ## set cache value to FASLE
  hiveData <- rxSparkCacheData(hiveData, FALSE)
 ## End(Not run) 
  
 
```
 
