--- 
 
# required metadata 
title: "ServiceDefinition,alias,artifact,artifacts,code_fn,code_str,deploy,description,inputs,models,objects,outputs,redeploy,version: Web Service Definition" 
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

# Web Service Definition


## Class ServiceDefinition



```
azureml.deploy.operationalization.ServiceDefinition(name, op)
```




Bases: [`azureml.deploy.operationalization.OperationalizationDefinition`](operationalization-definition.md#OperationalizationDefinition)

Service class defining a service’s properties on the fluent API.



```
alias(alias)
```




Set the service function name alias to call.


## Arguments


### alias


## Returns

Self [`OperationalizationDefinition`](operationalization-definition.md#OperationalizationDefinition) for fluent API.



```
artifact(artifact)
```




A single File artifact.


## Arguments


### artifact


## Returns

Self [`OperationalizationDefinition`](operationalization-definition.md#OperationalizationDefinition) for fluent API.



```
artifacts(artifacts)
```




File artifacts.


## Arguments


### artifacts


## Returns

Self [`OperationalizationDefinition`](operationalization-definition.md#OperationalizationDefinition) for fluent API.



```
code_fn(code, init=None)
```




Set the service consume function as a function.
:param code:
:param init:
:returns: Self [`OperationalizationDefinition`](operationalization-definition.md#OperationalizationDefinition) for fluent API.



```
code_str(code, init=None)
```




Set the service consume function as a block of python code.


## Arguments


### code


### init


## Returns

A [`ServiceDefinition`](azureml/deploy/operationalization/ServiceDefinition.md) for fluent API.



```
deploy()
```




Bundle up the definition properties and publish the service.


## Returns

Self [`OperationalizationDefinition`](operationalization-definition.md#OperationalizationDefinition) for fluent API.



```
description(description)
```




Set the service description.


## Arguments


### description


## Returns

Self [`OperationalizationDefinition`](operationalization-definition.md#OperationalizationDefinition) for fluent API.



```
inputs(**inputs)
```




Defines inputs.


## Arguments


### inputs


## Returns

Self [`OperationalizationDefinition`](operationalization-definition.md#OperationalizationDefinition) for fluent API.



```
models(**models)
```




Models.


## Arguments


### models


## Returns

Self [`OperationalizationDefinition`](operationalization-definition.md#OperationalizationDefinition) for fluent API.



```
objects(**objects)
```




Objects.


## Arguments


### objects


## Returns

Self [`OperationalizationDefinition`](operationalization-definition.md#OperationalizationDefinition) for fluent API.



```
outputs(**outputs)
```




Defines utputs.


## Arguments


### outputs


## Returns

Self [`OperationalizationDefinition`](operationalization-definition.md#OperationalizationDefinition) for fluent API.



```
redeploy(force=False)
```




Bundle up the definition properties and update the service.


## Arguments


### force


## Returns

Self [`OperationalizationDefinition`](operationalization-definition.md#OperationalizationDefinition) for fluent API.



```
version(version)
```




Set the service version.


## Arguments


### version


## Returns

Self [`OperationalizationDefinition`](operationalization-definition.md#OperationalizationDefinition) for fluent API.
