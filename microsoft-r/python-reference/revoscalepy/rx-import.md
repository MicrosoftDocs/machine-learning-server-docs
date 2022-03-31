--- 
 
# required metadata 
title: "rx_import: Import Data to .xdf or data frame (revoscalepy)" 
description: "Import data and store as an .xdf file on disk or in-memory as a data.frame object." 
keywords: "import, datasource" 
author: "chuckheinzelman"
ms.author: "charlhe" 
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

# rx_import


 


## Usage



```
revoscalepy.rx_import(input_data: typing.Union[revoscalepy.datasource.RxDataSource.RxDataSource,
    pandas.core.frame.DataFrame, str], output_file=None,
    vars_to_keep: list = None, vars_to_drop: list = None,
    row_selection: str = None, transforms: dict = None,
    transform_objects: dict = None, transform_function: <built-
    in function callable> = None,
    transform_variables: dict = None,
    transform_packages: dict = None, append: str = None,
    overwrite: bool = False, number_rows: int = None,
    strings_as_factors: bool = None, column_classes: dict = None,
    column_info: dict = None, rows_per_read: int = None,
    type: str = None, max_rows_by_columns: int = None,
    report_progress: int = None, verbose: int = None,
    xdf_compression_level: int = None,
    create_composite_set: bool = None,
    blocks_per_composite_file: int = None)
```





## Description

Import data and store as an .xdf file on disk or in-memory as a data.frame object.


## Arguments


### input_data

A character string with the path for the data to import
(delimited, fixed format, ODBC, or XDF). Alternatively, a data source
object representing the input data source can be specified.
If a Spark compute context is being used, this argument may also be an RxHiveData,
RxOrcData, RxParquetData or RxSparkDataFrame object or a Spark data frame object from pyspark.sql.DataFrame.


### output_file

A character string representing the output ‘.xdf’ file
or an RxXdfData object.
If None, a data frame will be returned in memory.
If a Spark compute context is being used, this argument may also be an RxHiveData,
RxOrcData, RxParquetData or RxSparkDataFrame object.


### vars_to_keep

List of strings of variable names to include when
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
See rx_data_step for examples.


### transform_function

Name of the function that will be used to modify the data.
The variables used in the transformation function must be specified in transform_objects.
See rx_data_step for examples.


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

Bool value. If True, the existing output_file will be
overwritten. Ignored if a dataframe is returned.


### number_rows

Integer value specifying the maximum number of rows to
import. If set to -1, all rows will be imported.


### strings_as_factors

Bool value indicating whether or not to
automatically convert strings to factors on import. This can be overridden
by specifying “character” in column_classes and column_info. If True, the factor
levels will be coded in the order encountered. Since this factor level
ordering is row dependent, the preferred method for handling factor columns
is to use column_info with specified “levels”.


### column_classes

Dictionary of column name to strings specifying the column types
to use when converting the data. The element names for the vector are used to
identify which column should be converted to which type.

Allowable column types are:
    ”bool” (stored as uchar),
    “integer” (stored as int32),
    “float32” (the default for floating point data for ‘.xdf’ files),
    “numeric” (stored as float64 as in R),
    “character” (stored as string),
    “factor” (stored as uint32),
    “ordered” (ordered factor stored as uint32. Ordered factors are
treated the same as factors in RevoScaleR analysis functions.),
    ”int16” (alternative to integer for smaller storage space),
    “uint16” (alternative to unsigned integer for smaller storage space),
    “Date” (stored as Date, i.e. float64. Not supported for import
types “textFast”, “fixedFast”, or “odbcFast”.)
    ”POSIXct” (stored as POSIXct, i.e. float64. Not supported for
import types “textFast”, “fixedFast”, or “odbcFast”.)
    Note for “factor” and “ordered” types, the levels will be coded in the

order encountered. Since this factor level ordering is row dependent,
the preferred method for handling factor columns is to use column_info with
specified “levels”.

Note that equivalent types share the same bullet in the list above; for
some types we allow both ‘R-friendly’ type names, as well as our own,
more specific type names for ‘.xdf’ data.

Note also that specifying the column as a “factor” type is currently
equivalent to “string” - for the moment, if you wish to import a column
as factor data you must use the column_info argument, documented below.


### column_info

List of named variable information lists. Each variable
information list contains one or more of the named elements given below.
When importing fixed format data, either column_info or an ‘.sts’ schema
file should be supplied. For fixed format text files, only the variables
specified will be imported. For all text types, the information supplied
for column_info overrides that supplied for column_classes.
Currently available properties for a column information list are:

