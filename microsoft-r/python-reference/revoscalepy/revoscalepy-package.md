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


## Compute Context

* [RxComputeContext](computecontext/RxComputeContext.md) 
* [rx_get_compute_context](computecontext/RxComputeContext-get.md) 
* [rx_set_compute_context](computecontext/RxComputeContext-set.md) 
* [RxInSqlServer](computecontext/RxInSqlServer.md) 
* [rx_exec](computecontext/RxInSqlServer-exec.md) 
* [rx_cancel_job](computecontext/RxJob-cancel.md) 
* [rx_get_job_status](computecontext/RxJob-status.md) 
* [rx_get_jobs](computecontext/RxJob-get.md) 
* [RxLocalSeq](computecontext/RxLocalSeq.md) 


## ETL

* [rx_import](etl/RxImport.md) 


## Visualization and Analysis

* [rx_delete_object](functions/RxDeleteObject.md) 
* [rx_btrees](functions/RxDTree-rx-btrees.md) 
* [rx_dforest](functions/RxDTree-rx-dforest.md) 
* [rx_dtree](functions/RxDTree-rx-dtree.md) 
* [rx_lin_mod](functions/RxLinMod.md) 
* [rx_list_keys](functions/RxListKeys.md) 
* [rx_logit](functions/RxLogit.md) 
* [rx_predict](functions/RxPredict.md) 
* [rx_predict_default](functions/RxPredict-default.md) 
* [rx_predict_rx_dforest](functions/RxPredict-dforest.md) 
* [rx_predict_rx_dtree](functions/RxPredict-dtree.md) 
* [rx_read_object](functions/RxReadObject.md) 
* [rx_summary](functions/RxSummary.md) 
* [rx_write_object](functions/RxWriteObject.md) 


## Utility

* [RxOptions](utils/RxOptions.md) 

## See also

 [Python function library help (SQL Server Machine Learning)](../introducing-python-package-reference.md)   