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

Microsoft R Server and R Client provide the open source R base language and interpreter, plus additional proprietary functional libraries like [RevoScaleR](scaler-user-guide-introduction.md) for practicing data science at scale. 

Although RevoScaleR is designed for large datasets, it helps to start with smaller data sets and simple analyses before graduating to larger data sets and more complex scenarios. This tutorial demonstrates data import using built-in sample data files.

## Prerequisites

To complete this tutorial as written, use an R console application. 

+ On Windows, go to \Program Files\Microsoft\R Client\R_SERVER\bin\x64 and double-click **Rgui.exe**.	
+ On Linux, at the command prompt, type **Revo64**.

The command prompt for a R is `>`. You can hand-type commands line by line, or copy-paste multi-line command. Press Enter to execute the statement.

## What you will learn in this tutorial

1. Convert CSV data to the XDF data file format.
2. Get information about the data
3. Summarize data
4. Fit a linear model to the data.
5. Create a new .xdf file, sub-setting the original data set and performing data transformations.

## Locate sample data

RevoScaleR includes numerous data files that are ready to use, including the *AirlineDemoSmall.csv* file. The sample data directory is registered with RevoScaleR, and the exact location can be returned through a "sampleDataDir" parameter on the *rxGetOption* function.

1. Open the R console or start a Revo64 session.

2. Retrieve the list of [sample data files](scaler-user-guide-sample-data.md) provided in RevoScaleR by entering the following command at the command prompt:

        list.files(rxGetOption("sampleDataDir"))

The open source R `list.files` command returns a file list provided by the RevoScaleR **rxGetOption** function and the **sampleDataDir** argument. 

> [!NOTE]
> If you are a Windows user and new to R, be aware that R script is case-sensitive. If you mistype the **rxGetOption** function or its parameter, you will get an error instead of the file list.

## About the airline data set

*AirlineDemoSmall.csv* is the dataset used in this tutorial. It is a subset of a data set containing information on flight arrival and departure details for all commercial flights within the USA, from October 1987 to April 2008. The *AirlineDemoSmall.csv* file contains three columns of data: two numeric columns, *ArrDelay* and *CRSDepTime*, and a column of strings, *DayOfWeek*. The file contains 600,000 rows of data in addition to a first row with variable names.

## Import text data into .xdf

RevoScaleR provides a data file format (.xdf) designed to be very efficient for reading arbitrary rows and columns. To convert the *AirlineDemoSmall.csv* text file into the .xdf data format, use the function *rxImport*. Using this function, you can convert the string column, *DayOfWeek*, to a factor variable.

In the *AirlineDemoSmall.csv* file, there are a total of 600,000 rows in the data file. By specifying the argument *rowsPerRead*, you can read and write the data in 3 blocks of 200,000 rows each.

The following steps show you how to import a .csv file, store the data in an .xdf file, and create a data object (airData) representing the data source. 

~~~~
	mysource <- file.path(rxGetOption("sampleDataDir"), "AirlineDemoSmall.csv")
	airXdfData <- rxImport(inData=mysource, outFile = "airExample.xdf",
	stringsAsFactors = TRUE, missingValueString = "M", rowsPerRead = 200000)
~~~~

### Specify the source input file

On first line,`mysource` is an object specifying the location of sample data. It uses R's `file.path` command to add path information. It gets the folder path using rxGetOption("sampleDataDir") for the file, and "AirlineDemoSmall.csv" for the file. The `sampleDataDir` is an argument of rxGetOption used to return the directory containing sample files registered with RevoScaleR. 

> [!Tip]
> This tutorial imports a single file, but you can create a source object for a list of files by using the R `list.files` function and providing a pattern for selecting specific file names" `mySourceFiles <- list.files(rxGetOption("sampleDataDir"), pattern = "mortDefaultSmall\\d*.csv", full.names=TRUE)`.


### Specify the destination output file

