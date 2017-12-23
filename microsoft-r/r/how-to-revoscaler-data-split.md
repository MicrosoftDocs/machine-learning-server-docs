---

# required metadata
title: "Split data into multiple files using rxSplit (Machine Learning Server) "
description: "Split data to train and test a model, or subdivide a large dataset into smaller files."
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

# How to split datasets for model training and testing

This article explains how to split a dataset in two for training and testing a model, but the same technique applies for any use case where subdividing data is required. For example, to manually partition data across multiple nodes in a cluster, you could use the functions and workflow described in this article for that purpose.

## Functions for splitting datasets

R developers, use [rxSplit (RevoScaleR)](../r-reference/revoscaler/rxsplitxdf.md). The following example is from [Run R code in R Client and Machine Learning Server](quickstart-run-r-code.md) quickstart.

```R
#Randomly split data (80% for training, 20% for testing).
   rxSplit(inData = outFileFinal,
           outFilesBase = paste0(td, "/modelData"),
           outFileSuffixes = c("Train", "Test"),
           splitByFactor = "splitVar",
           overwrite = TRUE,
           transforms = list(
             splitVar = factor(sample(c("Train", "Test"),
                                       size = .rxNumRows,
                                       replace = TRUE,
                                       prob = c(.80, .20)),
                                levels = c("Train", "Test"))),
            rngSeed = 17,
            consoleOutput = TRUE)
```

Python developers, we recommend [sci-kit](http://scikit-learn.org/stable/) methods for splitting data, as demonstrated in the [Example of binary classification with the microsoftml Python package](../python/quickstart-binary-classification-with-microsoftml.md) quickstart.

```python
from sklearn.model_selection import train_test_split

bc_train, bc_test = train_test_split(bc_df, test_size=0.2)

print("# of rows in training set = ",bc_train.size)
print("# of rows in test set = ",bc_test.size)
```

<a name="managing-distributed-data"></a>

## Create a distributed dataset on HDFS with rxSplit 

In HDFS, the data is distributed automatically, typically to a subset of the nodes, and the computations are also distributed to the nodes containing the required data. To work with data on HDFS, we recommend *composite* .xdf files, which are specialized files designed to be managed by HDFS. For more information, see [Import HDFS > Write a composite XDF](how-to-revoscaler-data-hdfs.md#write-a-composite-xdf).

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

## Data Analysis with Split Data on HDFS

To use split data in your distributed data analysis, the first step is generally to split the data using rxSplit, which as we have seen is a local operation. So the next step is then to copy the split data to your cluster nodes. 

Create an XDF source object:

    hdfsFS <- RxHdfsFileSystem()
    bigAirDS <- RxXdfData(airDataDir, fileSystem = hdfsFS ) 
    
Connect to the cluster:

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

## Distributed Predictions on HDFS

You can predict (or score) from a fitted model in a distributed context, but in this case, your data *must* be split. For example, if we fit our distributed linear model with *covCoef=TRUE* (and *cube=FALSE*), we can compute standard errors for the predicted values:

	AirlineLmDist <- rxLinMod(ArrDelay ~ DayOfWeek,
		data="DistAirlineData.xdf",  covCoef=TRUE, blocksPerRead=30)
	rxPredict(AirlineLmDist, data="DistAirlineData.xdf", 	outData="errDistAirlineData.xdf",
		computeStdErrors=TRUE, computeResiduals=TRUE)

> [!NOTE]
> The `blocksPerRead` argument is ignored if script runs locally using R Client.
>

The output data is also split, in this case holding fitted values, residuals, and standard errors for the predicted values.

## Split Training and Test Datasets on HDFS

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

## Perform Data Operations on Each Node

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


## See also

 [Machine Learning Server](../what-is-machine-learning-server.md) 
 [Install Machine Learning Server on Hadoop](../install/machine-learning-server-hadoop-install.md)
