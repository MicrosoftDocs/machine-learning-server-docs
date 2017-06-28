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

*Compute context* refers to the location of RevoScaleR computations, as executed by the RevoScaleR interpreter provided by an R Server or R Client. The ability to switch a compute context means that you can push execution to another RevoScaleR interpreter if you want to leverage data resources on that server.

The primary use case for switching the compute context is to bring calculations and analysis to the data itself. As such, the use cases for compute context typically leverage a database platform, such as SQL Server or Teradata, or data located on the Hadoop Distributed File System (HDFS) using Spark or MapReduce for processing layer.

Use case | Description | 
---------|-------------|
R Client to R Server on a database| Write and run script locally in R Client, pushing specific computations to a remote R Server instance. You can rotate calculations among systems having better processing capabilities or database assets.|
R Server to R Server | Push platform-specific computations to an R Server on a different platform. Supported platforms include SQL Server, Teradata, Hadoop (Spark or MapReduce). You can implement a distributed processing architecture: RxLocalSeq, RxSpark, RxInSqlServer. |

> [!Tip]
> If your objective is simply to use two or more R Server instances interchangeably, or to shift execution from R Client to a more powerful R Server, consider [remote execution](operationalize/remote-execution.md) rather than switching the compute context. Remote execution gives you the ability to run R sessions on any standalone R Server installation on Windows or Linux. 

## Prerequisites

Use same-version R Server instances, or compatible co-released versions of R Client and R Server (for example, R Client 3.3.3. and R Server 9.1). The specific dependency is to have the same version of RevoScaleR, but since RevoScaleR is only installed through R Client or R Server, the version prerequisite is satisfied through product installation.

## List of compute contexts

In Microsoft R, every cR session that loads the RevoScaleR function library has a compute context type. The default is a local compute context, available on all platforms. 

You can switch from local to any of the compute contexts in the following list. 

Context name | Alternative name | Allowed data sources |
-----------|--------------------|-----------------------|
[RxLocalSeq](scaler/packagehelp/rxlocalseq.md)      | local     | (all) |
[RxSpark](r-reference/revoscaler/rxspark.md)         | spark     | RxTextData](r-reference/revoscaler/rxtextdata.md), [RxXdfData](scaler/packagehelp/RxXdfData.md), [RxSparkData](r-reference/revoscaler/rxsparkdata.md) including RxHiveData, RxParquetData, RxOrcData  |
[RxHadoopMR](r-reference/revoscaler/rxhadoopmr.md)      | hadoopmr  | [RxTextData](r-reference/revoscaler/rxtextdata.md), [RxXdfData](scaler/packagehelp/RxXdfData.md) |
[RxInSqlServer](r-reference/revoscaler/rxinsqlserver.md)   | sqlserver | [RxSqlServerData](r-reference/revoscaler/rxsqlserverdata.md) |
[RxInTeradata](r-reference/revoscaler/rxinteradata.md)    | teradata  | [RxTeradataData](r-reference/revoscaler/rxteradata.md)  |

Compute context is often used to enable controlled, distributed computations relying on instructions you provide rather than a built-in scheduler on Hadoop. The following compute contexts are used for manual distributed computing:

Context name | Alternative name | 
-----------|--------------------|
[RxLocalParallel](r-reference/revoscaler/rxlocalparallel.md) | localpar  |  
[RxForeachDoPar](r-reference/revoscaler/rxforeachdopar.md)  | dopar     |  

## Get compute context

At an R command prompt, run `rxGetComputeContext()` to return the current compute context. Every platform supports the default local compute context **RxLocalSeq**. 

## Limitations when switching context

1. Not all RevoScaleR capability is available on every distributed computing platform, such as Hadoop. 
2. Only some RevoScaleR functions and rxExec run in a distributed manner.  
3. The main R script including any open source routines still run locally in a single threaded process. 
4. Data and objects needed for distributed execution of rxExec or a RevoScaleR function will need to be copied to the remote compute context if the object is not already there, such as to a cluster, database, or Hadoop. 
5. With limited exceptions (such as file copying to and from Hadoop), RevoScaleR does include functions for moving data.  
6. Some RevoScaleR functions only run locally, such as sort, merge, and import, so data may have to be moved back and forth between the local and remote environment during the course of overall program execution. 
7. rxPredict on a cluster is only possible if the data file is split.

## Example: Create a compute context

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