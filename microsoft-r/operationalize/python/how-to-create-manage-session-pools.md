---

# required metadata
title: "Create and manage session pools for fast web service connections in Python (Machine Learning Server)"
description: "Allocate resources for pre-loading web service connections and dependencies in Python solutions (Machine Learning Server ). "
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

# How to create and manage session pools for fast web service connections in Python

**Applies to: Machine Learning Server 9.3 (Python)**

Fast connections to a web service are possible when you create sessions and load dependencies in advance. Sessions are available in a pool dedicated to a specific web service, where each session includes an instance of the Python interpreter and a copy of dependencies required by the web service. 

For example, if you create ten sessions in advance for a web service that uses numpy, pandas, scikit, revoscalepy, microsoftml, and azureml-model-management-sdk, each session would have its own instance of the Python interpreter plus a copy of each module loaded into memory. 

A web service having a dedicated session pool never requests connections from the [generic session pool](../configure-evaluate-capacity.md#pool) shared resource, not even when maximum sessions are reached. The generic session pool services only those web services that do not have dedicated resources.

For Python script, the [MLServer](../../python-reference/azureml-model-management-sdk/mlserver.md) class in the [azureml-model-management-sdk](../../python-reference/azureml-model-management-sdk/azureml-model-management-sdk.md) function library provides three functions for creating and managing sessions:

+ [create_or_update_service_pool](../../python-reference/azureml-model-management-sdk/mlserver.md#create_or_update_service_pool)
+ [get_service_pool_status](../../python-reference/azureml-model-management-sdk/mlserver.md#get_service_pool_status)
+ [delete_service_pool](../../python-reference/azureml-model-management-sdk/mlserver.md#delete_service_pool)

## Create or modify a dedicated session pool

You can use a [Python interactive window](../../python/quickstart-python-tools.md) to run the following commands on the local server if you configured Machine Learning Server for one-box, or on a compute node if you have a distributed topology.

```python
 # load modules and classes
 from azureml.deploy import DeployClient
 from azureml.deploy.server import MLServer

 # Set up the connection
 # Be sure to replace the password placeholder
host = "http://localhost:12800"
ctx = ("admin", "password-placeholder")
client = DeployClient(host, use=MLServer, auth=ctx)

 # Return a list of web services to get the service and version 
 # Both service name and version number are required
 svc = client.list_services()
 print(svc[0]['name'])
 print(svc[0]['version'])

 # Create the session pool using a case-sensitive web service name
 # A status code of 200 is returned upon success
 svc = client.create_or_update_service_pool(name = "myWebservice1234", version = "v1.0.0", initial_pool_size = 5, max_pool_size = 10 )

 # Return status 
 # Pending indicates session creation is in progress. Success indicates sessions are ready.
 svc = client.get_service_pool_status(name = "myWebService1234", version = "v1.0.0")
 print(svc[0])
```

Currently, there are no commands or functions that return actual session pool usage. The log file is your best resource for analyzing connection and service activity. For more information, see [Trace user actions](../configure-run-diagnostics.md#trace-user-actions).

## Delete a session pool

On the compute node, run the following command to delete the session pool for a given service.

```python
 # Return a list of web services to get the service and version information
 svc = client.list_services()
 print(svc[0]['name'])
 print(svc[0]['version'])

 # Deletes the dedicated session pool and releases resources
 svc = client.delete_service_pool(name = "myWebService1234", version = "v1.0.0")
```

This feature is still under development. In rare cases, the [delete_service_pool](../../python-reference/azureml-model-management-sdk/mlserver.md#delete_service_pool) command may fail to actually delete the pool on the computeNode. If you encounter this situation, issue a new [delete_service_pool](../../python-reference/azureml-model-management-sdk/mlserver.md#delete_service_pool) request. Use the [get_service_pool_status](../../python-reference/azureml-model-management-sdk/mlserver.md#delete_service_pool) command to monitor the status of dedicated pools on the computeNode.
 ```python
 # Deletes the dedicated session pool and releases resources
 client.delete_service_ool(name = "myWebService1234", version = "v1.0.0")
 
 # Check the real-time status of dedicated pool
 client.get_service_pool_status(name = "myWebService1234", version = "v1.0.0")
 
 # make sure the returned status is NotFound on all computeNodes
 # if not, issues another delete_service_pool command again
```

## See also

 + [What are web services in Machine Learning Server?](../concept-what-are-web-services.md)
 + [Evaluate web service capacity: generic session pools](../configure-evaluate-capacity.md#pool)
