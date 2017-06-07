---

# required metadata
title: "Introducing Microsoft R Server"
description: "Microsoft R Server for hosting and managing parallel and distributed workloads of R processes on servers"
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "04/04/2017"
ms.topic: "article"
ms.prod: "microsoft-r"
ms.service: ""
ms.assetid: ""

# optional metadata
ROBOTS: ""
audience: ""
ms.devlang: ""
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.technology:
  - r-server
ms.custom: ""

---

# About Microsoft R Server

**R Server** is an enterprise class server for hosting and managing parallel and distributed workloads of R processes on servers (Linux and Windows) and clusters (Hadoop and Apache Spark). It provides an execution engine for solutions built using Microsoft R packages, extending open source R with support for high-performance analytics, statistical analysis, machine learning scenarios, and massively large datasets. Value-added functionality is provided through proprietary packages that install with the server.

You can install R Server on a supported server or cluster, and use an R IDE like [R Tools for Visual Studio](https://docs.microsoft.com/visualstudio/rtvs/installation) to adapt or create solutions to use additional capabilities. Although Microsoft R functions are not required in the solutions you deploy, the full value of Microsoft R is realized when you use machine learning, ScaleR technology, and other packages.

Although generic R scripts tend to run faster on Microsoft R Open via Intel MKL, the major benefit in terms of scale and performance comes from using RevoScaleR functions, which are available in both R Client and R Server. However, only R Server adds support for operationalizing R analytics, remote execution, remote compute contexts, data chunking, additional threads for multithreaded processing, parallel processing, and streaming.

In many enterprises, the final step is to deploy an interface to the underlying analysis to a broader audience within the organization who can then, in turn, consume the analytics. Microsoft R Server provides the operationalizing tools for doing just that; it is a full-featured web services software development kit for R that allows programmers to use in any language to integrate the R analysis output with a third party package. [Learn more about operationalizing](operationalize/about.md)

R Server is the next generation of the former Revolution R Enterprise server, acquired by Microsoft and distributed commercially for these platforms: Azure, Windows, Linux, Hadoop, Teradata, SQL Server. See [Supported Platforms](rserver-install-supported-platforms.md) for details.

<a href="https://www.microsoft.com/en-us/cloud-platform/r-server" target="_blank"><b>Watch this 2-minute video introduction to Microsoft R Server.</b></a> 

> [!Note]
> Performance can be affected by external factors outside the R code, including competing demands on server resources, the type of query plan that is created, schema changes, the need to update statistics or create a new query plan, fragmentation and so on. It is possible that a stored procedure containing R code might run in seconds under one workload, but take minutes when there are other services running. We recommend that you monitor multiple aspects of server performance, including networking for remote compute contexts, when quantifying R job performance.

>[!IMPORTANT]
>For information on SQL Server Machine Learning Services, please visit the corresponding documentation here: https://docs.microsoft.com/en-us/sql/advanced-analytics/r/sql-server-r-services.

## Why use R Server?

 **R**, along with many other statistical analysis products, is challenged by problems of capacity and speed. Users cannot perform data analysis because their data is too big to fit into memory, or even if it fits, there is not sufficient memory available to perform analysis. In R this is often a problem because copies of data are frequently made during analysis. Even without a capacity limit, computation may be too slow to be useful. R Server with RevoScaleR not only helps to overcome these challenges in R, but surpasses capabilities in other statistics products.

 Data scientists who start with R Client or open source R typically move to R Server when data size or computational scale require additional capacity.

 R Server provides the infrastructure for distributing a workload across multiple nodes (referred to as *data chunking*), running jobs in parallel, and then reassembling the results for further analysis and visualization.

In addition to capacity and scale, R Server offers machine learning features and allows you to [operationalize your analytics](overview-operationalize-analytics.md). R is a great modeling tool, but often **the challenge lies in how to effectively operationalize R**. Traditionally, this has not been an easy process (slow innovation and error-prone) and it can take months to rewrite these models before you can use them. You can use Microsoft R Server as the deployment engine for your advanced R analytics. Regardless of the source, language or method, you can simplify, deploy, and realize the promise and power of advanced analytics.

Reasons for choosing R Server include:

* Chunked data across multiple disks
* Increased threads for R worker processes running standard R packages and also ScaleR functions
* Performance and scalability through parallelization and streaming
* Supportability and service level agreements for mission-critical workloads
* Machine learning algorithms and transforms
* Deployment engine for operationalizing analytics for multi-server topologies with clustered web nodes and compute nodes
* R script running as a standalone web service
* Toggle between local and remote sessions on the command line

## Components of R Server

|Components | Description |
|----|---|
|[Microsoft R Open](https://mran.microsoft.com/open/) | Microsoft's distribution of open source R. This distribution ships standalone and as a component of Microsoft R Client and Microsoft R Server. |
|[Operationalized analytics](operationalize/about.md) |An engine only in R Server used to deploy R script or code as a web service with support for remote runtime execution and to consume such services. Console users can exercise the functions in [mrsdeploy package](mrsdeploy/mrsdeploy.md). Developers can use the Swagger-APIs to create programmatic solutions. |
|[ScaleR](scaler-getting-started-data-import-exploration.md) | ScaleR is a high performance computing and analytical engine used to partition massively large datasets into smaller chunks, distributed and analyzed in parallel, often on multiple nodes or on database platforms like SQL Server and Teradata. ScaleR is an R Server feature, but it also ships in R Client with limits on data size and processor utilization. ScaleR functions are provided by the [RevoScaleR package](scaler/scaler.md). |
|[Machine learning algorithms](microsoftml-introduction.md) |State-of-the-art machine learning algorithms are now available in Microsoft R. You can use these functions in R code or script for performing machine learning on a standalone R Server. Machine learning algorithms are also available in R Client, subject to data size limits (in-memory only) and processor limits (2). Functions are provided by the [MicrosoftML package](microsoftml/microsoftml.md).|
|Other packages | Additional packages are distributed with R Client and R Server, such as [RevoPemaR](pemar/pemar.md). For the complete list, see [Package reference on MSDN](package-reference.md). |
|Platform-specific Components | Windows, Linux, and Hadoop components are only available in R Server. Cloud services, like Azure HDInsight, integrate R Server internally so that you don't have to provision or manage the server manually. Platform-specific components are available when you install R server on that platform. For more information, see [installation links](#installationlinks) below.|
|Rgui.exe and R.exe| R Server includes console applications for command line execution in a local session.|

If you run Windows, we recommend that you also install [Visual Studio 2015](https://www.visualstudio.com/downloads/) followed by the [R Tools for Visual Studio (RTVS) add-in](https://www.visualstudio.com/vs/rtvs/). RTVS provides a new R project template that adds the R Interactive window, R script support, embedded help, package management, and more. Otherwise, you can use any R IDE that supports R packages.


## How to use R Server

R Server runs as a background process that starts up when you launch the Rgui or an R IDE  such as **R Tools for Visual Studio (RTVS)**, RStudio, or other applications. Generally, you can use any R IDE that can consume R packages.

Data scientists who use R Server typically connect over Remote Desktop, and then use RTVS or another to create or run solutions interactively. Solutions are usually script files that include a combination of R functions and functions from proprietary packages: RevoScaleR, MicrosoftML, mrsdeploy, RevoPema, and so forth.

This release also includes the interaction model to include remote execution via the `mrsdeploy` package on an R Server that was configured to operationalize your analytics. Assuming you have two or more installations of R Client 3.3.2/3.3.3 or R Server 9, you can interact with a remote node from the command line in a local console application or script. Working in this modality introduces requirements for encrypted connections and authentication. Supported authentication methodologies include Active Directory, Azure Active Directory, or LDAP in Active Directory (if you're using Linux or another non-Windows platform).

Developers can use Swagger APIs to automate R analytics over single and multi-server deployments. For more information, see the following articles on [operationalizing your analytics](operationalize/about.md) and [mrsdeploy](mrsdeploy/mrsdeploy.md).

## Operationalize your analytics

Being able to operationalize your analytics is another central capability in R Server. Formerly known as DeployR, this capability for operationalizing your code is now fully integrated into R Server. After installing R Server on select platforms (availability on all platforms is still pending), you'll have everything you need to [configure R Server to deploy, host, and consume R analytics web services and remote R sessions](operationalize/configuration-initial.md).  For details on which platforms, see [Supported platforms](rserver-install-supported-platforms.md).

[Learn more about operationalizing analytics with R Server.](overview-operationalize-analytics.md)

## Machine learning

In R Server, you can use the **MicrosoftML** package, which provides state of the art, fast, scalable machine learning algorithms and transforms. These functions enable you to tackle common machine learning and data science tasks such as text and image featurization, classification, anomaly detection, regression and ranking. The goal is to help developers, data scientists, and an increasing spectrum of information workers to the design and implement intelligent products, services and devices. This topic discusses these tasks and lists the key R functions provided by this package for transforming and modeling data that facilitate the completion of these data science tasks.

You can also install **pre-trained cognitive models** for **sentiment analysis** and **image featurization**, when you select them in R Server Setup. To learn more, see [Get started with MicrosoftML](microsoftml-get-started.md) and [How to install and deploy pre-trained machine learning models with MicrosoftML](deploy-pretrained-microsoftml-models.md).


## R Server installations

<a name="installationlinks"></a>

|Microsoft R Server platforms|Description|Install|Get Started|
|----------------------------|-----------|:-----:|:----------------:|
|R Server for Hadoop        |Scale your analysis transparently by distributing work across nodes without complex programming|[Doc](rserver-install-hadoop.md)|[Doc](scaler-hadoop-getting-started.md)|
|R Server for Teradata DB   |Run advanced analytics in-database for seamless data analysis on Teradata|[Doc](rserver-install-teradata-server.md)|[Doc](/scaler-teradata-getting-started.md)|
|R Server for Linux         |Bring predictive and prescriptive analytics power to your Linux environments|[Doc](rserver-install-linux-server.md)|[Doc](scaler-getting-started-data-import-exploration.md)|
|R Server for Windows|Bring predictive and prescriptive analytics power to your Windows environments|[Doc](rserver-install-windows.md)|[Doc](scaler-getting-started-data-import-exploration.md)|
|SQL Server Machine Learning Services  |Run advanced analytics in-database for seamless data analysis on SQL Server|[Doc](https://msdn.microsoft.com/library/mt696069.aspx)|[Doc](https://msdn.microsoft.com/library/mt604885.aspx)|

<br />
To review specific OS and database platform versions that can be used for an R Server deployment, see [Supported platforms](rserver-install-supported-platforms.md).


## Next steps

If you are new to Microsoft R, we recommend starting with R Client and an integrated development environment like R Tools for Visual Studio (RTVS). This configuration is free of charge. It gives you Microsoft R Open with full support of all base R functions so that you can write R-only solutions, but also includes the Microsoft R proprietary packages that run locally on your development computer.

Using just Microsoft R Open and RTVS, you can use the R Core Team manuals that are part of every R distribution to learn how to code in R. Built-in manuals include *An Introduction to R*, *The R Language Definition*, *Writing R Extensions* and more. Beyond the standard R manuals, there are many other resources. [Learn about them here](microsoft-r-more-resources.md).

However, because you have R Client, your script can also include functions from Microsoft R packages, including MicrosoftML, olapR, and RevoScaleR. All of these packages are available in both R Client and R Server, but at different levels of capacity.

**Tutorials**
Tutorials in Microsoft R product documentation will help you learn how to use the functions in the proprietary packages:  

+ [Practice data import and exploration](scaler-getting-started-data-import-exploration.md)
+ [Explore R and ScaleR in 25 functions](microsoft-r-getting-started-tutorial.md)
+ [Introduction to MicrosoftML](microsoftml-introduction.md)


**See also**

+ [What's new in R Server](rserver-whats-new.md)
+ [Learning Resources](microsoft-r-more-resources.md)
+ [Getting Started with ScaleR](scaler-getting-started-data-import-exploration.md)
