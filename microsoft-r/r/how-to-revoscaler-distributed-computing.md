---

# required metadata
title: "Distributed and parallel computing overview (ScaleR in Microsoft R)"
description: "Microsoft R Server in-database and cluster computing using the ScaleR engine and RevoScaleR package."
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "jhubbard"
ms.date: "04/02/2017"
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

# Distributed and parallel computing with ScaleR in Microsoft R

In Microsoft R, the ScaleR functions in the RevoScaleR package are built to leverage the processing power inherent in the computing platform. On a distributed platform like Hadoop, ScaleR automatically uses the available nodes in a cluster. On multi-processor machines, ScaleR automatically runs jobs in parallel, assuming the workload can be divided into smaller pieces and executed on multiple threads. To inform the ScaleR engine of platform capabilities, your script should include an object called a [compute context](how-to-revoscaler-distributed-computing-compute-context.md) that identifies the platform.

*Parallel processing*, which leverages the computing power of a single machine, is both a R Server and R Client capability. Examples of jobs that can run in parallel include data import and linear modeling .

The degree of parallel processing you can achieve depends on whether the ScaleR runs on R Client or R Server, and if R Server, on the computational resources of the platform. On [R Client](../r-client/what-is-microsoft-r-client.md), the free workstation version that runs Windows or Linux, parallelization is restricted to a maximum of two processors, even if the machine has more capability. Thus, R Client offers parallelization, but to a much smaller degree given the constraints of two processors.

*Distributed computing* across multiple nodes is an R Server-only capability. The platform must be Hadoop (MapReduce or Spark) or Teradata, both of which provide a job scheduler for allocating jobs, data nodes to run the jobs, and a master node for tracking the work and coordinating the results. 

On a distributed platform, developers and data scientists will often write script that runs locally on one node, such as an edge node in a Hadoop cluster, but shift execution to data nodes for bigger jobs. For example, you might use the local compute context on an edge node to prepare data or set up variables, and then shift to an `RxSpark` context to run data analysis on data nodes.

In practice, because some distributed platforms have specialized data handling requirements, you may also have to specify a context-specific data source along with the compute context, but the bulk of your analysis scripts can then proceed with no further changes.

> [!NOTE]
> R Client is limited to two threads for processing and in-memory datasets. To avoid paging data to disk, R Client is engineered to ignore the `blocksPerRead` argument, which results in all data being read into memory. If your datasets exceed memory, you should push the compute context to a Microsoft R Server instance on a supported platform (Hadoop, Linux, Windows, Teradata, SQL Server).
>

## Distributed computing overview

**RevoScaleR** provides two main approaches for distributed computing: master node and `rxExec`. 

The first, the *master node* path, exemplifies the high-performance analytics approach. By establishing a distributed computing context object which specifies your distributed computing resources, you can call any of the following **RevoScaleR** analysis functions and have the computation proceed in parallel on the specified computing resources and return the answer to you:

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

In the master node approach, you submit a job by calling a **RevoScaleR** analysis function (we will also call these functions *HPA functions*). One of your available computing resources takes the job, thereby becoming the master node for that job. The master node distributes the computation to itself and the other computing nodes; gathers the results of the independent, parallel computations; and finalizes and returns the results.

The second approach is via the **RevoScaleR** function `rxExec`, which allows you to run arbitrary R functions in a distributed fashion, using available nodes (computers) or available cores (the maximum of which is the sum over all available nodes of the processing cores on each node). The `rxExec` approach exemplifies the traditional high-performance computing approach: when using `rxExec`, you largely control how the computational tasks are distributed and you are responsible for any aggregation and final processing of results. 

<a name="managing-distributed-data"></a>
## Managing Distributed Data

There are several basic approaches to data management in distributed computing:

1.	On systems having a non-distributed, disk-by-disk file system (such as nfs or NTFS), you can either put all the data on all the nodes, or distribute only the data that a node requires for its computations to that particular node. In such file systems, it is important that the data be local to the nodes to avoid adding network latency to the computation time. On non-distributed file systems, we recommend using standard .xdf files or "split" .xdf files (see "Distributing Data with rxSplit" below).

2.	In HDFS, the data is distributed automatically, typically to a subset of the nodes, and the computations are also distributed to the nodes containing the required data. On this system, we recommend “composite” .xdf files, which are specialized files designed to be managed by HDFS.

3.	In a Teradata Distributed Data Warehouse, you can perform distributed computations in-database using the RxInTeradata compute context.

