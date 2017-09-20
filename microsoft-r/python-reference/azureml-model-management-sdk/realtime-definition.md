--- 
 
# required metadata 
title: "RealtimeDefinition,alias,deploy,description,redeploy,serialized_model,version: from azureml-model-management-sdk – Machine Learning Server | Microsoft Docs" 
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

# Class RealtimeDefinition


## RealtimeDefinition



```
azureml.deploy.operationalization.RealtimeDefinition(name, op)
```




Bases: [`azureml.deploy.operationalization.OperationalizationDefinition`](operationalization-definition#operationalizationDefinition)

Realtime class defining a service’s properties on the fluent API.

Defines a realtime service publish definition.


### Arguments


### name


### op



```python
alias(alias)
```




Set the service function name alias to call.


### Arguments


### alias


### Returns

Self [`OperationalizationDefinition`](operationalization-definition#operationalizationDefinition) for fluent API.



## deploy

```python
deploy()
```




Bundle up the definition properties and publish the service.


### Returns

Self [`OperationalizationDefinition`](operationalization-definition#operationalizationDefinition) for fluent API.



## description

```python
description(description)
```




Set the service description.


### Arguments


### description


### Returns

Self [`OperationalizationDefinition`](operationalization-definition#operationalizationDefinition) for fluent API.



## redeploy

```python
redeploy(force=False)
```




Bundle up the definition properties and update the service.


### Arguments


### force


### Returns

Self [`OperationalizationDefinition`](operationalization-definition#operationalizationDefinition) for fluent API.



## serialized_model

```python
serialized_model(model)
```




Serialized model.


### Arguments


### model


### Returns

Self [`OperationalizationDefinition`](operationalization-definition#operationalizationDefinition) for fluent API.



## version

```python
version(version)
```




Set the service version.


### Arguments


### version


### Returns

Self [`OperationalizationDefinition`](operationalization-definition#operationalizationDefinition) for fluent API.
