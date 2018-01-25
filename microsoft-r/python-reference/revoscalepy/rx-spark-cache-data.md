--- 
 
# required metadata 
title: "rx_spark_cache_data: Set the Cache Flag in Spark Compute Context" 
description: "Use this function to set the cache flag to control whether data objects should be cached in Spark  memory system, applicable for RxXdfData, RxHiveData, RxParquetData, RxOrcData and RxSparkDataFrame." 
keywords: "spark, data, cache" 
author: "heidist" 
manager: "cgronlun" 
ms.date: "01/19/2018" 
ms.topic: "reference" 
ms.prod: "microsoft-r" 
ms.service: "" 
ms.assetid: "" 
 
# optional metadata 
ROBOTS: "" 
audience: "" 
ms.devlang: "Python" 
ms.reviewer: "" 
ms.suite: "" 
ms.tgt_pltfrm: "" 
ms.technology: "r-server" 
ms.custom: "" 
 
---

# rx_spark_cache_data


 


## Usage



```
revoscalepy.rx_spark_cache_data(in_data: revoscalepy.datasource.RxDataSource.RxDataSource,
    cache: bool)
```





## Description

Use this function to set the cache flag to control whether data objects should be cached in Spark
    memory system, applicable for RxXdfData, RxHiveData, RxParquetData, RxOrcData and RxSparkDataFrame.


## Arguments


### in_data

A data source that can be RxXdfData, RxHiveData, RxParquetData, RxOrcData or RxSparkDataFrame.
RxTextData is currently not supported.


### cache

Bool value controlling whether the data should be cached or not. If TRUE the data will
be cached in the Spark memory system after the first use for performance enhancement.


## Returns

Data object with new cache flag.


## Example



```
from revoscalepy import RxHiveData, rx_spark_connect, rx_spark_cache_data
hive_data = RxHiveData(table = "mytable")

# The cache flag of hiveData is now set to TRUE.
hive_data = rx_spark_cache_data(hive_data, True)

# set cache value to False
hive_data = rx_spark_cache_data(hive_data, False)
```

