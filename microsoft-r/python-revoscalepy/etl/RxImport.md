--- 
 
# required metadata 
title: "Import Data to .xdf or data frame" 
description: "Import data into an ‘.xdf’ file or data.frame." 
keywords: "M, I, S, S, I, N, G,  , K, E, Y, W, O, R, D, S" 
author: "Microsoft Corporation Microsoft Technical Support" 
manager: "" 
ms.date: "06/27/2017" 
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

## rx_import_datasource


### Usage



```
revoscalepy.etl.RxImport.rx_import_datasource(input_data: revoscalepy.datasource.RxDataSource.RxDataSource, output_file=None, vars_to_keep: dict = None, vars_to_drop: dict = None, row_selection: str = None, transforms: dict = None, transform_objects: dict = None, transform_function: <built-in function callable> = None, transform_variables: dict = None, transform_packages: dict = None, transform_environment: dict = None, append: str = None, overwrite: bool = None, number_rows: int = None, strings_as_factors: bool = None, column_classes: dict = None, column_info: dict = None, rows_per_read: int = None, type: str = None, max_rows_by_columns: int = None, report_progress: int = None, verbose: int = None, xdf_compression_level: int = None, create_composite_set: bool = None, blocks_per_composite_file: int = None)
```




### Description

Import data into an ‘.xdf’ file or data.frame.


### Arguments


##### input_data

a character string with the path for the data to import
(delimited, fixed format, ODBC, or XDF). Alternatively, a data source
object representing the input data source can be specified. (See
RxTextData, and RxOdbcData.)


##### output_file

a character string representing the output ‘.xdf’ file,
or a RxXdfData object. If None, a data frame will be returned in memory.


##### vars_to_keep

character vector of variable names to include when
reading from the input data file. If None, argument is ignored. Cannot be
used with varsToDrop. Not supported for ODBC or fixed format text files.


##### vars_to_drop

character vector of variable names to exclude when
reading from the input data file. If None, argument is ignored. Cannot be
used with varsToKeep. Not supported for ODBC or fixed format text files.


##### row_selection

None. Not currently supported, reserved for future use.


##### transforms

None. Not currently supported, reserved for future use.


##### transform_objects

None. Not currently supported, reserved for
future use.


##### transform_function

variable transformation function. See
rxTransform for details.


##### transform_variables

character vector of input data set variables
needed for the transformation function. See rxTransform for details.


##### transform_packages

None. Not currently supported, reserved for future use.


##### transform_environment

None. Not currently supported, reserved for future use.


##### append

either “none” to create a new ‘.xdf’ file or “rows” to
append rows to an existing ‘.xdf’ file. If outFile exists and append is
“none”, the overwrite argument must be set to True. Ignored if a data frame
is returned.


##### overwrite

logical value. If True, the existing outData will be
overwritten. Ignored if a dataframe is returned.


##### number_rows

integer value specifying the maximum number of rows to
import. If set to -1, all rows will be imported.


##### strings_as_factors

logical indicating whether or not to
automatically convert strings to factors on import. This can be overridden
by specifying “character” in colClasses and colInfo. If True, the factor
levels will be coded in the order encountered. Since this factor level
ordering is row dependent, the preferred method for handling factor columns
is to use colInfo with specified “levels”.


##### column_classes

character vector specifying the column types to use
when converting the data. The element names for the vector are used to
identify which column should be converted to which type.

    Allowable column types are:
        ”logical” (stored as uchar),
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
the preferred method for handling factor columns is to use colInfo with
specified “levels”.

    Note that equivalent types share the same bullet in the list above; for

some types we allow both ‘R-friendly’ type names, as well as our own,
more specific type names for ‘.xdf’ data.

    Note also that specifying the column as a “factor” type is currently

equivalent to “string” - for the moment, if you wish to import a column
as factor data you must use the colInfo argument, documented below.


##### column_info

list of named variable information lists. Each variable
information list contains one or more of the named elements given below.
When importing fixed format data, either colInfo or an an ‘.sts’ schema
file should be supplied. For fixed format text files, only the variables
specified will be imported. For all text types, the information supplied
for colInfo overrides that supplied for colClasses.
Currently available properties for a column information list are:

    type: character string specifying the data type for the column. See
        colClasses argument description for the available types. If the
        type is not specified for fixed format data, it will be read as
        character data.

    newName: character string specifying a new name for the variable.
        description: character string specifying a description for the
        variable.

    levels: character vector containing the levels when type =
        ”factor”. If the levels property is not provided, factor levels
        will be determined by the values in the source column. If levels
        are provided, any value that does not match a provided level will
        be converted to a missing value.

    newLevels: new or replacement levels specified for a column of type
        “factor”. It must be used in conjunction with the levels argument.
        After reading in the original data, the labels for each level will
        be replaced with the newLevels.

    low: the minimum data value in the variable (used in computations
        using the F() function.)

    high: the maximum data value in the variable (used in computations
        using the F() function.)

    start: the left-most position, in bytes, for the column of a fixed
        format file respectively. When all elements of colInfo have start,
        the text file is designated as a fixed format file. When none of
        the elements have it, the text file is designated as a delimited
        file. Specification of start must always be accompanied by
        specification of width.

    width: the number of characters in a fixed-width character column
        or the column of a fixed format file. If width is specified for a
        character column, it will be imported as a fixed-width character
        variable. Any characters beyond the fixed width will be ignored.
        Specification of width is required for all columns of a fixed
        format file.

    decimalPlaces: the number of decimal places.


