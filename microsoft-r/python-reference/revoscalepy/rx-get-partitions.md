--- 
 
# required metadata 
title: "rx_get_partitions: Get Partitions of a Partitioned Xdf Data Source" 
description: "Get partitions enumeration of a partitioned Xdf data source." 
keywords: "partition, execby, " 
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

# rx_get_partitions


 


## Usage



```
revoscalepy.rx_get_partitions(input_data: revoscalepy.datasource.RxXdfData.RxXdfData = None,
    **kwargs)
```





## Description

Get partitions enumeration of a partitioned Xdf data source.


## Arguments


### input_data

An existing partitioned data source object which was
created by RxXdfData with create_partition_set = True and
constructed by rx_partition.


## Returns

A Pandas data frame with (n+1) columns, the first n columns are
partitioning columns specified by vars_to_partition in rx_partition
and the (n+1)th column is a data source column where each row contains
an Xdf data source object of one partition.


## See also

[`rx_exec_by`](rx-exec-by.md).
[`RxXdfData`](RxXdfData.md).
[`rx_partition`](rx-partition.md).


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

