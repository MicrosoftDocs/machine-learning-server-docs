---

# required metadata
title: "Tutorial: Basic workflow import-to-summary"
description: "Learn the basic workflow from import to data summarization using the RevoScaleR functions in Microsoft R."
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "05/12/2017"
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

# Tutorial: Learn the basic workflow from import to summary (Microsoft R)

Microsoft R Server and R Client provide the open source R base language and interpreter, plus additional proprietary functional libraries like [RevoScaleR](scaler-user-guide-introduction.md) for practicing data science at scale. 

Although RevoScaleR is designed for large datasets, it helps to start with smaller data sets and simple analyses before graduating to larger data sets and more complex scenarios. This tutorial offers two basic lessons demonstrating data import and analysis using built-in sample CSV data files.

## Prerequisites

To complete this tutorial as written, you will need about 15 minutes and an R console application. 

+ On Windows, go to \Program Files\Microsoft\R Client\R_SERVER\bin\x64 and double-click **Rgui.exe**.	
+ On Linux, at the command prompt, type **Revo64**.

The command prompt for a R is `>`. You can hand-type commands line by line, or copy-paste multiple commands at once. Press Enter to execute the statement.

## About the sample data set

*AirlineDemoSmall.csv* is the dataset used in this tutorial. It is built in, so there is nothing to download. The .csv file is derived from a larger data set about flight arrival and departure details for all commercial flights within the USA, from October 1987 to April 2008. The *AirlineDemoSmall.csv* file contains three columns of data: two numeric columns, *ArrDelay* and *CRSDepTime*, and a column of strings, *DayOfWeek*. The file contains 600,000 rows of data in addition to a first row with variable names.

# How to import data into R Server using CSV files, rxImport, and an XDF file

**Applies to: Microsoft R Server**

XDF is the native file format for persisted data in Microsoft R Server. CSVs are commonplace on almost every platform. In this Quickstart we bring them together in two exercises showing how to import a single CSV file, and then multiple CSV files, into one XDF. 

XDF files are not strictly required for statistical analysis and data mining, but when data sets are large or complex, XDF offers the ability to modularize data into chunks, with columnar storage for variables, for very fast read and write operations.

To create an XDF file, use the **rxImport** function in RevoScaleR to pipe external data to R Server. 

By default, **rxImport** loads data into an in-memory data frame, but by specifying the **outFile** parameter, **rxImport** creates an XDF file, which is the objective of this tutorial.

Before you begin this Quickstart, have the following ready:

- [R Server for Windows](rserver-install-windows.md) or [R Server Linux](rserver-install-linux-server.md).
- An R console application. On Windows, you can run **Rgui.exe**, located at \Program Files\Microsoft\R Server\R_SERVER\bin\x64. On Linux, you can type **Revo64** at the command line.

*Time estimate:*

If you have completed the prerequisites, this Quickstart will take approximately 5-10 minutes to complete.

## Locate sample data

This tutorial uses functions and the built-in sample data files from the RevoScaleR package, which is automatically loaded whenever you start an R Server session in a console application. 

1. Open the R console or start a Revo64 session.

2. Retrieve the list of [sample data files](scaler-user-guide-sample-data.md) provided in RevoScaleR by entering the following command at the command prompt:

        list.files(rxGetOption("sampleDataDir"))

The open source R `list.files` command returns a file list provided by the RevoScaleR **rxGetOption** function and the **sampleDataDir** argument. 

In the file list, notice the CSV files for mortgage defaults for the years 2000 through 2009 (such as mortDefaultSmall2000.csv). This tutorial uses those files to demonstrate the creation of an XDF file.

> [!NOTE]
> If you are a Windows user and new to R, be aware that R script is case-sensitive. If you mistype the **rxGetOption** function or its parameter, you will get an error instead of the file list.

## How to read in one file

In this section, you will import a single file and learn about functions and arguments.

### Set the source file location

**rxImport** takes several arguments, including **inData** used to specify the source of data. 

