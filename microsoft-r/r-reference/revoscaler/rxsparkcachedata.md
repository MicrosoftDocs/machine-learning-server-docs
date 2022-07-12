--- 

# required metadata 
title: "rxSparkCacheData {RevoScaleR} function (revoAnalytics) | Microsoft Docs" 
description: " Use this function to set the cache flag to control whether data objects should be cached in Spark memory system, applicable for RxXdfData, RxHiveData, RxParquetData, and RxOrcData.  " 
keywords: "(revoAnalytics), rxSparkCacheData {RevoScaleR}, rxSparkCacheData" 
author: "chuckheinzelman"
ms.author: "charlhe" 
manager: "cgronlun" 
ms.date: 07/15/2019
ms.topic: "reference" 
ms.prod: "mlserver" 
ms.service: "" 
ms.assetid: "" 

# optional metadata 
ROBOTS: "" 
audience: "" 
ms.devlang: "" 
ms.reviewer: "" 
ms.suite: "" 
ms.tgt_pltfrm: "" 
#ms.technology: "" 
ms.custom: "" 

--- 


 # rxSparkCacheData {RevoScaleR}:  Set the Cache Flag in Spark Compute Context  

 ## Description

Use this function to set the cache flag to control whether data objects should be cached in Spark memory system, applicable for `RxXdfData`, `RxHiveData`, `RxParquetData`, and `RxOrcData`. 



 ## Usage

```   
  rxSparkCacheData(data, cache = TRUE)

```


 ## Arguments



 ### `data`
 a data source that can be RxXdfData, RxHiveData, RxParquetData, or RxOrcData. RxTextData is currently not supported. 


 ### `cache`
 logical value controlling whether the data should be cached or not. If `TRUE`the data will be cached in the Spark memory system after the first use for performance enhancement. 




 ## Details

Note that `RxHiveData` with `saveAsTempTable=TRUE` is always cached in memory regardless of the `cache` parameter setting in this function.


 ## Value

data object with new cache flag. 


 ## See Also

[rxSparkListData](rxSparkDataOps.md), [rxSparkRemoveData](rxSparkDataOps.md), [RxXdfData](RxXdfData.md), [RxHiveData](RxSparkData.md), [RxParquetData](RxSparkData.md), [RxOrcData](RxSparkData.md).

 ## Examples

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

