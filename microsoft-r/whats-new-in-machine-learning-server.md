---

# required metadata
title: "Machine Learning Server 9.4 Release Notes"
description: "Features, improvements, and changes in release 9.4 of Machine Learning Server."
keywords: 
author: "DaniBunny"
ms.author: "dacoelho"
manager: "cgronlun"
ms.date: 07/15/2019
ms.topic: "overview"
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

# Machine Learning Server 9.4 Release Notes

[!INCLUDE [retirement banner](~/includes/machine-learning-server-retirement.md)]

Machine Learning Server provides powerful R and Python function libraries for data science and machine learning on small-to-massive data sets, in parallel on local or distributed systems, with modern algorithms for predictive analytics, supervised learning, and data mining. 

Functionality is delivered through proprietary R and Python packages, internal computational engines built on open-source R and Python, tools, solutions, and samples.

In this article, learn about the capabilities introduced in the latest packages and tools. If you develop in R, you might also want to review feature announcements from recent past releases. 

> [!Note]
> For updates to R Client, see [What's New for Microsoft R Client](r-client/what-is-microsoft-r-client.md#r-client-whats-new). For known issues and workarounds, see [Known issues in 9.4](resources-known-issues.md).

## Announced in 9.4

9.4 updates the R and Python engines and adds support for Spark 2.4 and CDH 6.1. Also, on CDH customers can install either R or Python or both.

## Announced in 9.3

New capabilities introduced in 9.3 are listed in the following table.

| Feature | Area | Details |
|---------|------|---------|
| [Administration command line interface](operationalize/configure-admin-cli-launch.md) | [Operationalization](what-is-operationalization.md) | Refactored tooling for Machine Learning Server configuration. The new command-line interface is similar to Azure CLIs and offers full parity with the previous utility. <br/><br/>Use the tool to enable web service deployment, web and compute node designations, and remote execution (R only). You can also manage ports, nodes, credentials; run diagnostic reports; and test the capacity and throughput of web services you create. |
| Dedicated session pools | [Operationalization](what-is-operationalization.md) | You can construct a dedicated session pool for a specific web service to provide ready-to-use connections with preloaded dependencies for fast access to production code. This capability is in addition to the [generic session pools](operationalize/configure-evaluate-capacity.md#pool) that you can establish server-wide as a shared resource for all web services. [Configure in R](operationalize/how-to-create-manage-session-pools.md) &#124; [Configure in Python](operationalize/python/how-to-create-manage-session-pools.md). <br/><br/>For R script, the [mrsdeploy](r-reference/mrsdeploy/mrsdeploy-package.md) function library provides three new functions for managing dedicated sessions: [configureServicePool](r-reference/mrsdeploy/configureServicePool.md), [getPoolStatus](r-reference/mrsdeploy/getPoolStatus.md), [deleteServicePool](r-reference/mrsdeploy/deleteServicePool.md). <br/><br/>For Python, the [azureml-model-management-sdk](/sql/machine-learning/python/reference/azureml-model-management-sdk/azureml-model-management-sdk) provides the following methods in the mlserver class: [create_or_update_service_pool](/sql/machine-learning/python/reference/azureml-model-management-sdk/mlserver#create_or_update_service_pool), [delete_service_pool](/sql/machine-learning/python/reference/azureml-model-management-sdk/mlserver#delete_service_pool), [get_service_pool_status](/sql/machine-learning/python/reference/azureml-model-management-sdk/mlserver#get_service_pool_status).| 
| CDH 5.12 | Supported platforms | Version 5.12 of Cloudera distribution of Apache Hadoop (CDH) is now supported for [Machine Learning Server for Hadoop](install/machine-learning-server-hadoop-install.md).|


<a name = "921"></a>

## Announced in 9.2.1

The 9.2.1 release was the first release of Machine Learning Server - based on R Server - expanded with Python libraries for developers and analysts who code in Python.

The following table summarizes the Python and R features that were introduced in the 9.2.1 release. For more information, see [release announcement for Machine Learning Server 9.2.1](https://aka.ms/mlserver92).

| Feature | Language | Details |
|---------|----------|---------|
| [revoscalepy](python-reference/revoscalepy/revoscalepy-package.md) | Python | The first release of this library, used for distributed computing, local compute context, remote compute context for SQL Server and Spark 2.0-2.1 over the Hadoop Distributed File System (HDFS), and high-performance algorithms for Python. This library is similar to [RevoScaleR](r-reference/revoscaler/revoscaler.md) for R. |
| [microsoftml](/sql/machine-learning/python/ref-py-microsoftml) | Python | The first release of this library, used for machine learning algorithms and data mining. This library is similar to [MicrosoftML](r-reference/microsoftml/microsoftml-package.md) for R.) |
| [Pre-trained models](install/microsoftml-install-pretrained-models.md) | Python | Ready-to-use machine learning models for image classification and sentiment detection articulated in Python. |
| [azureml-model-management-sdk library](/sql/machine-learning/python/reference/azureml-model-management-sdk/azureml-model-management-sdk) | Python | The first release of this library, used to programmatically build web services encapsulating your Python script. |
| [Standard web service support](operationalize/concept-what-are-web-services.md#standard-web-services) | Python | Contains Python code, models, and model assets. Accepts specific inputs and provides specific outputs for integration with other services and applications. |
| [Real-time web service support](operationalize/concept-what-are-web-services.md#realtime) | Python | A fully encapsulated web service with no inputs or outputs (operates on dataframes). |
| [Real-time model scoring](operationalize/how-to-deploy-web-service-publish-manage-in-r.md#realtime) | R | Now supported on Linux. |
|[Role-based access control](operationalize/configure-roles.md) (RBAC) | Both| RBAC was extended with a new explicit Reader role. |
| [Administration utility update](operationalize/configure-admin-cli-compute-uris.md) | Both | The utility simplifies registration of compute nodes with web nodes. |

### Python development

In Machine Learning Server, Python libraries used in script execute locally, or remotely in either Spark over Hadoop Distributed File System (HDFS) or in a SQL Server compute context. Libraries are built on Anaconda 4.2 over Python 3.5. You can run any 3.5-compatible library on a Python interpreter included in Machine Learning Server.

> [!Note]
> Remote execution is not available for Python scripts. For information about to do this in R, see [Remote execution in R](r/how-to-execute-code-remotely.md).

### R development

R function libraries are built on [Microsoft R Open (MRO)](https://mran.microsoft.com/open/), Microsoft's distribution of open-source R 3.4.1. 

The last several releases of R Server added substantial capability for R developers. To review recent additions to R functionality, see [feature announcements](whats-new-in-r-server.md) for previous versions.

## Previous versions

Visit [Feature announcements in R Server](whats-new-in-r-server.md) version 9.1 and earlier, for descriptions of features added in recent past releases.

## See Also

 + [Welcome to Machine Learning Server](what-is-machine-learning-server.md) 
 + [Install Machine Learning Server on Windows](install/r-server-install-windows.md)  
 + [Install Machine Learning Server on Linux](install/r-server-install-linux-server.md)  
 + [Install Machine Learning Server on Hadoop](install/r-server-install-hadoop.md)
 + [Configure Machine Learning Server to operationalize your analytics](operationalize/configure-start-for-administrators.md#configure-server-for-operationalization) 