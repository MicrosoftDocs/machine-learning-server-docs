---

# required metadata
title: "Distributed and parallel execution for high-performance computing (Machine Learning Server)"
description: "High performance computing (HPC) for distributed workloads using SQL Server in-database and Hadoop clusters computing RevoScaleR package for R and revoscalepy for Python."
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "cgronlun"
ms.date: "12/19/2017"
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

# Distributed and parallel computing in Machine Learning Server

Machine Learning Server's computing engine is built for distributed and parallel computing, automatically partitioning a workload across multiple nodes in a cluster, or on the available threads on multi-core machine. Access to the engine is through functions in our properietary packages: [RevoScaleR for R](../r-reference/revoscaler/revoscaler.md), [revoscalepy for Python](../python-reference/revoscalepy/revoscalepy-package.md), and machine learning algorithms in [MicrosoftML (R)](../r-reference/microsoftml/microsoftml-package.md) and [microsoftml (Python)](../python-reference/microsoftml/microsoftml-package.md), respsectively.  

Both RevoScaleR and revoscalepy function libraries are designed to process large data one chunk at a time, independently and in parallel. Each computing resource needs access only to that portion of the total data source required for its particular computation. This capability is amplified on distributed computing platforms like Spark. Instead of passing large amounts of data from node to node, the computations are farmed out to nodes in the cluster, executing on the node providing the data.

## Architecture supporting workload distribution

Distributed computing is conceptually similar to parallel computing, but in Machine Learning Server, it specifically refers to workload distribution across multiple physical servers. Distributed platforms provide the following: a job scheduler for allocating jobs, data nodes to run the jobs, and a master node for tracking the work and coordinating the results. 
 
On a single server with multiple cores, many jobs can run in parallel, assuming the workload can be divided into smaller pieces and executed on multiple threads. To inform the engine of platform capabilities, your script should include an object called a [compute context](concept-what-is-compute-context.md) that identifies the platform.

On a distributed platform, you might write script that runs locally on one node, such as an edge node in a Hadoop cluster, but shift execution to data nodes for bigger jobs. For example, you might use the local compute context on an edge node to prepare data or set up variables, and then shift to an `RxSpark` context to run data analysis on data nodes. In practice, because some distributed platforms have specialized data handling requirements, you may also have to specify a context-specific data source along with the compute context, but the bulk of your analysis scripts can then proceed with no further changes.

## Functions for distributed analysis

When executed on a distributed platform like Spark over Hadoop Distributed File System (HDFS), both revoscalepy and RevoScaleR automatically use the available nodes in a cluster. Both provide two main approaches for distributed computing: master-worker approach and [rxExec](../r-reference/revoscaler/rxexec.md). 

The master-worker approach is revo-managed, where the engine assigns a job to an available computing resource (a node in cluster, or a thread on a multi-core machine), thereby becoming the master node for that job. The master node distributes the computation to itself and the other computing resources; gathers the results of the independent, parallel computations; and finalizes and returns the results. If you also specified an RxSpark or RxHadoop compute context, the computing resources are worker nodes in the cluster.

You can call any of the following RevoScaleR analysis functions and have the computation proceed in parallel and return the answer to you:

- `rxSummary`
- `rxLinMod`
- `rxLogit`
- `rxGlm`
- `rxCovCor` (and its convenience functions, `rxCov`, `rxCor`, and `rxSSCP`)
- `rxCube` and `rxCrossTabs`
- `rxKmeans`
- `rxDTree`
- `rxDForest`
- `rxBTrees`
- `rxNaiveBayes`

The [rxExec](../r-reference/revoscaler/rxexec.md) approach allows you to run arbitrary R functions in a distributed fashion, using available nodes (computers) or available cores (the maximum of which is the sum over all available nodes of the processing cores on each node). When using `rxExec`, you largely control how the computational tasks are distributed and you are responsible for any aggregation and final processing of results. 

## See also

+ [Compute context](concept-what-is-compute-context.md) 
+ [What is RevoScaleR](concept-what-is-revoscaler.md) 


