---

# required metadata
title: "RevoScaleR User's  Guide--Introduction"
description: "Introductory material for ScaleR User's Guide."
keywords: ""
author: "richcalaway"
manager: "mblythe"
ms.date: "03/17/2016"
ms.topic: "get-started-article"
ms.prod: "rserver"
ms.service: ""
ms.assetid: ""

# optional metadata
ROBOTS: ""
audience: ""
ms.devlang: ""
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.technology: ""
ms.custom: ""

---

# Introduction

## Why RevoScaleR?

RevoScaleR™ provides tools for both High Performance Computing (HPC) and High Performance Analytics (HPA) with R. HPC capabilities allow you to distribute the execution of essentially any R function across cores and nodes, and deliver the results back to the user. HPA adds ‘big data’ to the challenge. This guide focuses on these HPA ‘big data’ capabilities. (See the [*RevoScaleR Distributed Computing Guide*](rserver-scaler-distributed-computing.md) for information on both HPC and HPA capabilities in a distributed context.)

RevoScaleR provides tools for scalable data management and analysis. These tools can be used with data sets in memory, and applied the same way to huge data sets stored on disk. It includes functionality for:

1.  Accessing external data sets (SAS, SPSS, ODBC, Teradata, and delimited and fixed format text) for analysis in R

2.  Efficiently storing and retrieving data in a high performance data file

3.  Cleaning, exploring, and manipulating data

4.  Fast, basic statistical analyses

RevoScaleR also includes an extensible framework for writing your own analyses for big data sets.

With RevoScaleR, you can use R to analyze data sets far larger than can be kept in memory. This is possible because RevoScaleR uses external memory algorithms that allow it to work on one chunk of data at a time (that is, a subset of the rows and perhaps the variables in the data set), update the results, and proceed through all available data.

### Accessing External Data Sets

Data can be stored in a wide-variety of formats. Typically, the first step in any RevoScaleR analysis is to make the data accessible. With RevoScaleR’s data import capability, you can access data from a SAS file, SPSS file, fixed format or delimited text file, an ODBC connection, or a Teradata data base, bringing it into a data frame in memory, or storing it for fast access in chunks on disk.

### Efficiently Storing and Retrieving Data

A key component of RevoScaleR is a data file format (.xdf) that is extremely efficient for both reading and writing data. You can create .xdf files by importing data files or from R data frames, and add rows or variables to an existing .xdf file. Once your data is in this file format you can use it directly with analysis functions provided with RevoScaleR, or quickly extract a subsample and read it into a data frame in memory for use in other R functions.

### Data Cleaning, Exploration, and Manipulation

When working with a new data set, the first step is to clean and explore. With RevoScaleR, you can quickly obtain information about your data set (e.g., how many rows and variables) and the variables in your data set (e.g., name, data type, value labels). With RevoScaleR’s summary statistics and cube functionality, you can examine summary information about your data and quickly plot histograms or relationships between variables.

RevoScaleR also provides all of the power of R to use in data transformations and manipulations. In RevoScaleR’s data step functionality, you can specify R expressions to transform specific variables and have them automatically applied to a single data frame or to each block of data as it is read in from an .xdf file. You can create new variables, recode variables, and set missing values with all the flexibility of the R language.


### Statistical Analysis

In addition to descriptive statistics and crosstabs, RevoScaleR provides functions for fitting linear and binary logistic regression models, generalized linear models, k-means models, and decision trees and forests. These functions access .xdf files or other data sources directly or operate on data frames in memory. Because these functions are so efficient and do not require that all of the data be in memory at one time, you can analyze huge data sets without requiring huge computing power. In particular, you can relax assumptions previously required. For example, instead of assuming a linear or polynomial functional form in a model, you can break independent variables into many categories providing a completely flexible functional form. The many degrees of freedom provided by large data sets, combined with RevoScaleR’s efficiency, make this approach feasible.

### Writing Your Own Analyses for Large Data Sets

All of the main analysis functions in RevoScaleR use updating or external memory algorithms, that is, they analyze a chunk of data, update the results, then move on to the next chunk of data and repeat the process. When all the data has been processed (sometimes multiple times), final results can be calculated and the analysis is complete. You can write your own functions to process a chunk of data and update results, and use RevoScaleR functionality to provide you with access to your data file chunk by chunk.

## Getting Started

