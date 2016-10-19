---

# required metadata
title: "Example: Analyzing airline flight departure data using ScaleR in Microsoft R"
description: "Learn the ScaleR functions found in Microsoft R Client and Microsoft R Server using this tutorial walkthrough."
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

# Example: Analyzing airline flight data with ScaleR (Microsoft R)

This tutorial show you how to get started with ScaleR, a collection of proprietary functions in Microsoft R Client and R Server that are used for practicing data science at scale. Functions can be loosely categorized as data-oriented or platform-specific to tap into capabilities of a particular database system, operating system, or distributed file system.

## What you will learn in this tutorial

This tutorial focuses on data-oriented functions. We use only platform-agnostic functions to minimize the dependencies. You will need a development tool such as **R Tools for Visual Studio** and ScaleR via Microsoft R Client, R Server, or R in Azure.

Tasks you'll perform include the following:

1.	Convert text data to the .xdf data file format.

2.	Summarize your data.

3.	Fit a linear model to the data.

4.	Create a new .xdf file, sub-setting the original data set and performing data transformations.

5.	Compute crosstabs.

6.	Fit a logistic regression model.

7.	Compute predicted values.

## Get the airline data set

*AirlineDemoSmall.csv* is the dataset used in this tutorial. It is a subset of a data set containing information on flight arrival and departure details for all commercial flights within the USA, from October 1987 to April 2008. The *AirlineDemoSmall.csv* file contains three columns of data: two numeric columns, *ArrDelay* and *CRSDepTime*, and a column of strings, *DayOfWeek*. The file contains 600,000 rows of data in addition to a first row with variable names.

## Import text data into .xdf

ScaleR provides a data file format (.xdf) designed to be very efficient for reading arbitrary rows and columns. To convert the *AirlineDemoSmall.csv* text file into the .xdf data format, use the function *rxImport*. Using this function, you can convert the string column, *DayOfWeek*, to a factor variable.

1. Enter the following command to create a variable named `sampleDataDir` representing the input file in the sample data directory. The location of this directory is stored as an option. It is initialized to the location of the *SampleData* directory included in the **RevoScaleR** package. You can use the **rxGetOption** function to retrieve this location and store it as `sampleDataDir`.

    ~~~~
    sampleDataDir <- rxGetOption("sampleDataDir")
    ~~~~

2. The *rxImport* function uses the current working directory to store the imported data. To see this location, use the R function *getwd()*. The output will be something like `C:/Users/<your-name>/documents/visual studio 2015/projects/<folder-name>/<file-name>`.

	~~~~
    getwd()
    ~~~~

3. Enter the following script to set the input file and use *rxImport* to import the text file into an .xdf file named *ADS* in your current working directory.

    ~~~~
	inputFile <- file.path(sampleDataDir, "AirlineDemoSmall.csv")

	airDS <- rxImport(inData = inputFile, outFile = "ADS.xdf",
		missingValueString = "M", stringsAsFactors = TRUE)
    ~~~~

   The input .csv file uses the letter M to represent missing values, rather than the default NA, so we specify this with the *missingValueString* argument.

   Setting *stringsAsFactors* to TRUE will set the levels specification for *DayOfWeek* to the unique strings found in that variable, listed in the order in which they are encountered. Since this order is arbitrary, and can easily vary from data set to data set, it is preferred to explicitly specify the levels in the desired order using the *colInfo* argument.

   Let's rerun the import operation with a fixed order for days of the week. When rerunning an import command, append the *overwrite* argument to replace the file previously imported:

    ~~~~
	colInfo <- list(DayOfWeek = list(type = "factor",
	    levels = c("Monday", "Tuesday", "Wednesday", "Thursday",
	    "Friday", "Saturday", "Sunday")))

	airDS <- rxImport(inData = inputFile, outFile = "ADS.xdf",
		missingValueString = "M", colInfo  = colInfo, overwrite = TRUE)
        ~~~~

Notice that once we supply the *colInfo* argument, we no longer need to specify *stringsAsFactors*; *DayOfWeek* is our only factor variable.

## Examining .xdf files

Using the small *airDS* object representing the ADS.xdf file, you can apply standard R methods to get basic information about the data set:

