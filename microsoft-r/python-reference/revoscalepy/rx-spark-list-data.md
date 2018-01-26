--- 
 
# required metadata 
title: "rx_spark_list_data: Functions for object management in Spark (revoscalepy)" 
description: "Use these functions to manage the objects cached in the Spark memory system. These functions are only applicable  when using RxSpark compute context." 
keywords: "spark, data" 
author: "HeidiSteen" 
manager: "cgronlun" 
ms.date: "01/26/2018" 
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

# rx_spark_list_data


 


## Usage



```
revoscalepy.rx_spark_list_data(show_description: bool,
    compute_context: revoscalepy.computecontext.RxComputeContext.RxComputeContext = None)
```





## Description

Use these functions to manage the objects cached in the Spark memory system. These functions are only applicable
    when using RxSpark compute context.


## Arguments


### show_description

Bool value, indicating whether or not to print out the detail to console.


### compute_context

RxSpark compute context object.


## Returns

List of all objects cached in Spark memory system for rxSparkListData.


## Example



```
from revoscalepy import RxOrcData, rx_spark_connect, rx_spark_list_data, rx_lin_mod, rx_spark_cache_data
rx_spark_connect()
col_info = {"DayOfWeek": {"type": "factor"}}
df = RxOrcData(file = "/share/sample_data/AirlineDemoSmallOrc", column_info = col_info)
df = rx_spark_cache_data(df, True)

# After the first run, a Spark data object is added into the list
rx_lin_mod("ArrDelay ~ DayOfWeek", data = df)
rx_spark_list_data(True)
```

