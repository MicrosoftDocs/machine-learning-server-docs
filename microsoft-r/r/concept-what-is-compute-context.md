---

# required metadata
title: "Compute context for script execution on Machine Learning Server "
description: "Compute context for local, distributed, and parallel processing on Machine Learning Server using revoscalepy or RevoScaleR packages." 
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "cgronlun"
ms.date: "01/03/2018"
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

# Compute context for script execution in Machine Learning Server

In Machine Learning Server, a *compute context* refers to the physical location of the computational engine handling a given workload. The default is local. However, if you have multiple machines, you can switch from local to remote, pushing execution of data-centric [RevoScaleR (R)](../r-reference/revoscaler/revoscaler.md), [revoscalepy (Python)](../python-reference/revoscalepy/revoscalepy-package.md), [MicrosoftML (R)](../r-reference/microsoftml/microsoftml-package.md) and [microsoftml (Python)](../python-reference/microsoftml/microsoftml-package.md) functions to a computational engine on another system. For example, script running locally in R Client can shift execution to a remote Machine Learning Server in a Spark cluster to process data there. 

The primary reason for shifting compute context is to eliminate data transfer over your network, bringing computations to where the data resides. This is particularly relevant for big data platforms like Hadoop, where data is distributed over multiple nodes, or for data sets that are simply too large for a client workstation.

## Compare "local" to "remote"

| Context | Usage |
|---------|----------|
| Local | Default, supported by all products (including R Client), on all platforms. Script executes on local interpreters using local machine resources. |
| Remote | Specifically targets a Machine Learning Server on selected data platforms: Spark over HDFS, Hadoop MapReduce, SQL Server. Clients, or servers acting in the capacity of a client, can initiate a remote compute context, but the target remote machine itself must be a Machine Learning Server installation.

## Compare "remote execution" to "remote compute context"

Although similarly named, remote execution is distinct from a remote compute context. 

| Concept | Language | Usage | Configuration |
|---------|------------|-------|---------------|
| Remote compute context | R and Python | Data-centric and function-specific. Script or code that runs in a remote compute context can include functions from our proprietary libraries: RevoScaleR (R), MicrosoftML (R), revoscalepy (Python), and microsoftml (Python).  | None required. If you have server or client installs at the same functional level, you can write script that shifts the compute context. |
| Remote execution | R only | Machine-oriented, using two or more Machine Learning Server instances interchangeably, or shifting execution from R Client to a more powerful Machine Learning Server on Windows or Linux. Remote execution is data and library agnostic: you can call functions from any library, including base R and third-party vendors. | An operationalization feature, enabled as a post-installation task. For more information, see [remote execution](how-to-execute-code-remotely.md). |

## Compute contexts & data sources for RevoScaleR

Remote computing is available for specific data sources on selected platforms. The following tables document the supported combinations.

Context name | Alias | Usage |
-----------|--------------------|-----------------------|
[`RxLocalSeq`](../r-reference/revoscaler/rxlocalseq.md)      | local     | All server and client configurations support a local compute context. |
[`RxSpark`](../r-reference/revoscaler/rxspark.md)         | spark     | Remote compute context. Target is a Spark cluster on Hadoop. |
[`RxHadoopMR`](../r-reference/revoscaler/rxhadoopmr.md)      | hadoopmr  | Remote compute context. Target is Hadoop MapReduce.|
[`RxInSqlServer`](../r-reference/revoscaler/rxinsqlserver.md)   | sqlserver | Remote compute context. Target server is a single database node (SQL Server 2016 R Services or SQL Server 2017 Machine Learning Services). Computation is parallel, but not distributed. |
[`RxLocalParallel`](../r-reference/revoscaler/rxlocalparallel.md) |localpar | Compute context is often used to enable controlled, distributed computations relying on instructions you provide rather than a built-in scheduler on Hadoop. You can use compute context for manual distributed computing. | 
[`RxForeachDoPar`](../r-reference/revoscaler/rxforeachdopar.md) | dopar | Use for manual distributed computing. |

### Data sources per compute context

Given a compute context, the following table shows which data sources are available (x indicates available):

| Data Source | [`RxLocalSeq`](../r-reference/revoscaler/rxlocalseq.md) | [`RxHadoopMR`](../r-reference/revoscaler/rxhadoopmr.md) | [`RxSpark`](../r-reference/revoscaler/rxspark.md) | [`RxInSqlServer`](../r-reference/revoscaler/rxinsqlserver.md) |
|-------------|------------|------------|--------------|---------------|
| [`RxTextData`](../r-reference/revoscaler/rxtextdata.md) | X |  X |   |   |
| [`RxXdfData`](../r-reference/revoscaler/rxxdfdata.md) | X | X | X  |   |
| [`RxHiveData`](../r-reference/revoscaler/rxsparkdata.md) |  X | X | X  |   |
| [`RxParquetData`](../r-reference/revoscaler/rxsparkdata.md) |  X | X | X  |   |
| [`RxOrcData`](../r-reference/revoscaler/rxsparkdata.md) |  X | X | X  |   |
| [`RxOdbcData`](../r-reference/revoscaler/rxodbcdata.md) | X |   | X  |  X |
| [`RxSqlServerData`](../r-reference/revoscaler/rxsqlserverdata.md) |   |   |   | X |
| [`RxSasData`](../r-reference/revoscaler/rxsasdata.md) | X |   |   |   |
| [`RxSpssData`](../r-reference/revoscaler/rxspssdata.md) | X |   |   |   |

