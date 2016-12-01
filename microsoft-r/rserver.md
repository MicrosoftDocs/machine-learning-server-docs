---

# required metadata
title: "Introducing Microsoft R Server"
description: "Microsoft R Server introduction"
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "12/01/2016"
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

# Introducing Microsoft R Server

**R Server** is an enterprise class server for hosting and managing parallel and distributed workloads of R processes on servers (Linux and Windows) and clusters (Hadoop and Apache Spark). It provides an execution engine for solutions built using Microsoft R packages, extending open source R with support for high-performance analytics, statistical analysis, machine learning scenarios, and massively large datasets. Value-added functionality is provided through proprietary packages that install with the server.

You can install R Server on a supported server or cluster, and use an R IDE like **R Tools for Visual Studio** to adapt or create solutions that use the additional capacity. Although Microsoft R functions are not required in the solutions you deploy, the full value of Microsoft R is realized when you use ScaleR technology and other packages.

R Server is the next generation of the former Revolution R Enterprise server, acquired by Microsoft and distributed commercially for these platforms: Azure, Windows, Linux, Hadoop, Teradata, SQL Server. See [Supported Platforms](rserver-install-supported-platforms.md) for details.

## Components of R Server

|Components | Description |
|----|---|
|[Microsoft R Open](r-open.md) | Microsoft's distribution of open source R. This distribution ships standalone and as a component of Microsoft R Client and Microsoft R Server. |
|[Operationalization](operationalize/about.md) |An operationalization engine only in R Server used to deploy R script or code as web services. It also enables an interactive experience across multiple nodes running R Server. Operationalization functions are provided by the [mrsdeploy package](mrsdeploy/mrsdeploy.md) |
|[ScaleR](scaler-getting-started) | ScaleR is a high performance computing and analytical engine used to partition massively large datasets into smaller chunks, distributed and analyzed in parallel, often on multiple nodes or on database platforms like SQL Server and Teradata. ScaleR is an R Server feature, but it also ships in R Client with limits on data size and processor utilization. ScaleR functions are provided by the [RevoScaleR package](scaler/scaler.md). |
|[Machine learning algorithms](microsoftml-introduction.md) |State-of-the-art machine learning algorithms are now available in Microsoft R. You can use these functions in R code or script for performing machine learning on a standalone R Server. Machine learning algorithms are also available in R Client, subject to data size limits (in-memory only) and processor limits (2). Machine learning functions are provided by the[MicrosoftML package](microsoftml/microsoftml.md).|
|Other packages | Additional packages are distributed with R Client and R Server, such as [RevoPemaR](pemar/pemar.md). For the complete list, see [Package reference on MSDN](package-reference.md). |
|Platform-specific Components | Windows, Linux, and Hadoop components are only available in R Server. Cloud services, like Azure HDInsight, integrate R Server internally so that you don't have to provision or manage the server manually. |
|Rgui.exe and R.exe| Console applications for command line execution in a local session.|

If you run Windows, we recommend that you also install [Visual Studio 2015](https://www.visualstudio.com/downloads/) followed by the [R Tools for Visual Studio (RTVS) add-in](https://www.visualstudio.com/vs/rtvs/). RTVS provides a new R project template that adds the R Interactive window, R script support, embedded help, package management, and more. Otherwise, you can use any R IDE that supports R packages.

## Why use R Server?

 **R**, along with many other statistical analysis products, is challenged by problems of capacity and speed. Users cannot perform data analysis because their data is too big to fit into memory, or even if it fits, there is not sufficient memory available to perform analysis. In R this is often a problem because copies of data are frequently made during analysis. Even without a capacity limit, computation may be too slow to be useful. R Server with ScaleR not only helps to overcome these challenges in R, but surpasses capabilities in other statistics products.

In addition to capacity and scale, R Server offers machine learning and operationalization features, both of which are new in this release.

## Benefits of R Server

Reasons for choosing R Server include:

* Chunked data across multiple disks
* Increased threads for R worker processes running standard R packages and also ScaleR functions
* Performance and scalability through parallelization and streaming
* Supportability and service level agreements for mission-critical workloads
* Machine learning algorithms and transforms
* Operationalization engine for enterprise deployment

## Interoperability with R language and across Microsoft R

R Server is built on open source R 3.3.2 and is 100% compatible with the R language. You can run any pure open source R solution on a Microsoft R Open, Microsoft R Client, or Microsoft R Server deployment.

Value-added packages like RevoScaleR, MicrosoftML, and mrsdeploy are available in both Microsoft R Client and Microsoft R Server. Although packages are equally available, the infrastructure backed by each product is substantially different. R Client is limited to in-memory data storage and can use a maximum of two processors.

R Server is the flagship product of the [Microsoft R product family](microsoft-r-getting-started.md) and supports much larger workloads. Data scientists typically switch to R Server when data and computational requirements cannot be accommodated on R Client.

Existing solutions developed with R Client can be deployed to R Server with minimal or no changes, but most developers make use of the additional functions, such as parallel and distributed computing that become available when you upgrade to R Server.

In this release, the [mrsdeploy package](mrsdeploy/mrsdeploy.md) gives you the ability to toggle between remote and local sessions in an R console application. As you change the compute context and make other adjustments to increase data size, you can set up a remote session and issue commands to validate your changes incrementally.

## Installation links

+ [R Server on Windows](rserver-install-windows.md)
+ [R Server on Linux](rserver-install-linux-server.md)
+ [R Server on Hadoop](rserver-install-hadoop.md)
+ [R Server on Teradata](rserver-install-teradata-server.md)
+ [R Server on SQL Server](https://msdn.microsoft.com/library/mt604845.aspx)

To review specific OS and database platform versions that can be used for an R Server deployment, see [Supported platforms](rserver-install-supported-platforms.md).

## See Also

[Learning Resources](microsoft-r-more-resources.md)

[What's new in R Server](rserver-whats-new.md)

[Getting Started with ScaleR](scaler-getting-started.md)
