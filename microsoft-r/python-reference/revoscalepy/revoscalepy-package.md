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

* [RxInSqlServer](computecontext/RxInSqlServer.md) 
* [RxLocalSeq](computecontext/RxLocalSeq.md) 

## Job functions
* [rx_exec](computecontext/RxInSqlServer-exec.md) 
* [rx_cancel_job](computecontext/RxJob-cancel.md) 
* [rx_cleanup_jobs](computecontext/RxJob-clean.md) 
* [rx_get_job_status](computecontext/RxJob-status.md) 
* [rx_get_jobs](computecontext/RxJob-get.md) 

* [RxJob class](THIS_IS_MISSING)
* [rx_get_job_info](THIS_IS_MISSING)
* [rx_wait_for_job](THIS_IS_MISSING)
* [rx_get_job_output](THIS_IS_MISSING)
* [rx_get_job_results](THIS_IS_MISSING)


## Data sources

* [RxTextData](THIS_IS_MISSING)
* [RxXdfData](THIS_IS_MISSING)
* [RxOdbcData](THIS_IS_MISSING)
* [RxSqlServerData](THIS_IS_MISSING)

## Data manipulation (ETL)

* [rx_import](etl/RxImport.md) 
* [rx_data_step](THIS_IS_MISSING)

## Analytic functions

* [rx_summary](functions/RxSummary.md) 
* [rx_lin_mod](functions/RxLinMod.md) 
* [rx_logit](functions/RxLogit.md) 
* [rx_dtree](functions/RxDTree-rx-dtree.md) 
* [rx_dforest](functions/RxDTree-rx-dforest.md) 
* [rx_btrees](functions/RxDTree-rx-btrees.md) 
* [rx_predict](functions/RxPredict.md) 

## Serialization functions

* [rx_serialize_mode](THIS_IS_MISSING) 
* [rx_read_object](functions/RxReadObject.md)
* [rx_write_object](functions/RxWriteObject.md)  
* [rx_delete_object](functions/RxDeleteObject.md) 
* [rx_list_keys](functions/RxListKeys.md) 

## Utility

* [RxOptions](utils/RxOptions.md) 

## See also

 [Python function library help (SQL Server Machine Learning)](../introducing-python-package-reference.md)   