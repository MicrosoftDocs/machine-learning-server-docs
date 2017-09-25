--- 
 
# required metadata 
title: "RealtimeDefinition,alias,deploy,description,redeploy,serialized_model,version: from azureml-model-management-sdk – Machine Learning Server | Microsoft Docs" 
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

# Class RealtimeDefinition


## RealtimeDefinition



```
azureml.deploy.operationalization.RealtimeDefinition(name, op)
```




Bases: [`azureml.deploy.operationalization.OperationalizationDefinition`](operationalization-definition.md)

Realtime class defining a *realtime* service’s properties for publishing.



```python
alias(alias)
```




Set the optional service function name alias to use in order to consume
the service.

**Example:**



```
service = client.service('score-service').alias('score').deploy()

# `score()` is the function that will call the `score-service`
result = service.score()
```



### Arguments


### alias

The service function name alias to use in order to consume
the service.


### Returns

Self [`OperationalizationDefinition`](operationalization-definition.md) for fluent API.



## deploy

```python
deploy()
```




Bundle up the definition properties and publish the service.


### Returns

A new instance of [`Service`](service.md) representing the
service *deployed*.



## description

```python
description(description)
```




Set the service’s optional description.


### Arguments


### description

The description of the service.


### Returns

Self [`OperationalizationDefinition`](operationalization-definition.md) for fluent API.



## redeploy

```python
redeploy(force=False)
```




Bundle up the definition properties and update the service.


### Returns

A new instance of [`Service`](service.md) representing the
service *deployed*.



## serialized_model

```python
serialized_model(model)
```




Serialized model.


### Arguments


### model

The required serialized model used for this realtime
service.


### Returns

Self [`OperationalizationDefinition`](operationalization-definition.md) for fluent API
chaining.



## version

```python
version(version)
```




Set the service’s optional version.


### Arguments


### version

The version of the service.


### Returns

Self [`OperationalizationDefinition`](operationalization-definition.md) for fluent API.
