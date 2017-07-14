--- 
 
# required metadata 
title: "revoscalepy (Function Library for Python) in SQL Server Machine Learning Server" 
description: "" 
keywords: "" 
author: "bradsev" 
manager: "jhubbard" 
ms.date: "07/13/2017" 
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

# revoscalepy (Function Library for Python)

**Applies to: SQL Server 2017 RC1**

The **revoscalepy** library is a proprietary Python package from Microsoft for use in SQL Server Machine Learning Server (Standalone) and SQL Server Machine Learning Services. Many functions in this library can be used with **microsoftml** package, which is part of the same release.

Use these functions to set the compute context for python script execution, load and manipulate data from external data sources, and manage objects. Data visualization and analysis functions are provided for the most common use cases.


## Compute context

| Function | Description |
|----------|-------------|
|[RxInSqlServer](computecontext/RxInSqlServer.md) | Creates a compute context for running revoscalepy analyses inside Microsoft SQL Server. |
|[RxLocalSeq](computecontext/RxLocalSeq.md) | Creates a local compute context object. Computations using rx_exec will be processed sequentially. This is the default compute context. |

## Job functions

| Function | Description |
|----------|-------------|
|[rx_exec](computecontext/RxInSqlServer-exec.md) | Allows distributed execution of a function in parallel across nodes (computers) or cores of a “compute context” such as a cluster. |
|[rx_cancel_job](computecontext/RxJob-cancel.md) | Removes all job-related artifacts from the distributed computing resources, including any job results. |
|[rx_cleanup_jobs](computecontext/RxJob-clean.md) |  Removes the artifacts for a specific job. |
|[rx_get_job_status](computecontext/RxJob-status.md) | Obtain distributed computing processing status for the specified job. |
|[rx_get_jobs](computecontext/RxJob-get.md) | Returns a list of job objects associated with the given compute context and matching the specified parameters. |
|[RxJob class](THIS_IS_MISSING) |  |
|[rx_get_job_info](THIS_IS_MISSING) |  |
|[rx_wait_for_job](THIS_IS_MISSING) |  |
|[rx_get_job_output](THIS_IS_MISSING) |  |
|[rx_get_job_results](THIS_IS_MISSING) |  |


## Data sources

| Function | Description |
|----------|-------------|
|[RxTextData](THIS_IS_MISSING) |  |
|[RxXdfData](THIS_IS_MISSING) |  |
|[RxOdbcData](THIS_IS_MISSING) |  |
|[RxSqlServerData](THIS_IS_MISSING) |  |

## Data manipulation (ETL)

| Function | Description |
|----------|-------------|
|[rx_import](etl/RxImport.md) | Import data into an .xdf file or data.frame.|
|[rx_data_step](THIS_IS_MISSING) | |

## Analytic functions

| Function | Description |
|----------|-------------|
|[rx_summary](functions/RxSummary.md)  | Produce univariate summaries of objects in revoscalepy. |
|[rx_lin_mod](functions/RxLinMod.md)  | Fit linear models on small or large data. |
|[rx_logit](functions/RxLogit.md)  | Use rx_logit to fit logistic regression models for small or large data. |
|[rx_dtree](functions/RxDTree-rx-dtree.md)  | Fit classification and regression trees on an ‘.xdf’ file or data frame for small or large data using parallel external memory algorithm. |
|[rx_dforest](functions/RxDTree-rx-dforest.md)  | Fit classification and regression decision forests on an ‘.xdf’ file or data frame for small or large data using parallel external memory algorithm. |
|[rx_btrees](functions/RxDTree-rx-btrees.md)  | Fit stochastic gradient boosted decision trees on an ‘.xdf’ file or data frame for small or large data using parallel external memory algorithm. |
|[rx_predict](functions/RxPredict.md)  | Generic function to compute predicted values and residuals using rx_lin_mod, rx_logit, rx_dtree, rx_dforest and rx_btrees objects. |

## Serialization functions

| Function | Description |
|----------|-------------|
|[rx_serialize_mode](THIS_IS_MISSING)  |  |
|[rx_read_object](functions/RxReadObject.md) | Retrieves an ODBC data source objects. |
|[rx_write_object](functions/RxWriteObject.md)   | Stores an ODBC data source object. |
|[rx_delete_object](functions/RxDeleteObject.md)  | Deletes an object from the ODBC data source. |
|[rx_list_keys](functions/RxListKeys.md)  | Enumerates all keys or versions for a given key, depending on the parameters. |

## Utility

| Function | Description |
|----------|-------------|
|[RxOptions](utils/RxOptions.md) | Specify and retrieve options needed for **revoscalepy** computations. |

## See also

 [Python function library help (SQL Server Machine Learning)](../introducing-python-package-reference.md)   