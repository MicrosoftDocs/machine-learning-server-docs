--- 
 
# required metadata 
title: "Generate Xdf Data Source Object" 
description: "Main generator for class RxXdfData, which extends RxDataSource." 
keywords: "datasource, xdf" 
author: "HeidiSteen" 
manager: "" 
ms.date: "" 
ms.topic: "reference" 
ms.prod: "microsoft-r" 
ms.service: "" 
ms.assetid: "" 
 
# optional metadata 
ROBOTS: "" 
audience: "" 
ms.devlang: "" 
ms.reviewer: "" 
ms.suite: "" 
ms.tgt_pltfrm: "" 
ms.technology: "r-server" 
ms.custom: "" 
 
---

## ``RxXdfData``


*Applies to:* SQL Server 2017, Machine Learning Services 9.3


### Usage



```
class revoscalepy.RxXdfData(file: str, vars_to_keep=None, vars_to_drop=None, return_data_frame=True, strings_as_factors=False, blocks_per_read=1, file_system: typing.Union[str, revoscalepy.datasource.RxFileSystem.RxFileSystem] = ‘native’, create_composite_set=None, create_partition_set=None, blocks_per_composite_file=3)
```




### Description

Main generator for class RxXdfData, which extends RxDataSource.


### Arguments


##### file

character string specifying the location of the data. For single
Xdf, it is a ‘.xdf’ file. For composite Xdf, it is a directory like
‘/tmp/airline’. When using distributed compute contexts like RxSpark, a
directory should be used since those compute contexts always use composite Xdf.


##### vars_to_keep

character vector of variable names to keep around during
operations. If None, argument is ignored. Cannot be used with vars_to_drop.


##### vars_to_drop

character vector of variable names to drop from
operations. If None, argument is ignored. Cannot be used with vars_to_keep.


##### return_data_frame

logical indicating whether or not to convert the
result to a data frame when reading with rxReadNext. If False, a list is
returned when reading with rxReadNext.


##### strings_as_factors

logical indicating whether or not to convert
strings into factors in R (for reader mode only).


##### blocks_per_read

number of blocks to read for each chunk of data read
from the data source.


##### file_system

character string or RxFileSystem object indicating type
of file system; “native” or RxNativeFileSystem object can be used for the
local operating system, or an RxHdfsFileSystem object for the Hadoop file
system. If None, the file system will be set to that in the current compute
context, if available, otherwise the fileSystem option.


##### create_composite_set

logical value or None. Used only when writing.
If True, a composite set of files will be created instead of a single ‘.xdf’
file. Subdirectories ‘data’ and ‘metadata’ will be created. In the ‘data’
subdirectory, the data will be split across a set of ‘.xdfd’ files (see
blocksPerCompositeFile below for determining how many blocks of data will be
in each file). In the ‘metadata’ subdirectory there is a single ‘.xdfm’ file,
which contains the meta data for all of the ‘.xdfd’ files in the ‘data’
subdirectory. When the compute context is RxHadoopMR or RxSpark, a composite
set of files are always created.


##### create_partition_set

logical value or None. Used only when writing.
If True, a set of files for partitioned Xdf will be created when assigning
this RxXdfData object for outData of rxPartition. Subdirectories ‘data’ and
‘metadata’ will be created. In the ‘data’ subdirectory, the data will be
split across a set of ‘.xdf’ files (each file stores data of a single data
partition, see rxPartition for details). In the ‘metadata’ subdirectory there
is a single ‘.xdfp’ file, which contains the meta data for all of the ‘.xdf’
files in the ‘data’ subdirectory. The partitioned Xdf object is currently
supported only in rxPartition and rxGetPartitions


##### blocks_per_composite_file

integer value. If
create_composite_set=True, and if the compute context is not RxHadoopMR, this
will be the number of blocks put into each ‘.xdfd’ file in the composite set.
When importing is being done on Hadoop using MapReduce, the number of rows
per ‘.xdfd’ file is determined by the rows assigned to each MapReduce task,
and the number of blocks per ‘.xdfd’ file is therefore determined by
rowsPerRead.


### Returns

object of class ``RxXdfData``.


### Example



```
import os
from revoscalepy import rx_data_step, RxOptions, RxXdfData
sample_data_path = RxOptions.get_option("sampleDataDir")
ds = RxXdfData(os.path.join(sample_data_path, "kyphosis.xdf"))
kyphosis = rx_data_step(ds)
```

