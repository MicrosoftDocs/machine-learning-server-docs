---

# required metadata
title: "Cluster classification using RevoScaleR (Machine Learning Server) "
description: "k-means clustering with RevoScaleR."
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "jhubbard"
ms.date: "03/17/2016"
ms.topic: "get-started-article"
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

# Cluster classification in RevoScaleR

*Clustering* is the general name for any of a large number of classification techniques that involve assigning observations to membership in one of two or more clusters on the basis of some distance metric.

### K-means Clustering

K-means clustering is a classification technique that groups observations of numeric data using one of several *iterative relocation* algorithms—that is, starting from some initial classification, which may be random, points are moved from cluster to another so as to minimize sums of squares. In RevoScaleR, the algorithm used is that of Lloyd.

To perform k-means clustering with RevoScaleR, use the *rxKmeans* function.

#### Clustering the Airline Data

As a first example of k-means clustering, we will cluster the arrival delay and scheduled departure time in the airline data 7% subsample. To start, we extract variables of interest into a new working data set to which we are writing additional information:

	#  K-means Clustering

	#   Clustering the Airline Data  
	bigDataDir <- "C:/MRS/Data"
	sampleAirData <- file.path(bigDataDir, "AirOnTime7Pct.xdf")
	rxDataStep(inData = sampleAirData, outFile = "AirlineDataClusterVars.xdf",
	  varsToKeep=c("DayOfWeek", "ArrDelay", "CRSDepTime", "DepDelay"))

We specify the variables to cluster as a formula, and specify the number of clusters we’d like. Initial centers for these clusters are then chosen at random.

	kclusts1 <- rxKmeans(formula= ~ArrDelay + CRSDepTime, 
		data = "AirlineDataClusterVars.xdf",
		seed = 10,
		outFile = "airlineDataClusterVars.xdf", numClusters=5)
	kclusts1

This produces the following output (because the initial centers are chosen at random, your output will probably look different):

	Call:
	rxKmeans(formula = ~ArrDelay + CRSDepTime, data = "AirlineDataClusterVars.xdf", 
	    outFile = "AirlineDataClusterVars.xdf", numClusters = 5)
	
	Data: "AirlineDataClusterVars.xdf"
	Number of valid observations: 10186272
	Number of missing observations: 213483 
	Clustering algorithm:  
	 
	K-means clustering with 5 clusters of sizes 922985, 38192, 4772791, 261779, 4190525
	
	Cluster means:
	    ArrDelay CRSDepTime
	1  45.258179   14.86596
	2 275.363820   14.81432
	3 -10.284426   13.08375
	4 118.365205   15.52079
	5   7.803893   13.53811
	
	Within cluster sum of squares by cluster:
	        1         2         3         4         5 
	223220709 501736748 354763376 233533349 312403604 
	
	Available components:
	 [1] "centers"       "size"          "withinss"      "valid.obs"    
	 [5] "missing.obs"   "numIterations" "tot.withinss"  "totss"        
	 [9] "betweenss"     "cluster"       "params"        "formula"      
	[13] "call"     


The value returned by *rxKmeans* is a list similar to the list returned by the standard R kmeans function. The printed output shows a subset of this information, including the number of valid and missing observations, the cluster sizes, the cluster centers, and the within-cluster sums of squares.

The cluster membership component is returned if the input is a data frame, but if the input is a .xdf file, cluster membership is returned only if *outFile* is specified, in which case it is returned not as part of the return object, but as a column in the specified file. In our example, we specified an *outFile*, and we see the cluster membership variable when we look at the file with *rxGetInfo*:

	rxGetInfo("AirlineDataClusterVars.xdf", getVarInfo=TRUE)
	 File name: AirlineDataClusterVars.xdf 
	 Number of observations: 10399755 
	 Number of variables: 5 
	 Number of blocks: 19 
	 Compression type: zlib 
	 Variable information: 
	 Var 1: DayOfWeek
	        7 factor levels: Mon Tues Wed Thur Fri Sat Sun
	 Var 2: ArrDelay, Type: integer, Low/High: (-1233, 2453)
	 Var 3: CRSDepTime, Type: numeric, Storage: float32, Low/High: (0.0000, 24.0000)
	 Var 4: DepDelay, Type: integer, Low/High: (-1199, 2467)
	 Var 5: .rxCluster, Type: integer, Low/High: (1, 5)

