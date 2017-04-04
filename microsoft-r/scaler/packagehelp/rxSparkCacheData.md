--- 
 
# required metadata 
title: " set cache value of data in Spark Compute Context, for RxXdfData, RxOrcData, RxParquetData or RxHiveData " 
description: " Use this function to set cache value of data in Spark Compute Context, for data types: `RxParquetData`, `RxHiveData`, `RxXdfData` and `RxOrcData`. " 
keywords: "RevoScaleR, rxSparkCacheData" 
author: "richcalaway" 
manager: "jhubbard" 
ms.date: "03/06/2017" 
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
 
 
 #`rxSparkCacheData`:  set cache value of data in Spark Compute Context, for RxXdfData, RxOrcData, RxParquetData or RxHiveData  
 ##Description
 
Use this function to set cache value of data in Spark Compute Context, for data types: `RxParquetData`, `RxHiveData`, `RxXdfData` and `RxOrcData`.
 
 
 ##Usage

```   
  rxSparkCacheData(data, cache = TRUE)
 
```
 
 
 ##Arguments

   
    
 ### `data`
 data need to be set cache value 
  
    
 ### `cache`
 logical cacheValue want to be turned to 
  
 
 
 
 ##Value
 
data object with new cache value
 
 
 
 
 ##See Also
 
rxSparkListData, rxSparkRemoveData, RxHiveData, RxParquetData, RxOrcData
   
 ##Examples

 ```
   
  ## Not run:
 
  hiveData <- RxHiveData(table = "table")
  ## set cache value to TRUE
  hiveData <- rxSparkCacheData(hiveData)
  ## set cache value to FASLE
  hiveData <- rxSparkCacheData(hiveData, FALSE)
 ## End(Not run) 
  
 
```
 