For distributing high volumes of data over large networks, custom-engineered network/file-server solutions are probably appropriate.

## Distributing Data with rxSplit  

For some computations, such as those involving distributed prediction, it is most efficient to perform the computations on a distributed data set, one in which each node sees only the data it is supposed to work on. You can split an .xdf file into portions suitable for distribution using the function *rxSplit*. For example, to split the large airline data into five files for distribution on a five node cluster, you could use *rxSplit* as follows:

	rxOptions(computeContext="local")
	bigAirlineData <- "C:/data/AirlineData87to08.xdf"
	rxSplit(bigAirlineData, numOutFiles=5)

By default, *rxSplit* simply appends a number in the sequence from 1 to *numOutFiles* to the base file name to create the new file names, and in this case the resulting file names, for example, “AirlineData87to081.xdf”, are a bit confusing. You can exercise greater control over the output file names by using the *outFilesBase* and *outFilesSuffixes* arguments. With *outFilesBase*, you can specify either a single character string to be used for all files or a character vector the same length as the desired number of files. The latter option is useful, for example, if you would like to create four files with the same file name, but different paths:

	nodepaths <- paste("compute", 10:13, sep="")
	basenames <- file.path("C:", nodepaths, "DistAirlineData")
	rxSplit(bigAirlineData, outFilesBase=basenames)

This creates the four directories C:/compute10, etc., and creates a file named “DistAirlineData.xdf” in each directory. You will want to do something like this when using distributed data with the standard **RevoScaleR** analysis functions such as rxLinMod and rxLogit.

You can supply the *outFilesSuffixes* arguments to exercise greater control over what is appended to the end of each file. Returning to our first example, we can add a hyphen between our base file name and the sequence 1 to 5 using *outFilesSuffixes* as follows:

	rxSplit(bigAirlineData, outFileSuffixes=paste("-", 1:5, sep=""))

The splitBy argument specifies whether to split your data file row-by-row or block-by-block. The default is *splitBy="rows"*, to split by blocks instead, specify *splitBy="blocks"*. The *splitBy* argument is ignored if you also specify the *splitByFactor* argument as a character string representing a valid factor variable. In this case, one file is created per level of the factor variable.

The *rxSplit* function works in the local compute context only; once you’ve split the file you need to distribute the resulting files to the individual nodes using the techniques of the previous sections. You should then specify a compute context with the flag *dataDistType* set to *"split"*. Once you have done this, HPA functions such as *rxLinMod* will know to split their computations according to the data on each node.

### Data Analysis with Split Data

To use split data in your distributed data analysis, the first step is generally to split the data using rxSplit, which as we have seen is a local operation. So the next step is then to copy the split data to your cluster nodes. 

Create an XDF source object:

    hdfsFS <- RxHdfsFileSystem()
    bigAirDS <- RxXdfData(airDataDir, fileSystem = hdfsFS ) 
    
Connect to a the cluster:

	myCluster <- RxSparkConnect(nameNode = "my-name-service-server", port = 8020, wait = TRUE)

Set the compute context to your cluster:

	rxSetComputeContext(myCluster)

We are now ready to fit a simple linear model:

	AirlineLmDist <- rxLinMod(ArrDelay ~ DayOfWeek,
		data="bigAirDS",  cube=TRUE, blocksPerRead=30)

When we print the object, we see that we obtain the same model as when computed with the full data on all nodes:

	Call:
	rxLinMod(formula = ArrDelay ~ DayOfWeek, data = "DistAirlineData.xdf",
	    cube = TRUE, blocksPerRead = 30)

	Cube Linear Regression Results for: ArrDelay ~ DayOfWeek
	File name: C:\data\distributed\DistAirlineData.xdf
	Dependent variable(s): ArrDelay
	Total independent variables: 7
	Number of valid observations: 120947440
	Number of missing observations: 2587529

	Coefficients:
	                    ArrDelay
	DayOfWeek=Monday    6.669515
	DayOfWeek=Tuesday   5.960421
	DayOfWeek=Wednesday 7.091502
	DayOfWeek=Thursday  8.945047
	DayOfWeek=Friday    9.606953
	DayOfWeek=Saturday  4.187419
	DayOfWeek=Sunday    6.525040

