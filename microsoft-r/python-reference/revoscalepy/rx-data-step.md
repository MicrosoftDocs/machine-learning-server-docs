--- 
 
# required metadata 
title: "rx_data_step: Transform data from input to output dataset (revoscalepy)" 
description: "Inline data transformations of an existing data set to an output data set" 
keywords: "datasource" 
author: "dphansen"
ms.author: "davidph" 
manager: "cgronlun" 
ms.date: 07/15/2019
ms.topic: "reference" 
ms.prod: "mlserver" 
ms.service: "" 
ms.assetid: "" 
 
# optional metadata 
ROBOTS: "" 
audience: "" 
ms.devlang: "Python" 
ms.reviewer: "" 
ms.suite: "" 
ms.tgt_pltfrm: "" 
#ms.technology: "" 
ms.custom: "" 
 
---

# rx_data_step


 


## Usage



```
revoscalepy.rx_data_step(input_data: typing.Union[revoscalepy.datasource.RxDataSource.RxDataSource,
    pandas.core.frame.DataFrame, str] = None,
    output_file: typing.Union[str, revoscalepy.datasource.RxXdfData.RxXdfData,
    revoscalepy.datasource.RxTextData.RxTextData] = None,
    vars_to_keep: list = None, vars_to_drop: list = None,
    row_selection: str = None, transforms: dict = None,
    transform_objects: dict = None, transform_function=None,
    transform_variables: list = None,
    transform_packages: list = None, append: list = None,
    overwrite: bool = False, row_variable_name: str = None,
    remove_missings_on_read: bool = False,
    remove_missings: bool = False, compute_low_high: bool = True,
    max_rows_by_cols: int = 3000000, rows_per_read: int = -1,
    start_row: int = 1, number_rows_read: int = -1,
    return_transform_objects: bool = False,
    blocks_per_read: int = None, report_progress: int = None,
    xdf_compression_level: int = 0, strings_as_factors: bool = None,
    **kwargs) -> typing.Union[revoscalepy.datasource.RxXdfData.RxXdfData,
    pandas.core.frame.DataFrame]
```





## Description

Inline data transformations of an existing data set to an output data set


## Arguments


### input_data

A character string with the path for the data to import
(delimited, fixed format, ODBC, or XDF). Alternatively, a data source
object representing the input data source can be specified.
If a Spark compute context is being used, this argument may also be an RxHiveData,
RxOrcData, RxParquetData or RxSparkDataFrame object or a Spark data frame object from pyspark.sql.DataFrame.


### output_file

A character string representing the output ‘.xdf’ file,
or a RxXdfData object. If None, a data frame will be returned in memory.
If a Spark compute context is being used, this argument may also be an RxHiveData,
RxOrcData, RxParquetData or RxSparkDataFrame object.


### vars_to_keep

list of strings of variable names to include when
reading from the input data file. If None, argument is ignored. Cannot be
used with vars_to_drop. Not supported for ODBC or fixed format text files.


### vars_to_drop

List of strings of variable names to exclude when
reading from the input data file. If None, argument is ignored. Cannot be
used with vars_to_keep. Not supported for ODBC or fixed format text files.


### row_selection

None. Not currently supported, reserved for future use.


### transforms

None. Not currently supported, reserved for future use.


### transform_objects

A dictionary of variables besides the data that are used in the transform function.
See the example.


### transform_function

Name of the function that will be used to modify the data.
The variables used in the transformation function must be specified in transform_variables.
See the example.


### transform_variables

List of strings of the column names needed
for the transform function.


### transform_packages

None. Not currently supported, reserved for future use.


### append

Either “none” to create a new ‘.xdf’ file or “rows” to
append rows to an existing ‘.xdf’ file. If output_file exists and append is
“none”, the overwrite argument must be set to True. Ignored if a data frame
is returned.


### overwrite

Bool value. If True, the existing outData will be
overwritten. Ignored if a dataframe is returned.


### row_variable_name

