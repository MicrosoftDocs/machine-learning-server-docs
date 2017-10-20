--- 
 
# required metadata 
title: "BatchResponse,api,completed_item_count,execution,execution_id,total_item_count: from azureml-model-management-sdk – Machine Learning Server " 
description: "" 
keywords: "" 
author: "Microsoft" 
manager: "Microsoft" 
ms.date: "09/20/2017" 
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

# Class BatchResponse


## BatchResponse



```
azureml.deploy.server.service.BatchResponse(api, execution_id, response,
    output_schema)
```




Represents a service’s entire batch execution response at a particular state
in time. Using this, a batch execution index can be supplied to the
`execution(index)` function in order to retrieve the service’s
[`ServiceResponse`](service-response.md).



## api

```python
api
```




Gets the api endpoint.



## completed_item_count

```python
completed_item_count
```




Gets the number of completed batch results processed thus far.
:returns: The number of completed batch results processed thus far.



## execution

```python
execution(index)
```




Extracts the service execution results within the batch at this
execution *index*.


### Arguments


### index

The batch execution index.


### Returns

The execution results [`ServiceResponse`](service-response.md).



## execution_id

```python
execution_id
```




Gets this batch’s execution identifier if a batch has been started,
otherwise `None`.
:returns: This batch’s execution identifier if a batch has been started,
otherwise `None`.



## total_item_count

```python
total_item_count
```




Gets the total number of batch results processed in any state.
:returns: The total number of batch results processed in any state.
