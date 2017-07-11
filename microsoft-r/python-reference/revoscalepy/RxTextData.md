--- 
 
# required metadata 
title: "Generate Text Data Source Object" 
description: "Main generator for class RxTextData, which extends RxDataSource." 
keywords: "datasource, text file" 
author: "bradsev" 
manager: "jhubbard" 
ms.date: "07/11/2017" 
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

## `RxTextData`


*Applies to:* SQL Server 2017, Machine Learning Services 9.3


### Usage



```
class revoscalepy.RxTextData(file: str, strings_as_factors: bool = False, column_classes: list = None, column_info: dict = None, vars_to_keep: list = None, vars_to_drop: list = None, missing_value_string: str = ‘NA’, rows_per_read: int = 500000, delimiter: str = None, combine_delimiters: bool = False, quote_mark: str = ‘”’, decimal_point: str = ‘.’, thousands_separator: str = None, read_date_format: str = ‘[%y[-][/]%m[-][/]%d]’, read_posixct_format: str = ‘%y[-][/]%m[-][/]%d [%H:%M[:%S]][%p]’, century_cutoff: int = 20, first_row_is_column_names=None, rows_to_sniff: int = 10000, rows_to_skip: int = 0, return_data_frame: bool = True, default_read_buffer_size: int = 10000, default_decimal_column_type: str = None, default_missing_column_type: str = None, write_precision: int = 7, strip_zeros: bool = False, quoted_delimiters: bool = False, is_fixed_format: bool = None, use_fast_read: bool = True, create_file_set: bool = None, rows_per_out_file: int = None, verbose: int = 0, check_vars_to_keep: bool = False, file_system: str = None, input_encoding: str = ‘utf-8’, write_factors_as_indexes: bool = False)
```




### Description

Main generator for class RxTextData, which extends RxDataSource.


### Arguments


##### file

character string specifying a text file. If it has an ‘.sts’
extension, it is interpreted as a fixed format schema file. If the column_info
argument contains start and width information, it is interpreted as a fixed
format data file. Otherwise, it is treated as a delimited text data file. See
the Details section for more information on using ‘.sts’ files.


##### return_data_frame

bool indicating whether or not to convert the
result from a list to a data frame (for use in rxReadNext only). If False, a
list is returned.


##### strings_as_factors

bool indicating whether or not to automatically
convert strings to factors on import. This can be overridden by specifying
“character” in column_classes and column_info. If True, the factor levels will
be coded in the order encountered. Since this factor level ordering is row
dependent, the preferred method for handling factor columns is to use
column_info with specified “levels”.


##### column_classes

list of strings specifying the column types to use when
converting the data. The element names for the vector are used to identify
which column should be converted to which type.

    Allowable column types are:
        ”bool” (stored as uchar),
        “integer” (stored as int32),
        “float32” (the default for floating point data for ‘.xdf’ files),
        “numeric” (stored as float64 as in R),
        “character” (stored as string),
        “factor” (stored as uint32),
        “ordered” (ordered factor stored as uint32. Ordered factors are treated

            the same as factors in RevoScaleR analysis functions.),

        ”int16” (alternative to integer for smaller storage space),
        “uint16” (alternative to unsigned integer for smaller storage space),
        “Date” (stored as Date, i.e. float64. Not supported if use_fast_read =

            True.)

        ”POSIXct” (stored as POSIXct, i.e. float64. Not supported if
            use_fast_read = True.)

    Note for “factor” and “ordered” types, the levels will be coded in the
    order encountered. Since this factor level ordering is row dependent, the
    preferred method for handling factor columns is to use column_info with
    specified “levels”.


##### column_info