##### rows_per_read

number of rows to read at a time.


##### type

character string set specifying file type of inData. This is
ignored if inData is a data source. Possible values are:
“auto”: file type is automatically detected by looking at file

    extensions and argument values.

”textFast”: delimited text import using faster, more limited import
    mode. By default variables containing the values True and False or T
    and F will be created as logical variables.

”text”: delimited text import using enhanced, slower import mode (not
    supported with HDFS). This allows for importing Date and POSIXct data
    types, handling the delimiter character inside a quoted string, and
    specifying decimal character and thousands separator. (See RxTextData.)

”fixedFast”: fixed format text import using faster, more limited import
    mode. You must specify a ‘.sts’ format file or colInfo specifications
    with start and width for each variable.

”fixed”: fixed format text import using enhanced, slower import mode
    (not supported with HDFS). This allows for importing Date and POSIXct
    data types and specifying decimal character and thousands separator.
    You must specify a ‘.sts’ format file or colInfo specifications with
    start and width for each variable.

”sas”: SAS data files. (See RxSasData.)
“spss”: SPSS data files. (See RxSpssData.)
“odbcFast”: ODBC import using faster, more limited import mode.
“odbc”: ODBC import using slower, enhanced import on Windows. (See

    RxOdbcData.)


##### max_rows_by_columns

the maximum size of a data frame that will be
read in if outData is set to None, measured by the number of rows times the
number of columns. If the number of rows times the number of columns being
imported exceeds this, a warning will be reported and a smaller number of
rows will be read in than requested. If maxRowsByCols is set to be too
large, you may experience problems from loading a huge data frame into
memory.


##### report_progress

integer value with options:
0: no progress is reported.
1: the number of processed rows is printed and updated.
2: rows processed and timings are reported.
3: rows processed and all timings are reported.


##### verbose

integer value. If 0, no additional output is printed. If 1,
information on the import type is printed if type is set to auto.


##### xdf_compression_level

integer in the range of -1 to 9. The higher
the value, the greater the amount of compression - resulting in smaller
files but a longer time to create them. If xdfCompressionLevel is set to 0,
there will be no compression and files will be compatible with the 6.0
release of Revolution R Enterprise. If set to -1, a default level of
compression will be used.


##### create_composite_set

logical value or None. If True, a composite
set of files will be created instead of a single ‘.xdf’ file. A directory
will be created whose name is the same as the ‘.xdf’ file that would
otherwise be created, but with no extension. Subdirectories ‘data’ and
‘metadata’ will be created. In the ‘data’ subdirectory, the data will be
split across a set of ‘.xdfd’ files (see blocksPerCompositeFile below for
determining how many blocks of data will be in each file). In the
‘metadata’ subdirectory there is a single ‘.xdfm’ file, which contains the
meta data for all of the ‘.xdfd’ files in the ‘data’ subdirectory. When the
compute context is RxHadoopMR a composite set of files is always created.


##### blocks_per_composite_file

integer value. If
createCompositeSet=True, and if the compute context is not RxHadoopMR, this
will be the number of blocks put into each ‘.xdfd’ file in the composite
set. When importing is being done on Hadoop using MapReduce, the number of
rows per ‘.xdfd’ file is determined by the rows assigned to each MapReduce
task, and the number of blocks per ‘.xdfd’ file is therefore determined by
rowsPerRead. If the outFile is an RxXdfData object, set the value for
blocksPerCompositeFile there instead.


##### kwargs

additional arguments to be passed directly to the underlying
data source objects to be imported.


### Returns

If an output_file is not specified, an output data frame is
returned. If an output_file is specified, an RxXdfData data source is
returned that can be used in subsequent RevoScalePy analysis.


### Author

Microsoft Corporation [Microsoft Technical Support](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409.md)


### See also


### Example



```
import os
from revoscalepy import rx_import_datasource, RxOptions, RxXdfData
sample_data_path = RxOptions.get_option("sampleDataDir")
ds = RxXdfData(os.path.join(sample_data_path, "kyphosis.xdf"))
kyphosis = rx_import_datasource(input_data = ds)
```

