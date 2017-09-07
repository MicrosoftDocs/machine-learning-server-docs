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

# Class azureml.deploy.server.service.Service(service, http_client)


Service object from metadata.



## batch





Register a set of input records for batch execution on this service.

### Usage

`batch(records, parallel_count=10)`

### Arguments


#### records

The *data.frame* or *list* of
input records to execute.


#### parallel_count

Number of threads used to process entries in
the batch. Default value is 10. Please make sure not to use too
high of a number because it might negatively impact performance.


### Returns

The *Batch* object to control service batching
lifecycle.



## capabilities





Gets the service holding capabilities.

### Usage

`capabilities()`

### Returns

A dict of key/values describing the service.



## get_batch





Retrieve the *Batch* based on an *execution id*

### Usage
`get_batch(execution_id)`

### Arguments


#### execution_id

The id of the batch execution.


### Returns

The *Batch*.



## list_batch_executions





Gets all batch executions currently queued for this service.

### Usage

`list_batch_executions()`

### Returns

A list of *execution ids*.



## swagger





Retrieves the *swagger.json* for this service (see [http://swagger.io/](http://swagger.io/)).
:return: The swagger document for this service.

### Usage
`swagger()`