~~~~
	nrow(airDS)
	ncol(airDS)
	head(airDS)

	> nrow(airDS)
	[1] 6e+05
	> ncol(airDS)
	[1] 3
	> head(airDS)
	  ArrDelay CRSDepTime DayOfWeek
	1        6   9.666666    Monday
	2       -8  19.916666    Monday
	3       -2  13.750000    Monday
	4        1  11.750000    Monday
	5       -2   6.416667    Monday
	6      -14  13.833333    Monday
~~~~

The *rxGetVarInfo* function provides additional variable information:

~~~~
	rxGetVarInfo(airDS)

	Var 1: ArrDelay, Type: integer, Low/High: (-86, 1490)
	Var 2: CRSDepTime, Type: numeric, Storage: float32, Low/High: (0.0167, 23.9833)
	Var 3: DayOfWeek
	       7 factor levels: Monday Tuesday Wednesday Thursday Friday Saturday Sunday
~~~~

You can also read an arbitrary chunk of the data set into a data frame for further examination. For example, read 10 rows into a data frame starting with the 100,000th row:

~~~~
	myData <- rxReadXdf(airDS, numRows=10, startRow=100000)
	myData
~~~~

This code should generate the following output:

~~~~
	   ArrDelay CRSDepTime DayOfWeek
	1        -2  11.416667  Saturday
	2        39   9.916667    Friday
	3        NA  10.033334    Monday
	4         1  17.000000    Friday
	5       -17   9.983334 Wednesday
	6         8  21.250000  Saturday
	7        -9   6.250000    Friday
	8       -11  15.000000    Friday
	9         4  20.916666    Sunday
	10       -8   6.500000  Thursday
~~~~

You can now look to see what the factor levels are for the *DayOfWeek* variable:

~~~~
	levels(myData$DayOfWeek)
~~~~

This command should generate the following output:

~~~~
	[1] "Monday"    "Tuesday"   "Wednesday" "Thursday"  "Friday"    "Saturday"
	[7] "Sunday"
~~~~

## Summarizing data

Use the *rxSummary* function to obtain descriptive statistics for your .xdf data file. The *rxSummary* function takes a formula as its first argument, and the name of the data set as the second. To get summary statistics for all of the data in your data file, you can alternatively use the *summary* method for the *airDS* object.

~~~~
	adsSummary <- rxSummary(~ArrDelay+CRSDepTime+DayOfWeek, data = airDS)
	adsSummary

or

	adsSummary <- summary( airDS )
	adsSummary
~~~~

Summary statistics will be computed on the variables in the formula. By default, *rxSummary* computes data summaries term-by-term, and missing values are omitted on a term-by-term basis. In earlier versions, summaries were computed on the complete table after observations with missing elements were omitted.

~~~~
	Call:
	rxSummary(formula = ~ArrDelay + CRSDepTime + DayOfWeek, data = airDS)

	Summary Statistics Results for: ~ArrDelay + CRSDepTime + DayOfWeek
    Data: airDS (RxXdfData Data Source)
	File name: C:\YourWorkingDir\ADS.xdf
	Number of valid observations: 6e+05

	 Name       Mean     StdDev    Min        Max        ValidObs MissingObs
	 ArrDelay   11.31794 40.688536 -86.000000 1490.00000 582628   17372     
	 CRSDepTime 13.48227  4.697566   0.016667   23.98333 600000       0     

	Category Counts for DayOfWeek
	Number of categories: 7
	Number of valid observations: 6e+05
	Number of missing observations: 0

	 DayOfWeek Counts
	 Monday    97975
	 Tuesday   77725
	 Wednesday 78875
	 Thursday  81304
	 Friday    82987
	 Saturday  86159
	 Sunday    94975
~~~~

Notice that the summary information shows cell counts for categorical variables, and appropriately does not provide summary statistics such as *Mean* and *StdDev*. Also notice that the *Call:* line will show the actual call you entered or the call provided by *summary*, so will appear differently in different circumstances.

You can also compute summary information by one or more categories by using interactions of a numeric variable with a factor variable.  For example, to compute summary statistics on Arrival Delay by Day of Week:

~~~~
	rxSummary(~ArrDelay:DayOfWeek, data = airDS)
