---

# required metadata
title: "Sort data in RevoScaleR using rxSort (Microsoft R)"
description: "How to sort data in a data frame or XDF file with the RevoScaleR rxSort function."
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "06/06/2017"
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

# How to sort data using rxSort (Microsoft R)

Many analysis and plotting algorithms require as a first step that the data be sorted. Sorting a massive data set is both memory-intensive and time-consuming, but the **rxSort** function provides an efficient solution. The **rxSort** function allows you to sort by one or many keys. A *stable* sorting routine is used, so that, in the case of ties, remaining columns are left in the same order as they were in the original data set.

As a simple example, we can sort the census worker data by *age* and *incwage*. We will sort first by *age*, using the default increasing sort, and then by *incwage*, which we will sort in decreasing order:

	#  Sorting Data
	
	censusWorkers <- file.path(rxGetOption("sampleDataDir"), "CensusWorkers.xdf")
	outXDF <- "censusWorkersSorted.xdf"
	rxSort(inData = censusWorkers, outFile = outXDF, sortByVars=c("age",
		"incwage"), decreasing=c(FALSE, TRUE))

The first few lines of the sorted file can be viewed as follows:

	rxGetInfo(outXDF, numRows=10)

	  File name: C:\YourOutputPath\censusWorkersSorted.xdf 
	  Number of observations: 351121 
	  Number of variables: 6 
	  Number of blocks: 6 
	  Compression type: zlib
	  Data (10 rows starting with row 1):
	     age incwage perwt    sex wkswork1      state
	  1   20  336000     3   Male       40 Washington
	  2   20  336000    23   Male       46 Washington
	  3   20  336000    11   Male       52 Washington
	  4   20  314000    33   Male       52    Indiana
	  5   20  168000    13   Male       24    Indiana
	  6   20  163000    16   Male       26 Washington
	  7   20  144000    27 Female       24    Indiana
	  8   20   96000    21   Male       48 Washington
	  9   20   93000    24   Male       24    Indiana
	  10  20   90000     6   Male       52 Washington

If the sort keys contain missing values, you can use the *missingsLow* flag to specify whether they are sorted as low values (*missingsLow=TRUE*, the default) or high values (*missingsLow=FALSE*).

## Remove duplicates during sort

In many situations, you are sorting a large data set by a particular key, for example, userID, but are looking for a sorted list of unique userIDs. The *removeDupKeys* argument to **rxSort** allows you to remove the duplicate entries from a sorted list. This argument is supported only for *type="auto"* and *type="mergeSort"*; it is ignored for *type="varByVar"*. 

