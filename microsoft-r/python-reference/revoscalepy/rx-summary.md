--- 
 
# required metadata 
title: "rx_summary: Generate summary statistics (revoscalepy)" 
description: "Produce univariate summaries of objects in revoscalepy." 
keywords: "summary" 
author: "HeidiSteen" 
manager: "cgronlun" 
ms.date: "01/26/2018" 
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

# rx_summary


 


## Usage



```
revoscalepy.rx_summary(formula: str, data, by_group_out_file=None,
    summary_stats: list = None, by_term: bool = True, pweights=None,
    fweights=None, row_selection: str = None, transforms=None,
    transform_objects=None, transform_function=None, transform_variables=None,
    transform_packages=None, transform_environment=None,
    overwrite: bool = False, use_sparse_cube: bool = None,
    remove_zero_counts: bool = None, blocks_per_read: int = None,
    rows_per_block: int = 100000, report_progress: int = None,
    verbose: int = 0, compute_context=None, **kwargs)
```





## Description

Produce univariate summaries of objects in revoscalepy.


## Arguments


### formula

Statistical model using symbolic formulas. The formula typically
does not contain a response variable, i.e. it should be of the form ~ terms.


### data

either a data source object, a character string specifying a
‘.xdf’ file, or a data frame object to summarize.
If a Spark compute context is being used, this argument may also be an RxHiveData,
RxOrcData, RxParquetData or RxSparkDataFrame object or a Spark data frame object from pyspark.sql.DataFrame.


### by_group_out_file

None, a character string or vector of character
strings specifying .xdf file names(s), or an RxXdfData object or list of
RxXdfData objects. If not None, and the formula includes computations by
factor, the by-group summary results will be written out to one or more
‘.xdf’ files. If more than one .xdf file is created and a single character
string is specified, an integer will be appended to the base by_group_out_file
name for additional file names. The resulting RxXdfData objects will be
listed in the categorical component of the output object.


### summary_stats

A list of strings containing one or more of the
following values: “Mean”, “StdDev”, “Min”, “Max”, “ValidObs”, “MissingObs”,
“Sum”.


### by_term

bool variable. If True, missings will be removed by term
(by variable or by interaction expression) before computing summary
statistics. If False, observations with missings in any term will be
removed before computations.


### pweights

Character string specifying the variable to use as
probability weights for the observations.


### fweights

Character string specifying the variable to use as
frequency weights for the observations.


### row_selection

None. Not currently supported, reserved for future use.


### transforms

None. Not currently supported, reserved for future use.


### transform_objects

None. Not currently supported, reserved for
future use.


### transform_function

Variable transformation function.


### transform_variables

List of strings of input data set variables
needed for the transformation function.


### transform_packages

None. Not currently supported, reserved for
future use.


### transform_environment

None. Not currently supported, reserved for
future use.


### overwrite

Bool value. If True, an existing byGroupOutFile will
be overwritten. overwrite is ignored byGroupOutFile is None.


### use_sparse_cube

Bool value. If True, sparse cube is used.


### remove_zero_counts

Bool flag. If True, rows with no observations
will be removed from the output for counts of categorical data. By default,
it has the same value as useSparseCube. For large summary computation, this
should be set to True, otherwise the Python interpreter may run out of
memory even if the internal C++ computation succeeds.


### blocks_per_read

Number of blocks to read for each chunk of data
read from the data source.


### rows_per_block

Maximum number of rows to write to each block in the
by_group_out_file (if it is not None).


### report_progress

Integer value with options:
0: No progress is reported.
1: The number of processed rows is printed and updated.
2: Rows processed and timings are reported.
3: Rows processed and all timings are reported.


### verbose

Integer value. If 0, no additional output is printed. If 1,
additional summary information is printed.


### compute_context

A valid RxComputeContext object.


### kwargs

Additional arguments to be passed directly to the Revolution
Compute Engine.


## Returns

An RxSummary object containing the following elements:
nobs.valid: Number of valid observations.
nobs.missing: Number of missing observations.
sDataFrame: Data frame containing summaries for continuous variables.
categorical: List of summaries for categorical variables.
categorical.type: Types of categorical summaries: can be “counts”, or “cube” (for crosstab counts) or “none” (if there is no categorical summaries).
formula: Formula used to obtain the summary.


## Example



```
import os
from revoscalepy import rx_summary, RxOptions, RxXdfData
sample_data_path = RxOptions.get_option("sampleDataDir")
ds = RxXdfData(os.path.join(sample_data_path, "AirlineDemoSmall.xdf"))
summary = rx_summary("ArrDelay+DayOfWeek", ds)
print(summary)
```

