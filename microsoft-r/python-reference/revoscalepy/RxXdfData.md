--- 
 
# required metadata 
title: "RxXdfData: Generate Xdf Data Source Object" 
description: "Main generator for class RxXdfData, which extends RxDataSource." 
keywords: "datasource, xdf" 
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

# RxXdfData


 



```
revoscalepy.RxXdfData(file: str, vars_to_keep=None, vars_to_drop=None,
    return_data_frame=True, strings_as_factors=False, blocks_per_read=1,
    file_system: typing.Union[str,
    revoscalepy.datasource.RxFileSystem.RxFileSystem] = 'native',
    create_composite_set=None, create_partition_set=None,
    blocks_per_composite_file=3)
```





## Description

Main generator for class RxXdfData, which extends RxDataSource.


## Arguments


### file

Character string specifying the location of the data. For single
Xdf, it is a ‘.xdf’ file. For composite Xdf, it is a directory like
‘/tmp/airline’.


### vars_to_keep

List of strings of variable names to keep around during
operations. If None, argument is ignored. Cannot be used with vars_to_drop.


### vars_to_drop

list of strings of variable names to drop from
operations. If None, argument is ignored. Cannot be used with vars_to_keep.


### return_data_frame

Bool value indicating whether or not to convert the
result to a data frame.


### strings_as_factors

Bool value indicating whether or not to convert
strings into factors (for reader mode only).


### blocks_per_read

Number of blocks to read for each chunk of data read
from the data source.


### file_system

Character string or RxFileSystem object indicating type
of file system; “native” or RxNativeFileSystem object can be used for the
local operating system. If None, the file system will be set to that in
the current compute context, if available, otherwise the fileSystem option.


### create_composite_set

Bool value or None. Used only when writing.
If True, a composite set of files will be created instead of a single ‘.xdf’
file. Subdirectories ‘data’ and ‘metadata’ will be created. In the ‘data’
subdirectory, the data will be split across a set of ‘.xdfd’ files (see
blocks_per_composite_file below for determining how many blocks of data will be
in each file). In the ‘metadata’ subdirectory there is a single ‘.xdfm’ file,
which contains the meta data for all of the ‘.xdfd’ files in the ‘data’
subdirectory.


### create_partition_set

Bool value or None. Used only when writing.
If True, a set of files for partitioned Xdf will be created when assigning
this RxXdfData object for outData of rxPartition. Subdirectories ‘data’ and
‘metadata’ will be created. In the ‘data’ subdirectory, the data will be
split across a set of ‘.xdf’ files (each file stores data of a single data
partition, see rxPartition for details). In the ‘metadata’ subdirectory there
is a single ‘.xdfp’ file, which contains the meta data for all of the ‘.xdf’
files in the ‘data’ subdirectory. The partitioned Xdf object is currently
supported only in rxPartition and rxGetPartitions


### blocks_per_composite_file

Integer value. If
create_composite_set=True, this will be the number of blocks put into each
‘.xdfd’ file in the composite set.


## Returns

Object of class `RxXdfData`.


## Example



```
import os
from revoscalepy import rx_data_step, RxOptions, RxXdfData
sample_data_path = RxOptions.get_option("sampleDataDir")
ds = RxXdfData(os.path.join(sample_data_path, "kyphosis.xdf"))
kyphosis = rx_data_step(ds)
```

