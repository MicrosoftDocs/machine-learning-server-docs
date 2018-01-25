--- 
 
# required metadata 
title: "rx_get_var_info: Get Variable Information for a Data Source" 
description: "Get variable information for a revoscalepy data source or data frame, including variable names, descriptions, and value labels" 
keywords: "info" 
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

# rx_get_var_info


 


## Usage



```
revoscalepy.rx_get_var_info(data, get_value_labels: bool = True,
    vars_to_keep: list = None, vars_to_drop: list = None,
    compute_info: bool = False, all_nodes: bool = False)
```





## Description

Get variable information for a revoscalepy data source or data frame,
including variable names, descriptions, and value labels


## Arguments


### data

a data frame, a character string specifying an “.xdf”, or an
RxDataSource object. If a local compute context is being used, this
argument may also be a list of data sources, in which case the output will
be returned in a named list. See the details section for more information.
If a Spark compute context is being used, this argument may also be an RxHiveData,
RxOrcData, RxParquetData or RxSparkDataFrame object or a Spark data frame object from pyspark.sql.DataFrame.


### get_value_labels

bool value. If True and get_var_info is True or
None, factor value labels are included in the output.


### vars_to_keep

list of strings of variable names for which
information is returned. If None or get_var_info is False, argument is
ignored. Cannot be used with vars_to_drop.


### vars_to_drop

list of strings of variable names for which
information is not returned. If None or get_var_info is False, argument is
ignored. Cannot be used with vars_to_keep.


### compute_info

bool value. If True, variable information (e.g., high/low values)
for non-xdf data sources will be computed by reading through the data set.


### all_nodes

bool value. Ignored if the active RxComputeContext
compute context is local or RxForeachDoPar. Otherwise, if True, a list
containing the information for the data set on each node in the active
compute context will be returned. If False, only information on the data
set on the master node will be returned. Note that the determination of the
master node is not controlled by the end user.


## Returns

list with named elements corresponding to the variables in the data set.

Each list element is also a list with with following possible elements:

    description: character string specifying the variable description
    varType: character string specifying the variable type
    storage: character string specifying the storage type
    low: numeric giving the low values, possibly generated through a

        temporary factor transformation F()

    high: numeric giving the high values, possibly generated through a
        temporary factor transformation F()

    levels: (factor only) a list of strings containing the factor levels
    valueInfoCodes: list of strings of value codes, for informational

        purposes only

    valueInfoLabels: list of strings of value labels that is the same
        length as valueInfoCodes, used for informational purposes only


## See also

[`rx_data_step`](rx-data-step.md),
[`rx_get_info`](rx-get-info.md).


## Example



```
import os
from revoscalepy import RxOptions, RxXdfData, rx_get_var_info

sample_data_path = RxOptions.get_option("sampleDataDir")
claims_ds = RxXdfData(os.path.join(sample_data_path, "claims.xdf"))
info = rx_get_var_info(claims_ds)
print(info)
```


Output:



```
Var 1: RowNum, Type: integer, Storage: int32, Low/High: (1.0000, 128.0000)
Var 2: age
	8 factor levels: ['17-20', '21-24', '25-29', '30-34', '35-39', '40-49', '50-59', '60+']
Var 3: car.age
	4 factor levels: ['0-3', '4-7', '8-9', '10+']
Var 4: type
	4 factor levels: ['A', 'B', 'C', 'D']
Var 5: cost, Type: numeric, Storage: float32, Low/High: (11.0000, 850.0000)
Var 6: number, Type: numeric, Storage: float32, Low/High: (0.0000, 434.0000)
```