~~~~

The output shows the summary statistics for *ArrDelay* for each day of the week:

~~~~
	Call:
	rxSummary(formula = ~ArrDelay:DayOfWeek, data = airDS)

	Summary Statistics Results for: ~ArrDelay:DayOfWeek
	File name: C:\YourWorkingDir\ADS.xdf
	Number of valid observations: 6e+05

	 Name               Mean     StdDev   Min Max  ValidObs MissingObs
	 ArrDelay:DayOfWeek 11.31794 40.68854 -86 1490 582628   17372     

	Statistics by category (7 categories):

	 Category                         DayOfWeek Means    StdDev  Min Max ValidObs
	 ArrDelay for DayOfWeek=Monday    Monday    12.025604 40.02463 -76 1017 95298   
	 ArrDelay for DayOfWeek=Tuesday   Tuesday   11.293808 43.66269 -70 1143 74011   
	 ArrDelay for DayOfWeek=Wednesday Wednesday 10.156539 39.58803 -81 1166 76786   
	 ArrDelay for DayOfWeek=Thursday  Thursday   8.658007 36.74724 -58 1053 79145   
	 ArrDelay for DayOfWeek=Friday    Friday    14.804335 41.79260 -78 1490 80142   
	 ArrDelay for DayOfWeek=Saturday  Saturday  11.875326 45.24540 -73 1370 83851   
	 ArrDelay for DayOfWeek=Sunday    Sunday    10.331806 37.33348 -86 1202 93395
~~~~

To get a better feel for the data, we can draw histograms for each variable. Run each *rxHistogram* function one at a time. In RTVS, the histogram is rendered in the R Plot pane.

~~~~
	options("device.ask.default" = T)
	rxHistogram(~ArrDelay, data = airDS)
	rxHistogram(~CRSDepTime, data = airDS)
	rxHistogram(~DayOfWeek, data = airDS)
~~~~

![ArrDelay Histogram](media/rserver-scaler-getting-started/arrdelay_histogram_1.png)

![CRSDepTime Histogram](media/rserver-scaler-getting-started/crsdeptime_histogram.png)

![DayOfWeek Histogram](media/rserver-scaler-getting-started/dayofweek_histogram.png)

We can also easily extract a subsample of the data file into a data frame in memory. For example, we can look at just the flights that were between 4 and 5 hours late:

~~~~
	myData <- rxDataStep(inData = airDS,
	rowSelection = ArrDelay > 240 & ArrDelay <= 300,
		varsToKeep = c("ArrDelay", "DayOfWeek"))
	rxHistogram(~ArrDelay, data = myData)
~~~~

![ArrDelay Histogram](media/rserver-scaler-getting-started/arrdelay_histogram_2.png)

## Fitting a linear model

This section of the tutorial demonstrates several approaches for fitting a model to the sample data.

### Fitting a Simple Model

Use the *rxLinMod* function to fit a linear model using your *ADS.xdf* file. Use a single dependent variable, the factor *DayOfWeek*:

~~~~
	arrDelayLm1 <- rxLinMod(ArrDelay ~ DayOfWeek, data = airDS)
	summary(arrDelayLm1)
~~~~

The resulting output is:

~~~~
	Call:
	rxLinMod(formula = ArrDelay ~ DayOfWeek, data = airDS)

	Linear Regression Results for: ArrDelay ~ DayOfWeek
	File name: C:\YourWorkingDir\ADS.xdf
	Dependent variable(s): ArrDelay
	Total independent variables: 8 (Including number dropped: 1)
	Number of valid observations: 582628
	Number of missing observations: 17372

	Coefficients: (1 not defined because of singularities)
	                    Estimate Std. Error t value Pr(>|t|)    
	(Intercept)          10.3318     0.1330  77.673 2.22e-16 ***
	DayOfWeek=Monday      1.6938     0.1872   9.049 2.22e-16 ***
	DayOfWeek=Tuesday     0.9620     0.2001   4.809 1.52e-06 ***
	DayOfWeek=Wednesday  -0.1753     0.1980  -0.885    0.376    
	DayOfWeek=Thursday   -1.6738     0.1964  -8.522 2.22e-16 ***
	DayOfWeek=Friday      4.4725     0.1957  22.850 2.22e-16 ***
	DayOfWeek=Saturday    1.5435     0.1934   7.981 2.22e-16 ***
	DayOfWeek=Sunday     Dropped    Dropped Dropped  Dropped    
	---
	Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

	Residual standard error: 40.65 on 582621 degrees of freedom
	Multiple R-squared: 0.001869
	Adjusted R-squared: 0.001858
	F-statistic: 181.8 on 6 and 582621 DF,  p-value: < 2.2e-16
	Condition number: 10.5595