When you use *removeDupKeys=TRUE*, the first record containing a unique combination of the *sortByVars* is retained; subsequent matching records are omitted from the sorted results, but, if desired, a count of the matching records is maintained in a new *dupFreqVar* output column. For example, the following artificial data set simulates a small amount of transaction data, with a user name, a state, and a transaction amount. When we sort by the variables *users* and *state* and specify *removeDupKeys=TRUE*, the *transAmt* shown for duplicate entries is the transaction amount for the *first* transaction encountered:

	set.seed(17)
	users <- sample(c("Aiden", "Ella", "Jayden", "Ava", "Max", "Grace", "Riley", 
		              "Lolita", "Liam", "Emma", "Ethan", "Elizabeth", "Jack", 
		              "Genevieve", "Avery", "Aurora", "Dylan", "Isabella",
		              "Caleb", "Bella"), 100, replace=TRUE)
	state <- sample(c("Washington", "California", "Texas", "North Carolina", 
		              "New York", "Massachusetts"), 100, replace=TRUE)
	transAmt <- round(runif(100)*100, digits=3)
	df <- data.frame(users=users, state=state, transAmt=transAmt)
	
	rxSort(df, sortByVars=c("users", "state"), removeDupKeys=TRUE, 
	    dupFreqVar = "DUP_COUNT")	
	  Number of rows written to file: 66, Variable(s): users, state, transAmt, DUP_COUNT, Total number of rows in file: 66
	  Time to sort data file: 0.100 seconds
	         users          state transAmt DUP_COUNT
	  1      Aiden       New York   11.010         1
	  2      Aiden North Carolina   73.307         1
	  3      Aiden          Texas    8.037         2
	  4     Aurora     California    8.787         1
	  5     Aurora  Massachusetts   55.187         1
	  6     Aurora       New York   91.648         1
	  7     Aurora          Texas   30.566         1
	  8     Aurora     Washington   27.374         1
	  9        Ava     California   70.638         2
	  10       Ava  Massachusetts   82.916         2
	  11       Ava       New York   45.683         2
	  12       Ava North Carolina   51.748         1
	  13       Ava          Texas   52.674         1
	  14     Avery     California    9.756         4
	  15     Avery  Massachusetts   63.715         1
	  16     Avery       New York   93.430         1
	  17     Avery North Carolina    1.889         2
	  18     Bella     California   60.258         2
	  19     Bella          Texas   32.684         1
	  20     Bella     Washington   60.230         2
	  21     Caleb  Massachusetts   94.527         1
	  22     Caleb North Carolina   89.259         2
	  23     Dylan     California   73.665         1
	  24     Dylan North Carolina   98.384         1
	  25     Dylan          Texas   27.067         1
	  26     Dylan     Washington   82.141         3
	  27 Elizabeth     California   95.497         2
	  28 Elizabeth       New York   35.546         3
	  29 Elizabeth North Carolina   18.892         2
	  30      Ella     California   11.644         1
	  31      Ella North Carolina   72.289         1
	  32      Ella     Washington   66.453         2
	  33      Emma  Massachusetts   28.502         1
	  34      Emma North Carolina   63.067         1
	  35     Ethan  Massachusetts   31.480         1
	  36     Ethan       New York   95.639         1
	  37     Ethan North Carolina    6.561         1
	  38     Ethan          Texas   29.963         1
	  39     Ethan     Washington   44.187         2
	  40 Genevieve  Massachusetts   90.783         1
	  41     Grace       New York   18.232         1
	  42     Grace North Carolina    5.355         2
	  43     Grace     Washington   91.084         1
	  44  Isabella       New York    4.115         1
	  45  Isabella North Carolina   12.942         2
	  46  Isabella          Texas    2.227         2
	  47      Jack     California   40.905         1
	  48      Jack  Massachusetts   98.080         2
	  49      Jack       New York    8.071         2
	  50      Jack North Carolina   11.304         3
	  51      Jack          Texas   18.795         2
	  52    Jayden  Massachusetts   83.949         1
	  53    Jayden North Carolina   67.769         1
	  54    Jayden     Washington    4.360         1
	  55      Liam     California    3.300         1
	  56      Liam  Massachusetts   87.585         1
	  57      Liam       New York   96.599         1
	  58      Liam North Carolina   32.997         1
	  59    Lolita     California   18.102         1
	  60    Lolita     Washington   30.649         2
	  61       Max  Massachusetts   21.683         2
	  62       Max       New York   14.852         2
	  63       Max North Carolina   79.982         2
	  64       Max          Texas   66.749         2
	  65       Max     Washington   67.326         2
	  66     Riley       New York   20.527         2
	  

Removing duplicates can be a useful way to reduce the size of a data set without losing information of interest. For example, consider an analysis of using data from the sample *AirlineDemoSmall* xdf file. It has 600,000 observations and contains the variables *DayOfWeek*, *CRSDepTime*, and *ArrDelay*. We can create a smaller data set reducing the number of observations, and adding a variable that contains the frequency of the duplicated observation.

	sampleDataDir <- rxGetOption("sampleDataDir")
	airDemo <- file.path(sampleDataDir, "AirlineDemoSmall.xdf")
	airDedup <- file.path(tempdir(), "rxAirDedup.xdf")
	rxSort(inData = airDemo, outFile = airDedup, 
		sortByVars =  c("DayOfWeek", "CRSDepTime", "ArrDelay"),
	  	removeDupKeys = TRUE, dupFreqVar = "FreqWt")
	rxGetInfo(airDedup)

The new data file contains about 1/3 of the observations and one additional variable:

	File name: C:\YourTempDir\rxAirDedup.xdf 
	Number of observations: 232451 
	Number of variables: 4 
	Number of blocks: 2
	Compression type: zlib

