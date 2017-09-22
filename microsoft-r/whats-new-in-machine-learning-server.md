---

# required metadata
title: "New in Machine Learning Server 9.2.1 | Microsoft Docs"
description: "New features, improvements, and changes in this release of Machine Learning Server."
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "jhubbard"
ms.date: "09/07/2017"
ms.topic: "article"
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

# What's new in Machine Learning Server

**Applies to: Machine Learning Server** &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[(Find R Server 9.x What's New)](whats-new-in-r-server.md) 

Machine Learning Server 9.2.1 expands upon Microsoft R Server 9.1 with new Python libaries for integrating machine learning and data science into analytical solutions in the enterprise. Language-specific development is available when you add Python or R support (or both) during setup.

> [!Note]
> Read our [release announcement for Machine Learning Serve 9.2.1](https://aka.ms/mlserver92). <br/><br/>For features in R Client, see [What's New for Microsoft R Client](r-client/what-is-microsoft-r-client.md#r-client-whats-new).

## Python development

In Machine Learning Server, Python support is through libraries that can be used in script executing locally, or remotely in either Spark over Hadoop Distributed File System (HDFS) or in a SQL Server compute context. 

Python libraries include **revoscalepy**, **microsoftml**, and **azureml-model-management-sdk**. Modules are built on Anaconda 4.2 over Python 3.5. You can run any 3.5-compatible library on a Python interpreter included in Machine Learning Server.

Together, the libraries provide full-spectrum data mining: data transformation and manipulation, analysis and visualization, model management. Machine learning algorithms, as well as pre-trained models provided by Microsoft, are now in Python. 

+ [revoscalepy](python-reference/revoscalepy/revoscalepy-package.md) is a library provided by Microsoft to support distributed computing, local compute context, remote compute context for SQL Server and Spark, and high-performance algorithms for Python, similar to RevoScaleR. 

 Originally introduced in SQL Server 2017, this library is extended in Machine Learning Server to support a remote compute context for Spark 2.0-2.4 over the Hadoop Distributed File System (HDFS), with Python functions that run jobs in parallel across multiple nodes. This [tutorial](python/quickstart-revoscalepy-linear-regression-model.md) gets you started.

+ [revoscalepy and pyspark interoperability](python/tutorial-revoscalepy-pyspark.md) in a [Spark compute context](python-reference/revoscalepy/rxSpark.md).

+ [Pre-trained models](install/microsoftml-install-pretrained-models.md) for image classification and sentiment detection articulated in Python.

+ [microsoftml](python-reference/microsoftml/microsoftml-package.md) machine learning algorithms and data mining. 

+ [Deploy Python models and code as web services](operationalize/python/quickstart-deploy-python-web-service.md) using the convenient Python classes and functions in the [azureml-model-management-sdk library](python-reference/azureml-model-management-sdk/azureml-model-management-sdk.md).

+ [Deploy realtime Python models as web services](operationalize/concept-what-are-web-services.md#realtime).

> [!Note]
> Remote execution is not available for Python scripts. For information about to do this in R, see [Remote execution in R](r/how-to-execute-code-remotely.md).

## R development

+ R function libraries are built on [Microsoft R Open (MRO)](https://mran.microsoft.com/open/), Microsoft's distribution of open source R 3.4.1. 

+ [R realtime model scoring](operationalize/how-to-deploy-web-service-publish-manage-in-r.md#realtime) is now also supported on Linux.

+ The last several releases of R Server added substantial capability for R developers. To review recent additions to R functionality, see [feature announcements](whats-new-in-r-server.md) for previous versions.

## Configuration

+ [Role-based access control](operationalize/configure-roles.md) (RBAC) has been extended with a new explicit Reader role.
 
+ [Register your compute nodes with your web nodes](operationalize/configure-use-admin-utility.md#uris) in a centralized and simplified way in the Administration Utility.

## Previous versions

Visit [Feature announcements in R Server](whats-new-in-r-server.md) version 9.1 and earlier, for descriptions of features added in recent past releases.

## See Also

 [Welcome to Machine Learning Server](what-is-machine-learning-server.md) 

 [Install Machine Learning Server on Windows](install/r-server-install-windows.md)  

 [Install Machine Learning Server on Linux](install/r-server-install-linux-server.md)  

 [Install Machine Learning Server on Hadoop](install/r-server-install-hadoop.md)

 [Configure Machine Learning Server to operationalize your analytics](operationalize/configure-start-for-administrators.md#configure-server-for-operationalization) 