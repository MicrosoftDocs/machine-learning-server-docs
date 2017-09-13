--- 
 
# required metadata 
title: "RxSparkDataFrame: Generate Spark Data Frame Data Source Object" 
description: "Main generator for class RxSparkDataFrame, which extends RxSparkData." 
keywords: "datasource" 
author: "bradsev" 
manager: "jhubbard" 
ms.date: "09/11/2017" 
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

# RxSparkDataFrame


 



```
revoscalepy.RxSparkDataFrame(data=None, column_info=None,
    write_factors_as_indexes: bool = False)
```





## Description

Main generator for class RxSparkDataFrame, which extends RxSparkData.


## Arguments


### data

Spark data frame obj from pyspark.sql.DataFrame.


### column_info

list of named variable information lists. Each variable
information list contains one or more of the named elements given below.
Currently available properties for a column information list are:
type: character string specifying the data type for the column.

    Supported types are:
        ”bool” (stored as uchar),
        “integer” (stored as int32),
        “int16” (alternative to integer for smaller storage space),
        “float32” (stored as FloatType),
        “numeric” (stored as float64),
        “character” (stored as string),
        “factor” (stored as uint32),
        “Date” (stored as Date, i.e. float64.)
        “POSIXct” (stored as POSIXct, i.e. float64.)

levels: list of strings containing the levels when type = “factor”. If
    the levels property is not provided, factor levels will be determined
    by the values in the source column. If levels are provided, any value
    that does not match a provided level will be converted to a missing
    value.


### write_factors_as_indexes

bool. If True, when writing to an
RxSparkDataFrame data source, underlying factor indexes will be written instead
of the string representations.


## Returns

object of class `RxSparkDataFrame`.


## Example



```
import os
from revoscalepy import rx_data_step, RxSparkDataFrame
from pyspark.sql import SparkSession
spark = SparkSession.builder.getOrCreate()
df = spark.createDataFrame([('Monday',3012),('Friday',1354),('Sunday',5452)], ["DayOfWeek", "Sale"])
col_info = {"DayOfWeek": {"type":"factor"}}
ds = RxSparkDataFrame(df, column_info=col_info)
out_ds = RxSparkDataFrame(write_factors_as_indexes=True)
rx_data_step(ds, out_ds)
out_df = out_ds.spark_data_frame
out_df.take(2)
# [Row(DayOfWeek=0, Sale=3012), Row(DayOfWeek=2, Sale=1354)]
```

