---

# required metadata
title: "Introduction to Microsoft R Server and R Client"
description: "Learn about Microsoft R features and components in R Server, R Client, R Open."
keywords: ""
author: "j-martens"
manager: "jhubbard"
ms.date: "05/10/2017"
ms.topic: "get-started-article"
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
  - r-client
  - r-server
ms.custom: ""

---
<<<<<<< HEAD

# About Microsoft R Server

Microsoft R Server is "R for the Enterprise" and solves the problem of deploying and consuming of R code. It offers big-data capable R distributions for servers, Hadoop clusters, and data warehouses. It supports a variety of big data statistics, predictive modeling and machine learning capabilities, and provides you with analytics that are fully compatible with the R language, the de facto standard for modern analytics users.

Although generic R scripts tend to run faster on MRO via Intel MKL, the major benefit in terms of scale and performance comes from using ScaleR functions. ScaleR is available in both R Client and R Server, but only R Server adds support for remote execution, remote compute contexts, data chunking, additional threads for multithreaded processing, parallel processing, and streaming.

In many enterprises, the final step is to deploy an interface to the underlying analysis to a broader audience within the organization who can then, in turn, consume the analytics. Microsoft R Server provides the operationalizing tools for doing just that; it is a full-featured web services software development kit for R that allows programmers to use in any language to integrate the R analysis output with a third party package. [Learn more about operationalizing](operationalize/about.md)

<a href="https://www.microsoft.com/en-us/cloud-platform/r-server" target="_blank"><b>Watch this 2-minute video introduction to Microsoft R Server.</b></a> 

> [!Note]
> Performance can be affected by external factors outside the R code, including competing demands on server resources, the type of query plan that is created, schema changes, the need to update statistics or create a new query plan, fragmentation and so on. It is possible that a stored procedure containing R code might run in seconds under one workload, but take minutes when there are other services running. We recommend that you monitor multiple aspects of server performance, including networking for remote compute contexts, when quantifying R job performance.

## Why choose R Server over R Client

R Server and R Client offer virtually identical packages, but each one targets different scenarios. R Client is intended for data scientists who create solutions that run locally. R Server is commercial software that runs on a range of platforms, at much greater scale, with infrastructure for handling major workloads, on client-server topologies that support remote access over authenticated connections. You can learn and develop on R Client, and then migrate your work to R Server when you need the scale, support, and infrastructure of an operationalized server.

### Scale

On R Server, the ScaleR technology in the RevoScaleR package offers almost unbounded scale in running R workloads in parallel and distributed configurations. Although you can call ScaleR functions on a system having just R Client, ScaleR is throttled on R Client: datasets must fit in memory, and processing is capped at a maximum of two processors on the local system. Only R Server gives you ScaleR at full capacity, with support for remote compute context, data chunking, parallelization, and distributed workloads.

Given a platform that supports it, functions in ScaleR provide high performance, parallelized, and distributable analytics functions that scale from small data sets in memory to huge data sets stored on disk on a cluster of computers. The analytics functions provided include summary statistics, cubes and crosstabs, linear models, logistic regression, generalized linear models, kmeans clustering, decision trees, and decision forests. These algorithms are parallelized and distributed automatically, and process data in chunks so that all of your data does not need to be in memory at one time; you can use the same analysis code for your giant data set as you do for a small data set in memory.

