---

# required metadata
title: "Quickstart for deploying Python models as web services - Machine Learning Server | Microsoft Docs"
description: "How to deploy an R model as a service"
keywords: "quickstart, Machine Learning Server, deploy python models"
author: "j-martens"
ms.author: "jmartens"
manager: "jhubbard"
ms.date: "9/25/2017"
ms.topic: "get-started-article"
ms.prod: "microsoft-r"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: "r"
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
ms.technology: 
  - deployr
  - r-server
#ms.custom: ""

---
# Deploy a Python model as a web service with azureml-model-management-sdk

**Applies to: Microsoft Learning Server 9.2**

## Objective

Learn how to deploy an Python model as a web service with Machine Learning Server. Data scientists work locally in their preferred Python IDE and favorite version control tools to build scripts and models. Using the [azureml-model-management-sdk Python package](../../python-reference/azureml-model-management-sdk/azureml-model-management-sdk.md) that ships with Machine Learning Server, you can develop, test, and ultimately deploy these Python analytics as web services in your production environment. 

In Machine Learning Server, a web service is a Python code execution on the [compute node](../configure-start-for-administrators.md#configure-server-for-operationalization). Each web service is uniquely defined by a `name` and `version`. You can use the functions in [the azureml-model-management-sdk Python library ](../python-reference/azureml-model-management-sdk/azureml-model-management-sdk.md) to gain access a service's lifecycle from a Python script. A set of [RESTful APIs](https://microsoft.github.io/deployr-api-docs/#services-management-apis) are also available to provide direct programmatic access to a service's lifecycle directly. 

## Time estimate

If you have completed the prerequisites, this task takes approximately *10* minutes to complete.

## Prerequisites

Before you begin this QuickStart, have the following ready:

+ An instance of [Anaconda and the azureml-model-management-sdk package installed](../../install/python-libraries-interpreter.md) on your local machine.    

+ An instance of [Machine Learning Server ](../../what-is-machine-learning-server.md) installed that has been [configured to operationalize analytics](../../operationalize/configure-start-for-administrators.md#configure-server-for-operationalization).

+ The connection details to that instance of Machine Learning Server. Contact your administrator for any missing connection details. After [connecting to Machine Learning Server](../../operationalize/python/how-to-authenticate-in-python.md) in Python, deploy your analytics as web services so others can consume them. 


## Example code

The example for this quickstart is stored in a Jupyter notebook. This notebook format allows you to not only see the code alongside detailed explanations, but also allows you to try out the code.

This example walks through the deployment of a Python model as a web service hosted in Machine Learning Server. We will build a simple linear model using the [rx_lin_mod](../../python-reference/revoscalepy/rx-lin-mod.md) function from the [revoscalepy package](../../python-reference/revoscalepy/revoscalepy-package.md) installed with Machine Learning Server or [locally](../../install/python-libraries-interpreter.md). This package requires a connection to Machine Learning Server.  

>[!IMPORTANT]
>Be sure to replace with the correct login details for your configuration. Connecting to Machine Learning Server using the azureml-model-management-sdk library is covered [in this article](../../operationalize/python/how-to-authenticate-in-python.md).

[**Try it out. Download the Jupyter notebook**](https://notebooks.azure.com/jmartens/libraries/pyservice17).

The notebook example walks you through:
+ Create and run a linear model locally
+ Authenticate with Machine Learning Server
+ Publish the model as a Python web service on Machine Learning Server
+ Consume the service and make a prediction
+ List all services on Machine Learning Server
+ Retrieve a web service object for consumption
+ Delete a service


## Next steps

After it has been deployed, the web service can be: 
+ Consumed directly in Python by another data scientist for testing purposes, for example 

+ [Integrated into an application by an application developer](how-to-build-api-clients-from-swagger-for-app-integration.md)  using the  Swagger-based .JSON file produced when the web service was published. 

## More resources

This section provides a quick summary of useful links for data scientists operationalizing analytics with Machine Learning Server.

>Use the table of contents to find all the documentation.

**Key Documents**
 + [About Operationalization](../what-is-operationalization.md)    
 + [Functions in azureml-model-management-sdk package](../python-reference/azureml-model-management-sdk/azureml-model-management-sdk.md)    
 + [Connecting to Machine Learning Server in Python](how-to-authenticate-in-python.md)    
 + [Working with web services in Python](how-to-deploy-manage-web-services.md)    
 + [Asynchronous batch execution of web services in Python](how-to-consume-web-service-batch.md)    
 + [How to integrate web services and authentication into your application](how-to-build-api-clients-from-swagger-for-app-integration.md)    
 + [Get started for administrators](configure-start-for-administrators.md)    

**Support Channel**
 + [User Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=microsoftr)