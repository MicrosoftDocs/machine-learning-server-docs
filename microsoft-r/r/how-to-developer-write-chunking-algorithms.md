---

# required metadata
title: "Write custom chunking algorithms in ScaleR (Machine Learning Server)"
description: "Learn how to use rxDataStep to apply arbitrary R functions on chunked data."
keywords: 
author: "dphansen"
ms.author: "davidph"
manager: "cgronlun"
ms.date: 10/18/2016
ms.topic: "how-to"
ms.prod: "mlserver"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
#ms.technology: ""
#ms.custom: ""
---

# Write custom chunking algorithms using rxDataStep in RevoScaleR

Scalability in RevoScaleR is based on chunking or external memory algorithms that can analyze chunks of data in parallel and then combine intermediate results into a single analysis. Because of the chunking algorithms, it's possible to analyze huge datasets that vastly exceed the memory capacity of any one machine.

All of the main analysis functions in RevoScaleR (*rxSummary*, *rxLinMod*, *rxLogit*, *rxGlm*, *rxCube*, *rxCrossTabs*, *rxCovCor*, *rxKmeans*, *rxDTree*, *rxBTrees*, *rxNaiveBayes*, and *rxDForest*) use chunking or external memory algorithms. However, for scenarios where specialized behaviors are needed, you can create a custom chunking algorithms using the *rxDataStep* function to automatically chunk through your data set and apply arbitrary R functions to process your data.

In this article, you'll step through an example that teaches a simple chunking algorithm for tabulating data implemented using *rxDataStep*. A more realistic tabulation approach is to use the *rxCrossTabs* or *rxCube* functions, but since *rxDataStep* is simpler, it's a better choice for instruction.

> [!Note] 
>If *rxDataStep* does not provide sufficient customization, you can build a custom parallel external memory algorithm from the ground up using functions in the **RevoPemaR** package. For more information, see [How to use the RevoPemaR library in Machine Learning Server](how-to-developer-pemar.md).

## Prerequisites

Sample data for this example is the *AirlineDemoSmall.xdf* file with a local compute context. For instructions on how to import this data set, see the tutorial in [Practice data import and exploration](tutorial-revoscaler-data-import-transform.md).

Chunking is supported on Machine Learning Server, but not on the free R Client. Because the dataset is small enough to reside in memory on most computers, most systems succeed in running this example locally. however, if the data does not fit in memory, you will need to use Machine Learning Server instead.

## About chunking algorithms

An updating algorithm takes a given set of values and a chunk of data, and then outputs a revised set of values cumulative for all chunks. The simplest example is an updating sum: sum is computed for the first chunk, followed by a second chunk, which each successive chunk contributing to a revised value until reaching the cumulative sum. 

Updating algorithms perform four main tasks:

- Initialization: declare and initialize variables needed in the computation.
- ProcessData: for each block, perform calculations on the data in the block.
- UpdateResults: combine all of the results of the ProcessData step.
- ProcessResults: when results from all blocks have been combined, do any final computations.

In this example, no initialization is required.

## Example: rxDataStep and sample airline data

The ProcessData step is performed within a *transformFunc* called by *rxDataStep* for each chunk of data. In this case we begin with a simple call to the *table* function after converting the chunk to a data frame. The results are then converted back to a data frame with a single row, which will be appended to a data set on disk. So, the call to *rxDataStep* reads in the data chunk-by-chunk and creates a new summary data set where each row represents the “intermediate results” of a chunk.

The *AggregateResults* function shown below combines the *UpdateResults* and *ProcessResults* tasks.  The summary data set is simply read into memory and the columns are summed.

To try this out, create a new script *chunkTable.R* with the following contents:

```
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
```

Note that the `blocksPerRead` argument is ignored if this script runs locally using R Client. Since Microsoft R Client can only process datasets that fit into the available memory, chunking is not supported in R Client. When run locally with R Client, all data must be read into memory. You can work around this limitation when you push the compute context to a [Machine Learning Server instance](../what-is-machine-learning-server.md).

To test the function, use the sample data *AirlineDemoSmall.xdf* file with a local compute context. For more information, see the tutorial in [Practice data import and exploration](tutorial-revoscaler-data-import-transform.md).

We’ll call our new *chunkTable* function, processing 1 block at a time so we can take a look at the intermediate results:

~~~~
	inDataSource <- file.path(rxGetOption("sampleDataDir"),
	    "AirlineDemoSmall.xdf")
	iroDataSource <- "iroFile.xdf"
	chunkOut <- chunkTable(inDataSource = inDataSource,
	    iroDataSource = iroDataSource, varsToKeep="DayOfWeek")
	chunkOut
~~~~
You will see the following results:
~~~~
	   Monday   Tuesday Wednesday  Thursday    Friday  Saturday    Sunday
	    97975     77725     78875     81304     82987     86159     94975
~~~~
To see the intermediate results, we can read the data set into memory:
~~~~
	rxDataStep(iroDataSource)

	  Monday Tuesday Wednesday Thursday Friday Saturday Sunday
	1  33137   27267     27942    28141  28184    25646  29683
	2  32407   25607     25915    26106  26211    29950  33804
	3  32431   24851     25018    27057  28592    30563  31488
~~~~
And to delete the intermediate results file:
~~~~
	file.remove(iroDataSource)
~~~~
## Next steps

This article provides an example of a simple chunking algorithm for tabulating data using *rxDataStep*. 

In practice, you might want to use the *rxCrossTabs* or *rxCube* functions. For more information, see [rxCrossTabs](~/r-reference/revoscaler/rxcrosstabs.md) and [rxCube](~/r-reference/revoscaler/rxcube.md), respectively.


## See Also

- [Machine Learning Server](../what-is-machine-learning-server.md)	
- [How-to guides in Machine Learning Server](how-to-introduction.md)
- [RevoScaleR Functions](~/r-reference/revoscaler/revoscaler.md)
- [Analyze large data with ScaleR](tutorial-revoscaler-large-data-airline.md)
- [Census data example for analyzing large data](tutorial-revoscaler-large-data-census.md)
- [Loan data example for analyzing large data](tutorial-revoscaler-large-data-loan.md)