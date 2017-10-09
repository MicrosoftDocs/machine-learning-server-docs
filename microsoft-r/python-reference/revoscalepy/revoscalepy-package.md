--- 
 
# required metadata 
title: "revoscalepy package for Python (SQL Server Machine Learning Server) | Microsoft Docs" 
description: "Function help reference for the revoscalepy Python package of SQL Server Machine Learning Server." 
keywords: "" 
author: "HeidiSteen" 
manager: "jhubbard" 
ms.author: "heidist"
ms.date: "09/29/2017" 
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

# revoscalepy package

The **revoscalepy** module is a collection of portable, scalable and distributable Python functions used for analyzing data at scale, at the point of origin. It includes data transformation and manipulation, visualization, predictions, and statistical analysis functions. You can do descriptive statistics, linear regression, logistic regression, classification and regression trees, and decision forests. The library also includes functions for controlling jobs, serializing data, and performing common utility tasks.

Functions run on the **revoscalepy** interpreter, built on open source Python, but extended to leverage the multithreaded and multinode architecture of the host platform.

**revoscalepy** runs locally on all platforms and remotely on Spark and SQL Server 2017 with Python. On a Spark cluster, for functions that execute on parallel architecture, the workload executes on all available cores and nodes. This capability translates into high performance computing for predictive and statistical analysis of big data in your cluster.  

