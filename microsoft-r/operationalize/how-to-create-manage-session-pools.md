---

# required metadata
title: "Create and manage session pools for fast web service connections in R (Machine Learning Server)"
description: "Allocate resources for pre-loading web service connections and dependencies in R solutions (Machine Learning Server ). "
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "cgronlun"
ms.date: "2/16/2018"
ms.topic: "conceptual"
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

# How to create and manage session pools for fast web service connections in R

**Applies to: Machine Learning Server 9.3 (R)**

Fast connections to a web service are possible when you create sessions and load dependencies in advance. Sessions are available in a pool dedicated to a specific web service, where each session includes an instance of the R interpreter and a copy of dependencies required by the web service. For example, if you create ten sessions in advance for a web service that uses Matplotlib, dplyr, cluster, RevoScaleR, MicrosoftML, and mrsdeploy, each session would have its own instance of the R interpreter plus a copy of each library loaded in memory. 

A web service having a dedicated session pool never requests connections from the [generic session pool](configure-evaluate-capacity.md#pool) shared resource, not even when maximum sessions are reached. The generic session pool services only those web services that do not have dedicated resources.

For R script, the [mrsdeploy](../r-reference/mrsdeploy/mrsdeploy-package.md) function library provides three functions for creating and managing sessions:

+ [configureServicePool](../r-reference/mrsdeploy/configureServicePool.md)
+ [getPoolStatus](../r-reference/mrsdeploy/getPoolStatus.md)
+ [deleteServicePool](../r-reference/mrsdeploy/deleteServicePool.md)

## Create or modify a dedicated session pool

Given an existing connection to a Machine Learning Server with [operationalization](../r-reference/mrsdeploy/configureServicePool.md) enabled, you can use [mrsdeploy](../r-reference/mrsdeploy/mrsdeploy-package.md) functions and an R console application such as Rgui.exe to run the following commands:

```r
 # load mrsdeploy and print the function list
 library(mrsdeploy)
 ls("package:mrsdeploy")

 # Return a list of web services to get the service and version 
 # Both service name and version number are required
 listServices()

 # Create the session pool using a case-sensitive web service name
 # A status code of 200 is returned upon success
 configureServicePool(name = "myWebservice1234", version = "v1.0.0", initialPoolSize = 5, maxPoolSize = 10 )

 # Return status 
 # Pending indicates session creation is in progress. Success indicates sessions are ready.
 getPoolStatus(name = "myWebService1234", version = "v1.0.0")
```
Currently, there are no commands or functions that return actual session pool usage. The log file is your best resource for analyzing connection and service activity. For more information, see [Trace user actions](configure-run-diagnostics.md#trace-user-actions).

## Delete a session pool

At the R console, on the compute node, run the following command to delete the session pool for a given service.

```R
 # Return a list of web services to get the service and version information
 listServices()

 # Deletes the dedicated session pool and releases resources
 deleteServicePool(name = "myWebService1234", version = "v1.0.0")
```
This feature is still under development. In rare cases, the [deleteServicePool](../r-reference/mrsdeploy/deleteServicePool.md) command may fail to actually delete the pool on the computeNode. If you encounter this situation, issue a new [deleteServicePool](../r-reference/mrsdeploy/deleteServicePool.md) request. Use the [getPoolStatus](../r-reference/mrsdeploy/getPoolStatus.md) command to monitor the status of dedicated pools on the computeNode.

 ```R
 # Deletes the dedicated session pool and releases resources
 deleteServicePool(name = "myWebService1234", version = "v1.0.0")
 
 # Check the real-time status of dedicated pool
 getPoolStatus(name = "myWebService1234", version = "v1.0.0")
 
 # make sure the return status is NotFound on all computeNodes
 # if not, issue anthor deleteServicePool command again
```
## See also

 + [What are web services in Machine Learning Server?](concept-what-are-web-services.md)
 + [Evaluate web service capacity: generic session pools](configure-evaluate-capacity.md#pool)
