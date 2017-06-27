--- 
 
# required metadata 
title: "Global Options for RevoScalePy" 
description: "Functions to specify and retrieve options needed for RevoScalePy computations. These need to be set only once to carry out multiple computations." 
keywords: "M, I, S, S, I, N, G,  , K, E, Y, W, O, R, D, S" 
author: "Microsoft Corporation Microsoft Technical Support" 
manager: "" 
<<<<<<< HEAD
ms.date: "06/26/2017" 
=======
ms.date: "06/27/2017" 
>>>>>>> heidist-revoscalepy
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

<<<<<<< HEAD
# RxOptions
=======
## RxOptions


### Usage
>>>>>>> heidist-revoscalepy



```
revoscalepy.utils.RxOptions.RxOptions()
```




<<<<<<< HEAD
## Description
=======
### Description
>>>>>>> heidist-revoscalepy

Functions to specify and retrieve options needed for RevoScalePy computations. These need to be set only once to carry out multiple computations.


<<<<<<< HEAD
## Parameters


### unit_test_data_dir
=======
### Arguments


##### unit_test_data_dir
>>>>>>> heidist-revoscalepy

character string specifying path to RevoScalePy’s
RUnit-based test data directory.


<<<<<<< HEAD
### sample_data_dir
=======
##### sample_data_dir
>>>>>>> heidist-revoscalepy

character string specifying path to RevoScalePy’s
sample data directory.


<<<<<<< HEAD
### blocks_per_read
=======
##### blocks_per_read
>>>>>>> heidist-revoscalepy

default value to use for blocksPerRead argument for
many RevoScalePy functions. Represents the number of blocks to read within
each read chunk.


<<<<<<< HEAD
### report_progress
=======
##### report_progress
>>>>>>> heidist-revoscalepy

default value to use for reportProgress argument for
many RevoScalePy functions. Options are:

    0: no progress is reported.
    1: the number of processed rows is printed and updated.
    2: rows processed and timings are reported.
    3: rows processed and all timings are reported.


<<<<<<< HEAD
### row_display_max
=======
##### row_display_max
>>>>>>> heidist-revoscalepy

scalar integer specifying the maximum number of rows
to display when using the verbose argument in RevoScalePy functions. The
default of -1 displays all available rows.


<<<<<<< HEAD
### mem_stats_reset
=======
##### mem_stats_reset
>>>>>>> heidist-revoscalepy

boolean integer. If 1, reset memory status


<<<<<<< HEAD
### mem_stats_diff
=======
##### mem_stats_diff
>>>>>>> heidist-revoscalepy

boolean integer. If 1, the change of memory status is
shown.


<<<<<<< HEAD
### num_cores_to_use
=======
##### num_cores_to_use
>>>>>>> heidist-revoscalepy

scalar integer specifying the number of cores to use.
If set to a value higher than the number of available cores, the number of
available cores will be used. If set to -1, the number of available cores
will be used. Increasing the number of cores to use will also increase the
amount of memory required for RevoScalePy analysis functions.


<<<<<<< HEAD
### num_digits
=======
##### num_digits
>>>>>>> heidist-revoscalepy

controls the number of digits to to use when converting
numeric data to or from strings, such as when printing numeric values or
importing numeric data as strings. The default is the current value of
options()$digits, which defaults to 7. Beyond fifteen digits, however,
results are likely to be unreliable.


<<<<<<< HEAD
### show_transform_fn
=======
##### show_transform_fn
>>>>>>> heidist-revoscalepy

logical value. If True, the transform function is
shown.


<<<<<<< HEAD
### data_path
=======
##### data_path
>>>>>>> heidist-revoscalepy

character vector containing paths to search for local data
sources. The default is to search just the current working directory. This
will be ignored if dataPath is specified in the active compute context. See
the Details section for more information regarding the path format.


<<<<<<< HEAD
### out_data_path
=======
##### out_data_path
>>>>>>> heidist-revoscalepy

character vector containing paths for writing new
output data files. New data files will be written to the first path that
exists. The default is to write to the current working directory. This will
be ignored if outDataPath is specified in the active compute context.