On line two, `airXdfData` is a data source object returned by **rxImport**. The **rxImport** function takes several arguments, including *inData* for source data, *outFile* for the XDF, *stringsAsFactors* to convert string columns to factor variables, *missingValueString* to , and *rowsPerRead* to organize data into multiple blocks within the .xdf

    ~~~~
	airXdfData <- rxImport(inData=mysource, outFile="airExample.xdf",
    ~~~~

If you omit the *outFile* argument, the return *airData* object is a data frame containing the data. Including *outFile* creates an .xdf file.

Given an .xdf input file, *rxImport* returns an .xdf data source object. This object represents the data source, but doesn’t take up much memory. It can be used in many other ScaleR objects interchangeably with a data frame.

XDF files are not strictly required for statistical analysis and data mining, but when data sets are large or complex, XDF has the ability to block data into chunks. XDF provides column storage for variables, which is more efficient if you want to selectively choose variables for further analysis.

### XDF file location

By default, RevoScaleR uses your working directory to store the file. You can use the open source R command to get its location: `getwd()`. If the output is something like `C:/Program Files/Microsoft/R Server/R_SERVER/bin/x64`, you won't have permission to save the XDF to this location. 

To change the location, do any of the following:

+ Run `setwd("/Users/TEMP")` to change it to a temp directory. 
+ Include the path in the file name: `C:/users/temp/outFile="airExample.xdf"`.  

Notice the direction of the path delimiter. By default, R script uses forward slashes as path delimiters.

At this point, the object is created, but the XDF file won't exist until you run **rxImport**. To test whether the file exists, enter this command: `file.exists(airXdfData)`.

### Run rxImport to create a multi-block XDF file

and then computes metadata for each variable and the XDF as a whole.

You are now ready to create and populate the file. Importing multiple files requires the **append** argument to avoid overwriting existing content from the first iteration. Each new chunk of data is imported as a block and added to the file in successive blocks. 

To iterate over multiple files, use the R `lapply` function and create a function to call **rxImport** with the **append** argument.

    lapply(mySourceFiles, FUN = function(csv_file) {
        rxImport(inData = csv_file, outFile=myLargeXdf, append = file.exists(myLargeXdf)) } )

Partial output from this operation reports out the processing details.

    Rows Read: 10000, Total Rows Processed: 10000, Total Chunk Time: 0.020 seconds 
    Rows Read: 10000, Total Rows Processed: 10000, Total Chunk Time: 0.020 seconds 
    Rows Read: 10000, Total Rows Processed: 10000, Total Chunk Time: 0.025 seconds 
    Rows Read: 10000, Total Rows Processed: 10000, Total Chunk Time: 0.022 seconds 
    Rows Read: 10000, Total Rows Processed: 10000, Total Chunk Time: 0.020 seconds 
    Rows Read: 10000, Total Rows Processed: 10000, Total Chunk Time: 0.020 seconds 
    Rows Read: 10000, Total Rows Processed: 10000, Total Chunk Time: 0.019 seconds 
    Rows Read: 10000, Total Rows Processed: 10000, Total Chunk Time: 0.020 seconds 
    Rows Read: 10000, Total Rows Processed: 10000, Total Chunk Time: 0.020 seconds 
    Rows Read: 10000, Total Rows Processed: 10000, Total Chunk Time: 0.020 seconds 


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

**Check results**

 Use the **rxGetInfo** function to return information about an object. In this case, the object is the XDF file created in a previous step, and the information returned is the precomputed metadata for each variable, plus a summary of observations, variables, blocks, and compression information.

    rxGetInfo(mySmallXdf, getVarInfo = TRUE)

 Output is below. Variables are based on fields in the CSV file. In this case, there are 6 variables. Precomputed metadata about each one appears in the output below.

    File name: C:\Users\TEMP\mortDefaultSmall2000.xdf 
    Number of observations: 10000 
    Number of variables: 6 
    Number of blocks: 1 
    Compression type: zlib 
    Variable information: 
    Var 1: creditScore, Type: integer, Low/High: (486, 895)
    Var 2: houseAge, Type: integer, Low/High: (0, 40)
    Var 3: yearsEmploy, Type: integer, Low/High: (0, 14)
    Var 4: ccDebt, Type: integer, Low/High: (0, 12275)
    Var 5: year, Type: integer, Low/High: (2000, 2000)
    Var 6: default, Type: integer, Low/High: (0, 1)

### Check results

As before, use **rxGetInfo** to view precomputed metadata. As you would expect, the block count reflects the presence of multiple concatenated data sets.

        rxGetInfo(myLargeXdf)

Results from this command prove that you have 10 blocks, one for each .csv file. On a distributed file system, you could place these blocks on separate nodes. You could also retrieve or overwrite individual blocks.

        File name: C:\Users\TEMP\mortgagelarge.xdf 
        Number of observations: 1e+05 
        Number of variables: 6 
        Number of blocks: 10 
        Compression type: zlib 



## Get data set information

To get basic information about the data set and variables, use *rxGetInfo* on the data source object you just created:

~~~~
	rxGetInfo(airXdfData, getVarInfo = TRUE)
~~~~

It returns in the following output:

~~~~
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
~~~~

Now let’s take a quick look at the data. Use the *rxHistogram* function to show the distribution in arrival delay by day of week. Internally, this function uses the *rxCube* function to calculate information for a histogram:
~~~~
	rxHistogram(~ArrDelay|DayOfWeek,  data = airData)
~~~~
![](media/rserver-scaler-user-guide-1-introduction/image2.png)

We can also compute descriptive statistics for the variable:
~~~~
	rxSummary(~ ArrDelay, data = airData )

		Call:
		rxSummary(formula = ~ArrDelay, data = airData)

		Summary Statistics Results for: ~ArrDelay
		Data: airData (RxXdfData Data Source)
		File name: airExample.xdf
		Number of valid observations: 6e+05

		Name     Mean     StdDev   Min Max  ValidObs MissingObs
		ArrDelay 11.31794 40.68854 -86 1490 582628   17372
~~~~

This first look at the data tells us two important things: arrival delay has a long “tail” for every day of the week, with a few flights delayed for well over two hours, and there are quite a few missing values (17,372). Presumably the missing values for arrival delay represent flights that did not arrive, that is, were canceled.

We can use RevoScaleR’s data step functionality to create a new variable, *VeryLate*, that represents flights that were either over two hours late or canceled. Since we have our original data safely stored in a text file, we will simply add this variable to our existing airline.xdf file using the *transforms* argument to *rxDataStep*. The *transforms* argument takes a list of one or more R expressions, typically in the form of assignment statements. In this case, we use the following expression: *list(VeryLate = (ArrDelay \> 120 | is.na(ArrDelay))*

The full RevoScaleR data step then consists of the following steps:

1.  Read in the *data* a block (200,000 rows) at a time.
2.  For each block, pass the *ArrDelay* data to the R interpreter for processing the transformation to create *VeryLate*.
3.  Write the data out to the data set a block at a time. The argument *overwrite=TRUE* allows us to overwrite the data file.

		airData <- rxDataStep(inData = airData, outFile = "airExample.xdf",
		    transforms=list(VeryLate = (ArrDelay > 120 | is.na(ArrDelay))),
		    overwrite = TRUE)

## Examine .xdf files

Using the small *airDS* object representing the ADS.xdf file, you can apply standard R methods to get basic information about the data set:

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

The *rxGetVarInfo* function provides additional variable information:

	rxGetVarInfo(airDS)

	Var 1: ArrDelay, Type: integer, Low/High: (-86, 1490)
	Var 2: CRSDepTime, Type: numeric, Storage: float32, Low/High: (0.0167, 23.9833)
	Var 3: DayOfWeek
	       7 factor levels: Monday Tuesday Wednesday Thursday Friday Saturday Sunday

You can also read an arbitrary chunk of the data set into a data frame for further examination. For example, read 10 rows into a data frame starting with the 100,000th row:

	myData <- rxReadXdf(airDS, numRows=10, startRow=100000)
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

## Summarizing data

Use the *rxSummary* function to obtain descriptive statistics for your .xdf data file. The *rxSummary* function takes a formula as its first argument, and the name of the data set as the second. To get summary statistics for all of the data in your data file, you can alternatively use the *summary* method for the *airDS* object.

	adsSummary <- rxSummary(~ArrDelay+CRSDepTime+DayOfWeek, data = airDS)
	adsSummary

or

	adsSummary <- summary( airDS )
	adsSummary

Summary statistics will be computed on the variables in the formula. By default, *rxSummary* computes data summaries term-by-term, and missing values are omitted on a term-by-term basis. In earlier versions, summaries were computed on the complete table after observations with missing elements were omitted.

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

Notice that the summary information shows cell counts for categorical variables, and appropriately does not provide summary statistics such as *Mean* and *StdDev*. Also notice that the *Call:* line will show the actual call you entered or the call provided by *summary*, so will appear differently in different circumstances.

You can also compute summary information by one or more categories by using interactions of a numeric variable with a factor variable.  For example, to compute summary statistics on Arrival Delay by Day of Week:

	rxSummary(~ArrDelay:DayOfWeek, data = airDS)

The output shows the summary statistics for *ArrDelay* for each day of the week:

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

To get a better feel for the data, we can draw histograms for each variable. Run each *rxHistogram* function one at a time. In RTVS, the histogram is rendered in the R Plot pane.

	options("device.ask.default" = T)
	rxHistogram(~ArrDelay, data = airDS)
	rxHistogram(~CRSDepTime, data = airDS)
	rxHistogram(~DayOfWeek, data = airDS)

![ArrDelay Histogram](media/rserver-scaler-getting-started/arrdelay_histogram_1.png)

![CRSDepTime Histogram](media/rserver-scaler-getting-started/crsdeptime_histogram.png)

![DayOfWeek Histogram](media/rserver-scaler-getting-started/dayofweek_histogram.png)

We can also easily extract a subsample of the data file into a data frame in memory. For example, we can look at just the flights that were between 4 and 5 hours late:

	myData <- rxDataStep(inData = airDS,
	rowSelection = ArrDelay > 240 & ArrDelay <= 300,
		varsToKeep = c("ArrDelay", "DayOfWeek"))
	rxHistogram(~ArrDelay, data = myData)

![ArrDelay Histogram](media/rserver-scaler-getting-started/arrdelay_histogram_2.png)

<a name="chunking"></a>

## Data chunking and RevoScaleR

A primary benefit of RevoScaleR is its ability to apportion data into multiple parts for processing, reassembling it later for analysis. This behavior is called *chunking*, and it's one of the key mechanisms by which RevoScaleR processes and analyzes very large data sets.

In Microsoft R products, chunking functionality is available only when RevoScaleR is accessed via R Server for Windows, Teradata, SQL Server, Linux, or Hadoop. You cannot use chunking on systems that have Microsoft R Client. R Client requires that data fit into available memory. Moreover, it can only use a maximum of two threads for analysis. Internally, when RevoScaleR is running in R Client, the `blocksPerRead` argument is ignored and all data must be read into memory. You can work around this limitation when you push the compute context to a Microsoft R Server instance. You can also upgrade to a SQL Server license with R Server for Windows. For more information, see [Microsoft R Server](rserver.md).

## Next steps

This quick start demonstrated a basic workflow, but there are several more tutorials that go into more detail and cover more scenarios, including instructions for working with bigger data sets.

  - [Practice data manipulation and statistical analysis](scaler-getting-started-data-manipulation.md)
  - [Analyze large data with RevoScaleR and airline flight delay data](scaler-getting-started-3-analyze-large-data.md)
  - [Example: Analyzing loan data with RevoScaleR](scaler-getting-started-1-example-loan-data.md)
  - [Example: Analyzing census data with RevoScaleR](scaler-getting-started-2-example-census-data.md)

### Try demo scripts

 Another way to learn about RevoScaleR is through demo scripts. Scripts provided in your Microsoft R installation contain code that's very similar to what you see in the tutorials. You can highlight portions of the script, right-click **Execute in Interactive** to run the script in RTVS.

 Demo scripts are located in the *demoScripts* subdirectory of your Microsoft R installation. On Windows, this is typically:

 	`C:\Program Files\Microsoft\R Client\R_SERVER\library\RevoScaleR\demoScripts`

### Watch this video

This 30-minute video is the second in a 4-part video series. It demonstrates RevoScaleR functions for data ingestion.

 <div align=center><iframe src="https://channel9.msdn.com/Series/Microsoft-R-Server-Series/Introduction-to-Microsoft-R-Server-Session-2--Data-Ingestion/player" width="600" height="400" allowFullScreen frameBorder="0"></iframe></div>

## See Also

 [Introduction to Microsoft R](microsoft-r-getting-started.md)  
 [Getting Started with Hadoop and RevoScaleR](scaler-hadoop-getting-started.md)    
 [Parallel and distributed computing in Microsoft R Server](scaler-distributed-computing.md)   
 [Importing data](scaler-user-guide-data-import.md)     
 [ODBC data import](scaler-odbc.md)     
 [RevoScaleR Functions](scaler/scaler.md)     