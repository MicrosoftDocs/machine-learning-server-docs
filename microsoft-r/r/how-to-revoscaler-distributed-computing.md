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

Machine Learning Server's computational engine is built for distributed and parallel processing, automatically partitioning a workload across multiple nodes in a cluster, or on the available threads on multi-core machine. Access to the engine is through functions in our properietary packages: [RevoScaleR (R)](../r-reference/revoscaler/revoscaler.md), [revoscalepy (Python)](../python-reference/revoscalepy/revoscalepy-package.md), and machine learning algorithms in [MicrosoftML (R)](../r-reference/microsoftml/microsoftml-package.md) and [microsoftml (Python)](../python-reference/microsoftml/microsoftml-package.md), respectively.  

RevoScaleR and revoscalepy provide data structures and data operations used in downstream analyses and predictions. Both libraries are designed to process large data one chunk at a time, independently and in parallel. Each computing resource needs access only to that portion of the total data source required for its particular computation. This capability is amplified on distributed computing platforms like Spark. Instead of passing large amounts of data from node to node, the computations are farmed out to nodes in the cluster, executing on the node providing the data.

## Architectures supporting workload distribution

On a single server with multiple cores, jobs run in parallel, assuming the workload can be divided into smaller pieces and executed on multiple threads. 

On a distributed platform like Hadoop, you might write script that runs locally on one node, such as an edge node in the cluster, but shift execution to worker nodes for bigger jobs. When executed on a distributed platform like Spark over Hadoop Distributed File System (HDFS), both revoscalepy and RevoScaleR automatically use all available cores on all nodes in a cluster.  

Distributed and parallel processing is revo-managed, where the engine assigns a job to an available computing resource (a node in cluster, or a thread on a multi-core machine), thereby becoming the logical master node for that job. The master node is responsible for the following operations:

1. Distributes the computation to itself and the other computing resources
2. Gathers the results of the independent, parallel computations
3. Finalizes and returns the results

To shift execution to the worker nodes in a cluster, you have to set the [compute context](concept-what-is-compute-context.md) to the platform. For example, you might use the local compute context on an edge node to prepare data or set up variables, and then shift context  to RxSpark or RxHadoopMR to run data analysis on worker nodes. 

Shifting to a Spark or HadoopMR compute context comes with a list of supported data sources for that platform. Assuming the data inputs you want to analyze are supported in a Spark or Hadoop compute context, your scripts for distributed analysis can include any of the functions noted in this article. For a list of supported data sources by compute context, see [Compute context for script execution in Machine Learning Server](concept-what-is-compute-context.md).

> [!Note]
> Distributed computing is conceptually similar to parallel computing, but in Machine Learning Server, it specifically refers to workload distribution across multiple physical servers. Distributed platforms contribute the following infrastructure used for managing the entire operation: a job scheduler for allocating jobs, data nodes to run the jobs, and a master node for tracking the work and coordinating the results. In practical terms, you can think of distributed computing as a capability provided by [Machine Learning Server for Hadoop and Spark](../install/machine-learning-server-hadoop-install.md).

## Functions for multi-threaded data operations

Import, merge, and step transformations are multi-threaded on a parallel architecture.

| RevoScaleR (R) | revoscalepy (Python) |
|----------------|----------------------|
| [RxImport](../r-reference/revoscaler/rximport.md) | [rx-import](../python-reference/revoscalepy/rx-import.md) | 
| [RxDataStep](../r-reference/revoscaler/rxdatastep.md) | [rx-data-step](../python-reference/revoscalepy/rx-data-step.md) | 
| [RxMerge](../r-reference/revoscaler/rxmerge.md) | [rx-merge](../python-reference/revoscalepy/rx-merge.md) |

## Functions for distributed analysis

The following analytical functions execute in parallel, with the results unified as a single response in the return object:

| RevoScaleR (R) | revoscalepy (Python) |
|----------------|----------------------|
| [rxSummary](../r-reference/revoscaler/rxsummary.md) | [rx-summary](../python-reference/revoscalepy/rx-summary.md) |
| [rxLinMod](../r-reference/revoscaler/rxlinmode.md) | [rx-lin-mod](../python-reference/revoscalepy/rx-lin-mod.md) |
| [rxLogit](../r-reference/revoscaler/rxlogit.md) | [rx-logit](../python-reference/revoscalepy/rx-logit.md) |
| [rxGlm](../r-reference/revoscaler/rxglm.md) | not available |
| [rxCovCor](../r-reference/revoscaler/rxglm.md) | not available |
| [rxCube](../r-reference/revoscaler/rxcube.md) | not available |
| [rxCrossTabs](../r-reference/revoscaler/rxcrosstabs.md) | not available |
| [rxKmeans](../r-reference/revoscaler/rxkmeans.md) | not available |
| [rxDTree](../r-reference/revoscaler/rxdtree.md) | [rx-dtree](../python-reference/revoscalepy/rx-dtree.md) |
| [rxDForest](../r-reference/revoscaler/rxdforest.md) | [rx-dforest](../python-reference/revoscalepy/rx-dforest.md) |
| [rxBTrees](../r-reference/revoscaler/rxbtrees.md) | [rx-btrees](../python-reference/revoscalepy/rx-btrees.md) |
| [rxNaiveBayes](../r-reference/revoscaler/rxnaivebayes.md) | not available |

> [!Tip]
> For examples and practical tips on working with high-performance analytical functions, see [Running distributed analyses using RevoScaleR](how-to-revoscaler-distributed-computing-distributed-analysis.md).

## User-defined distributed analysis

An alternative approach is to use the [rxExec (R)](../r-reference/revoscaler/rxexec.md) or [rx-exec (Python)](../python-reference/revoscalepy/rx-exec.md)function, which can run arbitrary R functions in a distributed fashion on available cores, and all available nodes in a cluster. You can create new functions or call existing functions in sequence, packaged as a single execution perfromed by rxExec.

When using rxExec, you largely control how the computational tasks are distributed and you are responsible for any aggregation and final processing of results. 

## See also

+ [Running distributed analyses using RevoScaleR](how-to-revoscaler-distributed-computing-distributed-analysis.md)
+ [Compute context](concept-what-is-compute-context.md) 
+ [What is RevoScaleR](concept-what-is-revoscaler.md) 