With data in the .xdf format, you have your choice of using the full data set or a split data set on each node. For other data sources, you must have the data split across the nodes. For example, the airline data’s original form is a set of .csv files, one for each year from 1987 to 2008. (Additional years are now available, but have not been included in our big airline data.) If we copy the year 2000 data to compute10, the year 2001 data to compute11, the year 2002 data to compute12, and the year 2003 data to compute13 with the file name SplitAirline.csv, we can analyze the data as follows:

	textDS <- RxTextData( file = "C:/data/distributed/SplitAirline.csv",
	              varsToKeep = c( "ArrDelay", "CRSDepTime", "DayOfWeek" ),
	              colInfo = list(ArrDelay   = list( type = "integer" ),
	                        CRSDepTime = list( type = "integer" ),
	                        DayOfWeek  = list( type = "integer" ) )
	           )
	rxSummary( ArrDelay ~ F( DayOfWeek, low = 1, high = 7 ), textDS )

We can then perform an rxLogit model to classify flights as “Late” as follows:

	computeLate <- function( dataList )
	{
	    dataList$Late <- dataList$ArrDelay>15
	    return( dataList )
	}

	rxLogitFitSplitCsv <- rxLogit( Late ~ CRSDepTime + F( DayOfWeek, low = 1,
	                          high = 7 ), data = textDS,
	                          transformVars = c( "ArrDelay" ),
	                          transformFunc=computeLate, verbose=1 )

### Distributed Prediction

You can predict (or score) from a fitted model in a distributed context, but in this case, your data *must* be split. For example, if we fit our distributed linear model with *covCoef=TRUE* (and *cube=FALSE*), we can compute standard errors for the predicted values:

	AirlineLmDist <- rxLinMod(ArrDelay ~ DayOfWeek,
		data="DistAirlineData.xdf",  covCoef=TRUE, blocksPerRead=30)
	rxPredict(AirlineLmDist, data="DistAirlineData.xdf", 	outData="errDistAirlineData.xdf",
		computeStdErrors=TRUE, computeResiduals=TRUE)

> [!NOTE]
> The `blocksPerRead` argument is ignored if script runs locally using R Client.
>

The output data is also split, in this case holding fitted values, residuals, and standard errors for the predicted values.

### Creating Split Training and Test Data Sets

One common technique for validating models is to break the data to be analyzed into training and test subsamples, then fit the model using the training data and score it by predicting on the test data. Once you have split your original data set onto your cluster nodes, you can split the data on the individual nodes by calling rxSplit again within a call to rxExec. If you specify the RNGseed argument to rxExec (see [Parallel Random Number Generation](how-to-revoscaler-distributed-computing-parallel-jobs.md#parallel-random-number-generation)), the split becomes reproducible:

	rxExec(rxSplit, inData="C:/data/distributed/DistAirlineData.xdf",
		outFilesBase="airlineData",
		outFileSuffixes=c("Test", "Train"),
		splitByFactor="testSplitVar",
		varsToKeep=c("Late", "ArrDelay", "DayOfWeek", "CRSDepTime"),
		overwrite=TRUE,
		transforms=list(testSplitVar = factor( sample(c("Test", "Train"),
			size=.rxNumRows, replace=TRUE, prob=c(.10, .9)),
			levels= c("Test", "Train"))), rngSeed=17, consoleOutput=TRUE)

The result is two new data files, airlineData.testSplitVar.Train.xdf and airlineData.testSplitVar.Test.xdf, on each of your nodes. We can fit the model to the training data and predict with the test data as follows:

	AirlineLmDist <- rxLinMod(ArrDelay ~ DayOfWeek,
		data="airlineData.testSplitVar.Train.xdf",  covCoef=TRUE, blocksPerRead=30)
	rxPredict(AirlineLmDist, data="airlineData.testSplitVar.Test.xdf",
		computeStdErrors=TRUE, computeResiduals=TRUE)

> [!NOTE]
> The `blocksPerRead` argument is ignored if script runs locally using R Client.
>

### Performing Data Operations on Each Node

To create or modify data on each node, use the data manipulation functions within rxExec. For example, suppose that after looking at the airline data we decide to create a “cleaner” version of it by keeping only the flights where: there is information on the arrival delay,  the flight did not depart more than one hour early, and the actual and scheduled flight time is positive. We can put a call to *rxDataStep* (and any other code we want processed) into a function to be processed on each node via *rxExec*:

	newAirData <-  function()
	{
	    airData <- "AirlineData87to08.xdf"
	    rxDataStep(inData = airData, outFile = "C:\\data\\airlineNew.xdf",
		   rowSelection = !is.na(ArrDelay) &
	        (DepDelay > -60) & (ActualElapsedTime > 0) & (CRSElapsedTime > 0),
			blocksPerRead = 20, overwrite = TRUE)
	}
	rxExec( newAirData )

