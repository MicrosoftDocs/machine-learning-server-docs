---

# required metadata
title: "Compute context for local, distributed, and parallel processing in Microsoft R"
description: "Microsoft R Server in-database and cluster computing using the RevoScaleR engine and RevoScaleR package."
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "03/23/2017"
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

# Compute context for distributed RevoScaleR processing on Hadoop (Microsoft R)

RevoScaleR functions can be used to distribute computations over more than one R Server instance, allowing you to run workloads on multiple computers. To get distributed computations, you create one or more *compute contexts*, and then shift script execution to a RevoScaleR interpreter on a different computer. We call this flexibility *Write Once, Deploy Anywhere*. 

A *compute context* specifies the computing resources to be used by RevoScaleR’s distributable computing functions. RevoScaleR functions like `RxSpark`, `RxHadoopMR`, or `RxInSQLServer` are used to set the compute context. 

RevoScaleR's distributed computing capabilities vary by platform and the details for creating a compute context vary depending upon the specific framework used to support those distributed computing capabilities. However, once you have established a computing context, you can use the same **RevoScaleR** commands to manage your data, analyze data, and control computations in all frameworks.

> [!NOTE]
> RevoScaleR is available in both R Server and R Client. You can develop script in R Client for execution on R Server.  However, because R Client is limited to two threads for processing and in-memory datasets, scripts might require deeper customizations if the scope of operations involve much larger datasets that introduce dependencies on chunking. Chunking is not supported in R Client. In R Client, the `blocksPerRead` argument is ignored and all data is read into memory. Large datasets that exceed memory must be pushed to a compute context of a Microsoft R Server instance.
>


<a name="scaler-compute-context"></a>
## Compute Contexts Overview

A *compute context object*, or more briefly a *compute context*, is the key to distributed computing with RevoScaleR. The default compute context tells the ScaleR engine to execute computations locally. In the default compute context, high-performance analytics (HPA) functions such as `rxLinMod` are distributed only to the local cores, if there is more than one, and high-performance computations (HPC) submitted via `rxExec` are done sequentially.

If you have distributed computing resources available to you, you can create a compute context object for those resources, set your compute context using `rxOptions`, and then use those distributed computing resources in subsequent calls to RevoScaleR.

You can create multiple compute context objects and switch between them easily. You can also modify existing compute context objects, for example, to add new computers as they come online.

The principal compute contexts are the following:

- `RxLocalSeq`: the default compute context. This compute context is available on all platforms.