ScaleR also provides traditional high performance computing (tools if you prefer to construct your own distributed computations. In addition, in many environments, there are full-featured tools for data cleaning and manipulation.

Functions in ScaleR are prefixed with ‘rx’ for analysis and data manipulation. Additionally, use ‘rxExec’ for high performance computing. If you are computing decision trees, also check out the included RevoTreeView package that allows you to interactively visualize your decision trees.


### Operationalizing Analytics

Being able to operationalize your analytics is a central capability in R Server, and one of the key reasons you would choose R Server over R Client. Formerly known as DeployR, this capability for operationalizing your code is now fully integrated into R Server. After installing R Server on select platforms (availability on all platforms is still pending), you'll have everything you need to [configure R Server to host R analytics web services and remote R sessions](operationalize/configuration-initial.md).  For details on which platforms, see [Supported platforms](rserver-install-supported-platforms.md).

In many enterprises, the final step is to deploy an interface to the underlying analysis to a broader audience within the organization who can then, in turn, consume the analytics. Microsoft R Server provides the operationalizing tools to deploy R analytics inside web, desktop, mobile, and dashboard applications as well as backend systems. R Server turns your R scripts into analytics web services, so R code can be easily executed by applications running on a secure server.

An operationalized R Server offers the ability to host and bundle R analytics into web services with minimal code changes. R Server accepts interactive commands through [mrsdeploy functions](mrsdeploy/mrsdeploy.md) for remote execution and web service deployment. Data scientists can use `mrsdeploy` functions  on the command line. Application developers can write code to instrument equivalent operations and integrate web services into their applications using [easy-to-consume Swagger-based APIs](operationalize/api.md) in any programming language.

You can configure R Server to operationalize analytics [on a single machine](operationalize/configuration-initial.md#onebox). It can also be [scaled](operationalize/configure-enterprise.md) for business-critical applications with multiple web and compute nodes on clustered servers for load balancing. This gives you the ability to pipeline data streams that are subsequently transformed, analyzed, and visualized into an R analytics web service.

In a Windows environment, multi-server topologies are supported through Windows clustering methodologies. Compute nodes can be made highly available using Windows server failover clusters in Active-Active mode. Web nodes can be scaled out using Windows network load balancing. R Server also supports production-grade workloads and seamless integration with popular [enterprise security solutions](operationalize/security.md).

For more information and next steps, see the articles on [the introduction to operationalizing analytics](operationalize/about.md) and [configuring R Server to operationalize](operationalize/configuration-initial.md).

> [!NOTE]
> In this context, clustered topologies are composed of standalone servers, not nodes in Hadoop or cloud services in Azure. The ability to use R Server to operationalize analytics is available in many, but not all, of the supported platforms. For the most up-to-date list, see [supported R Server platforms](rserver-install-supported-platforms.md).

### R Server Platforms

|Microsoft R Server platforms|Description|Install|Get Started|
|----------------------------|-----------|:-----:|:----------------:|
|R Server for Hadoop        |Scale your analysis transparently by distributing work across nodes without complex programming|[Doc](rserver-install-hadoop.md)|[Doc](scaler-hadoop-getting-started.md)|
|R Server for Teradata DB   |Run advanced analytics in-database for seamless data analysis on Teradata|[Doc](rserver-install-teradata-server.md)|[Doc](/scaler-teradata-getting-started.md)|
|R Server for Linux         |Bring predictive and prescriptive analytics power to your Linux environments|[Doc](rserver-install-linux-server.md)|[Doc](scaler-getting-started-data-import-exploration.md)|
|R Server for Windows|Bring predictive and prescriptive analytics power to your Windows environments|[Doc](rserver-install-windows.md)|[Doc](scaler-getting-started-data-import-exploration.md)|
|SQL Server Machine Learning Services  |Run advanced analytics in-database for seamless data analysis on SQL Server|[Doc](https://msdn.microsoft.com/library/mt696069.aspx)|[Doc](https://msdn.microsoft.com/library/mt604885.aspx)|

<br />
For a list of supported operating systems, see [Supported platforms in Microsoft R Server](rserver-install-supported-platforms.md).


## Next Steps

If you are new to Microsoft R, we recommend starting with R Client and an integrated development environment like **R Tools for Visual Studio (RTVS)**. This configuration is free of charge. It gives you Microsoft R Open with full support of all base R functions so that you can write R-only solutions, but also includes the Microsoft R proprietary packages that run locally on your development computer.

Using just MRO and RTVS, you can use the R Core Team manuals that are part of every R distribution to learn how to code in R. Built-in manuals include *An Introduction to R*, *The R Language Definition*, *Writing R Extensions* and more. Beyond the standard R manuals, there are many other resources. [Learn about them here](microsoft-r-more-resources.md).

However, because you have R Client, your script can also include functions from Microsoft R packages, including MicrosoftML, olapR, and RevoScaleR. All of these packages are available in both R Client and R Server, but at different levels of capacity.

Tutorials in Microsoft R product documentation will help you learn how to use the functions in the proprietary packages:  

+ [Practice data import and exploration](scaler-getting-started-data-import-exploration.md)

+ [Explore R and ScaleR in 25 functions](microsoft-r-tutorial-R2RevoScaleR.md)

+ [Introduction to MicrosoftML](microsoftml-introduction.md)

## See Also

[What's new in R Server](rserver-whats-new.md)



## Microsoft R Server overview

**R Server** is an enterprise class server for hosting and managing parallel and distributed workloads of R processes on servers (Linux and Windows) and clusters (Hadoop and Apache Spark). It provides an execution engine for solutions built using Microsoft R packages, extending open source R with support for high-performance analytics, statistical analysis, machine learning scenarios, and massively large datasets. Value-added functionality is provided through proprietary packages that install with the server.

You can install R Server on a supported server or cluster, and use an R IDE like **R Tools for Visual Studio** to adapt or create solutions to use additional capabilities. Although Microsoft R functions are not required in the solutions you deploy, the full value of Microsoft R is realized when you use machine learning, ScaleR technology, and other packages.

R Server is the next generation of the former Revolution R Enterprise server, acquired by Microsoft and distributed commercially for these platforms: Azure, Windows, Linux, Hadoop, Teradata, SQL Server. See [Supported Platforms](rserver-install-supported-platforms.md) for details.

## Components of R Server

|Components | Description |
|----|---|
|[Microsoft R Open](r-open.md) | Microsoft's distribution of open source R. This distribution ships standalone and as a component of Microsoft R Client and Microsoft R Server. |
|[Operationalized analytics](operationalize/about.md) |An engine only in R Server used to deploy R script or code as a web service with support for remote runtime execution and to consume such services. Console users can exercise the functions in [mrsdeploy package](mrsdeploy/mrsdeploy.md). Developers can use the Swagger-APIs to create programmatic solutions. |
|[ScaleR](scaler-getting-started-data-import-exploration.md) | ScaleR is a high performance computing and analytical engine used to partition massively large datasets into smaller chunks, distributed and analyzed in parallel, often on multiple nodes or on database platforms like SQL Server and Teradata. ScaleR is an R Server feature, but it also ships in R Client with limits on data size and processor utilization. ScaleR functions are provided by the [RevoScaleR package](scaler/scaler.md). |
|[Machine learning algorithms](microsoftml-introduction.md) |State-of-the-art machine learning algorithms are now available in Microsoft R. You can use these functions in R code or script for performing machine learning on a standalone R Server. Machine learning algorithms are also available in R Client, subject to data size limits (in-memory only) and processor limits (2). Functions are provided by the [MicrosoftML package](microsoftml/microsoftml.md).|
|Other packages | Additional packages are distributed with R Client and R Server, such as [RevoPemaR](pemar/pemar.md). For the complete list, see [Package reference on MSDN](package-reference.md). |
|Platform-specific Components | Windows, Linux, and Hadoop components are only available in R Server. Cloud services, like Azure HDInsight, integrate R Server internally so that you don't have to provision or manage the server manually. Platform-specific components are available when you install R server on that platform. For more information, see [installation links](#installationlinks) below.|
|Rgui.exe and R.exe| R Server includes console applications for command line execution in a local session.|

If you run Windows, we recommend that you also install [Visual Studio 2015](https://www.visualstudio.com/downloads/) followed by the [R Tools for Visual Studio (RTVS) add-in](https://www.visualstudio.com/vs/rtvs/). RTVS provides a new R project template that adds the R Interactive window, R script support, embedded help, package management, and more. Otherwise, you can use any R IDE that supports R packages.

## Why use R Server?

 **R**, along with many other statistical analysis products, is challenged by problems of capacity and speed. Users cannot perform data analysis because their data is too big to fit into memory, or even if it fits, there is not sufficient memory available to perform analysis. In R this is often a problem because copies of data are frequently made during analysis. Even without a capacity limit, computation may be too slow to be useful. R Server with ScaleR not only helps to overcome these challenges in R, but surpasses capabilities in other statistics products.

 Data scientists who start with R Client or open source R typically move to R Server when data size or computational scale require additional capacity.

 R Server provides the infrastructure for distributing a workload across multiple nodes (referred to as *data chunking*), running jobs in parallel, and then reassembling the results for further analysis and visualization.

In addition to capacity and scale, R Server offers machine learning features and allows you to  operationalize your analytics.

## How to use R Server

R Server runs as a background process that starts up when you use an R IDE  such as **R Tools for Visual Studio (RTVS)**, RStudio, or other applications. Generally, you can use any R IDE that can consume R packages.

Data scientists who use R Server typically connect over Remote Desktop, and then use RTVS or another to create or run solutions interactively. Solutions are usually script files that include a combination of R functions and functions from proprietary packages: RevoScaleR, MicrosoftML, mrsdeploy, RevoPema, and so forth.

This release also includes the interaction model to include remote execution via the `mrsdeploy` package on an R Server that was configured to operationalize your analytics. Assuming you have two or more installations of R Client 3.3.2/3.3.3 or R Server 9, you can interact with a remote node from the command line in a local console application or script. Working in this modality introduces requirements for encrypted connections and authentication. Supported authentication methodologies include Active Directory, Azure Active Directory, or LDAP in Active Directory (if you're using Linux or another non-Windows platform).

Developers can use Swagger APIs to automate R analytics over single and multi-server deployments. For more information, see the following articles on [operationalizing your analytics](operationalize/about.md) and [mrsdeploy](mrsdeploy/mrsdeploy.md).

## Benefits of R Server

Reasons for choosing R Server include:

* Chunked data across multiple disks
* Increased threads for R worker processes running standard R packages and also ScaleR functions
* Performance and scalability through parallelization and streaming
* Supportability and service level agreements for mission-critical workloads
* Machine learning algorithms and transforms
* R script running as a standalone web service
* Toggle between local and remote sessions on the command line
* Deployment engine for operationalizing analytics for multi-server topologies with clustered web nodes and compute nodes

## Interoperability with R language and across Microsoft R

R Server is built on open source R 3.3.2 and is 100% compatible with the R language. You can run any pure open source R solution on a Microsoft R Open, Microsoft R Client, or Microsoft R Server deployment.

Value-added packages like RevoScaleR, MicrosoftML, and mrsdeploy are available in both Microsoft R Client and Microsoft R Server. Although packages are equally available, the infrastructure backed by each product is substantially different. R Client is limited to in-memory data storage and can use a maximum of two processors.

R Server is the flagship product of the [Microsoft R product family](microsoft-r-getting-started.md) and supports much larger workloads. Data scientists typically switch to R Server when data and computational requirements cannot be accommodated on R Client.

Existing solutions developed with R Client can be deployed to R Server with minimal or no changes, but most developers make use of the additional functions, such as parallel and distributed computing that become available when you upgrade to R Server.

In this release, the [mrsdeploy package](mrsdeploy/mrsdeploy.md) gives you the ability to toggle between remote and local sessions in an R console application. As you change the compute context and make other adjustments to increase data size, you can set up a remote session and issue commands to validate your changes incrementally.

<a name="installationlinks"></a>
## Installation links

+ [R Server for Windows](rserver-install-windows.md)
+ [R Server for Linux](rserver-install-linux-server.md)
+ [R Server for Hadoop](rserver-install-hadoop.md)
+ [R Server for Teradata](rserver-install-teradata-server.md)
+ [R Server for SQL Server](https://msdn.microsoft.com/library/mt604845.aspx)

To review specific OS and database platform versions that can be used for an R Server deployment, see [Supported platforms](rserver-install-supported-platforms.md).

## See also

[Learning Resources](microsoft-r-more-resources.md)

[What's new in R Server](rserver-whats-new.md)

[Getting Started with ScaleR](scaler-getting-started-data-import-exploration.md)
=======
>>>>>>> dev