> [!Note]
> Within a data source type, you might find differences depending on the file system type and compute context. For example, the .xdf files created on the Hadoop Distributed File System (HDFS) are somewhat different from .xdf files created in a non-distributed file system such as Windows or Linux. For more information, see [How to use RevoScaleR on Hadoop](how-to-revoscaler-hadoop.md). 

## Compute contexts & data sources for revoscalepy

Remote computing is available for specific data sources on selected platforms. The following tables document the supported combinations for revoscalepy.

Context name | Alias | Usage |
-------------|-------|-------|
| [`RxLocalSeq`](../python-reference/revoscalepy/rxlocalseq.md)   | local  | All server and client configurations support a local compute context. |
| [`RxInSqlServer`](../python-reference/revoscalepy/rxinsqlserver.md) | sqlserver | Remote compute context. Target server is a single database node (SQL Server 2017 Machine Learning with Python support). Computation is parallel, but not distributed.| 
| [`rx-spark-connect`](../python-reference/revoscalepy/rx-spark-connect.md) | spark | Remote compute context. Target is a Spark 2.0-2.1 cluster over Hadoop Distributed File System (HDFS). |

### Data sources per compute context

Given a compute context, the following table shows which data sources are available (x indicates available):

| Data Source | [`RxLocalSeq`](../python-reference/revoscalepy/rxlocalseq.md) | [`rx-get-spark-connect`](../python-reference/revoscalepy/rx-spark-connect.md) | [`RxInSqlServer`](../python-reference/revoscalepy/rxinsqlserver.md) |
|-------------|------------|------------|--------------|
| [`RxTextData`](../python-reference/revoscalepy/rxtextdata.md) | X |  X |   | 
| [`RxXdfData`](../python-reference/revoscalepy/rxxdfdata.md) | X | X |  | 
| [`RxHiveData`](../python-reference/revoscalepy/rxhivedata.md) |  X | X |   | 
| [`RxParquetData`](../python-reference/revoscalepy/rxparquetdata.md) |  X | X |  | 
| [`RxOrcData`](../python-reference/revoscalepy/rxorcdata.md) |  X | X |  | 
| [`RxSparkDataFrame`](../python-reference/revoscalepy/rxsparkdataframe.md)  | X |  X  |    |
| [`RxOdbcData`](../python-reference/revoscalepy/rxodbcdata.md) | X |  X | X  |
| [`RxSqlServerData`](../python-reference/revoscalepy/rxsqlserverdata.md) | X |   |  X |

## When to switch context

The primary use case for switching the compute context is to bring calculations and analysis to the data itself. As such, the use cases for a remote compute context leverage database platforms, such as SQL Server, or data located on the Hadoop Distributed File System (HDFS) using Spark or MapReduce for processing layer.

Use case | Description | 
---------|-------------|
Client to Server| Write and run script locally in R Client, pushing specific computations to a remote Machine Learning Server instance. You can shift calculations to systems with more powerful processing capabilities or database assets.|
Server to Server | Push platform-specific computations to a server on a different platform. Supported platforms include SQL Server,  Hadoop (Spark or MapReduce). You can implement a distributed processing architecture: RxLocalSeq, RxSpark, RxInSqlServer. |

## Context and distributed computing

Many analytical functions in RevoScaleR, MicrosoftML, revoscalepy, and microsoftml can execute in parallel. On a multi-core computer, such functions run multi-threaded. On a distributed platform like Hadoop, the functions distribute workload execution to all available cores and nodes. This capability translates into high-performance computing for predictive and statistical analysis of big data, and is a major motivation for pushing a compute context to a remote Hadoop cluster. For more information, see [Distributed and parallel computing in Machine Learning Server](how-to-revoscaler-distributed-computing.md).

## Next steps

Step-by-step instructions on how to get, set, and manage compute context in [How to set and manage compute context in Machine Learning Server](how-to-revoscaler-distributed-computing-compute-context.md).

## See also

+ [Introduction to Machine Learning Server](../what-is-machine-learning-server.md) 
+ [Install Machine Learning Server on Windows](../install/machine-learning-server-windows-install.md)  
+ [Install Machine Learning Server on Linux](../install/machine-learning-server-linux-install.md)  
+ [Install Machine Learning Server on Hadoop](../install/machine-learning-server-hadoop-install.md)