- `RxHadoopMR`: the compute context used to distribute computations on a Hadoop cluster. This compute context can be used on a node (including an edge node) of a Cloudera or Hortonworks ` cluster with a RHEL operating system, or a client with an SSH connection to such a cluster. For details on creating and using `RxHadoopMR` compute contexts, see the [Practice data import and exploration on Hadoop](how-to-revoscaler-hadoop.md).

- `RxInTeradata`: the compute context used to distribute computations in a Teradata appliance. For details on creating and using `RxInTeradata` compute contexts, see the [Practice data import and exploration on Teradata](how-to-revoscaler-sql-server.md).

The `RxInSqlServer` compute context is a special case—it is similar to `RxInTeradata` in that it runs computations in-database, but it runs on only a single database node, so the computation is parallel, but not distributed. For details on creating and using `RxInSqlServer` compute contexts, see the [RevoScaleR SQL Server Introduction](concept-what-is-sql-server-r-services.md).

Two other specialized compute contexts, both of which are relevant only in HPC computations via `rxExec`, are discussed in ["Parallel Computing with rxExec"](how-to-revoscaler-distributed-computing-parallel-jobs.md).

## Set a Compute Context

The default compute context is local. The following examples illustrate the syntax for setting compute context for several platforms.

For Spark:

    myHadoopCluster <- RxSpark(myHadoopCluster)

For MapReduce:

    myHadoopCluster <- RxHadoopMR(myHadoopCluster)


## Compute Contexts and Data Sources

In the local compute context, all of RevoScaleR’s supported data sources are available to you. In a distributed compute context, however, your choice of data sources may be severely limited. The most extreme case is the `RxInTeradata` compute context, which supports only the RxTeradata data source—this makes sense, as the computations are being performed on data inside the Teradata database. The following table shows the available combinations of compute contexts and data sources (x indicates available):

| Data Source | `RxLocalSeq` | `RxHadoopMR` | `RxInTeraData` | `RxInSqlServer` |
|-------------|------------|------------|--------------|---------------|
| Delimited Text (`RxTextdata`) | X | X |   |   |
| Fixed-Format Text (`RxTextData`) | X |   |   |   |
| .xdf data files (`RxXdfData`) | X | X |   |   |
| SAS data files (`RxSasData`) | X |   |   |   |
| SPSS data files (`RxSpssData`) | X |   |   |   |
| ODBC data (`RxOdbcData`) | X |   |   |   |
| Teradata database (`RxTeradata`) | X |   | X |   |
| SQL Server database (`RxSqlServerData`) |   |   |   | X |

Within a data source type, you might find differences depending on the file system type and compute context. For example, the .xdf files created on the Hadoop Distributed File System (HDFS) are somewhat different from .xdf files created in a non-distributed file system such as Windows or Linux. For more information, see [Practice data import and exploration on Hadoop](how-to-revoscaler-hadoop.md). Similarly, predictions in a distributed compute context require that the data be split across the available nodes. See [Managing Distributed Data](how-to-revoscaler-distributed-computing.md#managing-distributed-data) for details.

## Waiting and Non-waiting Compute Contexts

By default, all jobs are "waiting jobs" or "blocking jobs" (control of the R prompt is not returned until the job is complete). For RevoScaleR jobs that complete quickly, this is an appropriate choice. However, for larger jobs that take several minutes to several hours ro tun on a cluster, it is often useful to send the job off to the cluster and then to be able to continue working in your local R session. In this case, you can specify the compute context to be *non-waiting* (or *non-blocking*), in which case an object containing information about the pending job is returned and can be used to retrieve results later. 

To set the compute context object to run "no wait" jobs, set the argument *wait* to *FALSE*. For more information on non-waiting jobs, see ["Non-Waiting Jobs"](how-to-revoscaler-distributed-computing-background-jobs.md#non-waiting-jobs).

Another use for non-waiting compute contexts is for massively parallel jobs involving multiple clusters. You can define a non-waiting compute context on each cluster, launch all your jobs, then aggregate the results from all the jobs once they complete.

## Automatically Retrieving Cluster Console Output

If you want console output from each of the cluster R processes printed to your user console, specify *consoleOutput=TRUE* in your compute context.

## Update a Compute Context

Once you have a compute context object, modify it using the same function that originally creates it. Pass the name of the original object as its first argument, and then specify only those arguments you wish to modify as additional arguments. 

For example, to change only the `suppressWarning` parameter of a Spark compute context *myHadoopCluster* from TRUE to FALSE:

    myHadoopCluster <- RxSpark(myHadoopCluster, suppressWarning = FALSE)

To list parameters and default values, use the `args` function with the name of the compute context constructor, for example:

	args(RxSpark)

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

	rxSetComputeContext(myHadoopCluster, wait=FALSE)

The `rxSetComputeContext` function returns the previous compute context, so it can be used in constructions like the following:

    oldContext <- rxSetComputeContext(myCluster, wait=FALSE)
    …
    # do some computing with a non-waiting compute context
    …
    # restore previous compute context
    rxSetComputeContext(oldContext)

You can specify the compute context by name, as we have done here, but you can also specify it by calling a compute context constructor in the call to `rxSetComputeContext`. For example, to return to the local sequential compute context after using a cluster context, you can call `rxSetComputeContext` as follows:

	rxSetComputeContext(RxLocalSeq())

In this case, you can also use the descriptive string "local" to do the same thing:

	rxSetComputeContext("local")

## Create Additional Compute Contexts

Given a set of distributed computing resources, you might wan to create multiple compute context objects to vary the configuration. For example, you might have one compute context for waiting or blocking jobs and another for no-wait or non-blocking jobs. Or you might define one that uses all available nodes and another that specifies a particular set of nodes. 

Because the initial specification of a compute context can be somewhat tedious, it is usually simplest to create additional compute contexts by modifying an existing compute context, in precisely the same way as we updated a compute context in the previous section. For example, suppose instead of simply modifying our existing compute context from *wait=TRUE* to *wait=FALSE*, we create a new compute context for non-waiting jobs:

	myNoWaitCluster <- RxSpark(myHadoopCluster, wait=FALSE)

> [!TIP]
> Store commonly used compute context objects in an R script, or add their definitions to an R startup file.
>

