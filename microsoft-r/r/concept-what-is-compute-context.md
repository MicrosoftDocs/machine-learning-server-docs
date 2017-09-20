---

# required metadata
title: "Compute context for script execution on Machine Learning Server | Microsoft Docs"
description: "Compute context for local, distributed, and parallel processing on Machine Learning Server using revoscalepy or RevoScaleR packages." 
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "jhubbard"
ms.date: "09/09/2017"
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

# Compute context for script execution on Machine Learning Server

*Compute context* refers to the location of computations, as executed by revoscalepy for Python, or by the RevoScaleR interpreter for R on a Machine Learning Server or R Client. The ability to switch a compute context means that you can push execution to an interpreter on another machine. Local is the default. Switching to a remote compute context is typically done to get better performance if the target system has more capability, or to minimize data transfer by bringing calculations to resident data. 

You can set a compute context using functions from RevoScaleR and revoscalepy.

## List of compute contexts

The revoscalepy library only supports the local compute context, RxSpark, and RxInSqlServer. It does not support remote executiong on Hadoop MapReduce in this release. On a Spark cluster, revoscalepy can execute locally or remote. Recall that spark must be 2.0-2.4 over Hadoop Distributed File System (HDFS).

The RevoScaleR library supports the compute contexts in the following table.

Context name | Alternative name | Allowed data sources |
-----------|--------------------|-----------------------|
[RxLocalSeq](../r-reference/revoscaler/rxlocalseq.md)      | local     | (all) |
[RxSpark](../r-reference/revoscaler/rxspark.md)         | spark     | [RxTextData](../r-reference/revoscaler/rxtextdata.md), [RxXdfData](../r-reference/revoscaler/rxxdfdata.md), [RxSparkData](../r-reference/revoscaler/rxsparkdata.md) including RxHiveData, RxParquetData, RxOrcData  |
[RxHadoopMR](../r-reference/revoscaler/rxhadoopmr.md)      | hadoopmr  | [RxTextData](../r-reference/revoscaler/rxtextdata.md), [RxXdfData](../r-reference/revoscaler/rxxdfdata.md) |
[RxInSqlServer](../r-reference/revoscaler/rxinsqlserver.md)   | sqlserver | [RxSqlServerData](../r-reference/revoscaler/rxsqlserverdata.md) |
[RxLocalParallel](../r-reference/revoscaler/rxlocalparallel.md) | localpar  |  
[RxForeachDoPar](../r-reference/revoscaler/rxforeachdopar.md)  | dopar     | 


## Use cases for switching context

The primary use case for switching the compute context is to bring calculations and analysis to the data itself. As such, the use cases for compute context typically leverage a database platform, such as SQL Server, or data located on the Hadoop Distributed File System (HDFS) using Spark or MapReduce for processing layer.

The remote system must have Machine Learning Server installed on it. Interpreters for revoscalepy or RevoScaleR cannot be installed independently of the infrastructure that supports them.

Use case | Description | 
---------|-------------|
Client to Server| Write and run script locally in R Client, pushing specific computations to a remote Machine Learning Server instance. You can rotate calculations among systems having better processing capabilities or database assets.|
Server to Server | Push platform-specific computations to a server on a different platform. Supported platforms include SQL Server,  Hadoop (Spark or MapReduce). You can implement a distributed processing architecture: RxLocalSeq, RxSpark, RxInSqlServer. |

> [!Tip]
> If your objective is simply to use two or more Machine Learning Server instances interchangeably, or to shift execution from R Client to a more powerful Machine Learning Server, consider [remote execution](how-to-execute-code-remotely.md) rather than switching the compute context. Remote execution gives you the ability to run R sessions on any standalone Machine Learning Server installation on Windows or Linux. 

## Use case for distributed computing

A compute context object is the key to distributed computing with RevoScaleR. The default compute context tells the RevoScaleR engine to execute computations locally. In the default compute context, high-performance analytics (HPA) functions such as `rxLinMod` are distributed only to the local cores, if there is more than one, and high-performance computations (HPC) submitted via `rxExec` are done sequentially.

If you have distributed computing resources available to you, you can create a compute context object for those resources, set your compute context using `rxOptions`, and then use those distributed computing resources in subsequent calls to RevoScaleR.

You can create multiple compute context objects and switch between them easily. You can also modify existing compute context objects, for example, to add new computers as they come online.

The principal compute contexts are the following:

- `RxLocalSeq`: the default compute context. This compute context is available on all platforms.

- `RxHadoopMR`: the compute context used to distribute computations on a Hadoop MapReduce cluster. This compute context can be used on a node (including an edge node) of a Cloudera or Hortonworks cluster with a RHEL operating system, or a client with an SSH connection to such a cluster. For details on creating and using `RxHadoopMR` compute contexts, see the [How to use RevoScaleR with Hadoop](how-to-revoscaler-hadoop.md).

- `RxSpark`: the compute context used to distribute computations on a Hadoop Spark cluster. For more information, see the [How to use RevoScaleR with Spark](how-to-revoscaler-spark.md).

The `RxInSqlServer` compute context is a special case in that it runs computations in-database, but it runs on only a single database node, so the computation is parallel, but not distributed. For details on creating and using `RxInSqlServer` compute contexts, see the [Introducing Machine Learning with SQL Server](concept-what-is-sql-server-r-services.md).

## Next steps

Learn how to get, set, and manage compute context in [How to set and manage compute context in Machine Learning Server](how-to-revoscaler-distributed-computing-compute-context.md).

## See Also

 [Introduction to Machine Learning Server](../what-is-machine-learning-server.md) 
 [Install Machine Learning Server on Windows](../install/machine-learning-server-windows-install.md)  
 [Install Machine Learning Server on Linux](../install/machine-learning-server-linux-install.md)  
 [Install Machine Learning Server on Hadoop](../install/machine-learning-server-hadoop-install.md)