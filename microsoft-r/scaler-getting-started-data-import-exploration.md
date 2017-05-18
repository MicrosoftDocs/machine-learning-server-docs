---

# required metadata
title: "Tutorial: Data import and exploration (Microsoft R)"
description: "Learn how to import and explore data using the RevoScaleR functions in Microsoft R."
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "05/16/2017"
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

# Tutorial: Practice data import and exploration (Microsoft R)

**Applies to: R Server and R Client** <sup>[**1**](#chunking)</sup>

Microsoft R provides a native binary data file format (.xdf) designed for efficient read operations of arbitrary rows and columns. In this tutorial, you will learn how to convert a CSV text file into XDF using **rxImport** from the RevoScaleR function library. The **rxImport** function takes source data as an input and returns a data source object. Optionally, by specifying an *outFile* parameter, it creates an XDF.

## What you will learn in this tutorial

1. Convert CSV data to the XDF data file format
2. Get information about the data source
3. Import parameters used for selective import and conversion
4. Summarize data
4. Subset and transform data

## Prerequisites

To complete this tutorial as written, use an R console application. 

+ On Windows, go to \Program Files\Microsoft\R Client\R_SERVER\bin\x64 and double-click **Rgui.exe**.	
+ On Linux, at the command prompt, type **Revo64**.

The command prompt for a R is `>`. You can hand-type commands line by line, or copy-paste multi-line command. Press Enter to execute the statement.

## Locate sample data

[RevoScaleR](scaler-user-guide-introduction.md) includes both functions and built-in data sets, including the *AirlineDemoSmall.csv* file used in this tutorial. The sample data directory is registered with RevoScaleR. Its location can be returned through a "sampleDataDir" parameter on the **rxGetOption** function.

1. Open the R console or start a Revo64 session.

2. Retrieve the list of [sample data files](scaler-user-guide-sample-data.md) provided in RevoScaleR by entering the following command at the `>` command prompt:

        list.files(rxGetOption("sampleDataDir"))

The open source R `list.files` command returns a file list provided by the RevoScaleR **rxGetOption** function and the **sampleDataDir** argument. 

> [!NOTE]
> If you are a Windows user and new to R, be aware that R script is case-sensitive. If you mistype rxGetOption or sampleDataDir, you will get an error instead of the file list.

## About the airline data set

*AirlineDemoSmall.csv* is the data set used in this tutorial. It is a subset of a data set containing information on flight arrival and departure details for all commercial flights within the USA, from October 1987 to April 2008. The *AirlineDemoSmall.csv* file contains three columns of data: *ArrDelay*, *CRSDepTime*, and *DayOfWeek*. There are a total of 600,000 rows in the data file.

## Check the working directory

This tutorial generates an XDF file. By default, RevoScaleR uses the *working directory* to store files. Depending on your environment, you might not have permission to this directory. Use the following open source R command to learn the working directory location: 

    getwd()

If the output is something like `C:/Program Files/Microsoft/R Server/R_SERVER/bin/x64`, you won't have permission to save files to this location. To change the directory location, use either one of these approaches:

+ Run `setwd("/Users/TEMP")` to change the working directory to a temp directory. 
+ Specify a fully-qualified path whenever a file name is required (for example, `outFile="C:/users/temp/airExample.xdf"`).  

Windows users, please notice the direction of the path delimiter. By default, R script uses forward slashes as path delimiters.

## Simple import

As a first step, let's do a preliminary import using source data and an *outFile*. Once the data source object exists, we can examine metadata that could lead to further transformations on successive imports.

The following commands specify a source .csv file, create an .xdf file, and return an object (airXdfData) representing the data source. 

~~~~
	mysource <- file.path(rxGetOption("sampleDataDir"), "AirlineDemoSmall.csv")
	airXdfData <- rxImport(inData=mysource, outFile = "airExample.xdf")
~~~~

On the first line,`mysource` is an object specifying source data. As you can see, the object is created using R's `file.path` command, where the folder path is provided through RevoScaleR's rxGetOption("sampleDataDir"), with "AirlineDemoSmall.csv" for the file. The *sampleDataDir* is an argument that returns the directory containing sample files registered with RevoScaleR. 

On line two, `airXdfData` is a data source object returned by **rxImport**. The **rxImport** function takes several arguments, including *outFile* for the XDF file name. If you omit the *outFile* argument or set it NULL, the return *airXdfData* object is a data frame containing the data. 

> [!Tip]
> XDF files are not strictly required for statistical analysis and data mining, but when data sets are large or complex, an XDF can help by compressing data and by allocating data into chunks that can be read and refreshed independently. Additionally, XDF provides column storage for variables, which is more efficient if you want to selectively choose variables to include in a particular analysis.

## Examine object metadata

Assuming you ran the previous two commands, you now have a `mysource` object and an `airXdfData` object representing the data source. As a preliminary step, use the **rxGetInfo** function to return metadata and learn more about the structure. Adding the  *getVarInfo* argument returns metadata about variables:

~~~~
    rxGetInfo(airXdfData, getVarInfo = TRUE)
~~~~

Output includes the precomputed metadata for each variable, plus a summary of observations, variables, blocks, and compression information. Variables are based on fields in the CSV file. In this case, there are 3 variables. 

    File name: C:\Users\TEMP\airExample.xdf 
    Number of observations: 6e+05 
    Number of variables: 3 
    Number of blocks: 2 
    Compression type: zlib 
    Variable information: 
    Var 1: ArrDelay, Type: character
    Var 2: CRSDepTime, Type: numeric, Storage: float32, Low/High: (0.0167, 23.9833)
    Var 3: DayOfWeek, Type: character

From the metadata read out, we can determine whether the number of observation is large enough to warrant subdivision into smaller blocks. Additionally, we can see which variables exist, the data type, and for numeric data, the range of values. The presence of string variables is a consideration. To use this data in subsequent analysis, we might need to convert strings to numeric data for ArrDelay. Likewise, we might want to convert DayOfWeek to a factor variable so that we can roll up the results accordingly. We can do all of these things in a subsequent import.

> [!Tip]
> rxImport can create a data source object without the .xdf file. If you omit the *outFile* argument or set it NULL, the return *airXdfData* object is a data frame containing the data. Another way to use rxImport is to set an existing .xdf as the input. Doing so instantiates a data source object for the .xdf file, but without loading its data. Having an object represent the data source can come in handy. It doesn’t take up much memory, and it can be used in many other RevoScaleR objects interchangeably with a data frame.

## Re-import with additional parameters

The **rxImport** function takes multiple parameters that we can use to shape the import operation. In this step, we will repeat the import, overwriting the original file, and changing its structure in the process. Through parameters on the operation, we can:

+ Convert the string column, *DayOfWeek*, to a factor variable using the *stringsAsFactors* argument.
+ Allocate rows into blocks that we can subsequently read and write in *chunks*. By specifying the argument *rowsPerRead*, we can read and write the data in 3 blocks of 200,000 rows each.
+ Convert missing values to a specific value, such as the letter M rather than the default NA.

~~~~
	mysource <- file.path(rxGetOption("sampleDataDir"), "AirlineDemoSmall.csv")
    
	airXdfData <- rxImport(inData=mysource, outFile="airExample.xdf",
	stringsAsFactors=TRUE, missingValueString="M", rowsPerRead=200000,
    overwrite=TRUE)
~~~~

Running this command refreshes the airXdfData data source object and replaces the .xdf file in our working directory.

Metadata for the new object reflects the allocation of rows among 3 blocks and conversion of string-to-numeric data for both ArrDelay and DayOfWeek. 

    > rxGetInfo(airXdfData, getVarInfo = TRUE)
    File name: C:\Users\TEMP\airExample.xdf 
    Number of observations: 6e+05 
    Number of variables: 3 
    Number of blocks: 3 
    Compression type: zlib 
    Variable information: 
    Var 1: ArrDelay, Type: integer, Low/High: (-86, 1490)
    Var 2: CRSDepTime, Type: numeric, Storage: float32, Low/High: (0.0167, 23.9833)
    Var 3: DayOfWeek
        7 factor levels: Monday Tuesday Wednesday Thursday Friday Saturday Sunday


### Controlling factor level ordering

Setting *stringsAsFactors* to TRUE sets the levels specification for *DayOfWeek* to the unique strings found in that variable, listed in the order in which they are encountered. Since this order is arbitrary, and can easily vary from data set to data set, it is better to explicitly specify the levels in a predefined order using the *colInfo* argument.

Let's rerun the import operation with a fixed order for days of the week:

    ~~~~
	colInfo <- list(DayOfWeek = list(type = "factor",
	    levels = c("Monday", "Tuesday", "Wednesday", "Thursday",
	    "Friday", "Saturday", "Sunday")))

	airXdfData <- rxImport(inData=mysource, outFile="airExample.xdf",
	missingValueString="M", rowsPerRead=200000, colInfo  = colInfo,
    overwrite=TRUE)
    ~~~~

Notice that once we supply the *colInfo* argument, we no longer need to specify *stringsAsFactors* because *DayOfWeek* is our only factor variable.

## More metadata

Using the small *airXdfData* object representing the airExample.xdf file, we can apply standard R methods to get addititional information about the data set, such as the number of rows, columns, and the first several rows of the data set.

	nrow(airXdfData)
	ncol(airXdfData)
	head(airXdfData)

	> nrow(airXdfData)
	[1] 6e+05
	> ncol(airXdfData)
	[1] 3
	> head(airXdfData)
	  ArrDelay CRSDepTime DayOfWeek
	1        6   9.666666    Monday
	2       -8  19.916666    Monday
	3       -2  13.750000    Monday
	4        1  11.750000    Monday
	5       -2   6.416667    Monday
	6      -14  13.833333    Monday

Recall that previously you used the rxGetOption function with the getVarInfo argument to return the variables. An alternative is to use the standalone *rxGetVarInfo* function, which takes the name of the data object.

	rxGetVarInfo(airXdfData)

	Var 1: ArrDelay, Type: integer, Low/High: (-86, 1490)
	Var 2: CRSDepTime, Type: numeric, Storage: float32, Low/High: (0.0167, 23.9833)
	Var 3: DayOfWeek
	       7 factor levels: Monday Tuesday Wednesday Thursday Friday Saturday Sunday

You can also read an arbitrary chunk of the data set into a data frame for further examination. For example, read 10 rows into a data frame starting with the 100,000th row:

	myData <- rxReadXdf(airXdfData, numRows=10, startRow=100000)
	myData

This code should generate the following output:

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

You can now look to see what the factor levels are for the *DayOfWeek* variable:

	levels(myData$DayOfWeek)

This command should generate the following output:

	[1] "Monday"    "Tuesday"   "Wednesday" "Thursday"  "Friday"    "Saturday"
	[7] "Sunday"

## Visualize data and compute statistics

Let's move on to visualization, using the **rxHistogram** function to show the distribution in arrival delay by day of week. Internally, this function uses the **rxCube** function to calculate information for a histogram:

~~~~
	rxHistogram(~ArrDelay|DayOfWeek,  data = airXdfData)
~~~~
![](media/rserver-scaler-user-guide-1-introduction/image2.png)

We can also compute descriptive statistics for the variable:
~~~~
	rxSummary(~ ArrDelay, data = airXdfData)

		Call:
		rxSummary(formula = ~ArrDelay, data = airXdfData)

		Summary Statistics Results for: ~ArrDelay
		Data: airXdfData (RxXdfData Data Source)
		File name: airExample.xdf
		Number of valid observations: 6e+05

		Name     Mean     StdDev   Min Max  ValidObs MissingObs
		ArrDelay 11.31794 40.68854 -86 1490 582628   17372
~~~~

This first look at the data tells us two important things: arrival delay has a long “tail” for every day of the week, with a few flights delayed for well over two hours, and there are quite a few missing values (17,372). Presumably the missing values for arrival delay represent flights that did not arrive, that is, were canceled.

We can use RevoScaleR’s data step functionality to create a new variable, *VeryLate*, that represents flights that were either over two hours late or canceled. Since we have our original data safely stored in a text file, we will simply add this variable to our existing XDF file using the *transforms* argument to *rxDataStep*. The *transforms* argument takes a list of one or more R expressions, typically in the form of assignment statements. In this case, we use the following expression: *list(VeryLate = (ArrDelay \> 120 | is.na(ArrDelay))*

The full RevoScaleR data step then consists of the following steps:

1.  Read in the *data* a block (200,000 rows) at a time.
2.  For each block, pass the *ArrDelay* data to the R interpreter for processing the transformation to create *VeryLate*.
3.  Write the data out to the data set a block at a time. The argument *overwrite=TRUE* allows us to overwrite the data file.

		airXdfData <- rxDataStep(inData = airXdfData, outFile = "airExample.xdf",
		    transforms=list(VeryLate = (ArrDelay > 120 | is.na(ArrDelay))),
		    overwrite = TRUE)
            
Output from this command reflects the creation of the new variable:

        > rxGetInfo(airXdfData, getVarInfo = TRUE)
        File name: C:\Users\TEMP\airExample.xdf 
        Number of observations: 6e+05 
        Number of variables: 4 
        Number of blocks: 3 
        Compression type: zlib 
        Variable information: 
        Var 1: ArrDelay, Type: integer, Low/High: (-86, 1490)
        Var 2: CRSDepTime, Type: numeric, Storage: float32, Low/High: (0.0167, 23.9833)
        Var 3: DayOfWeek
            7 factor levels: Monday Tuesday Wednesday Thursday Friday Saturday Sunday
        Var 4: VeryLate, Type: logical, Low/High: (0, 1)

## Summarizing data

We already used rxSummary to get an initial impression of data shape, but let's take a closer look at the function to increase our understanding. The *rxSummary* function takes a formula as its first argument, and the name of the data set as the second. To get summary statistics for all of the data in your data file, you can alternatively use the R *summary* method for the *airXdfData* object.

	adsSummary <- rxSummary(~ArrDelay+CRSDepTime+DayOfWeek, data=airXdfData)
	adsSummary

or

	adsSummary <- summary(airXdfData)
	adsSummary

Summary statistics will be computed on the variables in the formula. By default, *rxSummary* computes data summaries term-by-term, and missing values are omitted on a term-by-term basis.

	Call:
	rxSummary(formula = ~ArrDelay + CRSDepTime + DayOfWeek, data = airDS)

	Summary Statistics Results for: ~ArrDelay + CRSDepTime + DayOfWeek
    Data: airXdfData (RxXdfData Data Source)
    File name: C:\Users\TEMP\airExample.xdf
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

Notice that the summary information shows cell counts for categorical variables, and appropriately does not provide summary statistics such as *Mean* and *StdDev*. Also notice that the *Call:* line will show the actual call you entered or the call provided by *summary*, so will appear differently in different circumstances.

You can also compute summary information by one or more categories by using interactions of a numeric variable with a factor variable. For example, to compute summary statistics on Arrival Delay by Day of Week:

	rxSummary(~ArrDelay:DayOfWeek, data=airXdfData)

The output shows the summary statistics for *ArrDelay* for each day of the week:

	Call:
	rxSummary(formula = ~ArrDelay:DayOfWeek, data=airXdfData)

	Summary Statistics Results for: ~ArrDelay:DayOfWeek
	File name: C:\Users\TEMP\airExample.xdf
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

To get a better feel for the data, we can draw histograms for each variable. Run each *rxHistogram* function one at a time. In RTVS, the histogram is rendered in the R Plot pane.

	options("device.ask.default" = T)
	rxHistogram(~ArrDelay, data=airXdfData)
	rxHistogram(~CRSDepTime, data=airXdfData)
	rxHistogram(~DayOfWeek, data=airXdfData)

![ArrDelay Histogram](media/rserver-scaler-getting-started/arrdelay_histogram_1.png)

![CRSDepTime Histogram](media/rserver-scaler-getting-started/crsdeptime_histogram.png)

![DayOfWeek Histogram](media/rserver-scaler-getting-started/dayofweek_histogram.png)

We can also easily extract a subsample of the data file into a data frame in memory. For example, we can look at just the flights that were between 4 and 5 hours late:

	myData <- rxDataStep(inData = airXdfData,
	rowSelection = ArrDelay > 240 & ArrDelay <= 300,
		varsToKeep = c("ArrDelay", "DayOfWeek"))
	rxHistogram(~ArrDelay, data = myData)

![ArrDelay Histogram](media/rserver-scaler-getting-started/arrdelay_histogram_2.png)

<a name="chunking"></a>
## Data chunking and RevoScaleR

A primary benefit of RevoScaleR is its ability to apportion data into multiple parts for processing, reassembling it later for analysis. This behavior is called *chunking*, and it's one of the key mechanisms by which RevoScaleR processes and analyzes very large data sets.

In Microsoft R, chunking functionality is available only when RevoScaleR is accessed via R Server for Windows, Teradata, SQL Server, Linux, or Hadoop. You cannot use chunking on systems that have Microsoft R Client. R Client requires that data fit into available memory. Moreover, it can only use a maximum of two threads for analysis. Internally, when RevoScaleR is running in R Client, the `blocksPerRead` argument is ignored and all data must be read into memory. You can work around this limitation when you push the compute context to a Microsoft R Server instance. You can also upgrade to a SQL Server license with R Server for Windows. For more information, see [Microsoft R Server](rserver.md).

## Create a new data set with variable transformations

In this task, use the **rxDataStep** function to create a new data set containing the variables in *airXdfData* plus additional variables created through transformations. Typically additional variables are created using the *transforms* argument. See the [Transform functions](scaler-user-guide-transform-functions.md) for information. Remember that all expressions used in *transforms* must be able to be processed on a chunk of data at a time.

In the example below, three new variables are created. 

+ *Late* is a logical variable set to *TRUE* (or *1*) if the flight was more than 15 minutes late in arriving. 
+ *DepHour* is an integer variable indicating the hour of the departure. 
+ *Night* is also a logical variable, set to *TRUE* (or *1*) if the flight departed between 10 P.M. and 5 A.M. 

The **rxDataStep** function will read the existing data set and perform the transformations chunk by chunk, and create a new data set.

~~~~
	airExtraDS <- rxDataStep(inData=airXdfData, outFile="ADS2.xdf",
		transforms=list(
			Late = ArrDelay > 15,
			DepHour = as.integer(CRSDepTime),
			Night = DepHour >= 20 | DepHour <= 5))

	rxGetInfo(airExtraDS, getVarInfo=TRUE, numRows=5)
~~~~

You should see the following information in your output:

~~~~
	C:\Users\TEMP\ADS2.xdf 
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

## Next steps

This tutorial demonstrated data import and exploration, but there are several more tutorials covering more scenarios, including instructions for working with bigger data sets.

  - [Practice data manipulation and statistical analysis](scaler-getting-started-data-manipulation.md)
  - [Analyze large data with RevoScaleR and airline flight delay data](scaler-getting-started-3-analyze-large-data.md)
  - [Example: Analyzing loan data with RevoScaleR](scaler-getting-started-1-example-loan-data.md)
  - [Example: Analyzing census data with RevoScaleR](scaler-getting-started-2-example-census-data.md)


### Try demo scripts

 Another way to learn about RevoScaleR is through demo scripts. Scripts provided in your Microsoft R installation contain code that's very similar to what you see in the tutorials. You can highlight portions of the script, right-click **Execute in Interactive** to run the script in [R Tools for Visua Studio](https://www.visualstudio.com/vs/rtvs/).

 Demo scripts are located in the *demoScripts* subdirectory of your Microsoft R installation. On Windows, this is typically:

 	`C:\Program Files\Microsoft\R Client\R_SERVER\library\RevoScaleR\demoScripts`

### Watch this video

This 30-minute video is the second in a 4-part video series. It demonstrates RevoScaleR functions for data ingestion.

> [!Tip]
> This video shows you how to create a source object for a list of files by using the R `list.files` function and patterns for selecting specific file names. It uses the multiple mortDefaultSmall*.csv files to show the approach. To create an XDF comprised of all the files, use the following syntax: `mySourceFiles <- list.files(rxGetOption("sampleDataDir"), pattern = "mortDefaultSmall\\d*.csv", full.names=TRUE)`.

 <div align=center><iframe src="https://channel9.msdn.com/Series/Microsoft-R-Server-Series/Introduction-to-Microsoft-R-Server-Session-2--Data-Ingestion/player" width="600" height="400" allowFullScreen frameBorder="0"></iframe></div>

## See Also

 [Introduction to Microsoft R](microsoft-r-getting-started.md)  
 [Getting Started with Hadoop and RevoScaleR](scaler-hadoop-getting-started.md)    
 [Parallel and distributed computing in Microsoft R Server](scaler-distributed-computing.md)   
 [Importing data](scaler-user-guide-data-import.md)     
 [ODBC data import](scaler-odbc.md)     
 [RevoScaleR Functions](scaler/scaler.md)     