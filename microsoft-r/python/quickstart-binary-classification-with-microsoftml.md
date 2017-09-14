---

# required metadata
title: "Quickstart for binary classification with microsoftml - Machine Learning Server | Microsoft Docs"
description: "How to deploy an R model as a service"
keywords: "quickstart, Machine Learning Server, deploy python models"
author: "bradsev"
ms.author: "bradsev"
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
# Binary classifications with microsoftml

**Applies to: Microsoft Learning Server 9.2**

## Objective

Learn how to deploy an Python model as a web service with Machine Learning Server. Data scientists work locally in their preferred Python IDE and favorite version control tools to build scripts and models. Using the [azureml-model-management-sdk Python package](../../python-reference/azureml-model-management-sdk/azureml-model-management-sdk.md) that ships with Machine Learning Server, you can develop, test, and ultimately deploy these Python analytics as web services in your production environment. 

In Machine Learning Server, a web service is a Python code execution on the [compute node](../configure-start-for-administrators.md#configure-server-for-operationalization). Each web service is uniquely defined by a `name` and `version`. You can use the functions in [the azureml-model-management-sdk Python library ](../../python-reference/azureml-model-management-sdk/azureml-model-management-sdk.md) to gain access a service's lifecycle from a Python script. A set of [RESTful APIs](https://microsoft.github.io/deployr-api-docs/#services-management-apis) are also available to provide direct programmatic access to a service's lifecycle directly. 

## Time estimate

If you have completed the prerequisites, this task takes approximately *10* minutes to complete.

## Prerequisites

Before you begin this QuickStart, have the following ready:

+ An instance of [Anaconda and the azureml-model-management-sdk package installed](../../install/python-libraries-interpreter.md) on your local machine.    

+ An instance of [Machine Learning Server ](../../what-is-machine-learning-server.md) installed with the Python option, which has been [configured to operationalize analytics](../../operationalize/configure-start-for-administrators.md#configure-server-for-operationalization).

+ The connection details to that instance of Machine Learning Server. Contact your administrator for any missing connection details. After [connecting to Machine Learning Server](../../operationalize/python/how-to-authenticate-in-python.md) in Python, deploy your analytics as web services so others can consume them. 


## Example code

The example for this quickstart is stored in a Jupyter notebook. This notebook format allows you to not only see the code alongside detailed explanations, but also allows you to try out the code.

This example walks through the deployment of a Python model as a web service hosted in Machine Learning Server. We will build a simple linear model using the [rx_lin_mod](../../python-reference/revoscalepy/rx-lin-mod.md) function from the [revoscalepy package](../../python-reference/revoscalepy/revoscalepy-package.md) installed with Machine Learning Server or [locally](../../install/python-libraries-interpreter.md). This package requires a connection to Machine Learning Server.  

The notebook example walks you through how to:
+ something
+ something
+ something


### &#9658; [**Download the Jupyter notebook to try it out**](https://notebooks.azure.com/jmartens/libraries/pyservice17).

&nbsp;

>[!IMPORTANT]
>Be sure to replace with the correct login details for your configuration. Connecting to Machine Learning Server using the azureml-model-management-sdk library is covered [in this article](how-to-authenticate-in-python.md).

## Next steps

After it has been deployed, the web service can be: 
+ Consumed directly in Python by another data scientist for testing purposes, for example 

+ [Integrated into an application by an application developer](../how-to-build-api-clients-from-swagger-for-app-integration.md)  using the  Swagger-based .JSON file produced when the web service was published. 

## See also

This section provides a quick summary of useful links for data scientists operationalizing analytics with Machine Learning Server.

**Key Documents**

**Support Channel**
 + [User Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=microsoftr)


Find more information in the table of contents.
