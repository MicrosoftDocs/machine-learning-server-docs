--- 
 
# required metadata 
title: "Batch,api,execution_id,parallel_count,records,results,start: Batch Service" 
description: "" 
keywords: "" 
author: "Microsoft" 
manager: "Microsoft" 
ms.date: "09/15/2017" 
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

# Batch Service


## Class BatchService



```
azureml.deploy.server.service.Batch(service, records=[], parallel_count=10,
    execution_id=None)
```




Manager of a service’s batch execution lifecycle.



```
api
```




Gets the api endpoint.



```
execution_id
```




Gets this batch’s *execution id* if currently started, otherwise *None*.



```
parallel_count
```




Gets this batch’s parallel count of threads.



```
records
```




Gets the batch input records.



```
results(show_partial_results=True)
```




Poll batch results.


## Arguments


### show_partial_results

To get partial execution results or not.


## Returns

An execution Self [`BatchResponse`](batch-response.md#BatchResponse).



```
start()
```




Start a batch execution.


## Returns

Self
