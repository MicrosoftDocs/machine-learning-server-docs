--- 
 
# required metadata 
title: "OperationalizationDefinition,alias,deploy,description,redeploy,version: from azureml-model-management-sdk – Machine Learning Server | Microsoft Docs" 
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

# Class OperationalizationDefinition


## OperationalizationDefinition



```
azureml.deploy.operationalization.OperationalizationDefinition(name, op,
    defs_extent={})
```




Base abstract class defining a service’s properties on the fluent API.

Create a new publish definition.


### Arguments


### name

The web service name


### op


### defs_extent



```python
alias(alias)
```




Set the service function name alias to call.


### Arguments


### alias


### Returns

Self [`OperationalizationDefinition`](operationalization-definition#operationalization-definition) for fluent API.



## deploy

```python
deploy()
```




Bundle up the definition properties and publish the service.
:return:
:returns: Self [`OperationalizationDefinition`](operationalization-definition#operationalization-definition) for fluent API.



## description

```python
description(description)
```




Set the service description.


### Arguments


### description


### Returns

Self [`OperationalizationDefinition`](operationalization-definition#operationalization-definition) for fluent API.



## redeploy

```python
redeploy(force=False)
```




Bundle up the definition properties and update the service.
To be implemented by subclasses.


### Arguments


### force


### Returns

Self [`OperationalizationDefinition`](operationalization-definition#operationalization-definition) for fluent API.



## version

```python
version(version)
```




Set the service version.


### Arguments


### version


### Returns

Self [`OperationalizationDefinition`](operationalization-definition#operationalization-definition) for fluent API.