| Package details | |
|--------|-|
| Version: |  9.2.1 |
| Runs on: | [Machine Learning Server 9.2.1](../../what-is-machine-learning-server.md) </br>[SQL Server 2017 Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/python/sql-server-python-services) </br>[SQL Server 2017 Machine Learning Server (Standalone)](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone)(https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone#whats-new-in-microsoft-machine-learning-server) |
| Built on: | [Anaconda 4.2](https://www.continuum.io/why-anaconda) distribution of [Python 3.5](https://www.python.org/doc) (included when you add Python support during installation). |

## How to use revoscalepy

The **revoscalepy** module is found in Machine Learning Server or SQL Server Machine Learning when you add Python to your installation. You get the full collection of proprietary packages plus a Python distribution with its modules and interpreter. You can use any Python IDE to write Python script calling functions in **revoscalepy**, but the script must run on a computer having our proprietary modules.

The **revoscalepy** module runs locally on all platforms, and remotely in a [RxSpark](RxSpark.md) or RxInSQLServer compute context. For a review of common tasks, see [How to use revoscalepy with Spark](../../python/how-to-revoscalepy.md).

> [!Note]
> Some function names begin with `rx-` and others with `Rx`. The `Rx` function name prefix is used for class constructors for data sources and compute contexts.

### In a local compute context

**Revoscalepy** operations include statistical analysis, linear and logistic regressions, and predictive analytics. On a standalone Linux or windows system, data and operations are local to the machine. On Spark, a local compute context means that data and operations are local to the cluster. 

### In a remote compute context

In a remote compute context, the script running on a local Machine Learning Server shifts execution to a remote Machine Learning Server. For **revoscalepy**, this is supported for an [RxSpark](RxSpark.md) cluster. For example, script running on Windows might shift execution to a Spark cluster to process data there. Similarly, script might shift to a remote SQL Server instance using a [RxInSqlServer](RxInSqlServer.md) compute context. 

For SQL Server, there are two primary use cases: 

+ Call Python functions in T-SQL script or stored procedures running on SQL Server.  

+ Call **revoscalepy** functions in Python script executing in a SQL Server [compute context](../../r/concept-what-is-compute-context.md). In your script, you can set a compute context to shift execution of **revoscalepy** operations to a remote SQL Server instance that has the **revoscalepy** interpreter.

## Functions by category

This section lists the functions by category to give you an idea of how each one is used. You can also use the table of contents to find functions in alphabetical order.

## 1-Compute context functions

| Function | Description |
|----------|-------------|
|[RxInSqlServer](RxInSqlServer.md) | Creates a compute context for running revoscalepy analyses inside a remote Microsoft SQL Server. |
|[RxLocalSeq](RxLocalSeq.md) | This is the default but you can call it switch back to a local compute context if your script runs in multiple. Computations using rx_exec will be processed sequentially. |
|[rx_get_compute_context](rx-get-compute-context.md) | Returns the current compute context.|
|[rx_set_compute_context](rx-set-compute-context.md) | Change the compute context to a different one.|
|[RxSpark](RxSpark.md) | Creates a compute context for running revoscalepy analyses in a remote Spark cluster. |
|[rx_get_pyspark_connection](rx-get-pyspark-connection.md) | Gets a connection to a PySpark data set, in support of revoscalepy and PySpark interoperability. |
|[rx_spark_connect](rx-spark-connect.md) | Creates a persistent Spark Connection. |
|[rx_spark_disconnect](rx-spark-disconnect.md) | Closes the connection. |



## 2-Data source functions

Data sources are used by [microsoftml functions](../microsoftml/microsoftml-package.md) as well as **revoscalepy**.

| Function | Compute Context | Description | 
|----------|-----------------|-------------|
|[RxDataSource](RxDataSource.md) | All | Base class for all revoscalepy data sources.|
|[RxHdfsFileSystem](RxHdfsFileSystem.md) | Local, [RxSpark](RxSpark.md) | Data source is accessed through HDFS instead of Linux. |
|[RxNativeFileSystem](RxNativeFileSystem.md) | Local, [RxSpark](RxSpark.md) | Data source is accessed through Linux instead of HDFS. |
|[RxHiveData](RxHiveData.md) | Local, [RxSpark](RxSpark.md) | Generates a data source object from a Hive data file.|
|[RxTextData](RxTextData.md) | Local, [RxSpark](RxSpark.md) | Generates a data source object from a text data file.|
|[RxXdfData](RxXdfData.md) | All | Generates a data source object from an XDF data source.|
|[RxOdbcData](RxOdbcData.md) | All | Generates a data source object from an ODBC data source.|
|[RxOrcData](RxOrcData.md) | Local, [RxSpark](RxSpark.md)| Generates a data source object from an Orc data file.|
|[RxParquetData](RxParquetData.md) | Local, [RxSpark](RxSpark.md) | Generates a data source object from a Parquet data file.|
|[RxSparkData](RxSparkData.md) | Local, [RxSpark](RxSpark.md) | Generates a data source object from a Spark data source.|
|[RxSparkDataFrame](RxSparkDataFrame.md) | Local, [RxSpark](RxSpark.md) | Generates a data source object from a Spark data frame.|
|[rx_spark_cache_data](rx-spark-cache-data.md) | Local, [RxSpark](RxSpark.md) | Generates a data source object from cached data.|
|[rx_spark_list_data](rx-spark-list-data.md) | Local, [RxSpark](RxSpark.md) | Generates a data source object from a list.|
|[rx_spark_remove_data](rx-spark-remove-data.md) | Local, [RxSpark](RxSpark.md) | Deletes the Spark cached data source object.|
|[RxSqlServerData](RxSqlServerData.md) | Local, [RxInSqlServer](RxInSqlServer.md)  | Generates a data source object from a SQL table or query.|

## 3-Data manipulation (ETL) functions

| Function | Compute Context | Description |
|----------|-----------------|-------------|
|[rx_import](rx-import.md) | All | Import data into an .xdf file or data frame.|
|[rx_data_step](rx-data-step.md) | All | Transform data from an input data set to an output data set.|

## 4-Analytic functions

| Function | Compute Context | Description |
|----------|-----------------|-------------|
|[rx_exec_by](rx-exec-by.md) | Local, [RxSpark](RxSpark.md) | Execute an arbitrary function in parallel on multiple data nodes. |
| [rx_partition](rx-partition.md) | Local, [RxSpark](RxSpark.md) | Partition input data sources by key values and save the results to a partitioned .xdf on disk. |
|[rx_summary](rx-summary.md) | All | Produce univariate summaries of objects in revoscalepy. |
|[rx_lin_mod](rx-lin-mod.md) | All | Fit linear models on small or large data. |
|[rx_logit](rx-logit.md) | All | Use rx_logit to fit logistic regression models for small or large data. |
|[rx_dtree](rx-dtree.md) | All | Fit classification and regression trees on an ‘.xdf’ file or data frame for small or large data using parallel external memory algorithm. |
|[rx_dforest](rx-dforest.md) | All | Fit classification and regression decision forests on an ‘.xdf’ file or data frame for small or large data using parallel external memory algorithm. |
|[rx_btrees](rx-btrees.md) | All | Fit stochastic gradient boosted decision trees on an ‘.xdf’ file or data frame for small or large data using parallel external memory algorithm. |
|[rx_predict_default](rx-predict-default.md) | All | Compute predicted values and residuals using rx_lin_mod and rx_logit objects. |
|[rx_predict_rx_dforest](rx-predict-rx-dforest.md) | All | Calculate predicted or fitted values for a data set from an rx_dforest or rx_btrees object. |
|[rx_predict_rx_dtree](rx-predict-rx-dtree.md) | All | Calculate predicted or fitted values for a data set from an rx_dtree object. |

## 5-Job functions

In an [RxSpark](RxSpark.md) context, job management is built in. You only need job functions if you want to manually control the Yarn queue.

| Function | Compute Context | Description |
|----------|-----------------|-------------|
|[rx_exec](rx-exec.md) | All | Allows distributed execution of a function in parallel across nodes (computers) or cores of a “compute context” such as a cluster. |
|[rx_cancel_job](rx-cancel-job.md) | All | Removes all job-related artifacts from the distributed computing resources, including any job results. |
|[rx_cleanup_jobs](rx-cleanup-jobs.md) | All | Removes the artifacts for a specific job. |
|[RxRemoteJob class](RxRemoteJob.md) | All | Closes the remote job, purging all associated job-related data.|
|[RxRemoteJobStatus](RxRemoteJobStatus.md) | All | Represents the execution status of a remote Python job.|
|[rx_get_job_info](rx-get-job-info.md) | All | Contains complete information on the job’s compute context as well as other information needed by the distributed computing resources.|
|[rx_get_job_output](rx-get-job-output.md) | All | Returns console output for the nodes participating in a distributed computing job.|
|[rx_get_job_results](rx-get-job-results.md) | All | Returns results of the run or a message stating why results are not available.|
|[rx_get_job_status](rx-get-job-status.md) | All | Obtain distributed computing processing status for the specified job. |
|[rx_get_jobs](rx-get-jobs.md) | All | Returns a list of job objects associated with the given compute context and matching the specified parameters. |
|[rx_wait_for_job](rx-wait-for-job.md) | All | Block on an existing distributed job until completion, effectively turning a non-blocking job into a blocking job.|


## 6-Serialization functions

| Function | Compute Context | Description |
|----------|---------------- |-------------|
|[rx_serialize_model](rx-serialize-model.md)  | All | Serialize a given python model. |
|[rx_read_object](rx-read-object.md) | All | Retrieves an ODBC data source objects. |
|[rx_read_xdf](rx-read-xdf.md) | All | Read data from an .xdf file into a data frame.|
|[rx_write_object](rx-write-object.md)   | All | Stores an ODBC data source object. |
|[rx_delete_object](rx-delete-object.md)  | All | Deletes an object from the ODBC data source. |
|[rx_list_keys](rx-list-keys.md)  | All | Enumerates all keys or versions for a given key, depending on the parameters. |


## 7-Utility functions

| Function | Compute Context | Description |
|----------|---------------- |-------------|
|[RxOptions](RxOptions.md) | All | Specify and retrieve options needed for **revoscalepy** computations. |
|[rx_get_info](rx-get-info.md) | All | Get basic information about a **revoscalepy** data source or data frame. |
|[rx_get_var_info](rx-get-var-info.md) | All | Get variable information for a **revoscalepy** data source or data frame, including variable names, descriptions, and value labels.|
|[rx_get_var_names](rx-get-var-names.md)  | All | Read the variable names for data source or data frame. |
|[rx_set_var_info](rx-set-var-info.md)  | All | Set the variable information for an .xdf file, including variable names, descriptions, and value labels, or set attributes for variables in a data frame.|
|[RxMissingValues](RxMissingValues.md) | All | Provides missing values for various `NumPy` data types which you can use to mark missing values in a sequence of data in `ndarray`.|
|[rx_privacy_control](rx-privacy-control.md) | All | Opt out of [usage data collection](../../resources-opting-out.md). |
|[rx_hadoop_command](rx-hadoop-command.md) | Local, [RxSpark](RxSpark.md) | Execute arbitrary Hadoop commands and perform standard file operations in Hadoop. |
|[rx_hadoop_copy_from_local](rx-hadoop-copy-from-local.md) | Local, [RxSpark](RxSpark.md) |  Wraps the Hadoop `fs -copyFromLocal` command. |
|[rx_hadoop_copy_to_local](rx-hadoop-copy-to-local.md) | Local, [RxSpark](RxSpark.md) | Wraps the Hadoop `fs -copyToLocal` command. |
|[rx_hadoop_copy](rx-hadoop-copy.md) | Local, [RxSpark](RxSpark.md) | Wraps the Hadoop `fs -cp` command. |
|[rx_hadoop_file_exists](rx-hadoop-file-exists.md) | Local, [RxSpark](RxSpark.md) | Wraps the Hadoop `fs -test -e` command. |
|[rx_hadoop_list_files](rx-hadoop-list-files.md) | Local, [RxSpark](RxSpark.md) | Wraps the Hadoop `fs -ls or -lsr` command. |
|[rx_hadoop_make_dir](rx-hadoop-make-dir.md) | Local, [RxSpark](RxSpark.md) | Wraps the Hadoop `fs -mkdir -p` command. |
|[rx_hadoop_move](rx-hadoop-move.md) | Local, [RxSpark](RxSpark.md) | wraps the Hadoop `fs -mv` command. |
|[rx_hadoop_remove_dir](rx-hadoop-remove-dir.md) | Local, [RxSpark](RxSpark.md) | Wraps the Hadoop `fs -rm -r`  or `fs -rm -r -skipTrash` command. |
|[rx_hadoop_remove](rx-hadoop-remove.md) | Local, [RxSpark](RxSpark.md) | Wraps the Hadoop `fs -rm` or `fs -rm -skipTrash` command. |

## Next steps

For Machine Learning Server, try a quickstart as an introduction to **revoscalepy**:

+ [revoscalepy and PySpark interoperability](../../python/tutorial-revoscalepy-pyspark.md) 

For SQL Server, add both Python modules to your computer by running setup: 

+ [Set up Python Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/python/setup-python-machine-learning-services).

Follow these SQL Server tutorials for hands-on experience:

+ [Use revoscalepy to create a model](https://docs.microsoft.com/sql/advanced-analytics/tutorials/use-python-revoscalepy-to-create-model) 
+ [Run Python in T-SQL](https://docs.microsoft.com/sql/advanced-analytics/tutorials/run-python-using-t-sql) 

## See also

  [Machine Learning Server](../../what-is-machine-learning-server.md)  
  [SQL Server Machine Learning Services with Python](https://docs.microsoft.com/sql/advanced-analytics/python/sql-server-python-services)  
  [SQL Server Machine Learning Server (Standalone)](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone)