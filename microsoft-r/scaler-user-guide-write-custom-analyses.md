---

# required metadata
title: "Writing custom analyses for large data sets using ScaleR (Microsoft R)"
description: "Get started with custom analysis of big data using ScaleR functions in Microsoft R."
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "10/17/2016"
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

# Writing custom analyses for large data sets in ScaleR

This article explains how to use ScaleR to get data one chunk at a time, and then perform a custom analysis. There are four basic steps in the analysis:

1.  Initialize results
2.  Process data, a chunk at a time
3.  Update results, after processing each chunk
4.  Final processing of results

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

## Sample Data for Use with ScaleR

Sample data is available both within the RevoScaleR package and [online](http://go.microsoft.com/fwlink/?LinkID=698896&clcid=0x409). To view the files available within the package, use the following command:

	#  Sample Data for Use with RevoScaleR

	list.files(system.file("SampleData", package = "RevoScaleR"))

This should yield the following list:

	 [1] "AirlineDemo1kNoMissing.csv" "AirlineDemoSmall.csv"      
	 [3] "AirlineDemoSmall.xdf"       "CensusWorkers.xdf"         
	 [5] "claims.dat"                 "claims.sas7bdat"           
	 [7] "claims.sav"                 "claims.sd7"                
	 [9] "claims.sqlite"              "claims.sts"                
	[11] "claims.txt"                 "claims.xdf"                
	[13] "claimsExtra.txt"            "claimsQuote.txt"           
	[15] "claimsTab.txt"              "CustomerSurvey.xdf"        
	[17] "DJIAdaily.xdf"              "fourthgraders.xdf"         
	[19] "Kyphosis.xdf"               "mortDefaultSmall.xdf"      
	[21] "mortDefaultSmall2000.csv"   "mortDefaultSmall2001.csv"  
	[23] "mortDefaultSmall2002.csv"   "mortDefaultSmall2003.csv"  
	[25] "mortDefaultSmall2004.csv"   "mortDefaultSmall2005.csv"  
	[27] "mortDefaultSmall2006.csv"   "mortDefaultSmall2007.csv"  
	[29] "mortDefaultSmall2008.csv"   "mortDefaultSmall2009.csv" "


The location of the sample data directory is stored as an option in RevoScaleR, and you can access it with the following command:

	rxGetOption("sampleDataDir")

Larger data sets containing the full airline, census, and mortgage default data sets are available for download [online](http://go.microsoft.com/fwlink/?LinkID=698896&clcid=0x409). We make extensive use of the sample data sets both in this *User’s Guide* and the companion guides:
- [RevoScaleR Getting Started Guide](scaler-getting-started.md)
- [RevoScaleR Distributed Computing Guide](scaler-distributed-computing.md); see this guide for HPC examples

## See Also

[Introduction to Microsoft R](microsoft-r-getting-started.md)

[Diving into data analysis in Microsoft R](data-analysis-in-microsoft-r.md)

[RevoScaleR Functions](/scaler/scaler.md)
