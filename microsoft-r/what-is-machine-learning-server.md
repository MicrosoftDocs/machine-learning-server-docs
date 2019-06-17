---
# required metadata
title: "Welcome to Machine Learning Server "
description: "Develop machine learning models and scripts in Python and R for on-premises deployment behind the firewall. R Server, Python server, packages, and interpreters are included."
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
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

# What is Machine Learning Server

Microsoft Machine Learning Server is enterprise software for data science, providing R and Python interpreters, base distributions of R and Python, additional high-performance libraries from Microsoft, and an [operationalization capability](what-is-operationalization.md)  for advanced deployment scenarios. Solutions that you develop in R or Python can be deployed as web service for direct access or as an upstream component of other solutions. 

R support is built on a legacy of Microsoft R Server 9.x and Revolution R Enterprise products. Python support was added in the 9.2.1 release. 

Machine Learning Server runs [on-premises and in the cloud on a variety of platforms](install/r-server-install-supported-platforms.md), on a variety of operating systems, and can run in a distributed mode if you want to isolate functions on different computers (as web and compute nodes). [Web services](operationalize/concept-what-are-web-services.md) are hosted on a server grid on-premises or in the cloud and can be integrated with line-of-business applications. Additionally, Machine Learning Server integrates seamlessly with [Active Directory and Azure Active Directory](operationalize/configure-authentication.md) and includes [role-based access control](operationalize/configure-roles.md) to satisfy security and compliance needs of your enterprise. The ability to deploy to an elastic grid lets you scale seamlessly with the needs of your business, both for batch and real-time scoring.

On two data platforms, SQL Server and Apache Spark (on HDFS), the R and Python libraries from Microsoft can compute and analyze data locally, returning the results while eliminating the need to pull data across the network. This capability is called *remote compute context*. It requires the R and Python libraries and interpreters from Microsoft on both client and server systems, but the client versions are free of charge.

## Key features of Machine Learning Server

The following features are included in Machine Learning Server. For feature descriptions in this release, see [What's New in Machine Learning Server](whats-new-in-machine-learning-server.md).

| Feature category | Description |
|------------------|-------------|
| R_SERVER| [R packages](r-reference/introducing-r-server-r-package-reference.md) for solutions written in R, with an open-source distribution of Microsoft R Open and run-time infrastructure for script execution.  |
| PYTHON_SERVER| [Python modules](python-reference/introducing-python-package-reference.md) for solutions written in Python,  with an open-source distribution of Anaconda and run-time infrastructure for script execution.  
| [Pre-trained models](install/microsoftml-install-pretrained-models.md) | For visual analysis and text sentiment analysis, ready to score data you provide. |
| [Deploy and consume](what-is-operationalization.md) | Operationalize your server and deploy solutions as a web service. |
| [Remote execution](r/how-to-execute-code-remotely.md) | Start remote sessions on a Machine Learning Server on your network from your client workstation. |
| scale out on premises | Clustered topologies for [Apache Spark on Hadoop](install/machine-learning-server-hadoop-install.md), and Windows or Linux using the [operationalization capability](operationalize/configure-start-for-administrators.md) built into Machine Learning Server. |

## Next steps

[Install Machine Learning Server](install/machine-learning-server-install.md) on a supported platform. 

[Choose a quickstart](index.yml) to test-drive capabilities in 10 minutes or less. Move on to tutorials for in-depth exploration.

> [!Note]
> You can use any R or Python IDE to write code using libraries from Machine Learning Server. Microsoft offers Python for Visual Studio and R for Visual Studio. To use Jupyter Notebook, see [How to configure Jupyter Notebooks](python/how-to-revoscalepy-jupyter-nb-config.md).

## See also

+ [What's new in Machine Learning Server](whats-new-in-machine-learning-server.md)
+ [Learning Resources](resources-more.md)
