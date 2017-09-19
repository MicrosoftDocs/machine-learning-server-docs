--- 
 
# required metadata 
title: "ServiceDefinition,alias,artifact,artifacts,code_fn,code_str,deploy,description,inputs,models,objects,outputs,redeploy,version: from azureml-model-management-sdk – Machine Learning Server | Microsoft Docs" 
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

# ServiceDefinition


## Class ServiceDefinition



```
azureml.deploy.operationalization.ServiceDefinition(name, op)
```




Bases: [`azureml.deploy.operationalization.OperationalizationDefinition`](operationalization-definition#operationalizationdefinition)

Service class defining a service’s properties on the fluent API.



```python
alias(alias)
```




Set the service function name alias to call.


### Arguments


### alias


### Returns

Self [`OperationalizationDefinition`](operationalization-definition#operationalizationdefinition) for fluent API.



## artifact

```python
artifact(artifact)
```




A single File artifact.


### Arguments


### artifact


### Returns

Self [`OperationalizationDefinition`](operationalization-definition#operationalizationdefinition) for fluent API.



## artifacts

```python
artifacts(artifacts)
```




File artifacts.


### Arguments


### artifacts


### Returns

Self [`OperationalizationDefinition`](operationalization-definition#operationalizationdefinition) for fluent API.



## code_fn

```python
code_fn(code, init=None)
```




Set the service consume function as a function.
:param code:
:param init:
:returns: Self [`OperationalizationDefinition`](operationalization-definition#operationalizationdefinition) for fluent API.



## code_str

```python
code_str(code, init=None)
```




Set the service consume function as a block of python code.


### Arguments


### code


### init


### Returns

A [`ServiceDefinition`](service-definition#servicedefinition) for fluent API.



## deploy

```python
deploy()
```




Bundle up the definition properties and publish the service.


### Returns

Self [`OperationalizationDefinition`](operationalization-definition#operationalizationdefinition) for fluent API.



## description

```python
description(description)
```




Set the service description.


### Arguments


### description


### Returns

Self [`OperationalizationDefinition`](operationalization-definition#operationalizationdefinition) for fluent API.



## inputs

```python
inputs(**inputs)
```




Defines inputs.


### Arguments


### inputs


### Returns

Self [`OperationalizationDefinition`](operationalization-definition#operationalizationdefinition) for fluent API.



## models

```python
models(**models)
```




Models.


### Arguments


### models


### Returns

Self [`OperationalizationDefinition`](operationalization-definition#operationalizationdefinition) for fluent API.



## objects

```python
objects(**objects)
```




Objects.


### Arguments


### objects


### Returns

Self [`OperationalizationDefinition`](operationalization-definition#operationalizationdefinition) for fluent API.



## outputs

```python
outputs(**outputs)
```




Defines utputs.


### Arguments


### outputs


### Returns

Self [`OperationalizationDefinition`](operationalization-definition#operationalizationdefinition) for fluent API.



## redeploy

```python
redeploy(force=False)
```




Bundle up the definition properties and update the service.


### Arguments


### force


### Returns

Self [`OperationalizationDefinition`](operationalization-definition#operationalizationdefinition) for fluent API.



## version

```python
version(version)
```




Set the service version.


### Arguments


### version


### Returns

Self [`OperationalizationDefinition`](operationalization-definition#operationalizationdefinition) for fluent API.
