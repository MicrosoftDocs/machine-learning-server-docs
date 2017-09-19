--- 
 
# required metadata 
title: "Service,batch,capabilities,get_batch,list_batch_executions,swagger: from azureml-model-management-sdk – Machine Learning Server | Microsoft Docs" 
description: "" 
keywords: "" 
author: "Microsoft" 
manager: "Microsoft" 
ms.date: "09/19/2017" 
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

# Service


## Class Service



```
azureml.deploy.server.service.Service(service, http_client)
```




Service object from metadata.



```
batch(records, parallel_count=10)
```




Register a set of input records for batch execution on this service.


### Arguments


### records

The *data.frame* or *list* of
input records to execute.


### parallel_count

Number of threads used to process entries in
the batch. Default value is 10. Please make sure not to use too
high of a number because it might negatively impact performance.


### Returns

The *Batch* object to control service batching
lifecycle.



## capabilities

```python
capabilities()
```




Provides the following information describing the holdings of this
service:

* *api* -  The API REST endpoint. 

* *name* - The service name. 

* *version* - The service version. 

* *published_by* - The service publishing author. 

* *runtime* - The service runtime context _R|Python_. 

* *description* - The service description. 

* *creation_time* - The service publish timestamp. 

* *snapshot_id* - The snapshot identifier this service is bound with. 

* *inputs* - The input schema name/type definition. 

* *outputs* - The output schema name/type definition. 

* *inputs_encoded* - The input schema name/type encoded to python. 

* *outputs_encoded* - The output schema name/type encoded to python. 

* *artifacts* - The supported generated files. 

* *operation_id* - The function `alias`. 

* *swagger* - The API REST endpoint to this service’s *swagger.json*

      document.


### Returns

A `dict` of key/values describing the service.



## get_batch

```python
get_batch(execution_id)
```




Retrieve the service batch based on an execution identifier.


### Arguments


### execution_id

The identifier of the batch execution.


### Returns

The [`Batch`](batch.md#Batch) represented by the given execution
identifier.



## list_executions

```python
list_batch_executions()
```




Gets all batch executions currently queued for this service.


### Returns

A list of *execution ids*.



## swagger

```python
swagger\(\)
```




Retrieves the *swagger.json* for this service (see [http://swagger.io/](http://swagger.io/)).
:returns: The swagger document for this service as a json `str`.
