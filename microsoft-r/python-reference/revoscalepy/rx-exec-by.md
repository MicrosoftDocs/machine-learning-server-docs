--- 
 
# required metadata 
title: "rx_exec_by: Partition Data by Key Values and Execute User Function on Each Partition" 
description: "Partition input data source by keys and apply user defined function on individual partitions. If input data source is already partitioned, apply user defined function on partitions directly. Currently supported in local, localpar, RxInSqlServer and RxSpark compute contexts." 
keywords: "execby, groupby" 
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

# rx_exec_by


 


## Usage



```
revoscalepy.rx_exec_by(input_data: typing.Union[revoscalepy.datasource.RxDataSource.RxDataSource,
    pandas.core.frame.DataFrame, str], keys: typing.List[str] = None,
    function: typing.Callable = None,
    function_parameters: dict = None,
    filter_function: typing.Callable = None,
    compute_context: revoscalepy.computecontext.RxComputeContext.RxComputeContext = None,
    **kwargs) -> revoscalepy.functions.RxExecBy.RxExecByResults
```





## Description

Partition input data source by keys and apply user defined function on
individual partitions. If input data source is already partitioned, apply
user defined function on partitions directly. Currently supported in local,
localpar, RxInSqlServer and RxSpark compute contexts.


## Arguments


### input_data

A data source object supported in currently active compute context,
e.g. ‘RxSqlServerData’ for ‘RxInSqlServer’. In ‘RxLocalSeq’
and ‘RxLocalParallel’, a character string specifying a ‘.xdf’ file, or a data frame object
can be also used.
If a Spark compute context is being used, this argument may also be an RxHiveData,
RxOrcData, RxParquetData or RxSparkDataFrame object or a Spark data frame object from pyspark.sql.DataFrame.


### keys

List of strings of variable names to be used for partitioning
the input data set.


### function

The user function to be executed. The user function takes ‘keys’
and ‘data’ as two required input arguments where ‘keys’ determines the partitioning
values and ‘data’ is a data source object of the corresponding partition.
‘data’ can be a RxXdfData object or a RxODBCData object, which can be transformed
to a pandas data frame by using rx_data_step. ‘keys’ is a dict where keys of the dict
are variable names and values are variable values of the partition.
The nodes or cores on which it is running are determined by the currently active compute context.


### function_parameters

A dict which defines a list of additional arguments for
the user function func.


### filter_function

An user function that takes a Panda data frame of keys/values as
an input argument, applies filter to the keys/values and returns a data frame
containing rows whose keys/values satisfy the filter conditions. The input data frame
has similar format to the results returned by rx_partition which comprises of partitioning
variables and an additional variable of partition data source. This filter_function
allows user to control what data partitions to be applied by the user function ‘function’.
‘filter_function’ currently is not supported in RxHadoopMR and RxSpark compute contexts.


### compute_context

A RxComputeContext object.


### kwargs

Additional arguments.


## Returns

A RxExecByResults object inherited from dataframe with each row to be the result of each partition.
The indexes of dataframe are keys, columns are ‘result’ and ‘status’.
‘result’: the object returned from the user function from each partition.
‘status’: ‘OK’, if the user function on each partition runs success, otherwise, the exception object.


## See also

[`rx_partition`](rx-partition.md).
[`RxXdfData`](RxXdfData.md).


## Example



```
###
# Run rx_exec_by in local compute context
###
import os
from revoscalepy import RxLocalParallel, rx_set_compute_context, rx_exec_by, RxOptions, RxXdfData
data_path = RxOptions.get_option("sampleDataDir")

input_file = os.path.join(data_path, "claims.xdf")
input_ds = RxXdfData(input_file)

# count number of rows
def count(data, keys):
    from revoscalepy import rx_data_step
    df = rx_data_step(data)
    return len(df)

rx_set_compute_context(RxLocalParallel())
local_cc_results = rx_exec_by(input_data = input_ds, keys = ["car.age", "type"], function = count)
print(local_cc_results)

###
# Run rx_exec_by in SQL Server compute context with data table specified
###
from revoscalepy import RxInSqlServer, rx_set_compute_context, rx_exec_by, RxOptions, RxSqlServerData
connection_string = 'Driver=SQL Server;Server=.;Database=RevoTestDb;Trusted_Connection=True;'
ds = RxSqlServerData(table = "AirlineDemoSmall", connection_string=connection_string)

def count(keys, data):
    from revoscalepy import rx_import
    df = rx_import(data)
    return len(df)

# filter function
def filter_weekend(partitioned_df):
    return (partitioned_df.loc[(partitioned_df.DayOfWeek == "Saturday") | (partitioned_df.DayOfWeek == "Sunday")])

sql_cc = RxInSqlServer(connection_string = connection_string, num_tasks = 4)
rx_set_compute_context(sql_cc)
sql_cc_results = rx_exec_by(ds, keys = ["DayOfWeek"], function = count, filter_function = filter_weekend)
print(sql_cc_results)

###
# Run rx_exec_by in RxSpark compute context
###
from revoscalepy import *

# start Spark app
spark_cc = rx_spark_connect()

# define function to compute average delay
def average_delay(keys, data):
    df = rx_data_step(data)
    return df['ArrDelay'].mean(skipna=True)

# define colInfo
col_info = {
    'ArrDelay': {'type': "numeric"},
    'CRSDepTime': {'type': "numeric"},
    'DayOfWeek': {'type': "string"}
}

# create text data source with airline data
text_data = RxTextData(
    file = "/share/sample_data/AirlineDemoSmall.csv",
    first_row_is_column_names = True,
    column_info = col_info,
    file_system = RxHdfsFileSystem())

# group text_data by day of week and get average delay on each day
result = rx_exec_by(text_data, keys = ["DayOfWeek"], function = average_delay)

# get result in pandas dataframe format and sort multiindex
result_dataframe = result.to_dataframe()
result_dataframe.sort_index()
print(result_dataframe)

# stop Spark app
rx_spark_disconnect(spark_cc)
```

