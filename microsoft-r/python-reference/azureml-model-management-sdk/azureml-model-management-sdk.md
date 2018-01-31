---

# required metadata
title: "azureml-model-management-sdk package for Python - Machine Learning Server "
description: "azureml-model-management-sdk Functions"
keywords: "azureml-model-management-sdk package reference"
author: "j-martens"
ms.author: "jmartens"
manager: "cgronlun"
ms.date: "9/25/2017"
ms.topic: "reference"
ms.prod: "microsoft-r"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
ms.technology: "r-server"
#ms.custom: ""

---

# azureml-model-management-sdk package
 
**Applies to:  Machine Learning Server, SQL Server 2017**

'azureml-model-management-sdk' is a custom Python package developed by Microsoft. This package provides the classes and functions to deploy and interact with analytic web services. These web services are backed by code block and scripts in Python or R.  

This topic is a high-level description of package functionality. These classes and functions can be called directly. For syntax and other details, see the individual function help topics in the table of contents.

<br/>

| Package details | |
|--------|-|
| Current version: |  1.0.1b7 |
| Built on: | [Anaconda](https://www.continuum.io/why-anaconda) distribution of [Python 3.5](https://www.python.org/doc) |
| Package distribution: | [Machine Learning Server 9.x](../../what-is-machine-learning-server.md) </br>[SQL Server 2017 Machine Learning Server (Standalone)](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone#whats-new-in-microsoft-machine-learning-server) |



## How to use this package

The **azureml-model-management-sdk** package is installed as part of Machine Learning Server and SQL Server 2017 Machine Learning Server (Standalone) when you add Python to your installation. It is also [available locally on Windows](../../install/python-libraries-interpreter.md).  When you install these products, you get the full collection of proprietary packages plus a Python distribution with its modules and interpreters. 

You can use any Python IDE to write Python scripts that call the classes and functions in **azureml-model-management-sdk**. However, the script must run on a computer having Machine Learning Server or SQL Server 2017 Machine Learning Server (Standalone) with Python.

## Use cases

There are three primary use cases for this release: 

+ Adding authentication logic to your Python script
+ Deploying standard or realtime Python web services
+ Managing and consuming these web services

## Main classes and functions

* [DeployClient](deploy-client.md) 

* [MLServer](mlserver.md) 

* [Operationalization](operationalization.md) 

* [OperationalizationDefinition](operationalization-definition.md) 

* [ServiceDefinition](service-definition.md) 

* [RealtimeDefinition](realtime-definition.md) 

* [Service](service.md) 

* [ServiceResponse](service-response.md) 

* [Batch](batch.md) 

* [BatchResponse](batch-response.md) 




## Next steps

Add both Python modules to your computer by running setup: 

+ Set up [Machine Learning Server](../../install/machine-learning-server-install.md) for Python or [Python Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/python/setup-python-machine-learning-services).

Next, follow this quickstart to try it yourself:

+ [Quickstart: How to deploy Python model as a service](../../operationalize/python/quickstart-deploy-python-web-service.md) 

Or, read this how-to article:
+ [How to publish and manage web services in Python](../../operationalize/python/how-to-deploy-manage-web-services.md)


## See also

+ [Library Reference](../introducing-python-package-reference.md)

+ [Install Machine Learning Server](../../what-is-machine-learning-server.md)

+ [Install the Python interpreter and libraries on Windows](../../install/python-libraries-interpreter.md)

+ [How to authenticate in Python with this package](../../operationalize/python/how-to-authenticate-in-python.md)

+ [How to list, get, and consume services in Python with this package](../../operationalize/python/how-to-consume-web-services.md)