---

# required metadata
title: "How to set and manage compute context in Machine Learning Server "
description: "Push a compute context to Machine Learning Server on a different computer or platform for remote execution."
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "jhubbard"
ms.date: "10/11/2017"
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

# How to set and manage compute context in Machine Learning Server

In Machine Learning Server, every session that loads a function library has a [compute context](concept-what-is-compute-context.md). The default is local, available on all platforms. No action is required to use a local compute context.

You can switch to a remote compute context to shift script execution to a different server or platform. For example, you might want to bring calculations and analysis to where the data resides on a database platform, such as SQL Server, or Hadoop Distributed File System (HDFS) using Spark or MapReduce as the processing layer.

You can create multiple compute context objects: just use them one at a time. Often, functions operate identically in local and remote context. If script execution is successful locally, you can generally expect the same results on the remote server, subject to these [limitations](#limits-on-context-shift). 

This article is focused primarily on Hadoop Spark and MapReduce. For SQL Server compute contexts, see [Define and use a compute context](https://docs.microsoft.com/sql/advanced-analytics/tutorials/deepdive-define-and-use-compute-contexts) in the SQL Server documentation.

## Prerequisites

Shifting a compute context requires that the remote computer has Machine Learning Server at the same functional level as the client. A remote Machine Learning Server 9.2.1 can accept a compute context shift from another Machine Learning Server 9.2.1 interpreter, from a SQL Server 2017 Machine Learning Services instance, and for R developers, from R Client at the same functional level (3.4.1).

The following table is a recap of platform and data source requirements for each function library.

| Library | Platforms & Data Sources |
|---------|-------------------------|
| RevoScaleR | Spark 2-2.4 over HDFS, Hadoop MapReduce: Hive, Orc, Parquet, Text, XDF, ODBC <br/><br/>SQL Server: tables, views, local text and .xdf files <sup>(1)</sup>, ODBC data sources |
| revoscalepy | Spark 2-2.4 over HDFS: Hive, Orc, Parquet, Text, XDF, ODBC <br/><br/>SQL Server 2017 Machine Learning with Python: tables, views, local text and .xdf files <sup>(1)</sup>, ODBC data sources |

<sup>(1)</sup> You can load text or .xdf files locally, but be aware that code and data is run through SQL Server, which results in [implicit data type conversions](https://docs.microsoft.com/sql/advanced-analytics/r/r-libraries-and-data-types#r-and-sql-data-types).

> [!NOTE]
> RevoScaleR is available in both Machine Learning Server and R Client. You can develop script in R Client for execution on the server. However, because R Client is limited to two threads for processing and in-memory datasets, scripts might require deeper customizations if datasets are large and come with dependencies on data chunking. Chunking is not supported in R Client. In R Client, the `blocksPerRead` argument is ignored and all data is read into memory. Large datasets that exceed memory must be pushed to a compute context of a Machine Learning Server instance that provides data chunking.
>

## Get a compute context

As a starting point, practice the syntax for returning the local compute context. Every platform, product, and language supports the default local compute context.

```R
# RevoScaleR is loaded automatically so no import step is required
rxGetComputeContext()
```

```python
from revoscalepy import RxLocalSeq
localcc = RxLocalSeq()
```
For both languages, the return object should be Return object should be **RxLocalSeq**. 

## Set a remote compute context

This section uses examples to illustrate the syntax for setting compute context.

### For Spark

The compute context used to distribute computations on a Hadoop Spark 2-2.4 cluster. For more information, see the [How to use RevoScaleR with Spark](how-to-revoscaler-spark.md).

```R
    myHadoopCluster <- RxSpark(myHadoopCluster)
```

### For MapReduce

The compute context used to distribute computations on a Hadoop MapReduce cluster. This compute context can be used on a node (including an edge node) of a Cloudera or Hortonworks cluster with a RHEL operating system, or a client with an SSH connection to such a cluster. For details on creating and using `RxHadoopMR` compute contexts, see the [How to use RevoScaleR with Hadoop](how-to-revoscaler-hadoop.md)

```R
    myHadoopCluster <- RxHadoopMR(myHadoopCluster)
```

### For SQL Server (in-database)

The `RxInSqlServer` compute context is a special case in that it runs computations in-database, but it runs on only a single database node, so the computation is parallel, but not distributed. 

For setting up a remote compute context to SQL Server, we provide an example below, but point you to [Define and use a compute context](https://docs.microsoft.com/sql/advanced-analytics/tutorials/deepdive-define-and-use-compute-contexts) in the SQL Server documentation for additional instructions.

```R
# Requires RevoScaleR and SQL Server on same machine
connectionString <- "Server=(placeholder-server-name);Database=RevoAirlineDB;Trusted_Connection=true"

sqlQuery <- "WITH nb AS (SELECT 0 AS n UNION ALL SELECT n+1 FROM nb where n < 9) SELECT n1.n+10*n2.n+100*n3.n+1 AS n, ABS(CHECKSUM(NewId())) 

myServer <- RxComputeContext("RxInSqlServer", sqlQuery = sqlQuery, connectionString = connectionString)   
                   
rxSetComputeContext(computeContext = myServer)
```

<a name="limits-on-context-shift"></a>

## Limitations of remote compute context

A primary benefit of remote computing is to perform distributed analysis using the inherent capabilities of the host platform. For a list of analytical functions that are multithreaded and cluster aware, see [Running distributed analyses using RevoScaleR](how-to-revoscaler-distributed-computing-distributed-analysis.md). As a counter point, the concept of "remote compute context" is not optimized for data manipulation and transformaton workloads. As such, many data-related functions come with local compute requirements. The following table describes the exceptions to remote computing.

| Limit | Details |
|-------|---------|
| Script execution of open source routines | Scripts use a combination of open source and proprietary functions. Only revoscalepy and RevoScaleR functions, with the respective interpreters, are multithreaded and distributable. Open source routines in your script still run locally in a single threaded process. |
| Single-threaded proprietary functions | Analyses that do not run in parallel include [import and export functions](../r-reference/revoscaler/revoscaler.md#import-export-functions), [data transformation functions](../r-reference/revoscaler/revoscaler.md#data-transform-functions), [graphing functions](../r-reference/revoscaler/revoscaler.md#graphing-functions). |
| Date location and operational limits | Sort and merge must be local. Import is multithreaded, but not cluster-aware. If you execute rxImport in a Hadoop cluster,  <br/><br/>Data may have to be moved back and forth between the local and remote environments during the course of overall program execution. Except for file copying in Hadoop, RevoScaleR does not include functions for moving data. |


## Set a no-wait compute context

By default, all jobs are "waiting jobs" or "blocking jobs", where control of the command prompt is not returned until the job is complete. For jobs that complete quickly, this is an appropriate choice. 

For large jobs that run longer, execute the job but use a non-waiting compute context to continue working in your local session. In this case, you can specify the compute context to be *non-waiting* (or *non-blocking*), in which case an object containing information about the pending job is used to retrieve results later. 

To set the compute context object to run "no wait" jobs, set the argument *wait* to *FALSE*. 
R
```
    myHadoopCluster <- RxSpark(myHadoopCluster, wait=FALSE)
```

Another use for non-waiting compute contexts is for massively parallel jobs involving multiple clusters. You can define a non-waiting compute context on each cluster, launch all your jobs, then aggregate the results from all the jobs once they complete.

For more information, see [Non-Waiting Jobs](how-to-revoscaler-distributed-computing-background-jobs.md#non-waiting-jobs).

## Retrieve cluster console output

If you want console output from each of the cluster R processes printed to your user console, specify *consoleOutput=TRUE* in your compute context.

```R
    myHadoopCluster <- RxSpark(myHadoopCluster, consoleOutput=TRUE)
```

## Update a compute context

Once you have a compute context object, modify it using the same function that originally creates it. Pass the name of the original object as its first argument, and then specify only those arguments you wish to modify as additional arguments. 

For example, to change only the `suppressWarning` parameter of a Spark compute context *myHadoopCluster* from TRUE to FALSE:

```R
    myHadoopCluster <- RxSpark(myHadoopCluster, suppressWarning=FALSE)
```

To list parameters and default values, use the `args` function with the name of the compute context constructor, for example:

```R
	args(RxSpark)
```

which gives the following output:

	function (object, hdfsShareDir = paste("/user/RevoShare", Sys.info()[["user"]], 
		sep = "/"), shareDir = paste("/var/RevoShare", Sys.info()[["user"]], 
		sep = "/"), clientShareDir = rxGetDefaultTmpDirByOS(), sshUsername = Sys.info()[["user"]], 
		sshHostname = NULL, sshSwitches = "", sshProfileScript = NULL, 
		sshClientDir = "", nameNode = rxGetOption("hdfsHost"), jobTrackerURL = NULL, 
		port = rxGetOption("hdfsPort"), onClusterNode = NULL, wait = TRUE, 
		numExecutors = rxGetOption("spark.numExecutors"), executorCores = rxGetOption("spark.executorCores"), 
		executorMem = rxGetOption("spark.executorMem"), driverMem = "4g", 
		executorOverheadMem = rxGetOption("spark.executorOverheadMem"), 
		extraSparkConfig = "", persistentRun = FALSE, sparkReduceMethod = "auto", 
		idleTimeout = 3600, suppressWarning = TRUE, consoleOutput = FALSE, 
		showOutputWhileWaiting = TRUE, autoCleanup = TRUE, workingDir = NULL, 
		dataPath = NULL, outDataPath = NULL, fileSystem = NULL, packagesToLoad = NULL, 
		resultsTimeout = 15, ...) 

You can temporarily modify an existing compute context and set the modified context as the current compute context by calling `rxSetComputeContext`. For example, if you have defined *myHadoopCluster* to be a waiting cluster and want to set the current compute context to be non-waiting, you can call `rxSetComputeContext` as follows:

```R
	rxSetComputeContext(myHadoopCluster, wait=FALSE)
```

The `rxSetComputeContext` function returns the previous compute context, so it can be used in constructions like the following:

```R
    oldContext <- rxSetComputeContext(myCluster, wait=FALSE)
    …
    # do some computing with a non-waiting compute context
    …
    # restore previous compute context
    rxSetComputeContext(oldContext)
```

You can specify the compute context by name, as we have done here, but you can also specify it by calling a compute context constructor in the call to `rxSetComputeContext`. For example, to return to the local sequential compute context after using a cluster context, you can call `rxSetComputeContext` as follows:

```R
	rxSetComputeContext(RxLocalSeq())
```

In this case, you can also use the descriptive string "local" to do the same thing:

```R
	rxSetComputeContext("local")
```

## Create additional compute contexts

Given a set of distributed computing resources, you might want to create multiple compute context objects to vary the configuration. For example, you might have one compute context for waiting or blocking jobs and another for no-wait or non-blocking jobs. Or you might define one that uses all available nodes and another that specifies a particular set of nodes. 

Because the initial specification of a compute context can be somewhat tedious, it is usually simplest to create additional compute contexts by modifying an existing compute context, in precisely the same way as we updated a compute context in the previous section. For example, suppose instead of simply modifying our existing compute context from *wait=TRUE* to *wait=FALSE*, we create a new compute context for non-waiting jobs:

```R
	myNoWaitCluster <- RxSpark(myHadoopCluster, wait=FALSE)
```

> [!TIP]
> Store commonly used compute context objects in an R script, or add their definitions to an R startup file.
>

## See also

+ [Introduction to Machine Learning Server](../what-is-machine-learning-server.md) 
+ [Distributed computing](how-to-revoscaler-distributed-computing.md)
+ [Compute context](concept-what-is-compute-context.md)
+ [Define and use a compute context in SQL Server](https://docs.microsoft.com/sql/advanced-analytics/tutorials/deepdive-define-and-use-compute-contexts)
