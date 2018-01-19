--- 
 
# required metadata 
title: "RxOdbcData: Generate ODBC Data Source Object" 
description: "Main generator for class RxOdbcData, which extends RxDataSource." 
keywords: "odbc, datasource" 
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

# RxOdbcData


 



```
revoscalepy.RxOdbcData(connection_string: str = None,
    table: str = None, sql_query: str = None,
    dbms_name: str = None, database_name: str = None,
    use_fast_read: bool = True, trim_space: bool = True,
    row_buffering: bool = True, return_data_frame: bool = True,
    string_as_factors: bool = False, column_classes: dict = None,
    column_info: dict = None, rows_per_read: int = 500000,
    verbose: int = 0, write_factors_as_indexes: bool = False,
    **kwargs)
```





## Description

Main generator for class RxOdbcData, which extends RxDataSource.


## Arguments


### connection_string

None or character string specifying the
connection string.


### table

None or character string specifying the table name. Cannot be
used with sqlQuery.


### sql_query

None or character string specifying a valid SQL select
query. Cannot be used with table.


### dbms_name

None or character string specifying the Database
Management System (DBMS) name.


### database_name

None or character string specifying the name of the
database.


### use_fast_read

Bool specifying whether or not to use a direct
ODBC connection.


### trim_space

Bool specifying whether or not to trim the white
character of string data for reading.


### row_buffering

Bool specifying whether or not to buffer rows on
read from the database. If you are having problems with your ODBC driver,
try setting this to False.


### return_data_frame

Bool indicating whether or not to convert the
result from a list to a data frame (for use in rxReadNext only). If False,
a list is returned.


### string_as_factors

Bool indicating whether or not to
automatically convert strings to factors on import. This can be overridden
by specifying “character” in column_classes and column_info. If True, the
factor levels will be coded in the order encountered. Since this factor
level ordering is row dependent, the preferred method for handling factor
columns is to use column_info with specified “levels”.


### column_classes

Dictionary of column name to strings specifying the
column types to use when converting the data. The element names for the
vector are used to identify which column should be converted to which type.

Allowable column types are:

    ”bool” (stored as uchar),
    “integer” (stored as int32),
    “float32” (the default for floating point data for ‘.xdf’ files),
    “numeric” (stored as float64 as in R),
    “character” (stored as string),
    “factor” (stored as uint32),
    “int16” (alternative to integer for smaller storage space),
    “uint16” (alternative to unsigned integer for smaller storage space),
    “Date” (stored as Date, i.e. float64)

Note for “factor” type, the levels will be coded in the order
encountered. Since this factor level ordering is row dependent, the
preferred method for handling factor columns is to use column_info with
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
The information supplied for column_info overrides that supplied for
column_classes.

Currently available properties for a column information list are:

    type: character string specifying the data type for the column. See
        column_classes argument description for the available types. Specify
        “factorIndex” as the type for 0-based factor indexes. levels must also
        be specified.

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


### rows_per_read

Number of rows to read at a time.


### verbose

integer value. If 0, no additional output is printed. If 1,
information on the odbc data source type (odbc or odbcFast) is printed.


### write_factors_as_indexes

Bool value, If True, when writing to an
RxOdbcData data source, underlying factor indexes will be written instead
of the string representations.


### kwargs

Additional arguments to be passed directly to the underlying
functions.


## Returns

Object of class RxOdbcData.


## Example



```
from revoscalepy import RxOdbcData, RxOptions, rx_write_object, rx_read_object, RxXdfData
from numpy import array_equal
import os

# Establish a connection to an ODBC data source
connection_string = 'Driver=SQL Server;Server=.;Database=RevoTestDb;Trusted_Connection=True;'
dest = RxOdbcData(connection_string, table = "data")

# Write an array to the database
my_array = [1,2,3]
rx_write_object(dest = dest, key = "my_array", value = my_array)

# Retrieve the array from the database
array_ds = rx_read_object(src = dest, key = "my_array")
array_equal(my_array, array_ds) # True

# Write a XDF object to the database
sample_data_path  = RxOptions.get_option("sampleDataDir")
kyphosis = RxXdfData(os.path.join(sample_data_path, "kyphosis.xdf"))
rx_write_object(dest = dest, key = "kyphosis", value = kyphosis)

# Retrieve the data from the database
kyphosis_ds = rx_read_object(src = dest, key="kyphosis")
```

