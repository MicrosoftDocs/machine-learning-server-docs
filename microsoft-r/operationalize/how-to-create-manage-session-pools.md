---

# required metadata
title: "Create and manage session pools for fast web service connections in R (Machine Learning Server)"
description: "Allocate resources for pre-loading web service connections and dependencies in R solutions (Machine Learning Server ). "
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "cgronlun"
ms.date: "2/16/2018"
ms.topic: "article"
ms.prod: "mlserver"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
#ms.custom: ""
---

# How to create and manage session pools for fast web service connections

**Applies to: Machine Learning Server 9.3 (R)**

Fast connections to a web service are possible when you create sessions and load dependencies in advance. Sessions are available in a pool dedicated to a specific web service, where each session is an instance of the R interpreter. Dependencies are pre-loaded once per web service and shared by all connections. For example, creating ten sessions in advance for a web service that uses numpy, pandas, scikit, revoscalepy, microsoftml, and azureml-model-management-sdk would result in ten instances of the Python interpreter but only one copy of each module, shared by all connections. 

A dedicated session pool is a per-web-service, per-language construction. For each web service, two session pools are created, one each for R and Python, assuming both are installed on the Machine Learning Server.

A web service having a dedicated session pool never requests connections from the generic session pool, not even when maximum sessions are reached. The generic session pool services only those web services that do not have dedicated resources.

For R script, the [mrsdeploy](../r-reference/mrsdeploy/mrsdeploy-package.md) function library provides three functions for managing sessions:

+ [configureServicePool](../r-reference/mrsdeploy/configureServicePool.md)
+ [getPoolStatus](../r-reference/mrsdeploy/getPoolStatus.md)
+ [deleteServicePool](../r-reference/mrsdeploy/deleteServicePool.md)

## Create or modify a dedicated session pool

You can use an R console application, such as Rgui.exe, to run the following commands.

```r
 # load mrsdeploy and print the function list
 library(mrsdeploy)
 ls("package:mrsdeploy")
```

## Return status codes for current sessions

## Return a count of pools and connections

Is there a way to see the pool settings or which pools are out there?
You can see that in diagnostics under each compute node


## Delete a session pool

## Monitor resource utilization

## Monitor total connections/sessions



## See also

 + [What are web services in Machine Learning Server?](concept-what-are-web-services.md)
 + [Evaluate web service capacity: generic session pools](configure-evaluate-capacity.md#shell-pools)