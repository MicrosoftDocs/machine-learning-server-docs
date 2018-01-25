--- 
 
# required metadata 
title: "rx_partition: Partition Data by Key Values and Save the results to a Partitioned .Xdf" 
description: "Partition input data sources by key values and save the results to a partitioned Xdf on disk." 
keywords: "partition" 
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

# rx_partition


 


## Usage



```
revoscalepy.rx_partition(input_data: typing.Union[revoscalepy.datasource.RxDataSource.RxDataSource,
    pandas.core.frame.DataFrame, str],
    output_data: revoscalepy.datasource.RxXdfData.RxXdfData,
    vars_to_partition: typing.Union[typing.List[str], str],
    append: str = 'rows', overwrite: bool = False,
    **kwargs) -> pandas.core.frame.DataFrame
```





## Description

Partition input data sources by key values and save the results to a partitioned Xdf on disk.


## Arguments


### input_data

Either a data source object, a character string specifying
a ‘.xdf’ file, or a Pandas data frame object.


### output_data

A partitioned data source object created by RxXdfData
with create_partition_set = True.


### vars_to_partition

List of strings of variable names to be used for
partitioning the input data set.


### append

Either “none” to create a new file or “rows” to append rows to
an existing file. If output_data exists and append is “none”, the overwrite
argument must be set to True.


### overwrite

Bool value. If True, an existing output_data will be overwritten.
overwrite is ignored if appending rows.


### kwargs

Additional arguments.


## Returns

A Pandas data frame of partitioning values and data sources, each row in the
Pandas data frame represents one partition and the data source in the last variable
holds the data of a specific partition.


## See also

[`rx_exec_by`](rx-exec-by.md).
[`RxXdfData`](RxXdfData.md).


## Example



```
import os, tempfile
from revoscalepy import RxOptions, RxXdfData, rx_partition
data_path = RxOptions.get_option("sampleDataDir")

# input xdf data source
xdf_file = os.path.join(data_path, "claims.xdf")
xdf_ds = RxXdfData(xdf_file)

# create a partitioned xdf data source object
out_xdf_file = os.path.join(tempfile._get_default_tempdir(), "outPartitions")
out_xdf = RxXdfData(out_xdf_file, create_partition_set = True)

# do partitioning for input data set
partitions = rx_partition(input_data = xdf_ds, output_data = out_xdf, vars_to_partition = ["car.age","type"])
print(partitions)
```

