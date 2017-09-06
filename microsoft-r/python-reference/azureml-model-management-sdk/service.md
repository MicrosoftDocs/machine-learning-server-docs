--- 
 
# required metadata 
title: "Service,batch,capabilities,get_batch,list_batch_executions,swagger: " 
description: "" 
keywords: "" 
author: "Microsoft" 
manager: "Microsoft" 
ms.date: "09/05/2017" 
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



```
azureml.deploy.server.service.Service(service, http_client)
```




Service object from metadata.



```
batch(records, parallel_count=10)
```




Register a set of input records for batch execution on this service.


# Arguments


## records

The *data.frame* or *list* of
input records to execute.


## parallel_count

Number of threads used to process entries in
the batch. Default value is 10. Please make sure not to use too
high of a number because it might negatively impact performance.


# Returns

The *Batch* object to control service batching
lifecycle.



```
capabilities()
```




Gets the service holding capabilities.


# Returns

A dict of key/values describing the service.



```
get_batch(execution_id)
```




Retrieve the *Batch* based on an *execution id*


# Arguments


## execution_id

The id of the batch execution.


# Returns

The *Batch*.



```
list_batch_executions()
```




Gets all batch executions currently queued for this service.


# Returns

A list of *execution ids*.



```
swagger()
```




Retrieves the *swagger.json* for this service (see [http://swagger.io/](http://swagger.io/)).
:return: The swagger document for this service.
