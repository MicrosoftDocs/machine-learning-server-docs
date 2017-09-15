--- 
 
# required metadata 
title: "RealtimeDefinition,alias,deploy,description,redeploy,serialized_model,version: Realtime Service Definition Definition" 
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

# Realtime Service Definition


## Class RealtimeDefinition



```
azureml.deploy.operationalization.RealtimeDefinition(name, op)
```




Bases: [`azureml.deploy.operationalization.OperationalizationDefinition`](operationalization-definition.md#OperationalizationDefinition)

Realtime class defining a service’s properties on the fluent API.

Defines a realtime service publish definition.


## Arguments


### name


### op



```
alias(alias)
```




Set the service function name alias to call.


## Arguments


### alias


## Returns

Self [`OperationalizationDefinition`](operationalization-definition.md#OperationalizationDefinition) for fluent API.



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
redeploy(force=False)
```




Bundle up the definition properties and update the service.


## Arguments


### force


## Returns

Self [`OperationalizationDefinition`](operationalization-definition.md#OperationalizationDefinition) for fluent API.



```
serialized_model(model)
```




Serialized model.


## Arguments


### model


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
