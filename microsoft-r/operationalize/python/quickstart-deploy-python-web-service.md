---

# required metadata
title: "How to deploy Python models as web services - Machine Learning Server "
description: "How to deploy an python model as a service"
keywords: quickstart, Machine Learning Server, deploy python models
author: "dphansen"
ms.author: "davidph"
manager: "cgronlun"
ms.date: 2/16/2018
ms.topic: "quickstart"
ms.prod: "mlserver"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
#ms.technology: ""
#ms.custom: ""
---
# Quickstart: Deploy a Python model as a web service with azureml-model-management-sdk

**Applies to: Machine Learning Server 9.x**

Learn how to deploy a Python model as a [web service with Machine Learning Server](../concept-what-are-web-services.md). Data scientists work locally in their preferred Python IDE and favorite version control tools to build scripts and models. Using the [azureml-model-management-sdk Python package](../../python-reference/azureml-model-management-sdk/azureml-model-management-sdk.md) that ships with Machine Learning Server, you can develop, test, and ultimately deploy these Python analytics as web services in your production environment. 

In Machine Learning Server, a web service is a model and/or code that has been deployed and hosted in the server.  Each web service is uniquely defined by a `name` and `version`. When consumed, the service consists of the code execution on a [compute node](../configure-start-for-administrators.md#configure-server-for-operationalization). Learn more [about web services](../concept-what-are-web-services.md).

You can use the functions in [the azureml-model-management-sdk Python library ](../../python-reference/azureml-model-management-sdk/azureml-model-management-sdk.md) to manage the web service's lifecycle from a Python script. A set of [RESTful APIs](https://microsoft.github.io/deployr-api-docs/#services-management-apis) is also available to provide direct programmatic access to a service's lifecycle.


## Time estimate

After you have completed the prerequisites, this task takes approximately *10* minutes to complete.

## Prerequisites

Before you begin this QuickStart, have the following ready:

> [!div class="checklist"]
> * Machine Learning Server with Python that's **[configured to operationalize models](../../operationalize/configure-machine-learning-server-one-box.md)**. Try an [Azure Resource Management template](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) to quickly configure a server for operationalization.<br/>&nbsp;
> * The [connection details (host, username, password)](../../operationalize/python/how-to-authenticate-in-python.md)  to that server instance from your administrator.  <br/>&nbsp;
> * The [Python client libraries for remote access to Machine Learning Server](https://docs.microsoft.com/machine-learning-server/install/python-libraries-interpreter) installed on your machine.
> * Familiarity with Python. [Here's a video tutorial](https://mva.microsoft.com/en-us/training-courses/introduction-to-programming-with-python-8360?l=lqhuMxFz_8904984382).<br/>&nbsp;
> * Familiarity with Jupyter Notebooks. Learn [how to load sample Python notebooks](../../python/how-to-revoscalepy-jupyter-nb-config.md). 


## Example code

The example for this quickstart is stored in a Jupyter Notebook. This notebook format allows you to not only see the code alongside detailed explanations, but also allows you to try out the code.

This example walks through the deployment of a Python model as a web service hosted in Machine Learning Server. We will build a simple linear model using the [rx_lin_mod](../../python-reference/revoscalepy/rx-lin-mod.md) function from the [revoscalepy package](../../python-reference/revoscalepy/revoscalepy-package.md) installed with Machine Learning Server or [locally on Windows machine](../../install/python-libraries-interpreter.md). This package requires a connection to Machine Learning Server.  

The notebook example walks you through how to:
1. Create and run a linear model locally

1. [Authenticate with Machine Learning Server](how-to-authenticate-in-python.md) from your Python script

1. Publish the model as a Python web service to Machine Learning Server

1. Examine, test, and consume the service in the same session

1. Delete the service

You can try it yourself with the notebook. 

### &#9658; [**Download the Jupyter Notebook to try it out**](https://github.com/Microsoft/ML-Server-Python-Samples/blob/master/operationalize/Quickstart_Publish_Python_Web_Service.ipynb).

## Next steps

After it has been deployed, the web service can be: 

+ [Consumed directly in Python by someone else](how-to-consume-web-services.md) for testing purposes See an example in [this Jupyter Notebook](https://github.com/Microsoft/ML-Server-Python-Samples/blob/master/operationalize/Explore_Consume_Python_Web_Services.ipynb).

+ [Integrated into an application by an application developer](../how-to-build-api-clients-from-swagger-for-app-integration.md)  using the  Swagger-based .JSON file produced when the web service was published. 

## See also

This section provides a quick summary of useful links for data scientists looking to operationalize their analytics with Machine Learning Server.

 + [About Operationalization](../../what-is-operationalization.md)   
 + [What are web services](../concept-what-are-web-services.md) 
 + [Functions in azureml-model-management-sdk package](../../python-reference/azureml-model-management-sdk/azureml-model-management-sdk.md)    
 + [Connecting to Machine Learning Server in Python](how-to-authenticate-in-python.md)    
 + [Working with web services in Python](how-to-deploy-manage-web-services.md)  
 + [Example of deploying real-time web service](https://github.com/Microsoft/ML-Server-Python-Samples/blob/master/operationalize/Publish_Realtime_Web_Service_in_Python.ipynb)  
 + [How to consume web services in Python synchronously (request/response)](how-to-consume-web-services.md)    
 + [How to consume web services in Python asynchronously (batch)](how-to-consume-web-services-async.md)    
 + [How to integrate web services and authentication into your application](../how-to-build-api-clients-from-swagger-for-app-integration.md)    
 + [Get started for administrators](../configure-start-for-administrators.md)     
 + [User Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=microsoftr)
