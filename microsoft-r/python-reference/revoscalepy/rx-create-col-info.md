--- 
 
# required metadata 
title: "rx_create_col_info: Function to generate a ‘column_info’ ordered dictionary from a data source" 
description: "From the data source, generates an ordered dictionary of dictionaries with variable names as keys and column information dictionaries as values. It can be used in rx_import or an RxDataSource constructor. This function can be used to ensure consistent categorical (factor) levels when importing a series of text files to xdf. It is also useful for repeated analysis on non-xdf data sources." 
keywords: "column, information, col, info" 
author: "bradsev" 
manager: "jhubbard" 
ms.date: "09/11/2017" 
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

# `rx_create_col_info`


 


## Usage



```
revoscalepy.rx_create_col_info(data: typing.Union[revoscalepy.datasource.RxDataSource.RxDataSource,
    str, pandas.core.frame.DataFrame,
    revoscalepy.functions.RxGetInfoXdf.GetVarInfoResults],
    include_low_high: bool = False, factors_only: bool = False,
    vars_to_keep: list = None, sort_levels: bool = False,
    compute_info: bool = True,
    use_factor_index: bool = False) -> collections.OrderedDict
```





## Description

From the data source, generates an ordered dictionary of dictionaries with variable names as keys
and column information dictionaries as values. It can be used in rx_import or an RxDataSource constructor.
This function can be used to ensure consistent categorical (factor) levels when importing
a series of text files to xdf. It is also useful for repeated analysis on non-xdf data sources.

:param data : An RxDataSource object, a character string containing an ‘.xdf’ file name, or a data frame.
    An object returned from rx_get_var_info is also supported.

:param include_low_high : Bool value. If True, the low/high values will be included in the column_info object.
    Note that this will override any actual low/high values in the data set if the column_info object
    is applied to a different data source.

:param factors_only : Bool value. If True, only column information for categorical (factor) variables
    will be included in the output.

:param vars_to_keep : None to include all variables, or list of strings of variable names to include
:param sort_levels : Bool value. If True, categorical (factor) levels will be sorted.

    If categorical levels represent integers, they will be put in numeric order.

:param compute_info : Bool value. If True, a pass through the data will be taken for non-xdf data sources to compute
    categorical (factor) levels and low/high values.

:param use_factor_index : Bool value. If True, the factor_index variable type will be used instead of factor

:return : Ordered dict column_info of named variable information dicts. It can be used as input for rx_import
    and in data sources such as RxTextData and RxSqlServerData
    Each variable information dict contains one or more of the named elements given below..

        type: Character string specifying the data type for the column.
        levels: List of strings containing the levels when type = “factor”.
        low: The minimum data value in the variable (used in computations using the F() function).

            For factors, it is the index of the lowest level.

        high: The maximum data value in the variable (used in computations using the F() function).
            For factors, it is the index of the highest level.


## See also

`RxDataSource`,
`RxTextData`,
`RxSqlServerData`,
`RxOdbcData`,
`RxXdfData`,
`rx_import`.


## Example



```
 :execute:
 :print_output:

 import os
 from revoscalepy import RxOptions, RxTextData, rx_create_col_info, rx_import, rx_logit, rx_predict
 # Get the low/high values and factor levels before using a data source
 # for rx_import or analysis

 # Create a text data source, specifying that 'yearsEmploy' should be a factor
 mort = os.path.join(RxOptions.get_option("sampleDataDir"), "mortDefaultSmall2000.csv")
 mort_ds = RxTextData(file = mort, column_classes = {'yearsEmploy' : "factor", 'default' : "logical"})

 # By default, rx_create_col_info will make a pass through the data to compute factor levels
 # and low/high values.  We'll also request that the levels be sorted
 mort_column_info = rx_create_col_info(data = mort_ds, include_low_high = True, sort_levels = True)

 # Re-create the data source, now using the computed colInfo
 mort_ds = RxTextData(file = mort, column_info = mort_column_info)
 # Import the data
 mort_df = rx_import(mort_ds)
 print(mort_df['yearsEmploy'])

 # Or use the text data source directly in an analysis
 # (not needing a pass through the data to compute the factor levels)
 logit_obj = rx_logit('default~yearsEmploy', data = mort_ds)

 ##############################################################################################

 # Train a model on one imported data set, then score using another
 # Train a model on the first year of the data, importing it from text to a data frame

 mort1 = os.path.join(RxOptions.get_option("sampleDataDir"), "mortDefaultSmall2000.csv")
 mort1_ds = RxTextData(file = mort1, column_classes = {'yearsEmploy' : "factor", 'default' : "logical"})
 # Since we haven't specified factor levels, they will be created 'first come, first serve'
 mort1_df = rx_import(mort1_ds)
 mort1_df['yearsEmploy'].cat.categories
 # Estimate a logit model
 logit_obj = rx_logit('default~yearsEmploy', data = mort1_df)

 # Now import the second year of data
 mort2 = os.path.join(RxOptions.get_option("sampleDataDir"), "mortDefaultSmall2001.csv")
 mort2_ds = RxTextData(file = mort2, column_classes = {'yearsEmploy' : "factor", 'default' : "logical"})
 mort2_df = rx_import(mort2_ds)
 # The levels are in a different order
 mort2_df['yearsEmploy'].cat.categories

 # If we try to use the model estimated from the first data set to predict on the seoond,
 # pred_out = rx_predict(logit_obj, data = mort2_df)
 # We will get an error
 #ERROR: order of factor levels in the data are inconsistent with
 #the order of the model coefficients

 # Instead, we can extract the col_info from the first data set
 mort_col_info = rx_create_col_info(data = mort1_df)

 # And use it when importing the second
 mort2_ds = RxTextData(file = mort2, column_info = mort_col_info)
 mort2_df = rx_import(mort2_ds)
 pred_out = rx_predict(logit_obj, data = mort2_df)
 pred_out.head()

###############################################################
# Generating Column Information for a SQL Server Data Source
###############################################################
from revoscalepy import RxSqlServerData, rx_create_col_info, rx_set_temp_compute_context, RxLocalSeq
sql_source = RxSqlServerData(connection_string="Driver=SQL Server;Server=.;Database=RevoTestDb;Trusted_Connection=TRUE;",
                      table="AirlineDemoSmall",
                      rows_per_read=500000,
                      string_as_factors = True)

# currently, column_info for RxSqlServerData can be only created in local compute context
with rx_set_temp_compute_context(RxLocalSeq()):
    results = rx_create_col_info(sql_source)
```

