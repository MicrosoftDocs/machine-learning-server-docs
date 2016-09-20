---

# required metadata
title: "Introducing Microsoft R Server"
description: "Microsoft R Server introduction"
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "09/20/2016"
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

**R Server** is an enterprise class server for hosting and managing parallel and distributed workloads of R processes on servers (Linux and Windows) and clusters (Hadoop and Apache Spark). Along with **R Client**, R Server is an execution engine for solutions built on **ScaleR** technology, extending open source R with infrastructure and support for high-performance analytics, statistical analysis, machine learning scenarios, and massively large datasets. The primary difference between R Server and R Client is scale and workload distribution. Only R Server can accommodate big data on distributed file systems like Hadoop MapReduce and Spark.

You can install R Server on a supported server or cluster, and then use it to run existing solutions that you previously built using **R Client** and tools like **R Tools for Visual Studio**. Although ScaleR functions are not required in the solutions you deploy, the full value of Microsoft R is realized only with ScaleR technology.

R Server is the next generation of the former Revolution R Enterprise server, acquired by Microsoft and distributed commercially for these platforms: Azure, Windows, Linux, Hadoop, Teradata, SQL Server. See [Supported Platforms](rserver-install-supported-platforms.md) for details.

## Components of R Server

|Components | Description |
|----|---|
|Microsoft R Open | Microsoft's distribution of open source R. This distribution ships standalone and as a component of Microsoft R Client and Microsoft R Server. |
|ScaleR (RevoScaleR package) | ScaleR ships in Microsoft R Client and Microsoft R Server. Many of its functions are platform-agnostic, but others exist to unlock the capabilities of specific platforms. Generally, the full value of R Server requires adoption of ScaleR functions for a specific platform. |
|Other packages | Additional packages are distributed with R Client and R Server, such as RevoPemaR. For the complete list, see [Package reference on MSDN](package-reference.md). |
|Platform-specific Components | Windows, Linux, and Hadoop components are only available in R Server. Cloud services, like Azure HDInsight, integrate R Server internally so that you don't have to provision or manage the server manually. |

## Benefits of R Server

Reasons for choosing R Server include:

* Chunked data across multiple disks
* Increased threads for R worker processes running standard R packages and also ScaleR functions
* Performance and scalability through parallelization and streaming
* Supportability and service level agreements for mission-critical workloads

## Interoperability with R language and across Microsoft R

R Server is built on open source R and is 100% compatible with the R language. You can run any pure open source R solution on a Microsoft R Open, Microsoft R Client, or Microsoft R Server deployment.

ScaleR functions are proprietary and available only through Microsoft R Client and Microsoft R Server. Using the high-performance analytics of ScaleR will require a move from open source to either Microsoft R Client or Microsoft R Server.

In a progression of server capacity and capability, many customers start with the free, local R Client and then upgrade to R Server. R Client is a no-charge version of Microsoft R that is typically paired with an integrated development environment like R Tools for Visual Studio. R Client provides an engine for ScaleR functions. Occasionally, a ScaleR solution developed locally against an R Client can be deployed to R Server with minimal or no changes, but additional functions become available when you switch from local or remote compute contexts. Realistically, a small amount of adaptation is typically required to take advantage of workload distribution and chunking.

R Server is the next step forward when operational requirements include performance, scale, and service level agreements. These benefits are only available through the commercial R Server product.

## See Also

[Learning Resources](microsoft-r-more-resources.md)

[Getting Started with ScaleR](scaler-getting-started.md)