<<<<<<< HEAD
### xdf_compression_level
=======
##### xdf_compression_level
>>>>>>> heidist-revoscalepy

integer in the range of -1 to 9. The higher the
value, the greater the amount of compression - resulting in smaller files
but a longer time to create them.


<<<<<<< HEAD
### file_system
=======
##### file_system
>>>>>>> heidist-revoscalepy

character string or RxFileSystem object indicating type
of file system; “native” or RxNativeFileSystem object can be used for the
local operating system, or an RxHdfsFileSystem object for the Hadoop file
system.


<<<<<<< HEAD
### use_sparse_cube
=======
##### use_sparse_cube
>>>>>>> heidist-revoscalepy

logical value. If True, sparse cube is used.


<<<<<<< HEAD
### rng_bffer_size
=======
##### rng_bffer_size
>>>>>>> heidist-revoscalepy

a positive integer scalar specifying the buffer size
for the Parallel Random Number Generators (RNGs) in MKL.


<<<<<<< HEAD
### drop_main
=======
##### drop_main
>>>>>>> heidist-revoscalepy

logical value. If True, main-effect terms are dropped
before their interactions.


<<<<<<< HEAD
### coef_label_style
=======
##### coef_label_style
>>>>>>> heidist-revoscalepy

character string specifying the coefficient label
style. The default is “Revo”.


<<<<<<< HEAD
### num_tasks
=======
##### num_tasks
>>>>>>> heidist-revoscalepy

integer value. The default numTasks use in RxInSqlServer.


<<<<<<< HEAD
### hdfs_host
=======
##### hdfs_host
>>>>>>> heidist-revoscalepy

character string specifying the host name of your Hadoop
nameNode. Defaults to Sys.getenv(“REVOHADOOPHOST”), or “default” if no
REVOHADOOPHOST environment variable is set.


<<<<<<< HEAD
### hdfs_port
=======
##### hdfs_port
>>>>>>> heidist-revoscalepy

integer scalar specifying the port number of your Hadoop
nameNode, or a character string that can be coerced to numeric. Defaults to
as.integer(Sys.getenv(“REVOHADOOPPORT”)), or 0 if no REVOHADOOPPORT
environment variable is set.


<<<<<<< HEAD
### unix_python_path
=======
##### unix_python_path
>>>>>>> heidist-revoscalepy

The path to Python executable on a Unix/Linux node.
By default it points to a path corresponding to this client’s version.


<<<<<<< HEAD
### trace_level
=======
##### trace_level
>>>>>>> heidist-revoscalepy

Specifies the traceLevel that MRS will run with. This
parameter controls MRS Logging features as well as Runtime Tracing of
ScaleR functions. Levels are inclusive, (i.e. level 3:INFO includes levels
2:WARN and 1:ERROR log messages). The options are:

    0: DISABLED - Tracing/Logging disabled.
    1: ERROR - ERROR coded trace points are logged to MRS log files
    2: WARN - WARN and ERROR coded trace points are logged to MRS log files.
    3: INFO - INFO, WARN, and ERROR coded trace points are logged to MRS

        log files.

    4: DEBUG - All trace points are logged to MRS log files.
    5: RESERVED - If set, will log at DEBUG granularity
    6: RESERVED - If set, will log at DEBUG granularity
    7: TRACE - ScaleR functions Runtime Tracing is activated and MRS log

        level is set to DEBUG granularity.


<<<<<<< HEAD
## Returns
=======
### Returns
>>>>>>> heidist-revoscalepy

For RxOptions, a list containing the original RxOptions is returned. If there is no argument specified, the list is returned explicitly, otherwise the list is returned as an invisible object. For RxGetOption, the current value of the requested option is returned.


<<<<<<< HEAD
## Author
=======
### Author
>>>>>>> heidist-revoscalepy

Microsoft Corporation [Microsoft Technical Support](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409.md)


<<<<<<< HEAD
## See also


## Example
=======
### See also


### Example
>>>>>>> heidist-revoscalepy



```
import os
from revoscalepy import rx_data_step, RxOptions, RxXdfData
sample_data_path = RxOptions.get_option("sampleDataDir")
```

