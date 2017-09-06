--- 
 
# required metadata 
title: "RealtimeDefinition,alias,deploy,description,redeploy,serialized_model,version: " 
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

# RealtimeDefinition



```
azureml.deploy.operationalization.RealtimeDefinition(name, op)
```




Bases: [`azureml.deploy.operationalization.OperationalizationDefinition`](operationalization-definition.md)

Realtime class defining a service’s properties on the fluent API.



```
alias(alias)
```




Set the service function name alias to call.


# Arguments


## alias


# Returns

Self [`OperationalizationDefinition`](operationalization-definition.md) for fluent API.



```
deploy()
```




Bundle up the definition properties and publish the service.


# Returns

Self [`OperationalizationDefinition`](operationalization-definition.md) for fluent API.



```
description(description)
```




Set the service description.


# Arguments


## description


# Returns

Self [`OperationalizationDefinition`](operationalization-definition.md) for fluent API.



```
redeploy(force=False)
```




Bundle up the definition properties and update the service.


# Arguments


## force


# Returns

Self [`OperationalizationDefinition`](operationalization-definition.md) for fluent API.



```
serialized_model(model)
```




Serialized model.


# Arguments


## model


# Returns

Self [`OperationalizationDefinition`](operationalization-definition.md) for fluent API.



```
version(version)
```




Set the service version.


# Arguments


## version


# Returns

Self [`OperationalizationDefinition`](operationalization-definition.md) for fluent API.
