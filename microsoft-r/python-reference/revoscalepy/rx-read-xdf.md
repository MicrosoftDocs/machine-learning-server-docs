--- 
 
# required metadata 
title: "rx_read_xdf: Read .xdf File" 
description: "Read data from an “.xdf” file into a data frame." 
keywords: "xdf" 
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

# rx_read_xdf


**Applies to: SQL Server 2017**


## Usage



```
revoscalepy.rx_read_xdf(file: str, vars_to_keep: list = None,
    vars_to_drop: list = None, row_var_name: str = None,
    start_row: int = 1, num_rows: int = None,
    return_data_frame: bool = True,
    strings_as_factors: bool = False,
    max_rows_by_columns: int = None, report_progress: int = None,
    read_by_block: bool = False, cpp_interp: list = None)
```





## Description

Read data from an “.xdf” file into a data frame.


## Arguments


### file

either an RxXdfData object or a character string specifying
the “.xdf” file.


### vars_to_keep

list of strings of variable names to include when
reading from the input data file. If None, argument is ignored. Cannot be
used with vars_to_drop.


### vars_to_drop

list of strings of variable names to exclude when
reading from the input data file. If None, argument is ignored. Cannot be
used with vars_to_keep.


### row_var_name

optional character string specifying the variable in
the data file to use as row names for the output data frame.


### start_row

starting row for retrieval.


### num_rows

number of rows of data to retrieve. If -1, all are read.


### return_data_frame

bool indicating whether or not to create a
data frame. If False, a list is returned.


### strings_as_factors

bool indicating whether or not to convert
strings into factors.


### max_rows_by_columns

the maximum size of a data frame that will be
read in, measured by the number of rows times the number of columns. If the
numer of rows times the number of columns being extracted from the “.xdf”
file exceeds this, a warning will be reported and a smaller number of rows
will be read in than requested. If max_rows_by_columns is set to be too large,
you may experience problems from loading a huge data frame into memory. To
extract a subset of rows and/or columns from an “.xdf” file, use rx_data_step.


### report_progress

integer value with options:
0: no progress is reported.
1: the number of processed rows is printed and updated.
2: rows processed and timings are reported.
3: rows processed and all timings are reported.


### read_by_block

read data by blocks. This argument is deprecated.


### cpp_interp

list of information sent to C++ interpreter.


## Returns

a data frame.


## See also

[`rx_import`](rx-import.md),
[`rx_data_step`](rx-data-step.md).


## Example



```
import os
from revoscalepy import RxOptions, rx_read_xdf

sample_data_path = RxOptions.get_option("sampleDataDir")
mort_file = os.path.join(sample_data_path, "mortDefaultSmall.xdf")
mort_df = rx_read_xdf(mort_file, num_rows = 10)
print(mort_df)
```


Output:



```

Rows Processed: 10
Time to read data file: 0.00 secs.
Time to convert to data frame: 0.00 secs.
   creditScore  houseAge  yearsEmploy  ccDebt  year  default
0          691        16            9    6725  2000        0
1          691         4            4    5077  2000        0
2          743        18            3    3080  2000        0
3          728        22            1    4345  2000        0
4          745        17            3    2969  2000        0
5          539        15            3    4588  2000        0
6          724         6            5    4057  2000        0
7          659        14            3    6456  2000        0
8          621        18            3    1861  2000        0
9          720        14            7    4568  2000        0
```