In this step, you create an object named *mysourcedata* in the R session that you will pass to **inData** in a later step. Because you're using built-in data that RevoScale knows how to find, you can use the **sampleDataDir** argument to get the location of your source file.

    mysourcedata <- file.path(rxGetOption("sampleDataDir", "mortDefaultSmall2000.csv"))

### Set the XDF file location

**rxImport** takes an **outFile** parameter, used to specify the file path for the generated XDF. 

This step creates an object for a file that exists at an arbitrary location, such as C:\Users\Temp on the Windows file system. The object must provide the file location and name:

    mySmallXdf <- file.path("C:/users/temp/mortDefaultSmall2000.xdf")

Notice the direction of the path delimiter. By default, R script uses forward slashes as path delimiters.

Also by default, RevoScaleR uses your working directory to store the file. You can use the open source R command to get its location: `getwd()`. You can use `setwd` to change it. Or you can specify the path, as we've done here.

At this point, the object is created, but the XDF file won't exist until you run **rxImport**. To test whether the file exists, enter this command: `file.exists(mySmallXdf)`.

### Create an XDF

**rxImport** requires **inData** and  **outFile** (but only if you want an XDF). Since you have both, you are ready to run the command:

    rxImport(inData = mysourcedata, outFile = mySmallXdf)

**rxImport** creates the file, builds and populates columns for each variable in the dataset, and then computes metadata for each variable and the XDF as a whole.

Output returned from this operation is as follows:

    Rows Read: 10000, Total Rows Processed: 10000, Total Chunk Time: 0.022 seconds 

### Check results

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

## How to read in multiple files

Another equally important use case is to import multiple files at once. As you saw in the beginning, sample data includes mortgage default data for multiple years. In this exercise, you will import all of them to a single XDF by appending one after another, again using a combination of base R commands and RevoScaleR functions.

### Set a source folder location

This time, the source object is a list of files, obtained using the R `list.files` function with a pattern for selecting specific file names.

    mySourceFiles <- list.files(rxGetOption("sampleDataDir"), pattern = "mortDefaultSmall\\d*.csv", full.names=TRUE)

### Set XDF output

As before, create an object for the XDF file.

    myLargeXdf <- file.path("C:/users/temp/mortgagelarge.xdf")

### Run rxImport to create a multi-block XDF file

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

### Check results

As before, use **rxGetInfo** to view precomputed metadata. As you would expect, the block count reflects the presence of multiple concatenated data sets.

        rxGetInfo(myLargeXdf)

Results from this command prove that you have 10 blocks, one for each .csv file. On a distributed file system, you could place these blocks on separate nodes. You could also retrieve or overwrite individual blocks.

        File name: C:\Users\TEMP\mortgagelarge.xdf 
        Number of observations: 1e+05 
        Number of variables: 6 
        Number of blocks: 10 
        Compression type: zlib 

## (original) Import data

A very common way to store data is in text files. ScaleR includes a sample data directory providing numerous data files that are ready to use, including the *AirlineDemoSmall.csv* file. The sample data directory is registered with ScaleR; the exact location can be returned through a "sampleDataDir" parameter on the *rxGetOption* function.

The following commands import a .csv file, stores the data in an .xdf file, and creates a data object (airData) representing the data source. There are a total of 600,000 rows in the data file. By specifying the argument *rowsPerRead*, we read and write the data in 3 blocks of 200,000 rows each.

~~~~
	inFile <- file.path(rxGetOption("sampleDataDir"), "AirlineDemoSmall.csv")
	airData <- rxImport(inData=inFile, outFile = "airExample.xdf",
	stringsAsFactors = TRUE, missingValueString = "M", rowsPerRead = 200000)
~~~~

If you leave out the *outFile* argument, the return *airData* object is a data frame containing the data. Including *outFile* creates an .xdf file, a type of file recognized by ScaleR.

Given an .xdf input file, *rxImport* returns an .xdf data source object. This object represents the data source, but doesn’t take up much memory. It can be used in many other ScaleR objects interchangeably with a data frame.

To get basic information about the data set and variables, use *rxGetInfo*:
~~~~
	rxGetInfo(airData, getVarInfo = TRUE)
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