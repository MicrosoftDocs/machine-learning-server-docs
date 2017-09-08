--- 
 
# required metadata 
title: "rx_partition,rx_get_partitions: Partition Data by Key Values and Save the results to a Partitioned .Xdf" 
description: "Partition input data sources by key values and save the results to a partitioned Xdf on disk." 
keywords: "partition" 
author: "bradsev" 
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

# rx_partition


**Applies to: SQL Server 2017 RC2**


## Usage



```
revoscalepy.rx_partition(input_data: typing.Union[revoscalepy.datasource.RxDataSource.RxDataSource,
    pandas.core.frame.DataFrame, str] = None,
    output_data: revoscalepy.datasource.RxXdfData.RxXdfData = None,
    vars_to_partition: typing.Union[typing.List[str], str] = None,
    append: str = 'rows', overwrite: bool = False,
    **kwargs) -> pandas.core.frame.DataFrame
```





## Description

Partition input data sources by key values and save the results to a partitioned Xdf on disk.


## Arguments


### input_data

either a data source object, a character string specifying
a ‘.xdf’ file, or a Pandas data frame object.


### output_data

a partitioned data source object created by RxXdfData
with createPartitionSet = True.


### vars_to_partition

list of strings of variable names to be used for
partitioning the input data set.


### append

either “none” to create a new file or “rows” to append rows to
an existing file. If output_data exists and append is “none”, the overwrite
argument must be set to True.


### overwrite

bool value. If True, an existing output_data will be overwritten.
overwrite is ignored if appending rows.


### kwargs

additional arguments.


## Returns

a Pandas data frame of partitioning values and data sources, each row in the
Pandas data frame represents one partition and the data source in the last variable
holds the data of a specific partition.


## See also

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



## Usage



```
revoscalepy.rx_get_partitions(input_data: revoscalepy.datasource.RxXdfData.RxXdfData = None,
    **kwargs)
```





## Description

Get partitions enumeration of a partitioned Xdf data source.


## Arguments


### input_data

an existing partitioned data source object which was
created by RxXdfData with create_partition_set = True and
constructed by rx_partition.


## Returns

a Pandas data frame with (n+1) columns, the first n columns are
partitioning columns specified by vars_to_partition in rx_partition
and the (n+1)th column is a data source column where each row contains
an Xdf data source object of one partition.


## See also

[`RxXdfData`](RxXdfData.md).
[`rx_partition`](revoscalepy/rx-partition.md).


## Example



```
import os, tempfile
from revoscalepy import RxOptions, RxXdfData, rx_partition, rx_get_partitions
data_path = RxOptions.get_option("sampleDataDir")

# input xdf data source
xdf_file = os.path.join(data_path, "claims.xdf")
xdf_ds = RxXdfData(xdf_file)

# create a partitioned xdf data source object
out_xdf_file = os.path.join(tempfile._get_default_tempdir(), "outPartitions")
out_xdf = RxXdfData(out_xdf_file, create_partition_set = True)

# do partitioning for input data set
partitions = rx_partition(input_data = xdf_ds, output_data = out_xdf, vars_to_partition = ["car.age","type"], append = "none", overwrite = True)

# use rx_get_partitions to load an existing partitioned xdf
out_xdf_1 = RxXdfData(out_xdf_file)
partitions_1 = rx_get_partitions(out_xdf_1)
print(partitions_1)
```

