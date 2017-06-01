---

# required metadata
title: "Compute context in Microsoft R"
description: "Push a compute context to R Server on a different platform for remote execution."
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "05/30/2017"
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
ms.technology: "r-server"
ms.custom: ""

---

# Compute context (Microsoft R)

**Applies to: Microsoft R Client, Microsoft R Server**

*Compute context* refers to the location of RevoScaleR computations, as executed by the RevoScaleR interpreter provided by an R Server or R Client. The ability to switch a compute context means that you can push execution to another RevoScaleR interpreter if you want to leverage data resources on that server.

The primary use case for switching the compute context is to bring calculations and analysis to the data itself. As such, the use cases for compute context typically leverage a database platform, such as SQL Server or Teradata, or data located on the Hadoop Distributed File System (HDFS) using Spark or MapReduce for processing layer.

Use case | Description | 
---------|-------------|
R Client to R Server on a database| Write and run script locally in R Client, pushing specific computations to a remote R Server instance. |
R Server to R Server | Push platform-specific computations to an R Server on a different platform. Supported platforms include SQL Server, Teradata, Hadoop (Spark or MapReduce) |

If your objective is simply to use two or more R Server instances interchangeably, or to shift execution from R Client to a more powerful R Server, you would enable *remote execution* rather than switch the compute context. Remote execution gives you the ability to run R sessions on any standalone R Server installation on Windows or Linux. 

## Why compute contexts?

To rotate calculations among systems having better processing capabilities or database assets.

To implement a distributed processing architecture: RxLocalSeq, RxSpark, RxInSqlServer.

To 


## Prerequisites

Use same-version R Server instances, or compatible co-released versions of R Client and R Server (for example, R Client 3.3.3. and R Server 9.1). The specific dependency is to have the same version of RevoScaleR, but since RevoScaleR is only installed through R Client or R Server, the version prerequisite is satisfied through product installation.

## Types

In Microsoft R, every cR session that loads the RevoScaleR function library has a compute context type. The default is a local compute context, available on all platforms. 

You can switch from local to any of the compute contexts in the following list. 

Context name | Alternative name | Allowed data sources |
-----------|--------------------|-----------------------|
[RxLocalSeq](scaler/packagehelp/rxlocalseq)      | local     | (all) |
[RxSpark]()         | spark     |   |
[RxHadoopMR]()      | hadoopmr  |   |
[RxInSqlServer]()   | sqlserver |   |
[RxInTeradata]()    | teradata  |   |

Compute context is often used to enable controlled, distributed computations relying on instructions you provide rather than a built-in scheduler. The following compute contexts are used for distributed computing:

Context name | Alternative name | Allowed data sources |
-----------|--------------------|-----------------------|
[RxLocalParallel]() | localpar  |   |
[RxForeachDoPar]()  | dopar     |   |

## Get compute context

At an R command prompt, run `rxGetComputeContext()` to return the current compute context. Every platform supports the default local compute context **RxLocalSeq**. 

## Compute context on various platforms and data sources

matrix

Common properties and operations

## Switch compute context

## Create and multiple compute contexts

## Examples

~~~~
# Set a SQL Server compute context (requires RevoScaleR and SQL Server on same machine)

connectionString <- "Server=(placeholder-server-name);Database=RevoAirlineDB;Trusted_Connection=true"

sqlQuery <- "WITH nb AS (SELECT 0 AS n UNION ALL SELECT n+1 FROM nb where n < 9) SELECT n1.n+10*n2.n+100*n3.n+1 AS n, ABS(CHECKSUM(NewId())) 

myServer <- RxComputeContext("RxInSqlServer", sqlQuery = sqlQuery, connectionString = connectionString)   
                   
rxSetComputeContext(computeContext = myServer)
~~~~

## See Also

 [Introduction to R Server](rserver.md) 
 [Install R Server on Windows](rserver-install-windows.md)  
 [Install R Server on Linux](rserver-install-linux-server.md)  
 [Install R Server on Hadoop](rserver-install-hadoop.md)