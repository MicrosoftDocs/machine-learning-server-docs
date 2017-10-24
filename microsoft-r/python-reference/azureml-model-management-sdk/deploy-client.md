--- 
 
# required metadata 
title: "DeployClient: from azureml-model-management-sdk – Machine Learning Server " 
description: "" 
keywords: "" 
author: "Microsoft" 
manager: "Microsoft" 
ms.date: "09/20/2017" 
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

# Class DeployClient


## DeployClient



```
azureml.deploy.DeployClient(host, auth=None, use=None)
```




Defines the factory for creating Deployment Clients.

**Basic Usage Module implementation plugin with `use` property:**

Find and Load *module* from an import reference:



```
from azureml.deploy import DeployClient
from azureml.deploy.server import MLServer

host = 'http://localhost:12800'
ctx = ('username', 'password')
mls_client = DeployClient(host, use=MLServer, auth=ctx)
```


Find and Load *module* as defined by *use* from namespace str:



```
host = 'http://localhost:12800'
ctx = ('username', 'password')

mls_client = DeployClient(host, use=MLServer, auth=ctx)
mls_client = DeployClient(host, use='azureml.deploy.server.MLServer',
auth=ctx)
```


Find and Load *module* from a file/path tuple:



```
host = 'http://localhost:12800'
ctx = ('username', 'password')

use = ('azureml.deploy.server.MLServer', '/path/to/mlserver.py')
mls_client = DeployClient(host, use=use, auth=ctx)
```


Create a new Deployment Client.


### Arguments


### host

Server HTTP/HTTPS endpoint, including the port number.


### auth

(optional) Authentication context. Not all deployment
clients require authentication. The *auth* is  **required** for
**MLServer**


### use

(required) Deployment implementation to use (ex)
*use=’MLServer’* to use The ML Server.
