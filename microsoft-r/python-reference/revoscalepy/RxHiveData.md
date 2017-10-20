--- 
 
# required metadata 
title: "RxHiveData: Generate Hive Data Source Object" 
description: "Main generator for class RxHiveData, which extends RxSparkData." 
keywords: "datasource, hive" 
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

# RxHiveData


 



```
revoscalepy.RxHiveData(query=None, table=None, save_as_temp_table=False,
    write_factors_as_indexes: bool = False, column_info=None)
```





## Description

Main generator for class RxHiveData, which extends RxSparkData.


## Arguments


### query

character string specifying a Hive query, e.g. “select * from >>sample_<<
table”. Cannot be used with ‘table’.


### table

character string specifying the name of a Hive table, e.g. “sample_table”.
Cannot be used with ‘query’.


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


### save_as_temp_table

bool. Only applicable when using as output with “table” parameter.
If “TRUE” register a temporary Hive table in Spark memory system otherwise generate a
persistent Hive table. The temporary Hive table is always cached in Spark memory system.


### write_factors_as_indexes

bool. If True, when writing to an
RxHiveData data source, underlying factor indexes will be written instead
of the string representations.


## Returns

object of class `RxHiveData`.


## Example



```
import os
from revoscalepy import rx_data_step, RxOptions, RxHiveData
sample_data_path = RxOptions.get_option("sampleDataDir")
colInfo = {"DayOfWeek": {"type":"factor"}}
ds1 = RxHiveData(table = "AirlineDemo")
result = rx_data_step(ds1)
ds2 = RxHiveData(query = "select * from hivesampletable")
result = rx_data_step(ds2)
```

