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

**Applies to: Microsoft Learning Server 9.2.1**

## Objective

Learn how to deploy an Python model as a [web service with Machine Learning Server](../concept-what-are-web-services.md). Data scientists work locally in their preferred Python IDE and favorite version control tools to build scripts and models. Using the [azureml-model-management-sdk Python package](../../python-reference/azureml-model-management-sdk/azureml-model-management-sdk.md) that ships with Machine Learning Server, you can develop, test, and ultimately deploy these Python analytics as web services in your production environment. 

In Machine Learning Server, a web service is a Python code execution on the [compute node](../configure-start-for-administrators.md#configure-server-for-operationalization). Each web service is uniquely defined by a `name` and `version`. You can use the functions in [the azureml-model-management-sdk Python library ](../../python-reference/azureml-model-management-sdk/azureml-model-management-sdk.md) to manage a service's lifecycle from a Python script. A set of [RESTful APIs](https://microsoft.github.io/deployr-api-docs/#services-management-apis) are also available to provide direct programmatic access to a service's lifecycle. 

[Learn more about web services](../concept-what-are-web-services.md).

## Time estimate

If you have completed the prerequisites, this task takes approximately *10* minutes to complete.

## Prerequisites

Before you begin this QuickStart, have the following ready:

> [!div class="checklist"]
> * Machine Learning Server with Python that's **[configured to operationalize models](../../operationalize/configure-machine-learning-server-one-box.md)**. Try an [Azure Resource Management template](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview#template-deployment) to quickly configure a server for operationalization.<br/>
> * The [connection details (host, username, password)](../../operationalize/python/how-to-authenticate-in-python.md)  to that server instance from your administrator.  <br/>
> * Familiarity with Python. [Here's a video tutorial](https://mva.microsoft.com/en-us/training-courses/introduction-to-programming-with-python-8360?l=lqhuMxFz_8904984382).<br/>
> * Familiarity with Jupyter notebooks. Learn [how to load sample Python notebooks](../../python/how-to-revoscalepy-jupyter-nb-config.md). 


## Example code

The example for this quickstart is stored in a Jupyter notebook. This notebook format allows you to not only see the code alongside detailed explanations, but also allows you to try out the code.

This example walks through the deployment of a Python model as a web service hosted in Machine Learning Server. We will build a simple linear model using the [rx_lin_mod](../../python-reference/revoscalepy/rx-lin-mod.md) function from the [revoscalepy package](../../python-reference/revoscalepy/revoscalepy-package.md) installed with Machine Learning Server or [locally on Windows machine](../../install/python-libraries-interpreter.md). This package requires a connection to Machine Learning Server.  

The notebook example walks you through how to:
* Create and run a linear model locally
* Authenticate with Machine Learning Server
* Publish the model as a Python web service on Machine Learning Server
* Examine, test, and consume the service
* Delete the service


### &#9658; [**Download the Jupyter notebook to try it out**](https://github.com/Microsoft/ML-Server-Python-Samples/blob/master/operationalize/Quickstart_Publish_Python_Web_Service.ipynb).

>[!IMPORTANT]
>Use the correct login details for your configuration when trying the notebook. Authentication is covered [in this article](how-to-authenticate-in-python.md).

## Next steps

After it has been deployed, the web service can be: 
+ Consumed directly in Python by another data scientist for testing purposes, for example 

+ [Integrated into an application by an application developer](../how-to-build-api-clients-from-swagger-for-app-integration.md)  using the  Swagger-based .JSON file produced when the web service was published. 

## See also

This section provides a quick summary of useful links for data scientists looking to operationalize their analytics with Machine Learning Server.

 + [About Operationalization](../../what-is-operationalization.md)   
 + [What are web services](../concept-what-are-web-services.md) 
 + [Functions in azureml-model-management-sdk package](../../python-reference/azureml-model-management-sdk/azureml-model-management-sdk.md)    
 + [Connecting to Machine Learning Server in Python](how-to-authenticate-in-python.md)    
 + [Working with web services in Python](how-to-deploy-manage-web-services.md)    
 + [How to integrate web services and authentication into your application](../how-to-build-api-clients-from-swagger-for-app-integration.md)    
 + [Get started for administrators](../configure-start-for-administrators.md)     
 + [User Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=microsoftr)
