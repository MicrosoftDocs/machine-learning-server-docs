--- 
 
# required metadata 
title: "rx_spark_remove_data: Manage cached data in Spark (revoscalepy)" 
description: "Use these rx_spark_remove_data functions to manage the objects cached in the Spark memory system. These functions are only applicable  when using RxSpark compute context." 
keywords: "spark, data" 
author: "dphansen"
ms.author: "davidph" 
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

# rx_spark_remove_data


 


## Usage



```
revoscalepy.rx_spark_remove_data(remove_list,
    compute_context: revoscalepy.computecontext.RxComputeContext.RxComputeContext = None)
```





## Description

Use these functions to manage the objects cached in the Spark memory system. These functions are only applicable
    when using RxSpark compute context.


## Arguments


### remove_list

Cached object or a list of cached objects need to be deleted.


### compute_context

RxSpark compute context object.


## Returns

No return objs for rxSparkRemoveData.


## Example



```
from revoscalepy import RxOrcData, rx_spark_connect, rx_spark_disconnect, rx_spark_list_data, rx_spark_cache_data, rx_spark_remove_data, rx_lin_mod
cc=rx_spark_connect()
col_info = {"DayOfWeek": {"type": "factor"}}

df = RxOrcData(file = "/share/sample_data/AirlineDemoSmallOrc", column_info = col_info)
df = rx_spark_cache_data(df, True)

# After the first run, a Spark data object is added into the list
rx_lin_mod("ArrDelay ~ DayOfWeek", data = df)

rx_spark_remove_data(df)
rx_spark_list_data(True)
rx_spark_disconnect(cc)
```

