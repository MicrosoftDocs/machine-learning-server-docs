---

# required metadata
title: "New in Machine Learning Server 9.3 "
description: "New features, improvements, and changes in release 9.3 of Machine Learning Server."
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "cgronlun"
ms.date: "01/26/2018"
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

# What's new in Machine Learning Server 9.3

Machine Learning Server 9.3 provides powerful R and Python function libraries for data science and machine learning on small-to-massive data sets, in parallel on local or distributed systems, with leading-edge algorithms for predictive analytics, supervised and unsupervised learning, and data mining. 

Function libraries are distributed in propertietary packages from Microsoft, with command/script execution on computational engines running on the server and in related Microsoft technologies (for example, RevoScaleR and revoscalepy run on R Client or on R Server for HDInsight, in addition to Machine Learning Server). 

Open-source R and Python are built into the run-time architecture. This means your code can call open-source functions, third-party functions built for open-source distributions, and the proprietary Python functions that run only on Machine Learning Server and related Microsoft technologies.

In this article, learn about the new capabilities introduced in this release, or review feature announcements from recent past releases. 

> [!Note]
> Read our [release announcement for Machine Learning Server 9.3](UPDATE THIS LINK, was https://aka.ms/mlserver92). For features in R Client, see [What's New for Microsoft R Client](r-client/what-is-microsoft-r-client.md#r-client-whats-new).

## Announced in 9.3

The 9.3 release is the backbone for new-feature integration in Azure Machine Learning so that you can use the same packages (RevoScaleR, revoscalepy, and so forth) in more tools. If you want to run the same R and Python code on Azure Machine Learning and Machine Learning Server, you should install version 9.3.

| Feature | Area | Details |
|---------|------|---------|
| [Administration command line interface](operationalize/configure-use-admin-cli.md) | Operationalize | Configure Machine Learning Server to support web service deployment, web and compute node designations, and remote execution (R only). You can also manage ports, nodes, credentials; run diagnostic reports; and test the capacity and througput of web services you created. This tool is similar to Azure CLI and replaces the previous administration utility. |
| Dedicated service pools | Operationalization | TBD |

## Announced in 9.2.1

The 9.2.1 release was the first release of Machine Learning Server - based on R Server - expanded with Python libraries for developers and analysts who code in Python. 

The following table summarizes the Python and R features that were introduced in the 9.2.1 release.

| Feature | Language | Details |
|---------|----------|---------|
| [revoscalepy](python-reference/revoscalepy/revoscalepy-package.md) | Python | The first release of this library, used for distributed computing, local compute context, remote compute context for SQL Server and Spark 2.0-2.1 over the Hadoop Distributed File System (HDFS), and high-performance algorithms for Python. This library is similar to [RevoScaleR](r-reference/revoscaler/revoscaler.md) for R. |
| [microsoftml](python-reference/microsoftml/microsoftml-package.md) | Python | The first release of this library, used for machine learning algorithms and data mining. This library is similar to [MicrosoftML](r-reference/microsoftml/microsoftml-package.md) for R.) |
| [Pre-trained models](install/microsoftml-install-pretrained-models.md) | Python | Ready-to-use machine learning models for image classification and sentiment detection articulated in Python. |
| [azureml-model-management-sdk library](python-reference/azureml-model-management-sdk/azureml-model-management-sdk.md) | Python | The first release of this library, used to programmatically build web services encapsulating your Python script. |
| [Standard web service support](operationalize/concept-what-are-web-services.md#standard-web-services) | Python | Contains Python code, models, and model assets. Accepts specific inputs and provides specific outputs for integration with other services and applications. |
| [Realtime web service support](operationalize/concept-what-are-web-services.md#realtime) | Python | A fully encapsulated web service with no inputs or outputs (operates on dataframes). |
| [Realtime model scoring](operationalize/how-to-deploy-web-service-publish-manage-in-r.md#realtime) | R | Now supported on Linux. |
|[Role-based access control](operationalize/configure-roles.md) (RBAC) | Both| RBAC was extended with a new explicit Reader role. |
| [Administration utility update](operationalize/configure-use-admin-utility.md#uris) | Both | The utility simplifies registration of compute nodes with web nodes. |

### Python development

In Machine Learning Server, Python libraries used in script execute locally, or remotely in either Spark over Hadoop Distributed File System (HDFS) or in a SQL Server compute context. Libraries are built on Anaconda 4.2 over Python 3.5. You can run any 3.5-compatible library on a Python interpreter included in Machine Learning Server.

> [!Note]
> Remote execution is not available for Python scripts. For information about to do this in R, see [Remote execution in R](r/how-to-execute-code-remotely.md).

### R development

R function libraries are built on [Microsoft R Open (MRO)](https://mran.microsoft.com/open/), Microsoft's distribution of open source R 3.4.1. 

The last several releases of R Server added substantial capability for R developers. To review recent additions to R functionality, see [feature announcements](whats-new-in-r-server.md) for previous versions.

## Previous versions

Visit [Feature announcements in R Server](whats-new-in-r-server.md) version 9.1 and earlier, for descriptions of features added in recent past releases.

## See Also

 + [Welcome to Machine Learning Server](what-is-machine-learning-server.md) 
 + [Install Machine Learning Server on Windows](install/r-server-install-windows.md)  
 + [Install Machine Learning Server on Linux](install/r-server-install-linux-server.md)  
 + [Install Machine Learning Server on Hadoop](install/r-server-install-hadoop.md)
 + [Configure Machine Learning Server to operationalize your analytics](operationalize/configure-start-for-administrators.md#configure-server-for-operationalization) 