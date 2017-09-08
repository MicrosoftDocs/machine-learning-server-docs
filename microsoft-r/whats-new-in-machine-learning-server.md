---

# required metadata
title: "New in Machine Learning Server 9.2 | Microsoft Docs"
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

Machine Learning Server 9.2 is based on Microsoft R Server 9.1, now with libraries and an interpreter for solutions scripted in Python. Language-specific development is available when you add Python or R support (or both) during setup.

> [!Note]
> For features in R Client, see [What's New for Microsoft R Client](r-client/what-is-microsoft-r-client.md#r-client-whats-new).

## Python development

Python libraries include **revoscalepy**, **microsoftml**, and **azureml-model-management-sdk**. Modules are built on Anaconda 4.2 over Python 3.5. You can run any 3.5-compatible library on a Python interpreter included in Machine Learning Server.

+ [revoscalepy](python-reference/revoscalepy/revoscalepy-package.md) for Spark 2.0-2.4 over HDFS. When you install revoscalepy through Machine Learning Server, you can call [rxec_by](python-reference/revoscalepy/rxexec-by.md) and [rx_partition](python-reference/revoscalepy/rx-partition.md) for parallelization of workloads.

+ revoscalepy and pyspark interoperations in a [Spark compute context](python-reference/revoscalepy/rxSparkConnect.md).

+ [Pretrained models](install/microsoftml-install-pretrained-models.md) for image featurizaton and sentiment analysis articulated in Python.

+ [microsoftml](python-reference/microsoftml/microsoftml-package.md) parallelzation on Spark 2.0-2.4 over HDFS through[ensembling. 

+ [Deploy Python models and code as web services](operationalize/python/quickstart-deploy-python-web-service.md) using the convenient Python classes and functions in the [azureml-model-management-sdk library](python-reference/azureml-model-management-sdk/azureml-model-management-sdk.md).

+ Deploy realtime Python models as web services.


## R development

R function libraries are built on Microsoft R Open (MRO), Microsoft's distribution of open source R 3.4.1. Enhancements for R in Machine Learning Server include the following items.

+ [R realtime model scoring](operationalize/how-to-deploy-web-service-publish-manage-in-r.md#realtime) is now also supported on Linux.


## Configuration

+ [Role-based access control](operationalize/configure-roles.md) (RBAC) has been extended with a new explicit Reader role.
 
+ [Register your compute nodes with your web nodes](operationalize/configure-use-admin-utility.md#uris) in a centralized and simplified way in the Administration Utility.

## Previous versions

See [Feature announcements in R Server](whats-new-in-r-server.md), version 9.1 and earlier.

## See Also

 [Welcome to Machine Learning Server](what-is-machine-learning-server.md) 

 [Install Machine Learning Server on Windows](install/r-server-install-windows.md)  

 [Install Machine Learning Server on Linux](install/r-server-install-linux-server.md)  

 [Install Machine Learning Server on Hadoop](install/r-server-install-hadoop.md)

 [Configure Machine Learning Server to operationalize your analytics](operationalize/configure-start-for-administrators.md#configure-server-for-operationalization) 