In this initial chapter, we take a quick walk through the five feature areas described above using a sample data set. More detailed information about the functions used is available in the other chapters of this manual. We then briefly describe other sample data available for use with RevoScaleR. Finally, we describe the online help available. For additional introductory material, see the [*RevoScaleR Getting Started Guide*](rserver-scaler-getting-started.md). For information on using RevoScaleR for distributing computations over more than one computer, see the [*RevoScaleR Distributed Computing Guide*](rserver-scaler-distributed-computing.md) and the various Getting Started guides for particular distributed computing platforms:

 [*RevoScaleR MapReduce Getting Started Guide*](rserver-scaler-hadoop-getting-started.md)
 
 [*RevoScaleR Spark Getting Started Guide*](rserver-scaler-spark-getting-started.md)

 [*RevoScaleR Teradata Getting Started Guide*](rserver-scaler-teradata-getting-started.md)

 For more information on accessing databases via ODBC, see the [*RevoScaleR ODBC Import Guide*](rserver-scaler-odbc.md).

### Accessing External Data Sets

The most common way to store data is in a text file. For example, a comma-delimited, text data file containing a subsample of information on airline departures and arrivals in the United States is available in the RevoScaleR sample data directory. (More examples using this data file are available in the [*Getting Started Guide*](rserver-scaler-getting-started.md].) The sample code below will import it using the *rxImport* function. There are a total of 600,000 rows in the data file. By specifying the argument *rowsPerRead*, we read and write the data in 3 blocks of 200,000 rows each.

	######################################################## 
	# Chapter 1: Introduction
	Ch1Start <- Sys.time()
	
	inFile <- file.path(rxGetOption("sampleDataDir"), "AirlineDemoSmall.csv")
	airData <- rxImport(inData=inFile, outFile = "airExample.xdf", 
	stringsAsFactors = TRUE, missingValueString = "M", rowsPerRead = 200000)

If the *outFile* argument had been omitted, the returned *airData* object would be a data frame containing the data. Since the imported data is being stored in an .xdf file, *rxImport* returns an .xdf data source object. This object represents the .xdf data file, but doesn’t take up much memory. It can be used in many other RevoScaleR objects interchangeably with a data frame.

To get basic information about the data set and variables, we use *rxGetInfo*:

	rxGetInfo(airData, getVarInfo = TRUE)

which results in the following output:

	File name: C:\YourOutputPath\airExample.xdf 
	Number of observations: 6e+05 
	Number of variables: 3 
	Number of blocks: 3 
	Compression type: zlib
	Variable information: 
	Var 1: ArrDelay, Type: integer, Low/High: (-86, 1490)
	Var 2: CRSDepTime, Type: numeric, Storage: float32, Low/High: (0.0167, 23.9833)
	Var 3: DayOfWeek
		7 factor levels: Monday Tuesday Wednesday Thursday Friday Saturday Sunday


Let’s take a quick look at the data. We can use the *rxHistogram* function to show the distribution in arrival delay by day of week. It uses the *rxCube* function (described later in this manual) to calculate information for a histogram:

	rxHistogram(~ArrDelay|DayOfWeek,  data = airData)

![](media/rserver-scaler-user-guide-1-introduction/image2.png)

We can also compute descriptive statistics for the variable:

	rxSummary(~ ArrDelay, data = airData )
	
		Call:
		rxSummary(formula = ~ArrDelay, data = airData)
		
		Summary Statistics Results for: ~ArrDelay
		Data: airData (RxXdfData Data Source)
		File name: airExample.xdf
		Number of valid observations: 6e+05 
		
		Name     Mean     StdDev   Min Max  ValidObs MissingObs
		ArrDelay 11.31794 40.68854 -86 1490 582628   17372


Our first look tells us two important things: arrival delay has a long “tail” for every day of the week, with a few flights delayed for well over two hours, and there are quite a few missing values (17,372). Presumably the missing values for arrival delay represent flights that did not arrive, that is, were cancelled.

