--- 
 
# required metadata 
title: "RxOptions: Global Options for revoscalepy" 
description: "Functions to specify and retrieve options needed for revoscalepy computations. These need to be set only once to carry out multiple computations." 
keywords: "options" 
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

# RxOptions


 



```
revoscalepy.RxOptions
```





## Description

Functions to specify and retrieve options needed for revoscalepy computations. These need to be set only once to carry out multiple computations.


## Arguments


### unitTestDataDir

Character string specifying path to revoscalepy’s
test data directory.


### sampleDataDir

Character string specifying path to revoscalepy’s
sample data directory.


### blocksPerRead

Default value to use for blocksPerRead argument for
many revoscalepy functions. Represents the number of blocks to read within
each read chunk.


### reportProgress

Default value to use for reportProgress argument for
many revoscalepy functions. Options are:

    0: no progress is reported.
    1: the number of processed rows is printed and updated.
    2: rows processed and timings are reported.
    3: rows processed and all timings are reported.


### RowDisplayMax

Integer value specifying the maximum number of rows
to display when using the verbose argument in revoscalepy functions. The
default of -1 displays all available rows.


### MemStatsReset

Boolean integer. If 1, reset memory status


### MemStatsDiff

Boolean integer. If 1, the change of memory status is
shown.


### NumCoresToUse

Integer value specifying the number of cores to use.
If set to a value higher than the number of available cores, the number of
available cores will be used. If set to -1, the number of available cores
will be used. Increasing the number of cores to use will also increase the
amount of memory required for revoscalepy analysis functions.


### NumDigits

Controls the number of digits to to use when converting
numeric data to or from strings, such as when printing numeric values or
importing numeric data as strings. The default is the current value of
options()$digits, which defaults to 7. Beyond fifteen digits, however,
results are likely to be unreliable.


### ShowTransformFn

Bool value. If True, the transform function is
shown.


### DataPath

List of strings containing paths to search for local data
sources. The default is to search just the current working directory. This
will be ignored if dataPath is specified in the active compute context. See
the Details section for more information regarding the path format.


### OutDataPath

List of strings containing paths for writing new
output data files. New data files will be written to the first path that
exists. The default is to write to the current working directory. This will
be ignored if outDataPath is specified in the active compute context.


### XdfCompressionLevel

Integer in the range of -1 to 9. The higher the
value, the greater the amount of compression - resulting in smaller files
but a longer time to create them.


### FileSystem

Character string or RxFileSystem object indicating type
of file system; “native” or RxNativeFileSystem object can be used for the
local operating system, or an RxHdfsFileSystem object for the Hadoop file
system.


### UseSparseCube

Bool value. If True, sparse cube is used.


### RngBfferSize

A positive integer scalar specifying the buffer size
for the Parallel Random Number Generators (RNGs) in MKL.


### DropMain

Bool value. If True, main-effect terms are dropped
before their interactions.


### CoefLabelStyle

Character string specifying the coefficient label
style. The default is “Revo”.


### NumTasks

Integer value. The default numTasks use in RxInSqlServer.


### unixPythonPath

The path to Python executable on a Unix/Linux node.
By default it points to a path corresponding to this client’s version.


### traceLevel

Specifies the traceLevel that ML server will run with. This
parameter controls ML Server Logging features as well as Runtime Tracing of
ScalePy functions. Levels are inclusive, (i.e. level 3:INFO includes levels
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


## Returns

For RxOptions, a list containing the original RxOptions is returned.

If there is no argument specified, the list is returned explicitly, otherwise
the list is returned as an invisible object. For RxGetOption, the current value
of the requested option is returned.


## Example



```
from revoscalepy import RxOptions
sample_data_path = RxOptions.get_option("sampleDataDir")
```

