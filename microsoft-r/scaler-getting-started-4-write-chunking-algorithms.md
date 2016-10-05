---

# required metadata
title: "Write custom chunking algorithms in ScaleR (Microsoft R)"
description: ""
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "10/05/2016"
ms.topic: "get-started-article"
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

## Write custom chunking algorithms in ScaleR

All of the main analysis functions in RevoScaleR (*rxSummary*, *rxLinMod*, *rxLogit*, *rxGlm*, *rxCube*, *rxCrossTabs*, *rxCovCor*, *rxKmeans*, *rxDTree*, *rxBTrees*, *rxNaiveBayes*, and *rxDForest*) use chunking or external memory algorithms, that is, they analyze each chunk of data separately, combining intermediate results. When all the data has been processed, final results can be calculated and the analysis is complete. Because all of the data does not need to be in memory at one time, you can analyze huge data sets with this type of algorithm.

You can create your own chunking algorithms, using the *rxDataStep* function to automatically chunk through your data set, and for each chunk using arbitrary R functions to process your data. In this section, we will show a simple chunking algorithm for tabulating data implemented using *rxDataStep*. (If you actually have huge data sets to tabulate, use the *rxCrossTabs* or *rxCube* functions built into **RevoScaleR**; this example is meant for instructional purposes only.)

(The Microsoft package RevoPemaR provides another, more systematic way to create R applications using your own parallel external memory algorithms. See the [RevoPemaR Getting Started Guide](http://go.microsoft.com/fwlink/?LinkID=698568&clcid=0x409).)

All updating algorithms can be broken down into four main tasks:

- Initialization: declare and initialize variables needed in the computation
- ProcessData: for each block, perform calculations on the data in the block.
- UpdateResults: combine all of the results of the ProcessData step.
- ProcessResults: when results from all blocks have been combined, do any final computations.

In this example, no initialization is required.

The Process Data step is performed within a *transformFunc* that is called by *rxDataStep* for each chunk of data. In this case we begin with a simple call to the *table* function after converting the chunk to a data frame. The results are then converted back to a data frame with a single row, which will be appended to a data set on disk. So, the call to *rxDataStep* reads in the data chunk-by-chunk and creates a new summary data set where each row represents the “intermediate results” of a chunk.

The AggregateResults function shown below combines the UpdateResults and ProcessResults tasks.  The summary data set is simply read into memory and the columns are summed.

To try this out, create a new script *chunkTable.R* with the following contents:

>The `blocksPerRead` argument is ignored if run locally using R Client. [Learn more...](#chunking)

	chunkTable <- function(inDataSource, iroDataSource, varsToKeep = NULL,
	     blocksPerRead = 1 )
	{
		ProcessChunk <- function( dataList)
		{
		    # Process Data
		    chunkTable <- table(as.data.frame(dataList))
		    # Convert table to data frame with single row
		    varNames <- names(chunkTable)
	          varValues <- as.vector(chunkTable)
	          dim(varValues) <- c(1, length(varNames))
	          chunkDF <- as.data.frame(varValues)
	          names(chunkDF) <- varNames
	          # Return the data frame
	   	    return( chunkDF )
		}

		rxDataStep( inData = inDataSource, outFile = iroDataSource,
	            varsToKeep = varsToKeep,
			blocksPerRead = blocksPerRead,
			transformFunc = ProcessChunk,
			reportProgress = 0, overwrite = TRUE)

	       AggregateResults <- function()    
	       {
		     iroResults <- rxDataStep(iroDataSource)
	           return(colSums(iroResults))
	       }

		return(AggregateResults())
	}

To test the function, use the sample data *AirlineDemoSmall.xdf* file with a local compute context.  We’ll call our new *chunkTable* function, processing 1 block at a time so we can take a look at the intermediate results:

	inDataSource <- file.path(rxGetOption("sampleDataDir"),
	    "AirlineDemoSmall.xdf")
	iroDataSource <- "iroFile.xdf"
	chunkOut <- chunkTable(inDataSource = inDataSource,
	    iroDataSource = iroDataSource, varsToKeep="DayOfWeek")
	chunkOut

You will see the following results:

	   Monday   Tuesday Wednesday  Thursday    Friday  Saturday    Sunday
	    97975     77725     78875     81304     82987     86159     94975

To see the intermediate results, we can read the data set into memory:

	rxDataStep(iroDataSource)

	  Monday Tuesday Wednesday Thursday Friday Saturday Sunday
	1  33137   27267     27942    28141  28184    25646  29683
	2  32407   25607     25915    26106  26211    29950  33804
	3  32431   24851     25018    27057  28592    30563  31488

And to delete the intermediate results file:

	file.remove(iroDataSource)
