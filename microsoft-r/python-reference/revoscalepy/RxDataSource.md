--- 
 
# required metadata 
title: "RxDataSource,head,tail: RxDataSource" 
description: "Base class for all revoscalepy data sources." 
keywords: "head" 
author: "Microsoft Corporation Microsoft Technical Support" 
manager: "jhubbard" 
ms.date: "09/06/2017" 
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

# `RxDataSource`


**Applies to: SQL Server 2017 RC2**



```
revoscalepy.RxDataSource(column_info: dict = None)
```





## Description

Base class for all revoscalepy data sources.


## Author

Microsoft Corporation [Microsoft Technical Support](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)



```
head(num_rows: int = 6, report_progress: int = None)
```





## Description

Displays the first rows of the datasource.


## Arguments


### num_rows

integer value specifying the number of rows to display starting from the beginning of the dataset.
If not specified, the default of 6 will be used.


### report_progress

integer value with options:

* 0: no progress is reported. 

* 1: the number of processed rows is printed and updated. 

* 2: rows processed and timings are reported. 

* 3: rows processed and all timings are reported. 


## Example



```
import os
from revoscalepy import RxOptions, RxXdfData
sample_data_path = RxOptions.get_option("sampleDataDir")
ds = RxXdfData(os.path.join(sample_data_path, "AirlineDemoSmall.xdf"))
ds.head(num_rows=4)
```




```
tail(num_rows: int = 6, report_progress: int = None)
```





## Description

Displays the last rows of the datasource.


## Arguments


### num_rows

integer value specifying the number of rows to display starting from the end of the dataset.
If not specified, the default of 6 will be used.


### report_progress

integer value with options:

* 0: no progress is reported. 

* 1: the number of processed rows is printed and updated. 

* 2: rows processed and timings are reported. 

* 3: rows processed and all timings are reported. 


## Example



```
import os
from revoscalepy import RxOptions, RxXdfData
sample_data_path = RxOptions.get_option("sampleDataDir")
ds = RxXdfData(os.path.join(sample_data_path, "AirlineDemoSmall.xdf"))
ds.tail(num_rows=4)
```