#### Using the Cluster Membership Information

A common follow-up to clustering is to use the cluster membership information to see whether a given model varies appreciably from cluster to cluster. Since we can use the *rowSelection* argument to extract a single cluster on the fly, there is no need to sort the data first. As an example, we fit our original linear model of ArrDelay by DayOfWeek for two of the clusters:

	#   Using the Cluster Membership Information
	  
	clust1Lm <- rxLinMod(ArrDelay ~ DayOfWeek, "AirlineDataClusterVars.xdf",
		rowSelection = .rxCluste r == 1 )
	clust5Lm <- rxLinMod(ArrDelay ~ DayOfWeek, "AirlineDataClusterVars.xdf", 
		rowSelection = .rxCluster == 5)
	summary(clust1Lm)
	summary(clust5Lm)

Looking at the summary for *clust1Lm* shows the following:

	Call:
	rxLinMod(formula = ArrDelay ~ DayOfWeek, data = "AirlineDataClusterVars.xdf", 
	    rowSelection = .rxCluster == 1)
	
	Linear Regression Results for: ArrDelay ~ DayOfWeek
	File name: AirlineDataClusterVars.xdf
	Dependent variable(s): ArrDelay
	Total independent variables: 8 (Including number dropped: 1)
	Number of valid observations: 922985
	Number of missing observations: 0 
	 
	Coefficients: (1 not defined because of singularities)
	               Estimate Std. Error  t value Pr(>|t|)    
	(Intercept)    45.21591    0.04237 1067.199 2.22e-16 ***
	DayOfWeek=Mon   0.23053    0.05893    3.912 9.16e-05 ***
	DayOfWeek=Tues -0.06496    0.05968   -1.089   0.2764    
	DayOfWeek=Wed   0.10139    0.05869    1.727   0.0841 .  
	DayOfWeek=Thur  0.06098    0.05708    1.068   0.2854    
	DayOfWeek=Fri   0.23222    0.05660    4.103 4.08e-05 ***
	DayOfWeek=Sat  -0.43444    0.06364   -6.827 8.68e-12 ***
	DayOfWeek=Sun   Dropped    Dropped  Dropped  Dropped    
	---
	Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
	
	Residual standard error: 14.89 on 922978 degrees of freedom
	Multiple R-squared: 0.0001705 
	Adjusted R-squared: 0.000164 
	F-statistic: 26.24 on 6 and 922978 DF,  p-value: < 2.2e-16 
	Condition number: 12.8655   

Similarly, the summary for clust5Lm shows the following:

	Call:
	rxLinMod(formula = ArrDelay ~ DayOfWeek, data = "AirlineDataClusterVars.xdf", 
	    rowSelection = .rxCluster == 5)
	
	Linear Regression Results for: ArrDelay ~ DayOfWeek
	File name: AirlineDataClusterVars.xdf
	Dependent variable(s): ArrDelay
	Total independent variables: 8 (Including number dropped: 1)
	Number of valid observations: 4190525
	Number of missing observations: 0 
	 
	Coefficients: (1 not defined because of singularities)
	                Estimate Std. Error t value Pr(>|t|)    
	(Intercept)     7.808093   0.009593 813.960 2.22e-16 ***
	DayOfWeek=Mon  -0.131001   0.013320  -9.835 2.22e-16 ***
	DayOfWeek=Tues -0.228087   0.013374 -17.055 2.22e-16 ***
	DayOfWeek=Wed  -0.035954   0.013292  -2.705  0.00683 ** 
	DayOfWeek=Thur  0.231958   0.013170  17.613 2.22e-16 ***
	DayOfWeek=Fri   0.313961   0.013171  23.838 2.22e-16 ***
	DayOfWeek=Sat  -0.257716   0.014036 -18.361 2.22e-16 ***
	DayOfWeek=Sun    Dropped    Dropped Dropped  Dropped    
	---
	Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
	
	Residual standard error: 7.238 on 4190518 degrees of freedom
	Multiple R-squared: 0.0007911 
	Adjusted R-squared: 0.0007897 
	F-statistic:   553 on 6 and 4190518 DF,  p-value: < 2.2e-16 
	Condition number: 12.0006