~~~~

### Using the cube argument and plotting results

If you are using categorical data in your regression, you can use the *cube* argument. If *cube* is set to TRUE and the first term of the regression is categorical (a factor or an interaction of factors), the regression is done using a partitioned inverse, which may be faster and use less memory than standard regression computation. Averages/counts of the category "bins" can be computed in addition to standard regression results. The intercept will also be automatically dropped, so that each category level will have an estimated coefficient (unlike the previous example). If *cube* is set to TRUE and the first term is not categorical, you get an error message.

Re-estimate the linear model, this time setting cube to TRUE. Then print a summary of the results:

~~~~
	arrDelayLm2 <- rxLinMod(ArrDelay ~ DayOfWeek, data = airDS,
	cube = TRUE)
	summary(arrDelayLm2)
~~~~

You should see the following output:

~~~~
	Call:
	rxLinMod(formula = ArrDelay ~ DayOfWeek, data = airDS, cube = TRUE)

	Cube Linear Regression Results for: ArrDelay ~ DayOfWeek
	File name: C:\YourWorkingDir\ADS.xdf
	Dependent variable(s): ArrDelay
	Total independent variables: 7
	Number of valid observations: 582628
	Number of missing observations: 17372

	Coefficients:
	                    Estimate Std. Error t value Pr(>|t|)     | Counts
	DayOfWeek=Monday     12.0256     0.1317   91.32 2.22e-16 *** |  95298
	DayOfWeek=Tuesday    11.2938     0.1494   75.58 2.22e-16 *** |  74011
	DayOfWeek=Wednesday  10.1565     0.1467   69.23 2.22e-16 *** |  76786
	DayOfWeek=Thursday    8.6580     0.1445   59.92 2.22e-16 *** |  79145
	DayOfWeek=Friday     14.8043     0.1436  103.10 2.22e-16 *** |  80142
	DayOfWeek=Saturday   11.8753     0.1404   84.59 2.22e-16 *** |  83851
	DayOfWeek=Sunday     10.3318     0.1330   77.67 2.22e-16 *** |  93395
	---
	Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

	Residual standard error: 40.65 on 582621 degrees of freedom
	Multiple R-squared: 0.001869 (as if intercept included)
	Adjusted R-squared: 0.001858
	F-statistic: 181.8 on 6 and 582621 DF,  p-value: < 2.2e-16
	Condition number: 1
~~~~

The coefficient estimates are the mean arrival delay for each day of the week.

When the cube argument is set to TRUE and the model has only one term as an independent variable, the "countDF" component of the results object also contains category means. This data frame can be extracted using the *rxResultsDF* function, and is particularly convenient for plotting.

~~~~
	countsDF <- rxResultsDF(arrDelayLm2, type = "counts")
	countsDF
~~~~

You should see the following output:

~~~~
	  DayOfWeek  ArrDelay Counts
	1    Monday 12.025604  95298
	2   Tuesday 11.293808  74011
	3 Wednesday 10.156539  76786
	4  Thursday  8.658007  79145
	5    Friday 14.804335  80142
	6  Saturday 11.875326  83851
	7    Sunday 10.331806  93395
~~~~

Now plot the average arrival delay for each day of the week:

~~~~
	rxLinePlot(ArrDelay~DayOfWeek, data = countsDF,
	    main = "Average Arrival Delay by Day of Week")
~~~~

The following plot is generated, showing the lowest average arrival delay on Thursdays:

![DayOfWeek Plot](media/rserver-scaler-getting-started/dayofweek_plot.png)

### A Linear Model with Multiple Independent Variables

