--- 
 
# required metadata 
title: "BatchResponse,api,completed_item_count,execution,execution_id,total_item_count: from azureml-model-management-sdk – Machine Learning Server | Microsoft Docs" 
description: "" 
keywords: "" 
author: "Microsoft" 
manager: "Microsoft" 
ms.date: "09/18/2017" 
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

# BatchResponse


## Class BatchResponse



```
azureml.deploy.server.service.BatchResponse(api, execution_id, response,
    output_schema)
```




Create a new Response Object by service name and raw service metadata.



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



## execution

```python
execution(index)
```




Extracts the service execution results within the batch at this
execution *index*.
:param index: The batch execution index.
:returns: An execution Self [`ServiceResponse`](service-response.md#ServiceResponse).



## execution_id

```python
execution_id
```




Gets this batch’s *execution id* if currently started, otherwise *None*.



## total_item_count

```python
total_item_count
```




Gets the total number of batch results processed in any state.
