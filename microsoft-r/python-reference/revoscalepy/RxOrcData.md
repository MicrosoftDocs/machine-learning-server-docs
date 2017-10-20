--- 
 
# required metadata 
title: "RxOrcData: Generate Orc Data Source Object" 
description: "Main generator for class RxOrcData, which extends RxSparkData." 
keywords: "datasource, orc" 
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

# RxOrcData


 



```
revoscalepy.RxOrcData(file: str = None, column_info=None,
    file_system: revoscalepy.datasource.RxFileSystem.RxFileSystem = None,
    write_factors_as_indexes: bool = False)
```





## Description

Main generator for class RxOrcData, which extends RxSparkData.


## Arguments


### file

character string specifying the location of the data. e.g.
“/tmp/AirlineDemoSmall.orc”.


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

levels: list of strings containing the levels when type = “factor”. If
    the levels property is not provided, factor levels will be determined
    by the values in the source column. If levels are provided, any value
    that does not match a provided level will be converted to a missing
    value.


### file_system

character string or RxFileSystem object indicating type
of file system; It supports native HDFS and other HDFS compatible systems,
e.g., Azure Blob and Azure Data Lake. Local file system is not supported.


### write_factors_as_indexes

bool. If True, when writing to an
RxOrcData data source, underlying factor indexes will be written instead
of the string representations.


## Returns

object of class `RxOrcData`.


## Example



```
import os
from revoscalepy import rx_data_step, RxOptions, RxOrcData
sample_data_path = RxOptions.get_option("sampleDataDir")
colInfo = {"DayOfWeek": {"type":"factor"}}
ds = RxOrcData(os.path.join(sample_data_path, "AirlineDemoSmall.orc"))
result = rx_data_step(ds)
```