list of named variable information lists. Each variable
information list contains one or more of the named elements given below. When
importing fixed format data, either column_info or an an ‘.sts’ schema file
should be supplied. For such fixed format data, only the variables specified
will be imported. For all text types, the information supplied for column_info
overrides that supplied for column_classes.

    Currently available properties for a column information list are:
        type: character string specifying the data type for the column. See
            column_classes argument description for the available types. If the
            type is not specified for fixed format data, it will be read as
            character data.

        newName: character string specifying a new name for the variable.
        description: character string specifying a description for the variable.
        levels: list of strings containing the levels when type = “factor”. If

            the levels property is not provided, factor levels will be determined
            by the values in the source column. If levels are provided, any value
            that does not match a provided level will be converted to a missing
            value.

        newLevels: new or replacement levels specified for a column of type
            “factor”. It must be used in conjunction with the levels argument.
            After reading in the original data, the labels for each level will be
            replaced with the newLevels.

        low: the minimum data value in the variable (used in computations using
            the F() function.

        high: the maximum data value in the variable (used in computations
            using the F() function.

        start: the left-most position, in bytes, for the column of a fixed
            format file respectively. When all elements of column_info have
            “start”, the text file is designated as a fixed format file. When none
            of the elements have it, the text file is designated as a delimited
            file. Specification of start must always be accompanied by
            specification of width.

        width: the number of characters in a fixed-width character column or
            the column of a fixed format file. If width is specified for a
            character column, it will be imported as a fixed-width character
            variable. Any characters beyond the fixed width will be ignored.
            Specification of width is required for all columns of a fixed format
            file (if not provided in an ‘.sts’ file).

        decimalPlaces: the number of decimal places.
        index: column index in the original delimited text data file. It is

            used as an alternative to naming the variable information list if the
            original delimited text file does not contain column names. Ignored if
            a name for the list is specified. Should not be used with fixed format
            files.


##### vars_to_keep

list of strings of variable names to include when reading
from the input data file. If None, argument is ignored. Cannot be used with
vars_to_drop.


##### vars_to_drop

list of strings of variable names to exclude when reading
from the input data file. If None, argument is ignored. Cannot be used with
vars_to_keep.


##### missing_value_string

character string containing the missing value
code. It can be an empty string: “”.


##### rows_per_read

number of rows to read at a time.


##### delimiter

character string containing the character to use as the
separator between variables. If None and the column_info argument does not
contain “start” and “width” information (which implies a fixed-formatted file),
the delimiter is auto-sensed from the list “,”, ”       “, “;”, and ” “.


##### combine_delimiters

bool indicating whether or not to treat
consecutive non-space (” “) delimiters as a single delimiter. Space ” ”
delimiters are always combined.


##### quote_mark

character string containing the quotation mark. It can be an
empty string: “”.


##### decimal_point

character string containing the decimal point. Not
supported when use_fast_read is set to True.


##### thousands_separator

character string containing the thousands
separator. Not supported when use_fast_read is set to True.


##### read_date_format

character string containing the time date format to
use during read operations. Not supported when use_fast_read is set to True.
Valid formats are:

    %c Skip a single character (see also %w).
    %Nc Skip N characters.
    %$c Skip the rest of the input string.
    %d Day of the month as integer (01-31).
    %m Month as integer (01-12) or as character string.
    %w Skip a whitespace delimited word (see also %c).
    %y Year. If less than 100, century_cutoff is used to determine the actual
    year.
    %Y Year as found in the input string.
    %%, %[, %] input the %, [, and ] characters from the input string.
    […] square brackets within format specifications indicate optional
    components; if present, they are used, but they need not be there.


##### read_posixct_format

character string containing the time date format to
use during read operations. Not supported when use_fast_read is set to True.
Valid formats are:

    %c Skip a single character (see also %w).
    %Nc Skip N characters.
    %$c Skip the rest of the input string.
    %d Day of the month as integer (01-31).
    %H Hour as integer (00-23).
    %m Month as integer (01-12) or as character string.
    %M Minute as integer (00-59).
    %n Milliseconds as integer (00-999).
    %N Milliseconds or tenths or hundredths of second.
    %p Character string defining ‘am’/‘pm’.
    %S Second as integer (00-59).
    %w Skip a whitespace delimited word (see also %c).
    %y Year. If less than 100, century_cutoff is used to determine the actual
    year.
    %Y Year as found in the input string.
    %%, %[, %] input the %, [, and ] characters from the input string.
    […] square brackets within format specifications indicate optional

components; if present, they are used, but they need not be there.


##### century_cutoff

integer specifying the changeover year between the
twentieth and twenty-first century if two-digit years are read. Values less
than century_cutoff are prefixed by 20 and those greater than or equal to
century_cutoff are prefixed by 19. If you specify 0, all two digit dates are
interpreted as years in the twentieth century. Not supported when use_fast_read
is set to True.


##### first_row_is_column_names

bool indicating if the first row
represents column names for reading text. If first_row_is_column_names is None,
then column names are auto- detected. The logic for auto-detection is: if the
first row contains all values that are interpreted as character and the second
row contains at least one value that is interpreted as numeric, then the first
row is considered to be column names; otherwise the first row is considered to
be the first data row. Not relevant for fixed format data. As for writing, it
specifies to write column names as the first row. If first_row_is_column_names
is None, the default is to write the column names as the first row.


##### rows_to_sniff

number of rows to use in determining column type.


##### rows_to_skip

integer indicating number of leading rows to ignore. Only
supported for use_fast_read = True.


##### default_read_buffer_size

number of rows to read into a temporary
buffer. This value could affect the speed of import.


##### default_decimal_column_type

Used to specify a column’s data type when
only decimal values (possibly mixed with missing (NA) values) are encountered
upon first read of the data and the column’s type information is not specified
via column_info or column_classes. Supported types are “float32” and “numeric”,
for 32-bit floating point and 64-bit floating point values, respectively.


##### default_missing_column_type

Used to specify a given column’s data type
when only missings (NAs) or blanks are encountered upon first read of the data
and the column’s type information is not specified via column_info or
column_classes. Supported types are “float32”, “numeric”, and “character” for
32-bit floating point, 64-bit floating point and string values, respectively.
Only supported for use_fast_read = True.


##### write_precision

integer specifying the precision to use when writing
numeric data to a file.


##### strip_zeros

bool value. If True, if there are only zeros after the
decimal point for a numeric, it will be written as an integer to a file.


##### quoted_delimiters

bool value. If True, delimiters within quoted
strings will be ignored. This requires a slower parsing process. Only
applicable to use_fast_read is set to True; delimiters within quotes are always
supported when use_fast_read is set to False.


##### is_fixed_format

bool value. If True, the input data file is treated
as a fixed format file. Fixed format files must have a ‘.sts’ file or a
column_info argument specifying the start and width of each variable. If False,
the input data file is treated as a delimited text file. If None, the text file
type (fixed or delimited text) is auto-determined.


##### use_fast_read

None or bool value. If True, the data file is
accessed directly by the Microsoft R Services Compute Engine. If False,
StatTransfer is used to access the data file. If None, the type of text import
is auto-determined. use_fast_read should be set to False if importing Date or
POSIXct data types, if the data set contains the delimiter character inside a
quoted string, if the decimalPoint is not “.”, if the thousandsSeparator is not
“,”, if the quoteMark is not “”“, or if combineDelimiters is set to True.
use_fast_read should be set to True if rowsToSkip or defaultMissingColType are
set. If use_fast_read is True, by default variables containing the values True
and False or T and F will be created as bool variables. use_fast_read cannot
be set to False if an HDFS file system is being used. If an incompatible
setting of use_fast_read is detected, a warning will be issued and the value
will be changed.


##### create_file_set

bool value or None. Used only when writing. If True,
a file folder of text files will be created instead of a single text file. A
directory will be created whose name is the same as the text file that would
otherwise be created, but with no extension. In the directory, the data will be
split across a set of text files (see rows_per_out_file below for determining how
many rows of data will be in each file). If False, a single text file will be
created. If None, a folder of files will be created if the input data set is a
composite XDF file or a folder of text files. This argument is ignored if the
text file is fixed format.


##### rows_per_out_file

numeric value or None. If a directory of text files
is being created, this will be the number of rows of data put into each
file in the directory.


##### verbose

integer value. If 0, no additional output is printed. If 1,
information on the text type (text or textFast) is printed.


##### check_vars_to_keep

bool value. If True variable names specified in
vars_to_keep will be checked against variables in the data set to make sure
they exist. An error will be reported if not found. If there are more than 500
variables in the data set, this flag is ignored and no checking is performed.


##### file_system

character string or RxFileSystem object indicating type of
file system; “native” or RxNativeFileSystem object can be used for the local
operating system.


##### input_encoding

character string indicating the encoding used by input
text. May be set to either “utf-8” (the default), or “gb18030”, a standard
Chinese encoding. Not supported when use_fast_read is set to True.


##### write_factors_as_indexes

bool. If True, when writing to an
RxTextData data source, underlying factor indexes will be written instead of
the string representations. Not supported when use_fast_read is set to False.


### Returns

object of class `RxTextData`.


### Example



```
import os
from revoscalepy import RxOptions, RxTextData, rx_lin_mod, rx_data_step
sample_data_path = RxOptions.get_option("sampleDataDir")
text_data_source = RxTextData(os.path.join(sample_data_path, "AirlineDemoSmall.csv"))
xdf_data_file = os.path.join(data_path, "AirlineDemoSmall.xdf")
results = rx_get_info(file=[text_data_source, xdf_data_file])
```