We can run a more complex model examining the dependency of arrival delay on both day of week and the departure time. We’ll estimate the model using the *F* expression to have the *CRSDepTime* variable interpreted as a categorical or factor variable.

*F()* is not an R function, although it looks like one when used inside ScaleR formulas. An *F* expression tells ScaleR to create a factor by creating one level for each integer in the range *(floor(min(x)), floor(max(x)))* and binning all the observations into the resulting set of levels. You can look up the *rxFormula* [package help](#get-help) for more information.

By interacting *DayOfWeek* with *F(CRSDepTime)* we are creating a dummy variable for every combination of departure hour and day of the week.

~~~~
	arrDelayLm3 <- rxLinMod(ArrDelay ~ DayOfWeek:F(CRSDepTime),
	data = airDS, cube = TRUE)
	arrDelayDT <- rxResultsDF(arrDelayLm3, type = "counts")
	head(arrDelayDT, 15)
~~~~

The output shows the first fifteen rows of the counts data frame.  The variable *CRSDepTime* gives the hour of the departure time and *ArrDelay* shows the average departure delay for that hour for that day of week. Counts gives the number of observations that contain that combination of day of week and departure hour.

~~~~
	   DayOfWeek CRSDepTime   ArrDelay Counts
	1     Monday          0  7.4360902    133
	2    Tuesday          0  7.0000000    107
	3  Wednesday          0  3.7857143     98
	4   Thursday          0  3.0097087    103
	5     Friday          0  4.4752475    101
	6   Saturday          0  4.5460993    141
	7     Sunday          0  0.9243697    119
	8     Monday          1  2.6590909     44
	9    Tuesday          1  5.5428571     35
	10 Wednesday          1 -5.9696970     33
	11  Thursday          1  5.1714286     35
	12    Friday          1 -4.9062500     32
	13  Saturday          1 -4.2727273     44
	14    Sunday          1 -1.7368421     38
	15    Monday          2 -0.2000000     15
~~~~

Now plot the results:

~~~~
	rxLinePlot( ArrDelay~CRSDepTime|DayOfWeek, data = arrDelayDT,
	    title = "Average Arrival Delay by Day of Week by Departure Hour")
~~~~

You should see the following plot:

![CRSDepTime Plot](media/rserver-scaler-getting-started/crsdeptime_plot.png)

## Subset the data and compute a crosstab

Create a new data set containing a subset of rows and variables.  This is convenient if you intend to do lots of analysis on a subset of a large data set. To do this, we use the *rxDataStep* function with the following arguments:

- *outFile:* the name of the new data set
- *inData:*  the name of an .xdf file or an RxXdfData object representing the original data set you are subsetting
- *varsToDrop:* a character vector of variables to drop from the new data set, here CRSDepTime  
- *rowSelection:* keep only the rows where the flight was more than 15 minutes late

The resulting call is as follows:

~~~~
	airLateDS <- rxDataStep(inData = airDS, outFile = "ADS1.xdf",
	    varsToDrop = c("CRSDepTime"),
	    rowSelection = ArrDelay > 15)
	ncol(airLateDS)
	nrow(airLateDS)
~~~~

You will see that the new data set has only two variables and has dropped from 600,000 to 148,526 observations.

Compute a crosstab showing the mean arrival delay by day of week.

~~~~
	myTab <- rxCrossTabs(ArrDelay~DayOfWeek, data = airLateDS)
	summary(myTab, output = "means")
~~~~

The results show that in this data set “late” flights are on average over 10 minutes later on Tuesdays than on Sundays:

~~~~
	Call:
	rxCrossTabs(formula = ArrDelay ~ DayOfWeek, data = airLateDS)

	Cross Tabulation Results for: ArrDelay ~ DayOfWeek
	File name: C:\YourWorkingDir\ADS1.xdf
	Dependent variable(s): ArrDelay
	Number of valid observations: 148526
	Number of missing observations: 0
	Statistic: means

	ArrDelay (means):
	             means  means %
	Monday    56.94491 13.96327
	Tuesday   64.28248 15.76249
	Wednesday 60.12724 14.74360
	Thursday  55.07093 13.50376
	Friday    56.11783 13.76047
	Saturday  61.92247 15.18380
	Sunday    53.35339 13.08261
	Col Mean  57.96692   
~~~~

## Create a new data set with variable transformations

Now use the *rxDataStep* function to create a new data set containing the variables in *ADS.xdf* plus additional variables created through transformations. Typically additional variables are created using the *transforms* argument. See the [ScaleR User's Guide](scaler-user-guide-introduction.md) for information on doing more complex transformations using a transform function. Remember that all expressions used in *transforms* must be able to be processed on a chunk of data at a time.

In the example below, three new variables are created. The variable *Late* is a logical variable set to *TRUE* (or *1*) if the flight was more than 15 minutes late in arriving. The variable *DepHour* is an integer variable indicating the hour of the departure. The variable *Night* is also a logical variable, set to *TRUE* (or *1*) if the flight departed between 10 P.M. and 5 A.M.

The *rxDataStep* function will read the existing data set and perform the transformations chunk by chunk, and create a new data set.

~~~~
	airExtraDS <- rxDataStep(inData = airDS, outFile="ADS2.xdf",
		transforms=list(
			Late = ArrDelay > 15,
			DepHour = as.integer(CRSDepTime),
			Night = DepHour >= 20 | DepHour <= 5))

	rxGetInfo(airExtraDS, getVarInfo=TRUE, numRows=5)
~~~~

You should see the following information in your output:

~~~~
	File name: C:\YourWorkingDir\ADS2.xdf
	Number of observations: 6e+05
	Number of variables: 6
	Number of blocks: 2
	Compression type: zlib
	Variable information:
	Var 1: ArrDelay, Type: integer, Low/High: (-86, 1490)
	Var 2: CRSDepTime, Type: numeric, Storage: float32, Low/High: (0.0167, 23.9833)
	Var 3: DayOfWeek
	       7 factor levels: Monday Tuesday Wednesday Thursday Friday Saturday Sunday
	Var 4: Late, Type: logical, Low/High: (0, 1)
	Var 5: DepHour, Type: integer, Low/High: (0, 23)
	Var 6: Night, Type: logical, Low/High: (0, 1)
	Data (5 rows starting with row 1):
	  ArrDelay CRSDepTime DayOfWeek  Late DepHour Night
	1        6   9.666666    Monday FALSE       9 FALSE
	2       -8  19.916666    Monday FALSE      19 FALSE
	3       -2  13.750000    Monday FALSE      13 FALSE
	4        1  11.750000    Monday FALSE      11 FALSE
	5       -2   6.416667    Monday FALSE       6 FALSE
~~~~

## Run a logistic regression on the new data

The function *rxLogit* takes a binary dependent variable. Here we will use the variable *Late*, which is *TRUE* (or *1*) if the plane was more than 15 minutes late arriving. For dependent variables we will use the *DepHour*, the departure hour, and *Night*, indicating whether or not the flight departed at night.

~~~~
	logitObj <- rxLogit(Late~DepHour + Night, data = airExtraDS)
	summary(logitObj)
~~~~

You should see the following results:

~~~~
	Call:
	rxLogit(formula = Late ~ DepHour + Night, data = airExtraDS)

	Logistic Regression Results for: Late ~ DepHour + Night
	File name: C:\Users\sue\SVN\bigAnalytics\trunk\revoAnalytics\ADS2.xdf
	Dependent variable(s): Late
	Total independent variables: 3
	Number of valid observations: 582628
	Number of missing observations: 17372
	-2*LogLikelihood: 649607.8613 (Residual deviance on 582625 degrees of freedom)

	Coefficients:
	              Estimate Std. Error z value Pr(>|z|)    
	(Intercept) -2.0990076  0.0104460 -200.94 2.22e-16 ***
	DepHour      0.0790215  0.0007671  103.01 2.22e-16 ***
	Night       -0.3027030  0.0109914  -27.54 2.22e-16 ***
	---
	Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

	Condition number of final variance-covariance matrix: 3.0178
	Number of iterations: 4
~~~~

## Computing predicted values

You can use the object returned from the call to *rxLogit* in the previous section to compute predicted values. In addition to the model object, we specify the data set on which to compute the predicted values and the data set in which to put the newly computed predicted values. In the call below, we use the same dataset for both. In general, the data set on which to compute the predicted values must be similar to the original data set used to estimate the model in the following ways; it should have the same variable names and types, and factor variables must have the same levels in the same order.

~~~~
	predictDS <- rxPredict(modelObject = logitObj, data = airExtraDS,
		outData = airExtraDS)
	rxGetInfo(predictDS, getVarInfo=TRUE, numRows=5)
~~~~

You should see the following information:

~~~~
	File name: C:\YourWorkingDir\ADS2.xdf
	Number of observations: 6e+05
	Number of variables: 7
	Number of blocks: 2
	Compression type: zlib
	Variable information:
	Var 1: ArrDelay, Type: integer, Low/High: (-86, 1490)
	Var 2: CRSDepTime, Type: numeric, Storage: float32, Low/High: (0.0167, 23.9833)
	Var 3: DayOfWeek
	       7 factor levels: Monday Tuesday Wednesday Thursday Friday Saturday Sunday
	Var 4: Late, Type: logical, Low/High: (0, 1)
	Var 5: DepHour, Type: integer, Low/High: (0, 23)
	Var 6: Night, Type: logical, Low/High: (0, 1)
	Var 7: Late_Pred, Type: numeric, Low/High: (0.0830, 0.3580)
	Data (5 rows starting with row 1):
	  ArrDelay CRSDepTime DayOfWeek  Late DepHour Night Late_Pred
	1        6   9.666666    Monday FALSE       9 FALSE 0.1997569
	2       -8  19.916666    Monday FALSE      19 FALSE 0.3548931
	3       -2  13.750000    Monday FALSE      13 FALSE 0.2550745
	4        1  11.750000    Monday FALSE      11 FALSE 0.2262214
	5       -2   6.416667    Monday FALSE       6 FALSE 0.1645331
~~~~

## Next steps

This tutorial demonstrated how to use several important functions, but on a small data set. Next up are two additional tutorials that explore ScaleR with bigger data sets, and customization approaches if built-in functions are not quite enough.

 - [Analyze large data with ScaleR](scaler-getting-started-3-analyze-large-data.md)
 - [Write custom chunking algorithms](scaler-getting-started-4-write-chunking-algorithms.md)

### Try demo scripts

 Another way to learn about ScaleR is through demo scripts. Scripts provided in your Microsoft R installation contain code that's very similar to what you see in this tutorial. You can highlight portions of the script, right-click **Execute in Interactive** to run the script in RTVS.

 Demo scripts are located in the *demoScripts* subdirectory of your Microsoft R installation. On Windows, this is typically:

 	`C:\Program Files\Microsoft\R Client\R_SERVER\library\RevoScaleR\demoScripts`

### Watch this video

This 30-minute video is the second in a 4-part video series. It demonstrates ScaleR functions for data ingestion.

 <div align=center><iframe src="https://channel9.msdn.com/Series/Microsoft-R-Server-Series/Introduction-to-Microsoft-R-Server-Session-2--Data-Ingestion/player" width="600" height="400" allowFullScreen frameBorder="0"></iframe></div>

<a name="get-help"></a>
### Get function help

  R packages typically include embedded package help reference and ScaleR is no exception. To view embedded help, use the **R Help** tab, located next to Solution Explorer.

  - In R Help, click the Home button.
  - Click **Packages**.
  - Scroll down and click **RevoScaleR** to open the package help. All ScaleR functions are documented here. A subset of more commonly used functions have [help pages on MSDN](../scaler/scaler.md).

### Get more information

Continue building up your knowledge of ScaleR with these additional guides and tutorials.

- [ScaleR Getting Started with Hadoop](scaler-hadoop-getting-started.md)
- [ScaleR Getting Started with Teradata](scaler-teradata-getting-started.md)
- [ScaleR User’s Guide](scaler-user-guide-introduction.md)
- [ScaleR Distributed Computing Guide](scaler-distributed-computing.md)
- [ScaleR ODBC Data Import Guide](scaler-odbc.md)

## See Also

[Introduction to Microsoft R](microsoft-r-getting-started.md)

[Diving into data analysis in Microsoft R](data-analysis-in-microsoft-r.md)

[RevoScaleR Functions](../scaler/scaler.md)