By using the frequency weights argument, we can use many of the RevoScaleR analysis functions on this smaller data set and get same results as we would using the full data set. For example, a linear model for Arrival Delay can be specified as follows, using the *fweights* argument:

	linModObj <- rxLinMod(ArrDelay~CRSDepTime + DayOfWeek, data = airDedup, 
		fweights = "FreqWt")
	summary(linModObj)
	 
	 Call:
	 rxLinMod(formula = ArrDelay ~ CRSDepTime + DayOfWeek, data = airDedup, 
	     fweights = "FreqWt")
	 
	 Linear Regression Results for: ArrDelay ~ CRSDepTime + DayOfWeek
	 File name: C:\YourTempDir\rxAirDedup.xdf
	 Frequency weights: FreqWt
	 Dependent variable(s): ArrDelay
	 Total independent variables: 9 (Including number dropped: 1)
	 Sum of weights of valid observations: 582628
	 Number of missing observations: 3503 
	  
	 Coefficients: (1 not defined because of singularities)
	                     Estimate Std. Error t value Pr(>|t|)    
	 (Intercept)         -3.19458    0.20413 -15.650 2.22e-16 ***
	 CRSDepTime           0.97862    0.01126  86.948 2.22e-16 ***
	 DayOfWeek=Monday     2.08100    0.18602  11.187 2.22e-16 ***
	 DayOfWeek=Tuesday    1.34015    0.19881   6.741 1.58e-11 ***
	 DayOfWeek=Wednesday  0.15155    0.19679   0.770    0.441    
	 DayOfWeek=Thursday  -1.32301    0.19518  -6.778 1.22e-11 ***
	 DayOfWeek=Friday     4.80042    0.19452  24.679 2.22e-16 ***
	 DayOfWeek=Saturday   2.18965    0.19229  11.387 2.22e-16 ***
	 DayOfWeek=Sunday     Dropped    Dropped Dropped  Dropped    
	 ---
	 Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1 
	 
	 Residual standard error: 40.39 on 582620 degrees of freedom
	 Multiple R-squared: 0.01465 
	 Adjusted R-squared: 0.01464 
	 F-statistic:  1238 on 7 and 582620 DF,  p-value: < 2.2e-16 
	 Condition number: 10.6542
	 
Using the full data set, we get the following results:

	linModObjBig <- rxLinMod(ArrDelay~CRSDepTime + DayOfWeek, data = airDemo)
	summary(linModObjBig)
	  
	  Call:
	  rxLinMod(formula = ArrDelay ~ CRSDepTime + DayOfWeek, data = airDemo)
	  
	  Linear Regression Results for: ArrDelay ~ CRSDepTime + DayOfWeek
	  File name:    C:\YourSampleDir\AirlineDemoSmall.xdf
	  Dependent variable(s): ArrDelay
	  Total independent variables: 9 (Including number dropped: 1)
	  Number of valid observations: 582628
	  Number of missing observations: 17372 
	   
	  Coefficients: (1 not defined because of singularities)
	                      Estimate Std. Error t value Pr(>|t|)    
	  (Intercept)         -3.19458    0.20413 -15.650 2.22e-16 ***
	  CRSDepTime           0.97862    0.01126  86.948 2.22e-16 ***
	  DayOfWeek=Monday     2.08100    0.18602  11.187 2.22e-16 ***
	  DayOfWeek=Tuesday    1.34015    0.19881   6.741 1.58e-11 ***
	  DayOfWeek=Wednesday  0.15155    0.19679   0.770    0.441    
	  DayOfWeek=Thursday  -1.32301    0.19518  -6.778 1.22e-11 ***
	  DayOfWeek=Friday     4.80042    0.19452  24.679 2.22e-16 ***
	  DayOfWeek=Saturday   2.18965    0.19229  11.387 2.22e-16 ***
	  DayOfWeek=Sunday     Dropped    Dropped Dropped  Dropped    
	  ---
	  Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1 
	  
	  Residual standard error: 40.39 on 582620 degrees of freedom
	  Multiple R-squared: 0.01465 
	  Adjusted R-squared: 0.01464 
	  F-statistic:  1238 on 7 and 582620 DF,  p-value: < 2.2e-16 
	  Condition number: 10.6542
	  
## The rxQuantile Function and the Five-Number Summary

Sorting data is, in the general case, a prerequisite to finding exact quantiles, including medians. However, it is possible to compute approximate quantiles by counting binned data then computing a linear interpolation of the empirical cdf. If the data are integers, or can be converted to integers by exact multiplication, and integral bins are used, the computation is exact. The RevoScaleR function rxQuantile does this computation, and by default returns an approximate five-number summary:

	#  The rxQuantile Function and the Five-Number Summary
	
	readPath <- rxGetOption("sampleDataDir")
	AirlinePath <- file.path(readPath, "AirlineDemoSmall.xdf")
	rxQuantile("ArrDelay", AirlinePath)
	 Rows Processed: 600000 
	  0%  25%  50%  75% 100% 
	 -86   -9    0   16 1490

## See Also

 [Introduction to R Server](../what-is-microsoft-r-server.md) 
 [Install R Server on Windows](../install/r-server-install-windows.md)  
 [Install R Server on Linux](../install/r-server-install-linux-server.md)  
 [Install R Server on Hadoop](../install/r-server-install-hadoop.md)