We can use RevoScaleR’s data step functionality to create a new variable, *VeryLate*, that represents flights that were either over two hours late or canceled. Since we have our original data safely stored in a text file, we will simply add this variable to our existing airline.xdf file using the *transforms* argument to *rxDataStep*. The *transforms* argument takes a list of one or more R expressions, typically in the form of assignment statements. In this case, we use the following expression: *list(VeryLate = (ArrDelay \> 120 | is.na(ArrDelay))*

The full RevoScaleR data step then consists of the following steps:

1.  Read in the *data* a block (200,000 rows) at a time.
2.  For each block, pass the *ArrDelay* data to the R interpreter for processing the transformation to create *VeryLate*.
3.  Write the data out to the data set a block at a time. The argument *overwrite=TRUE* allows us to overwrite the data file.

		airData <- rxDataStep(inData = airData, outFile = "airExample.xdf", 
		    transforms=list(VeryLate = (ArrDelay > 120 | is.na(ArrDelay))), 
		    overwrite = TRUE)

### Statistical Analysis

We can perform statistical analysis using a data frame or .xdf file. For example, we can estimate a logistic regression on whether or not a flight is “very late” depending on the day of week using *rxLogit*:

	logitResults <- rxLogit(VeryLate ~ DayOfWeek, data = airData )
	summary(logitResults)

		Call:
		rxLogit(formula = VeryLate ~ DayOfWeek, data = airData)
		
		Logistic Regression Results for: VeryLate ~ DayOfWeek
		File name:
		    C:\YourOutputPath\airExample.xdf
		Dependent variable(s): VeryLate
		Total independent variables: 8 (Including number dropped: 1)
		Number of valid observations: 6e+05
		Number of missing observations: 0 
		-2*LogLikelihood: 251244.7201 (Residual deviance on 599993 degrees of freedom)
		 
		Coefficients:
		                    Estimate Std. Error z value Pr(>|z|)    
		(Intercept)         -3.29095    0.01745 -188.64 2.22e-16 ***
		DayOfWeek=Monday     0.40086    0.02256   17.77 2.22e-16 ***
		DayOfWeek=Tuesday    0.84018    0.02192   38.33 2.22e-16 ***
		DayOfWeek=Wednesday  0.36982    0.02378   15.55 2.22e-16 ***
		DayOfWeek=Thursday   0.29396    0.02400   12.25 2.22e-16 ***
		DayOfWeek=Friday     0.54427    0.02274   23.93 2.22e-16 ***
		DayOfWeek=Saturday   0.48319    0.02282   21.18 2.22e-16 ***
		DayOfWeek=Sunday     Dropped    Dropped Dropped  Dropped    
		---
		Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1 
		
		Condition number of final variance-covariance matrix: 16.7804 
		Number of iterations: 3


The results show that in this sample, a flight on Tuesday is most likely to be very late or cancelled, followed by flights departing on Friday. In this model, Sunday is the control group, so that coefficient estimates for other days of the week are relative to Sunday. The intercept shown is the same as the coefficient you would get for Sunday if you omitted the intercept term:

	logitResults2 <- rxLogit(VeryLate ~ DayOfWeek - 1, data = airData )
		summary(logitResults2)
		Call:
		rxLogit(formula = VeryLate ~ DayOfWeek - 1, data = airData)
		
		Logistic Regression Results for: VeryLate ~ DayOfWeek - 1
		File name:
		    C:\YourOutputPath\airExample.xdf
		Dependent variable(s): VeryLate
		Total independent variables: 7 
		Number of valid observations: 6e+05
		Number of missing observations: 0 
		-2*LogLikelihood: 251244.7201 (Residual deviance on 599993 degrees of freedom)
		 
		Coefficients:
		                    Estimate Std. Error z value Pr(>|z|)    
		DayOfWeek=Monday    -2.89008    0.01431  -202.0 2.22e-16 ***
		DayOfWeek=Tuesday   -2.45077    0.01327  -184.7 2.22e-16 ***
		DayOfWeek=Wednesday -2.92113    0.01617  -180.7 2.22e-16 ***
		DayOfWeek=Thursday  -2.99699    0.01648  -181.9 2.22e-16 ***
		DayOfWeek=Friday    -2.74668    0.01459  -188.3 2.22e-16 ***
		DayOfWeek=Saturday  -2.80776    0.01471  -190.9 2.22e-16 ***
		DayOfWeek=Sunday    -3.29095    0.01745  -188.6 2.22e-16 ***
		---
		Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1 
		
		Condition number of final variance-covariance matrix: 1 
		Number of iterations: 7

### Writing Your Own Analyses for Large Data Sets

We can have RevoScaleR provide data to us, one chunk at a time, and perform our own custom analysis. There are four basic steps in the analysis:

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

### Sample Data for Use with RevoScaleR

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

Larger data sets containing the full airline, census, and mortgage default data sets are available for download [online](http://go.microsoft.com/fwlink/?LinkID=698896&clcid=0x409). We make extensive use of the sample data sets both in this *User’s Guide* and the companion documents *RevoScaleR Getting Started Guide* (RevoScaleR\_Getting\_Started.pdf) and *RevoScaleR Distributed Computing Guide* (RevoScaleR\_Distributed\_Computing.pdf).

## Managing Threads

A process is an instance of a software program, running under an operating system. RevoScaleR runs in two separate processes, one running R and R-related code, and one running RevoScaleR’s underlying compute engine.

In order to perform parallel computations, a process must provide blocks of executable code to the operating system that can be run in parallel.

There are two primary approaches to this problem. One approach is to use threads. Threads can be thought of as sub-programs that are still bound to the original process that creates them. They communicate with the creating process, while still being autonomous enough to allow the operating system to schedule their operation independently, thus allowing parallelization.

By way of contrast, forking a process (a technique only available on Unix-based systems) creates a complete copy of one process that differs only by a flag available within each of the resulting processes.

Unfortunately, these two methods of parallelization are not completely compatible unless steps are taken to ensure that they do not interfere with each other and cause the software to malfunction. Specifically, program failures can be caused by forking a Unix process while the objects used to control communication and synchronization between threads are in use.

In order to optimize performance, RevoScaleR is capable of establishing and maintaining a pool of threads. This way, there is no overhead in creating these threads to parallelize a computation. However, given the above issues, this functionality is, by default, disabled on Linux (this is not an issue on Windows, as Windows does not use fork; hence, the thread pool is always instantiated and ready for use on Windows).

If you know that you will not be forking your R process (such as spawning it using nohup or creating multiple R processes with the multicore package), or if you know exactly when you might fork a process, you can turn the thread pool on and off.  RevoScaleR provides an interface to activate the thread pool:

	rxSetEnableThreadPool(TRUE)

Similarly, the thread pool may be disabled as follows:

	rxSetEnableThreadPool(FALSE)

Enabling the thread pool will improve performance, but should not be done if there is any chance you will fork your R process. If you want to ensure that the thread pool is always enabled, you can add the above command to a *.First* function defined in either your own Rprofile startup file, or the system Rprofile.site file. For example, you can add the following lines after the closing right parenthesis of the existing Rprofile.site file:

	.First <- function()
	{
		.First.sys()
		invisible(rxSetEnableThreadPool(TRUE))
	}


The *.First.sys* function is normally run after all other initialization is complete, including the evaluation of the *.First* function. We need the call to *rxSetEnableThreadPool* to occur after RevoScaleR is loaded, and that is done by *.First.sys*, so we call *.First.sys* first.

R has extensive facilities for managing random number generation, and these are all fully supported in RevoScaleR. In addition, RevoScaleR provides an interface to random number generators supplied by the Vector Statistical Library that is part of Intel’s Math Kernel Library. To use one of these generators, call the RevoScaleR function *rxRngNewStream*, specifying the desired generator (the default is a version of the Mersenne-Twister, MT-2203), the desired substream (if applicable), and a seed (if desired). See the help file for a complete list of the available generators and examples of their use.

The VSL random number generators are most useful in a distributed setting, where they allow parallel processes to generate uncorrelated random number streams. See the *RevoScaleR Distributed Computing Guide* (RevoScaleR\_Distributed\_Computing.pdf) for complete details.

RevoScaleR depends on a number of packages that are specified as “default packages” in Microsoft R Services. To ensure these packages are loaded when using Rscript, you must include the flag *–-default-packages=* (with nothing on the right-hand side), and not use the flag *–-vanilla* when invoking Rscript. In particular, the methods package is required for correct operation of RevoScaleR.

The RevoScaleR package includes comprehensive help for all of its functions and data sets, along with an overview topic describing the package as a whole. To view this overview topic, use the R ? operator at the R prompt as follows:

	?RevoScaleR

To obtain help on a RevoScaleR function, use the R *?* operator at the R prompt together with the function name. For example, the following command displays help about *rxLinMod*:

	?rxLinMod

We will often refer to help topic pages as a function’s help file, as in “see the *rxLinMod* help file for details.” This always means to type the *?* operator followed by the function name.