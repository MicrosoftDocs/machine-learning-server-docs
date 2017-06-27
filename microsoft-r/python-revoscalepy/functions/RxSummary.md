--- 
 
# required metadata 
title: "Object Summaries" 
description: "Produce univariate summaries of objects in RevoScalePy." 
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
# rx_summary
=======
## rx_summary


### Usage
>>>>>>> heidist-revoscalepy



```
revoscalepy.functions.RxSummary.rx_summary(formula, data, by_group_out_file=None, summary_stats: list = None, by_term: bool = True, pweights=None, fweights=None, row_selection: str = None, transforms=None, transform_objects=None, transform_function=None, transform_variables=None, transform_packages=None, transform_environment=None, overwrite: bool = False, use_sparse_cube: bool = None, remove_zero_counts: bool = None, blocks_per_read: int = None, rows_per_block: int = 100000, report_progress: int = None, verbose: int = 0, compute_context=None, **kwargs)
```




<<<<<<< HEAD
## Description
=======
### Description
>>>>>>> heidist-revoscalepy

Produce univariate summaries of objects in RevoScalePy.


<<<<<<< HEAD
## Parameters


### formula
=======
### Arguments


##### formula
>>>>>>> heidist-revoscalepy

formula, as described in rxFormula. The formula typically
does not contain a response variable, i.e. it should be of the form ~ terms.


<<<<<<< HEAD
### data
=======
##### data
>>>>>>> heidist-revoscalepy

either a data source object, a character string specifying a
‘.xdf’ file, or a data frame object to summarize.


<<<<<<< HEAD
### by_group_out_file
=======
##### by_group_out_file
>>>>>>> heidist-revoscalepy

None, a character string or vector of character
strings specifying .xdf file names(s), or an RxXdfData object or list of
RxXdfData objects. If not None, and the formula includes computations by
factor, the by-group summary results will be written out to one or more
‘.xdf’ files. If more than one .xdf file is created and a single character
string is specified, an integer will be appended to the base byGroupOutFile
name for additional file names. The resulting RxXdfData objects will be
listed in the categorical component of the output object.


<<<<<<< HEAD
### summary_stats
=======
##### summary_stats
>>>>>>> heidist-revoscalepy

a character vector containing one or more of the
following values: “Mean”, “StdDev”, “Min”, “Max”, “ValidObs”, “MissingObs”,
“Sum”.


<<<<<<< HEAD
### by_term
=======
##### by_term
>>>>>>> heidist-revoscalepy

logical variable. If True, missings will be removed by term
(by variable or by interaction expression) before computing summary
statistics. If False, observations with missings in any term will be
removed before computations.


<<<<<<< HEAD
### pweights
=======
##### pweights
>>>>>>> heidist-revoscalepy

character string specifying the variable to use as
probability weights for the observations.


<<<<<<< HEAD
### fweights
=======
##### fweights
>>>>>>> heidist-revoscalepy

character string specifying the variable to use as
frequency weights for the observations.


<<<<<<< HEAD
### row_selection
=======
##### row_selection
>>>>>>> heidist-revoscalepy

None. Not currently supported, reserved for future use.


<<<<<<< HEAD
### transforms
=======
##### transforms
>>>>>>> heidist-revoscalepy

None. Not currently supported, reserved for future use.


<<<<<<< HEAD
### transform_objects
=======
##### transform_objects
>>>>>>> heidist-revoscalepy

None. Not currently supported, reserved for
future use.


<<<<<<< HEAD
### transform_function
=======
##### transform_function
>>>>>>> heidist-revoscalepy

variable transformation function. See
rxTransform for details.


<<<<<<< HEAD
### transform_variables
=======
##### transform_variables
>>>>>>> heidist-revoscalepy

character vector of input data set variables
needed for the transformation function. See rxTransform for details.


<<<<<<< HEAD
### transform_packages
=======
##### transform_packages
>>>>>>> heidist-revoscalepy

None. Not currently supported, reserved for
future use.


<<<<<<< HEAD
### transform_environment
=======
##### transform_environment
>>>>>>> heidist-revoscalepy

None. Not currently supported, reserved for
future use.


<<<<<<< HEAD
### overwrite
=======
##### overwrite
>>>>>>> heidist-revoscalepy

logical value. If True, an existing byGroupOutFile will
be overwritten. overwrite is ignored byGroupOutFile is None.


<<<<<<< HEAD
### use_sparse_cube
=======
##### use_sparse_cube
>>>>>>> heidist-revoscalepy

logical value. If True, sparse cube is used.


<<<<<<< HEAD
### remove_zero_counts
=======
##### remove_zero_counts
>>>>>>> heidist-revoscalepy

logical flag. If True, rows with no observations
will be removed from the output for counts of categorical data. By default,
it has the same value as useSparseCube. For large summary computation, this
should be set to True, otherwise R may run out of memory even if the
internal C++ computation succeeds.


<<<<<<< HEAD
### blocks_per_read
=======
##### blocks_per_read
>>>>>>> heidist-revoscalepy

number of blocks to read for each chunk of data
read from the data source.


<<<<<<< HEAD
### rows_per_block
=======
##### rows_per_block
>>>>>>> heidist-revoscalepy

maximum number of rows to write to each block in the
byGroupOutFile (if it is not None).


<<<<<<< HEAD
### report_progress
=======
##### report_progress
>>>>>>> heidist-revoscalepy

integer value with options:
0: no progress is reported.
1: the number of processed rows is printed and updated.
2: rows processed and timings are reported.
3: rows processed and all timings are reported.


<<<<<<< HEAD
### verbose
=======
##### verbose
>>>>>>> heidist-revoscalepy

integer value. If 0, no additional output is printed. If 1,
additional summary information is printed.


<<<<<<< HEAD
### compute_context
=======
##### compute_context
>>>>>>> heidist-revoscalepy

a valid RxComputeContext object.


<<<<<<< HEAD
### kwargs
=======
##### kwargs
>>>>>>> heidist-revoscalepy

additional arguments to be passed directly to the Revolution
Compute Engine.


<<<<<<< HEAD
## Returns
=======
### Returns
>>>>>>> heidist-revoscalepy

an RxSummary object containing the following elements:
nobs.valid: number of valid observations.
nobs.missing: number of missing observations.
sDataFrame: data frame containing summaries for continuous variables.
categorical: list of summaries for categorical variables.
categorical.type: types of categorical summaries: can be “counts”, or “cube” (for crosstab counts) or “none” (if there is no categorical summaries).
formula: formula used to obtain the summary.


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
from revoscalepy import rx_summary, RxOptions, RxXdfData
sample_data_path = RxOptions.get_option("sampleDataDir")
ds = RxXdfData(os.path.join(sample_data_path, "AirlineDemoSmall.xdf"))
summary = rx_summary("ArrDelay+DayOfWeek", ds)
print(summary)
```

