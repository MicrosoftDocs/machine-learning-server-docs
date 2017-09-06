--- 
 
# required metadata 
title: "DeployClient: " 
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

# DeployClient



```
azureml.deploy.DeployClient(host, auth=None, use=None)
```




Factory for creating Deployment Clients.

**Basic Usage:**

>>> auth = ('username', 'password')
>>> client = DeployClient('url', use='MLServer', auth=auth)

**Module implementation plugin with `use` property:**

Find and Load *module* as defined by *use* from namespace str:

>>> client = DeployClient('url', use='azureml.deploy.server.MLServer')
>>> client = DeployClient('url', use='MLServer')

Find and Load *module* from a file/path tuple:

>>> use = ('azureml.deploy.server.MLServer', '/path/to/mlserver.py')
>>> client = DeployClient('url', use=use)

Find and Load *module* from an import reference:

>>> from azureml.deploy.server import MLServer
>>> client = DeployClient('url', use=MLServer)
