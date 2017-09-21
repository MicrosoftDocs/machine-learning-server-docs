--- 
 
# required metadata 
title: "rx_get_info: Get Data Source Information" 
description: "Get basic information about an revoscalepy data source or data frame" 
keywords: "xdf" 
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

# rx_get_info


 


## Usage



```
revoscalepy.rx_get_info(data, get_var_info: bool = False,
    get_block_sizes: bool = False, get_value_labels: bool = None,
    vars_to_keep: list = None, vars_to_drop: list = None,
    start_row: int = 1, num_rows: int = 0,
    compute_info: bool = False, all_nodes: bool = False,
    verbose: int = 0)
```





## Description

Get basic information about an revoscalepy data source or data frame


## Arguments


### data

a data frame, a character string specifying an “.xdf”, or an
RxDataSource object. If a local compute context is being used, this
argument may also be a list of data sources, in which case the output will
be returned in a named list. See the details section for more information.
If a Spark compute context is being used, this argument may also be an RxHiveData,
RxOrcData, RxParquetData or RxSparkDataFrame object or a Spark data frame object from pyspark.sql.DataFrame.


### get_var_info

bool value. If True, variable information is
returned.


### get_block_sizes

bool value. If True, block sizes are returned in
the output for an “.xdf” file, and when printed the first 10 block sizes
are shown.


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


### start_row

starting row for retrieval of data if a data frame or
“.xdf” file.


### num_rows

number of rows of data to retrieve if a data frame or
“.xdf” file.


### compute_info

bool value. If True, and get_var_info is True,
variable information (e.g., high/low values) for non-xdf data sources will
be computed by reading through the data set. If True, and get_var_info is
False, the number of variables will be gotten from from non-xdf data
sources (but not the number of rows).


### all_nodes

bool value. Ignored if the active RxComputeContext
compute context is local or RxForeachDoPar. Otherwise, if True, a list
containing the information for the data set on each node in the active
compute context will be returned. If False, only information on the data
set on the master node will be returned. Note that the determination of the
master node is not controlled by the end user.


### verbose

integer value. If 0, no additional output is printed. If 1,
additional summary information is printed for an “.xdf” file.


## Returns

list containing the following possible elements:

fileName: character string containing the file name and path (if an
    ”.xdf” file).

objName: character string containing object name (if not an “.xdf” file).
class: class of the object.
length: length of the object if it is not an “.xdf” file or data frame.
numCompositeFiles: number of composite data files(if a composite “.xdf”

    file).

numRows: number of rows in the data set.
numVars: number of variables in the data set.
numBlocks: number of blocks in the data set.
varInfo: list of variable information where each element is a list

    describing a variable.

rowsPerBlock: integer vector containing number of rows in each block
    (if get_block_sizes is set to True). Set to None if data is a data frame.

data: data frame containing the data (if num_rows > 0)


## See also

[`rx_data_step`](rx-data-step.md),
[`rx_get_var_info`](rx-get-var-info.md).


## Example



```
import os
from revoscalepy import RxOptions, RxXdfData, rx_get_info

sample_data_path = RxOptions.get_option("sampleDataDir")
claims_ds = RxXdfData(os.path.join(sample_data_path, "claims.xdf"))
info = rx_get_info(claims_ds, get_var_info=True)
print(info)
```


Output:



```
File name:C:\swarm\workspace\bigAnalytics-9.2.1\python\revoscalepy\revoscalepy\data\sample_data\claims.xdf
Number of observations:128.0
Number of variables:6.0
Number of blocks:1.0
Compression type:zlib
Variable information: 
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

