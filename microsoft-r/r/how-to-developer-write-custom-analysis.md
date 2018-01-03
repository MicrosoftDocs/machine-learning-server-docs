---

# required metadata
title: "Writing custom analyses for large data sets using RevoScaleR (Machine Learning Server) "
description: "Get started with custom analysis of big data using RevoScaleR functions in Machine Learning Server."
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "cgronlun"
ms.date: "01/03/2018"
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

# Writing custom analyses for large data sets in RevoScaleR

This article explains how to use RevoScaleR to get data one chunk at a time, and then perform a custom analysis. There are four basic steps in the analysis:

1.  Initialize results
2.  Process data, a chunk at a time
3.  Update results, after processing each chunk
4.  Final processing of results

## Prerequisites

Sample data is the airline flight delay dataset. For information on how to obtain this data, see [Sample data for RevoScaleR and revoscalepy](sample-built-in-data.md).

## Tutorial walkthrough: Custom chunk operations

For illustrative purposes, suppose that we want to compute the average arrival delay for flights that leave in the morning, afternoon, and evening. For each chunk of data, we need to compute the sum of arrival delay for each of the three time intervals, as well as the counts for each interval. We will accumulate these results in a list of “transformObjects” containing the six values. At the end after processing all the data, we will divide the accumulated totals by the accumulated counts to compute the averages.

Most of the work takes place within a transformation function, which processes the data and updates the results for each chunk of data that is read in. We use the .rxGet and .rxSet functions to store information from one pass of the data to the next. Because we are processing data and not creating newly transformed variables, we return NULL from the function:

		#  Writing Your Own Analyses for Large Data Sets

		ProcessAndUpdateData <- function( data )
		{
		    # Process Data
		    notMissing <- !is.na(data$ArrDelay)
		    morning <- data$CRSDepTime >= 6 & data$CRSDepTime < 12 & notMissing
		    afternoon <- data$CRSDepTime >= 12 & data$CRSDepTime < 17 & notMissing
		    evening <- data$CRSDepTime >= 17 & data$CRSDepTime < 23 & notMissing
		    mornArr <- sum(data$ArrDelay[morning], na.rm = TRUE)      
		    mornCounts <- sum(morning, na.rm = TRUE)
		    afterArr <- sum(data$ArrDelay[afternoon], na.rm = TRUE)
		    afterCounts <- sum(afternoon, na.rm = TRUE)
		    evenArr <- sum(data$ArrDelay[evening], na.rm = TRUE)
		    evenCounts <- sum(evening, na.rm = TRUE)


		 # Update Results
		   .rxSet("toMornArr", mornArr + .rxGet("toMornArr"))
		   .rxSet("toMornCounts", mornCounts + .rxGet("toMornCounts"))
		   .rxSet("toAfterArr", afterArr + .rxGet("toAfterArr"))
		   .rxSet("toAfterCounts", afterCounts + .rxGet("toAfterCounts"))
		   .rxSet("toEvenArr", evenArr + .rxGet("toEvenArr"))
		   .rxSet("toEvenCounts", evenCounts + .rxGet("toEvenCounts"))

		    return( NULL )
		}


Our transformation object values are initialized in a list passed into rxDataStep. We also use the argument *returnTransformObjects* to indicate that we want updated values of the *transformObjects* returned rather than a transformed data set:

	totalRes <- rxDataStep( inData = airData , returnTransformObjects = TRUE,
	    transformObjects =
	        list(toMornArr = 0, toAfterArr = 0, toEvenArr = 0,
	        toMornCounts = 0, toAfterCounts = 0, toEvenCounts = 0),
	    transformFunc = ProcessAndUpdateData,
	    transformVars = c("ArrDelay", "CRSDepTime"))


The rxDataStep function will automatically chunk through the data for us. All we need to do is process the final results:

	FinalizeResults <- function(totalRes)
	{
	    return(data.frame(
	        AveMorningDelay = totalRes$toMornArr / totalRes$toMornCounts,
	        AveAfternoonDelay = totalRes$toAfterArr / totalRes$toAfterCounts,
	        AveEveningDelay = totalRes$toEvenArr / totalRes$toEvenCounts))
	}
	FinalizeResults(totalRes)


The calculated results are:

	  AveMorningDelay AveAfternoonDelay AveEveningDelay
	1        6.146039          13.66912        16.71271


In this case we can check our results by using the *rowSelection* argument in *rxSummary*:

	rxSummary(~ArrDelay, data = "airExample.xdf",
	rowSelection = CRSDepTime >= 6 & CRSDepTime < 12 & !is.na(ArrDelay))
	Call:
	rxSummary(formula = ~ArrDelay, data = "airExample.xdf",
	rowSelection = CRSDepTime >= 6 & CRSDepTime < 12 & !is.na(ArrDelay))

	Summary Statistics Results for: ~ArrDelay
	File name:
	    C:\YourOutputPath\airExample.xdf
	Number of valid observations: 234403

	 Name     Mean     StdDev  Min Max  ValidObs MissingObs
	 ArrDelay 6.146039 35.4734 -85 1490 234403   0


## See Also

+ [Machine Learning Server](../what-is-machine-learning-server.md)
+ [How-to guides in Machine Learning Server](how-to-introduction.md)
+ [RevoScaleR Functions](~/r-reference/revoscaler/revoscaler.md)
