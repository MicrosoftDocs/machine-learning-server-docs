--- 
 
# required metadata 
title: "rx_get_pyspark_connection: Get pyspark spark_context connection from Spark compute context (revoscalepy)" 
description: "Gets a PySpark connection from the current Spark compute context." 
keywords: "" 
author: "DaniBunny"
ms.author: "dacoelho" 
manager: "cgronlun" 
ms.date: 07/15/2019
ms.topic: "reference" 
ms.prod: "mlserver" 
ms.service: "" 
ms.assetid: "" 
 
# optional metadata 
ROBOTS: "" 
audience: "" 
ms.devlang: "Python" 
ms.reviewer: "" 
ms.suite: "" 
ms.tgt_pltfrm: "" 
#ms.technology: "" 
ms.custom: "" 
 
---

# rx_get_pyspark_connection


 


## Usage



```
revoscalepy.rx_get_pyspark_connection(compute_context: revoscalepy.computecontext.RxSpark.RxSpark)
```





## Description

Gets a PySpark connection from the current Spark compute context.


## Arguments


### compute_context

Compute context get created by rx_spark_connect.


## Returns

Object of python.context.SparkContext.


## Example



```
from revoscalepy import rx_spark_connect, rx_get_pyspark_connection
from pyspark.sql import SparkSession
cc = rx_spark_connect(interop = "pyspark")
sc = rx_get_pyspark_connection(cc)
spark = SparkSession(sc)
df = spark.createDataFrame([('Monday',3012),('Friday',1354),('Sunday',5452)], ["DayOfWeek", "Sale"])
```

