--- 
 
# required metadata 
title: "RxDataSource: Base class generator for data source objects (revoscalepy)" 
description: "Base class for all revoscalepy data sources. Can be used with head() and tail() to display the first and last rows of the data set." 
keywords: "head, tail" 
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

# RxDataSource


 



```
revoscalepy.RxDataSource(column_info: dict = None)
```





## Description

Base class for all revoscalepy data sources. Can be used with head()
and tail() to display the first and last rows of the data set.


## Arguments


### num_rows

Integer value specifying the number of rows to display starting from the beginning of the dataset.
If not specified, the default of 6 will be used.


### report_progress

Integer value with options:
* 0: no progress is reported.
* 1: the number of processed rows is printed and updated.
* 2: rows processed and timings are reported.
* 3: rows processed and all timings are reported.


## Example



```
# Return the first 4 rows
import os
from revoscalepy import RxOptions, RxXdfData
sample_data_path = RxOptions.get_option("sampleDataDir")
ds = RxXdfData(os.path.join(sample_data_path, "AirlineDemoSmall.xdf"))
ds.head(num_rows=4)

# Return the last 4 rows
import os
from revoscalepy import RxOptions, RxXdfData
sample_data_path = RxOptions.get_option("sampleDataDir")
ds = RxXdfData(os.path.join(sample_data_path, "AirlineDemoSmall.xdf"))
ds.tail(num_rows=4)
```

