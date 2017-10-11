---

# required metadata
title: "Compute context for script execution on Machine Learning Server | Microsoft Docs"
description: "Compute context for local, distributed, and parallel processing on Machine Learning Server using revoscalepy or RevoScaleR packages." 
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "jhubbard"
ms.date: "09/28/2017"
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

# Compute context for script execution

*Compute context* refers to the location of computations. You can switch from local to remote compute context to push execution of data-centric **revoscalepy** or  **RevoScaleR** functions to an interpreter on another machine.

The primary reason for shifting compute context is to eliminate data transfer over your network, bringing computations to resident data. This is particularly relevent for big data platforms like Hadoop, where data is distributed and managed over multiple nodes, or for data sets that are simply too large for a client workstation.

| Context | How used |
|---------|----------|
| Local | Local is the default, supported by all products, on all platforms. Script executes usin local machine resources. |
| Remote | Remote is a server-only feature, available for selected data platforms: Spark over HDFS, Hadoop MapReduce, SQL Server. For example, script running locally in R Client can shift execution to a remote Machine Learning Server in a Spark cluster to process data there. 

Many analytical functions can execute in parallel. On distributed platforms, such as Hadoop processing frameworks (Spark and MapReduce), **revoscalepy** and **RevoScaleR** can distribute workload execution to all available cores and nodes. This capability translates into high performance computing for predictive and statistical analysis of big data, enabled when you set a remote compute context to your Hadoop cluster.

> [!Tip]
> If your objective is simply to use two or more Machine Learning Server instances interchangeably, or to shift execution from R Client to a more powerful Machine Learning Server on Windows or Linux, consider [remote execution](how-to-execute-code-remotely.md). Remote execution is data platform and library agnostic. From a local R session, you can switch to a remote R session and call functions from any library, including base R and third-party vendors. 

## revoscalepy: compute context list

The library supports the local compute context and remote compute context for Spark and SQL Server only.

Context name | Alternative name | Usage |
-----------|--------------------|-----------------------|
[RxLocalSeq](../r-reference/revoscaler/rxlocalseq.md)      | local     | All server and client configurations support a local compute context. |
[RxSpark](../r-reference/revoscaler/rxspark.md)         | spark     | Use for a remote compute context where the target is a Spark 2.0-2.4 cluster over Hadoop Distributed File System (HDFS). |
[RxInSqlServer](../r-reference/revoscaler/rxinsqlserver.md)   | sqlserver | Use for a remote compute context where the target server is SQL Server 2017 Machine Learning with Python support. |

## RevoScaleR: compute context list

Context name | Alternative name | Usage |
-----------|--------------------|-----------------------|
[RxLocalSeq](../r-reference/revoscaler/rxlocalseq.md)      | local     | All server and client configurations support a local compute context. |
[RxSpark](../r-reference/revoscaler/rxspark.md)         | spark     | Use for a remote compute context where the target is a Spark cluster on Hadoop. |
[RxHadoopMR](../r-reference/revoscaler/rxhadoopmr.md)      | hadoopmr  | Use for a remote compute context where the target is Hadoop MapReduce.|
[RxInSqlServer](../r-reference/revoscaler/rxinsqlserver.md)   | sqlserver | Use for a remote compute context where the target server is SQL Server 2016 or later with R Server. |
[RxLocalParallel](../r-reference/revoscaler/rxlocalparallel.md) |localpar | Compute context is often used to enable controlled, distributed computations relying on instructions you provide rather than a built-in scheduler on Hadoop. You can use compute context for manual distributed computing. | 
[RxForeachDoPar](../r-reference/revoscaler/rxforeachdopar.md) | dopar | Use for manual distributed computing. |

## RevoScaleR: Data sources by compute context

In the local compute context, all of RevoScaleR’s supported data sources are available to you. In a distributed compute context, however, your choice of data sources may be limited. The following table shows the available combinations of compute contexts and data sources (x indicates available):

