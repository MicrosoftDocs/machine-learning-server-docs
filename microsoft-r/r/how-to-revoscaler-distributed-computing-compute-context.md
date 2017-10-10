---

# required metadata
title: "How to set and manage compute context in Machine Learning Server | Microsoft Docs"
description: "Push a compute context to Machine Learning Server on a different computer or platform for remote execution."
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "jhubbard"
ms.date: "03/23/2017"
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

You can switch to a remote compute context to shift script execution to a different server or platform. For example, you might want to bring calculations and analysis to where the data resides on a database platform, such as SQL Server, or on the Hadoop Distributed File System (HDFS) using Spark or MapReduce as the processing layer.

## Prerequisites

For Python, the compute context must be Spark 2.0-2.4 on HDFS.

For R, the specific dependency is to have the same version of RevoScaleR. This means you must have same-versioned server instances for both local and remote, or compatible co-released versions of R Client and Machine Learning Server (for example, R Client 3.3.4. and Machine Learning Server 9.2.1). Because RevoScaleR is only installed through R Client or Machine Learning Server, the version prerequisite can only be satisfied through product installation.

## Get a compute context

At a command prompt, run `rxGetComputeContext()` to return the current compute context. Every platform supports the default local compute context **RxLocalSeq**. 


## Set a compute context

This section uses examples to illustrate the syntax for setting compute context for several platforms.

### For Spark:

    myHadoopCluster <- RxSpark(myHadoopCluster)

### For MapReduce:

    myHadoopCluster <- RxHadoopMR(myHadoopCluster)

### For SQL Server (in-database)

~~~~
# Requires RevoScaleR and SQL Server on same machine

connectionString <- "Server=(placeholder-server-name);Database=RevoAirlineDB;Trusted_Connection=true"

sqlQuery <- "WITH nb AS (SELECT 0 AS n UNION ALL SELECT n+1 FROM nb where n < 9) SELECT n1.n+10*n2.n+100*n3.n+1 AS n, ABS(CHECKSUM(NewId())) 

myServer <- RxComputeContext("RxInSqlServer", sqlQuery = sqlQuery, connectionString = connectionString)   
                   
rxSetComputeContext(computeContext = myServer)
~~~~

## Set a compute context for distributed processing 

RevoScaleR functions can be used to distribute computations over more than one server instance, allowing you to run workloads on multiple computers. To get distributed computations, you create one or more *compute contexts*, and then shift script execution to a RevoScaleR interpreter on a different computer. 

RevoScaleR's distributed computing capabilities vary by platform and the details for creating a compute context vary depending upon the specific framework used to support those distributed computing capabilities. However, once you have established a computing context, you can use the same **RevoScaleR** commands to manage your data, analyze data, and control computations in all frameworks.

> [!NOTE]
> RevoScaleR is available in both Machine Learning Server and R Client. You can develop script in R Client for execution on the server.  However, because R Client is limited to two threads for processing and in-memory datasets, scripts might require deeper customizations if the scope of operations involves much larger datasets that introduce dependencies on chunking. Chunking is not supported in R Client. In R Client, the `blocksPerRead` argument is ignored and all data is read into memory. Large datasets that exceed memory must be pushed to a compute context of a Machine Learning Server instance.
>

## Limitations on switching

1. Not all RevoScaleR capability is available on every distributed computing platform, such as Hadoop. 
2. Only some RevoScaleR functions and rxExec run in a distributed manner.  
3. The main R script including any open source routines still runs locally in a single threaded process. 
4. Data and objects needed for distributed execution of rxExec or a RevoScaleR function needs to be copied to the remote compute context if the object is not already there, such as to a cluster, database, or Hadoop. 
5. With limited exceptions (such as file copying to and from Hadoop), RevoScaleR does not include functions for moving data.  
6. Some RevoScaleR functions only run locally, such as sort, merge, and import, so data may have to be moved back and forth between the local and remote environments during the course of overall program execution. 
7. rxPredict on a cluster is only possible if the data file is split.

## Waiting and Non-waiting compute contexts

By default, all jobs are "waiting jobs" or "blocking jobs", where control of the command prompt is not returned until the job is complete. For jobs that complete quickly, this is an appropriate choice. However, for larger jobs that take several minutes to several hours to run on a cluster, it is often useful to send the job off to the cluster and then to be able to continue working in your local session. In this case, you can specify the compute context to be *non-waiting* (or *non-blocking*), in which case an object containing information about the pending job is returned and can be used to retrieve results later. 

To set the compute context object to run "no wait" jobs, set the argument *wait* to *FALSE*. For more information on non-waiting jobs, see ["Non-Waiting Jobs"](how-to-revoscaler-distributed-computing-background-jobs.md#non-waiting-jobs).

Another use for non-waiting compute contexts is for massively parallel jobs involving multiple clusters. You can define a non-waiting compute context on each cluster, launch all your jobs, then aggregate the results from all the jobs once they complete.

## Automatically retrieve cluster console output

If you want console output from each of the cluster R processes printed to your user console, specify *consoleOutput=TRUE* in your compute context.

## Update a compute context

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

## Create additional compute contexts

Given a set of distributed computing resources, you might want to create multiple compute context objects to vary the configuration. For example, you might have one compute context for waiting or blocking jobs and another for no-wait or non-blocking jobs. Or you might define one that uses all available nodes and another that specifies a particular set of nodes. 

Because the initial specification of a compute context can be somewhat tedious, it is usually simplest to create additional compute contexts by modifying an existing compute context, in precisely the same way as we updated a compute context in the previous section. For example, suppose instead of simply modifying our existing compute context from *wait=TRUE* to *wait=FALSE*, we create a new compute context for non-waiting jobs:

	myNoWaitCluster <- RxSpark(myHadoopCluster, wait=FALSE)

> [!TIP]
> Store commonly used compute context objects in an R script, or add their definitions to an R startup file.
>