```
type: Character string specifying the data type for the column. See
    column_classes argument description for the available types. If the
    type is not specified for fixed format data, it will be read as
    character data.

newName: Character string specifying a new name for the variable.
    description: character string specifying a description for the
    variable.

levels: List of strings containing the levels when type =
    ”factor”. If the levels property is not provided, factor levels
    will be determined by the values in the source column. If levels
    are provided, any value that does not match a provided level will
    be converted to a missing value.

newLevels: New or replacement levels specified for a column of type
    “factor”. It must be used in conjunction with the levels argument.
    After reading in the original data, the labels for each level will
    be replaced with the newLevels.

low: The minimum data value in the variable (used in computations
    using the F() function.)

high: The maximum data value in the variable (used in computations
    using the F() function.)

start: The left-most position, in bytes, for the column of a fixed
    format file respectively. When all elements of column_info have start,
    the text file is designated as a fixed format file. When none of
    the elements have it, the text file is designated as a delimited
    file. Specification of start must always be accompanied by
    specification of width.

width: The number of characters in a fixed-width character column
    or the column of a fixed format file. If width is specified for a
    character column, it will be imported as a fixed-width character
    variable. Any characters beyond the fixed width will be ignored.
    Specification of width is required for all columns of a fixed
    format file.

decimalPlaces: The number of decimal places.
```

### rows_per_read

Number of rows to read at a time.


### type

Character string set specifying file type of input_data. This is
ignored if input_data is a data source. Possible values are:
“auto”: File type is automatically detected by looking at file extensions and argument values.

”textFast”: Delimited text import using faster, more limited import
    mode. By default variables containing the values True and False or T
    and F will be created as bool variables.

”text”: Delimited text import using enhanced, slower import mode. This
    allows for importing Date and POSIXct data types, handling the
    delimiter character inside a quoted string, and specifying decimal
    character and thousands separator. (See RxTextData.)

”fixedFast”: Fixed format text import using faster, more limited import
    mode. You must specify a ‘.sts’ format file or column_info specifications
    with start and width for each variable.

”fixed”: Fixed format text import using enhanced, slower import mode.
    This allows for importing Date and POSIXct data types and specifying
    decimal character and thousands separator.
    You must specify a ‘.sts’ format file or column_info specifications with
    start and width for each variable.

”odbcFast”: ODBC import using faster, more limited import mode.
“odbc”: ODBC import using slower, enhanced import on Windows. (See
    RxOdbcData.)


### max_rows_by_columns

The maximum size of a data frame that will be
read in if output_file is set to None, measured by the number of rows times the
number of columns. If the number of rows times the number of columns being
imported exceeds this, a warning will be reported and a smaller number of
rows will be read in than requested. If max_rows_by_columns is set to be too
large, you may experience problems from loading a huge data frame into
memory.


### report_progress

Integer value with options:
0: no progress is reported.
1: the number of processed rows is printed and updated.
2: rows processed and timings are reported.
3: rows processed and all timings are reported.


### verbose

Integer value. If 0, no additional output is printed. If 1,
information on the import type is printed if type is set to auto.


### xdf_compression_level

Integer in the range of -1 to 9. The higher
the value, the greater the amount of compression - resulting in smaller
files but a longer time to create them. If xdfCompressionLevel is set to 0,
there will be no compression and files will be compatible with the 6.0
release of Revolution R Enterprise. If set to -1, a default level of
compression will be used.


### create_composite_set

Bool value or None. If True, a composite
set of files will be created instead of a single ‘.xdf’ file. A directory
will be created whose name is the same as the ‘.xdf’ file that would
otherwise be created, but with no extension. Subdirectories ‘data’ and
‘metadata’ will be created. In the ‘data’ subdirectory, the data will be
split across a set of ‘.xdfd’ files (see blocks_per_composite_file below for
determining how many blocks of data will be in each file). In the
‘metadata’ subdirectory there is a single ‘.xdfm’ file, which contains the
meta data for all of the ‘.xdfd’ files in the ‘data’ subdirectory.


### blocks_per_composite_file

Integer value. If
create_composite_set=True, this will be the number of blocks put into
each ‘.xdfd’ file in the composite set. If the output_file is an RxXdfData
object, set the value for blocks_per_composite_file there instead.


### kwargs

Additional arguments to be passed directly to the underlying
data source objects to be imported.


## Returns

If an output_file is not specified, an output data frame is
returned. If an output_file is specified, an RxXdfData data source is
returned that can be used in subsequent revoscalepy analysis.


## Example



```
import os
from revoscalepy import rx_import, RxOptions, RxXdfData
sample_data_path = RxOptions.get_option("sampleDataDir")
ds = RxXdfData(os.path.join(sample_data_path, "kyphosis.xdf"))
kyphosis = rx_import(input_data = ds)
kyphosis.head()
```