Character string or None. If inData is a data.frame:
If None, the data frame’s row names will be dropped. If a character string, an
additional variable of that name will be added to the data set containing the
data frame’s row names. If a data.frame is being returned, a variable with the
name row_variable_name will be removed as a column from the data frame and will be
used as the row names.


### remove_missings_on_read

Bool value. If True, rows with missing
values will be removed on read.


### remove_missings

Bool value. If True, rows with missing values will
not be included in the output data.


### compute_low_high

Bool value. If False, low and high values will not
automatically be computed. This should only be set to False in special
circumstances, such as when append is being used repeatedly. Ignored for data
frames.


### max_rows_by_cols

The maximum size of a data frame that will be returned
if output_file is set to None and inData is an ‘.xdf’ file, measured by the number
of rows times the number of columns. If the number of rows times the number of
columns being created from the ‘.xdf’ file exceeds this, a warning will be
reported and the number of rows in the returned data frame will be truncated.
If max_rows_by_cols is set to be too large, you may experience problems from
loading a huge data frame into memory.


### rows_per_read

Number of rows to read for each chunk of data read from
the input data source. Use this argument for finer control of the number of
rows per block in the output data source. If greater than 0, blocks_per_read is
ignored. Cannot be used if input_data is the same as output_file. The default value of
-1 specifies that data should be read by blocks according to the
blocks_per_read argument.


### start_row

The starting row to read from the input data source. Cannot
be used if input_data is the same as output_file.


### number_rows_read

Number of rows to read from the input data source. If
remove_missings are used, the output data set may have fewer
rows than specified by number_rows_read. Cannot be used if input_data is the same
as output_file.


### return_transform_objects

Bool value. If True, the list of
transformObjects will be returned instead of a data frame or data source
object. If the input transformObjects have been modified, by using .rxSet or
.rxModify in the transformFunc, the updated values will be returned. Any data
returned from the transformFunc is ignored. If no transformObjects are used,
None is returned. This argument allows for user-defined computations within a
transform_function without creating new data.


### blocks_per_read

Number of blocks to read for each chunk of data read
from the data source. Ignored for data frames or if rows_per_read is positive.


### report_progress

Integer value with options:
0: no progress is reported.
1: the number of processed rows is printed and updated.
2: rows processed and timings are reported.
3: rows processed and all timings are reported.


### xdf_compression_level

Integer in the range of -1 to 9. The higher the
value, the greater the amount of compression - resulting in smaller files but a
longer time to create them. If xdf_compression_level is set to 0, there will be
no compression and files will be compatible with the 6.0 release of Revolution
R Enterprise. If set to -1, a default level of compression will be used.


### strings_as_factors

Bool indicating whether or not to
automatically convert strings to factors on import. This can be overridden
by specifying “character” in column_classes and column_info. If True, the factor
levels will be coded in the order encountered. Since this factor level
ordering is row dependent, the preferred method for handling factor columns
is to use column_info with specified “levels”.


### kwargs

Additional arguments to be passed to the input data source.


## Returns

If an output_file is not specified, an output data frame is
returned. If an output_file is specified, an RxXdfData data source is
returned that can be used in subsequent revoscalepy analysis.


## Example



```
import os
from revoscalepy import RxOptions, RxXdfData, rx_data_step
sample_data_path = RxOptions.get_option("sampleDataDir")
kyphosis = RxXdfData(os.path.join(sample_data_path, "kyphosis.xdf"))

# Function to label whether the age in month is over 120 months.
month_limit = 120
new_col_name = "Over10Yr"
def transformFunc(data, cutoff, new_col_name):
    ret = data
    ret[new_col_name] = data.apply(lambda row: True if row.Age > cutoff else False, axis=1)
    return ret

kyphosis_df = rx_data_step(input_data=kyphosis, transform_function=transformFunc,
                           transform_objects={"cutoff": month_limit, "new_col_name": new_col_name})
kyphosis_df.head()
```