| Data Source | [`RxLocalSeq`](../r-reference/revoscaler/rxlocalseq.md) | [`RxHadoopMR`](../r-reference/revoscaler/rxhadoopmr.md) | [`RxSpark`](../r-reference/revoscaler/rxspark.md) | [`RxInSqlServer`](../r-reference/revoscaler/rxinsqlserver.md) |
|-------------|------------|------------|--------------|---------------|
| Fixed-Format Text ([`RxTextData`](../r-reference/revoscaler/rxtextdata.md)`) | X |  X |   |   |
| Delimited Text ([`RxTextData`](../r-reference/revoscaler/rxtextdata.md)) | X | X | X  |   |
| .xdf data files ([`RxXdfData`](../r-reference/revoscaler/rxxdfdata.md)`) | X | X | X  |   |
| [RxSparkData](../r-reference/revoscaler/rxsparkdata.md) including `RxHiveData`, `RxParquetData`, `RxOrcData`  |  X | X | X  |   |
| ODBC data ([`RxOdbcData`](../r-reference/revoscaler/rxodbcdata.md)) | X |   | X  |  X |
| SQL Server database ([`RxSqlServerData`](../r-reference/revoscaler/rxsqlserverdata.md)) |   |   |   | X |
| SAS data files ([`RxSasData`](../r-reference/revoscaler/rxsasdata.md)) | X |   |   |   |
| SPSS data files ([`RxSpssData`](../r-reference/revoscaler/rxspssdata.md)) | X |   |   |   |

Within a data source type, you might find differences depending on the file system type and compute context. For example, the .xdf files created on the Hadoop Distributed File System (HDFS) are somewhat different from .xdf files created in a non-distributed file system such as Windows or Linux. For more information, see [How to use RevoScaleR on Hadoop](how-to-revoscaler-hadoop.md). Similarly, predictions in a distributed compute context require that the data be split across the available nodes. See [Managing Distributed Data](how-to-revoscaler-distributed-computing.md#managing-distributed-data) for details.

## Use cases for switching context

The primary use case for switching the compute context is to bring calculations and analysis to the data itself. As such, the use cases for compute context typically leverage a database platform, such as SQL Server, or data located on the Hadoop Distributed File System (HDFS) using Spark or MapReduce for processing layer.

The remote system must have Machine Learning Server installed on it. Interpreters for revoscalepy or RevoScaleR cannot be installed independently of the infrastructure that supports them.

Use case | Description | 
---------|-------------|
Client to Server| Write and run script locally in R Client, pushing specific computations to a remote Machine Learning Server instance. You can rotate calculations among systems having better processing capabilities or database assets.|
Server to Server | Push platform-specific computations to a server on a different platform. Supported platforms include SQL Server,  Hadoop (Spark or MapReduce). You can implement a distributed processing architecture: RxLocalSeq, RxSpark, RxInSqlServer. |



## Use case for distributed computing

A compute context object is the key to distributed computing with RevoScaleR. The default compute context tells the RevoScaleR engine to execute computations locally. In the default compute context, high-performance analytics  functions such as `rxLinMod` are distributed only to the local cores, if there is more than one, and high-performance computations submitted via `rxExec` are done sequentially.

If you have distributed computing resources available to you, you can create a compute context object for those resources, set your compute context using `rxOptions`, and then use those distributed computing resources in subsequent calls to RevoScaleR.

## Next steps

Learn how to get, set, and manage compute context in [How to set and manage compute context in Machine Learning Server](how-to-revoscaler-distributed-computing-compute-context.md).

## See also

+ [Introduction to Machine Learning Server](../what-is-machine-learning-server.md) 
+ [Install Machine Learning Server on Windows](../install/machine-learning-server-windows-install.md)  
+ [Install Machine Learning Server on Linux](../install/machine-learning-server-linux-install.md)  
+ [Install Machine Learning Server on Hadoop](../install/machine-learning-server-hadoop-install.md)