--- 
 
# required metadata 
title: "rx_get_var_names: Variable names for a data source or data frame (revoscalepy)" 
description: "Read the variable names for data source or data frame" 
keywords: "variables" 
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

# rx_get_var_names


 


## Usage



```
revoscalepy.rx_get_var_names(data)
```





## Description

Read the variable names for data source or data frame


## Arguments


### data

an RxDataSource object, a character string specifying the “.xdf” file, or a data frame.
If a Spark compute context is being used, this argument may also be an RxHiveData,
RxOrcData, RxParquetData or RxSparkDataFrame object or a Spark data frame object from pyspark.sql.DataFrame.


## Returns

list of strings containing the names of the variables in the data source or data frame.


## See also

[`rx_data_step`](rx-data-step.md),
[`rx_get_info`](rx-get-info.md),
[`rx_get_var_info`](rx-get-var-info.md).


## Example



```
import os
from revoscalepy import RxOptions, RxXdfData, rx_get_var_names

sample_data_path = RxOptions.get_option("sampleDataDir")
claims_ds = RxXdfData(os.path.join(sample_data_path, "claims.xdf"))
info = rx_get_var_names(claims_ds)
print(info)
```


Output:



```
RowNum age car.age type cost number
```

