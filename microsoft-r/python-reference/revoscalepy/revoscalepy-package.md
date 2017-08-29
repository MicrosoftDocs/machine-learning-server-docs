--- 
 
# required metadata 
title: "revoscalepy package for Python (SQL Server Machine Learning Server) | Microsoft Docs" 
description: "Function help reference for the revoscalepy Python package of SQL Server Machine Learning Server." 
keywords: "" 
author: "bradsev" 
manager: "jhubbard" 
ms.date: "08/29/2017" 
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

The **revoscalepy** module is a collection of Python functions used for analyzing data at the point of origin. Functions include data transformation and manipulation, visualization, and statistical analysis. It also includes functions for controlling jobs and serializing data.

| Package details | |
|--------|-|
| Version: |  9.2.0 |
| Supported on: | [SQL Server 2017 Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/python/sql-server-python-services) </br>[SQL Server 2017 Machine Learning Server (Standalone)](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone#whats-new-in-microsoft-machine-learning-server) |
| Built on: | [Anaconda](https://www.continuum.io/why-anaconda) distribution of [Python 3.5](https://www.python.org/doc) (included when you add Python support during installation). |

## How to use revoscalepy

The **revoscalepy** module is installed as part of SQL Server Machine Learning when you add Python to your installation. You get the full collection of proprietary packages plus a Python distribution with its modules and interpreters. You can use any Python IDE to write Python script calling functions in **revoscalepy**, but the script must run on a computer having SQL Server Machine Learning with Python.

There are two primary use cases for this release: 

+ Calling Python functions in T-SQL script or stored procedures running on SQL Server.  
+ Calling **revoscalepy** functions in Python script executing in a SQL Server [compute context](../../r/concept-what-is-compute-context.md). In your script, you can set a compute context to shift execution of **revoscalepy** operations to a remote SQL Server instance that has the **revoscalepy** interpreter.

## Functions by category

This section lists the functions by category to give you an idea of how each one is used. You can also use the table of contents to find functions in alphabetical order.

## 1-Compute context functions

| Function | Description |
|----------|-------------|
|[RxInSqlServer](RxInSqlServer.md) | Creates a compute context for running revoscalepy analyses inside Microsoft SQL Server. |
|[RxLocalSeq](RxLocalSeq.md) | Creates a local compute context object. Computations using rx_exec will be processed sequentially. This is the default compute context. |
|[rx_get_compute_context](rx-get-compute-context.md) | Returns the current compute context.|
|[rx_set_compute_context](rx-set-compute-context.md) | Change the compute context to a different one.|

## 2-Job functions

| Function | Description |
|----------|-------------|
|[rx_exec](rx-exec.md) | Allows distributed execution of a function in parallel across nodes (computers) or cores of a “compute context” such as a cluster. |
|[rx_cancel_job](rx-cancel-job.md) | Removes all job-related artifacts from the distributed computing resources, including any job results. |
|[rx_cleanup_jobs](rx-cleanup-jobs.md) |  Removes the artifacts for a specific job. |
|[rx_get_job_status](rx-get-job-status.md) | Obtain distributed computing processing status for the specified job. |
|[rx_get_jobs](rx-get-jobs.md) | Returns a list of job objects associated with the given compute context and matching the specified parameters. |
|[RxRemoteJob class](RxRemoteJob.md) | Closes the remote job, purging all associated job-related data.|
|[RxRemoteJobStatus](RxRemoteJobStatus.md) | Represents the execution status of a remote Python job.|
|[rx_get_job_info](rx-get-job-info.md) | Contains complete information on the job’s compute context as well as other information needed by the distributed computing resources.|
|[rx_wait_for_job](rx-wait-for-job.md) | Block on an existing distributed job until completion, effectively turning a non-blocking job into a blocking job.|
|[rx_get_job_output](rx-get-job-output.md) | Returns console output for the nodes participating in a distributed computing job.|
|[rx_get_job_results](rx-get-job-results.md) | Returns results of the run or a message stating why results are not available.|


## 3-Data source functions

| Function | Description |
|----------|-------------|
|[RxDataSource](RxDataSource.md) | Base class for all revoscalepy data sources.|
|[RxNativeFileSystem](RxNativeFileSystem.md) | Data source is accessed through Linux instead of HDFS. |
|[RxTextData](RxTextData.md) |  Generates a data source object from a text data. file|
|[RxXdfData](RxXdfData.md) |  Generates a data source object from an XDF data source.|
|[RxOdbcData](RxOdbcData.md) | Generates a data source object from an ODBC data source.|
|[RxSqlServerData](RxSqlServerData.md) | Generates a data source object from a SQL table or query.|

## 4-Data manipulation (ETL) functions

| Function | Description |
|----------|-------------|
|[rx_import](rx-import.md) | Import data into an .xdf file or data frame.|
|[rx_data_step](rx-data-step.md) | Transform data from an input data set to an output data set.|

## 5-Analytic functions

| Function | Description |
|----------|-------------|
|[rx_summary](rx-summary.md) | Produce univariate summaries of objects in revoscalepy. |
|[rx_lin_mod](rx-lin-mod.md) | Fit linear models on small or large data. |
|[rx_logit](rx-logit.md) | Use rx_logit to fit logistic regression models for small or large data. |
|[rx_dtree](rx-dtree.md) | Fit classification and regression trees on an ‘.xdf’ file or data frame for small or large data using parallel external memory algorithm. |
|[rx_dforest](rx-dforest.md) | Fit classification and regression decision forests on an ‘.xdf’ file or data frame for small or large data using parallel external memory algorithm. |
|[rx_btrees](rx-btrees.md) | Fit stochastic gradient boosted decision trees on an ‘.xdf’ file or data frame for small or large data using parallel external memory algorithm. |
|[rx_predict](rx-predict.md) | Generic function to compute predicted values and residuals using rx_lin_mod, rx_logit, rx_dtree, rx_dforest and rx_btrees objects. |

## 6-Serialization functions

| Function | Description |
|----------|-------------|
|[rx_serialize_model](rx-serialize-model.md)  |  Serialize a given python model. |
|[rx_read_object](rx-read-object.md) | Retrieves an ODBC data source objects. |
|[rx_read_xdf](rx-read-xdf.md) | Read data from an .xdf file into a data frame.|
|[rx_write_object](rx-write-object.md)   | Stores an ODBC data source object. |
|[rx_delete_object](rx-delete-object.md)  | Deletes an object from the ODBC data source. |
|[rx_list_keys](rx-list-keys.md)  | Enumerates all keys or versions for a given key, depending on the parameters. |


## 7-Utility functions

| Function | Description |
|----------|-------------|
|[RxOptions](RxOptions.md) | Specify and retrieve options needed for **revoscalepy** computations. |
|[rx_get_info](rx-get-info.md) | Get basic information about a **revoscalepy** data source or data frame. |
|[rx_get_var_info](rx-get-var-info.md) | Get variable information for a R**revoscalepy** data source or data frame, including variable names, descriptions, and value labels.|
|[rx_get_var_names](rx-get-var-names.md)  | Read the variable names for data source or data frame. |
|[rx_set_var_info](rx-set-var-info.md)  | Set the variable information for an .xdf file, including variable names, descriptions, and value labels, or set attributes for variables in a data frame.|
|[RxMissingValues](RxMissingValues.md) | Provides missing values for various `NumPy` data types which you can use to mark missing values in a sequence of data in `ndarray`.|

## Next steps

Add both Python modules to your computer by running setup: 

+ [Set up Python Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/python/setup-python-machine-learning-services).

Next, follow these tutorials for hands on experience::

+ [Use revoscalpy to create a model](https://docs.microsoft.com/sql/advanced-analytics/tutorials/use-python-revoscalepy-to-create-model) 
+ [Run Python in T-SQL](https://docs.microsoft.com/sql/advanced-analytics/tutorials/run-python-using-t-sql) 

## See also

  [SQL Server Machine Learning Services with Python](https://docs.microsoft.com/sql/advanced-analytics/python/sql-server-python-services)  
  [SQL Server Machine Learning Server (Standalone)](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone)