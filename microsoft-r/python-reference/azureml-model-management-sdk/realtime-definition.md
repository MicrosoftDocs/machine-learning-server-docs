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

# Class azureml.deploy.operationalization.RealtimeDefinition(name, op)





Bases: [`azureml.deploy.operationalization.OperationalizationDefinition`](operationalization-definition.md)

Realtime class defining a service’s properties on the fluent API.



## alias





Set the service function name alias to call.

### Usage

`alias(alias)`

### Arguments


#### alias


### Returns

Self [`OperationalizationDefinition`](operationalization-definition.md) for fluent API.



## deploy





Bundle up the definition properties and publish the service.

### Usage

`deploy()`

### Returns

Self [`OperationalizationDefinition`](operationalization-definition.md) for fluent API.



## description




Set the service description.

### Usage

`description(description)`

### Arguments


### description


### Returns

Self [`OperationalizationDefinition`](operationalization-definition.md) for fluent API.



## redeploy





Bundle up the definition properties and update the service.

### Usage

`redeploy(force=False)`

### Arguments


#### force


### Returns

Self [`OperationalizationDefinition`](operationalization-definition.md) for fluent API.


## serialized_model





Serialized model.

### Usage

`serialized_model(model)`

### Arguments


#### model


### Returns

Self [`OperationalizationDefinition`](operationalization-definition.md) for fluent API.



## version





Set the service version.

### Usage

`version(version)`

### Arguments


#### version


### Returns

Self [`OperationalizationDefinition`](operationalization-definition.md) for fluent API.
