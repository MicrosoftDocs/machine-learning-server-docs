--- 
 
# required metadata 
title: "RxSqlServerData: Generate SqlServer Data Source Object" 
description: "Main generator for class RxSqlServerData, which extends RxDataSource." 
keywords: "datasource, sql" 
author: "bradsev" 
manager: "jhubbard" 
ms.date: "08/31/2017" 
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

# `RxSqlServerData`


**Applies to: SQL Server 2017**


## Usage



```
class revoscalepy.RxSqlServerData(connection_string: str = None, table: str = None, sql_query: str = None, row_buffering: bool = True, return_data_frame: bool = True, string_as_factors: bool = False, column_classes: dict = None, column_info: dict = None, rows_per_read: int = 50000, verbose: bool = False, use_fast_read: bool = True, server: str = None, database_name: str = None, user: str = None, password: str = None, write_factors_as_indexes: bool = False, **kwargs)
```




## Description

Main generator for class RxSqlServerData, which extends RxDataSource.


## Arguments


### connection_string

None or character string specifying the
connection string.


### table

None or character string specifying the table name. Cannot be
used with sql_query.


### sql_query

None or character string specifying a valid SQL select
query. Cannot contain hidden characters such as tabs or newlines. Cannot be
used with table. If you want to use TABLESAMPLE clause in sqlQuery, set
row_buffering argument to False.


### row_buffering

bool specifying whether or not to buffer rows on
read from the database. If you are having problems with your ODBC driver,
try setting this to False.


### return_data_frame

bool indicating whether or not to convert the
result from a list to a data frame (for use in rxReadNext only). If False,
a list is returned.


### string_as_factors

bool indicating whether or not to
automatically convert strings to factors on import. This can be overridden
by specifying “character” in column_classes and column_info. If True, the
factor levels will be coded in the order encountered. Since this factor
level ordering is row dependent, the preferred method for handling factor
columns is to use column_info with specified “levels”.


### column_classes

dictionary of column name to strings specifying the
column types to use when converting the data. The element names for the
vector are used to  identify which column should be converted to which type.
Allowable column types are:
“bool” (stored as uchar),
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

list of named variable information lists. Each variable
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

number of rows to read at a time.


### verbose

integer value. If 0, no additional output is printed. If 1,
information on the odbc data source type (odbc or odbcFast) is printed.


### use_fast_read

bool specifying whether or not to use a direct
ODBC connection. On Linux systems, this is the only ODBC connection
available.


### server

Target SQL Server instance. Can also be specified in the
connection string with the Server keyword.


### database_name

Database on the target SQL Server instance. Can also
be specified in the connection string with the Database keyword.


### user

SQL login to connect to the SQL Server instance. SQL login can
also be specified in the connection string with the uid keyword.


### password

Password associated with the SQL login. Can also be
specified in the connection string with the pwd keyword.


### write_factors_as_indexes

bool. If True, when writing to an
RxOdbcData data source, underlying factor indexes will be written instead
of the string representations.


### kwargs

additional arguments to be passed directly to the underlying
functions.


## Returns

object of class `RxSqlServerData`.


## Example



```
from revoscalepy import RxSqlServerData, rx_data_step

connection_string="Driver=SQL Server;Server=.;Database=RevoTestDB;Trusted_Connection=True"
query="select top 100 [ArrDelay],[CRSDepTime],[DayOfWeek] FROM airlinedemosmall"

ds = RxSqlServerData(sql_query = query, connection_string = connection_string)
df = rx_data_step(ds)
df.head()
```

