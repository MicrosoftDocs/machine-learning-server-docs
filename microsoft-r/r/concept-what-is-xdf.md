---

# required metadata
title: "Create an XDF file (Machine Learning Server) "
description: "How to import CSV and other files to create an XDF file on Machine Learning Server"
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "cgronlun"
ms.date: "02/16/2018"
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

# Create an XDF file in Machine Learning Server

XDF is the native file format for persisted data in Machine Learning Server and it offers the following benefits:

+ Compression, applied when the file is written to disk. 
+ Columnar storage, one column per variable, for efficient read-write operations of variable data. In data science and machine learning, variables rather than rowsets are the data structures typically used in analysis.  
+ Modular data access and management so that you can work with chunks of data at a time.    

XDF files are not strictly required for statistical analysis and data mining, but when data sets are large or complex, XDF offers stability in the form of persisted data under your control, plus the ability to subset and transform data for repeated analysis.

To create an XDF file, use the **rxImport** function in RevoScaleR to pipe external data to Machine Learning Server. By default, **rxImport** loads data into an in-memory data frame, but by specifying the *outFile* parameter, **rxImport** creates an XDF file.

## Example: Create an XDF

You can create an XDF using any data that can be loaded by **rxImport**, and by specifying an *outFile* consisting of a file path to a writable directory.

This example uses an R console and [sample data](sample-built-in-data.md) to create an XDF using data from a single CSV file. On Windows, you can run **Rgui.exe**, located at \Program Files\Microsoft\ML Server\R_SERVER\bin\x64. On Linux, you can type **Revo64** at the command line.

    # Set the source file location using inData argument
    > mysourcedata <- file.path(rxGetOption("sampleDataDir", "mortDefaultSmall2000.csv"))

    # Set the XDF file location using the outFile argument
    > myNewXdf <- file.path("C:/users/temp/mortDefaultSmall2000.xdf")

Notice the direction of the path delimiter. By default, R script uses forward slashes as path delimiters.

At this point, the object is created, but the XDF file won't exist until you run **rxImport**. 

    # Create an XDF
    > rxImport(inData = mysourcedata, outFile = myNewXdf)

**rxImport** creates the file, builds and populates columns for each variable in the dataset, and then computes metadata for each variable and the XDF as a whole.

Output returned from this operation is as follows:

    Rows Read: 10000, Total Rows Processed: 10000, Total Chunk Time: 0.022 seconds 

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

## Set compression levels

You can set data compression via the *xdfCompressionLevel* argument to **rxImport** (and most other RevoScaleR functions that write .xdf files). You specify this as an integer in the range -1 to 9. The value -1 tells *rxImport* to use the current default compression value. The integers 1 through 9 specify increasing levels of compression, where higher numbers perform more compression, but take more time. The value 0 specifies no compression.

The .xdf format allows different blocks to have different compression levels (but not within a single call to *rxImport*). This can be useful, for example, when appending to an existing data set of unknown compression level—you can specify the compression for the new data without affecting the compression of the existing data.

You can specify a standard compression level for all future .xdf file writes by setting *xdfCompressionLevel* using *rxOptions*. For example, to specify a compression level of 3, use *rxOptions* as follows:

	> rxOptions(xdfCompressionLevel = 3)

The default value of this option is 1.

If you have one or more existing .xdf files and would like to compress them, you can use the function *rxCompressXdf*. You can specify a single file or xdf data source, a character vector of files and data sources, or the path to a directory containing .xdf files. For example, to compress all the .xdf files in the C:\\data directory, you would call *rxCompressXdf* as follows:

	> rxCompressXdf("C:/data", xdfCompressionLevel = 1, overwrite = TRUE)

## Append new observations

If you have observations on the same variables in multiple input files, you can use the *append* argument to **rxImport** to combine them into one file. For example, we could append another copy of the claims text data set in a second block to the claimCAOrdered2.xdf file:
	
	#  Appending to an Existing File
	
	inFile <- file.path(rxGetOption("sampleDataDir"), "claims.txt")
	colInfoList <- list("car.age" = list(type = "factor", levels = c("0-3", 
	    "4-7", "8-9", "10+")))
	outfileCAOrdered2 <- "claimsCAOrdered2.xdf"
	
	claimsAppend <- rxImport(inFile, outFile = outfileCAOrdered2, 
		colClasses = c(number = "integer"),
	     colInfo = colInfoList, stringsAsFactors = TRUE, append = "rows")
	rxGetInfo(claimsAppend, getVarInfo=TRUE) 
	
	File name: C:\YourOutputPath\claimsCAOrdered2.xdf 
	Number of observations: 256 
	Number of variables: 6 
	Number of blocks: 2 
	Compression type: zlib
	Variable information: 
	Var 1: RowNum, Type: integer, Low/High: (1, 128)
	Var 2: age
	       8 factor levels: 17-20 21-24 25-29 30-34 35-39 40-49 50-59 60+
	Var 3: car.age
	       4 factor levels: 0-3 4-7 8-9 10+
	Var 4: type
	       4 factor levels: A B C D
	Var 5: cost, Type: numeric, Storage: float32, Low/High: (11.0000, 850.0000)
	Var 6: number, Type: integer, Low/High: (0, 434)

## Read XDF data into a data frame

It is often convenient to store a large amount of data in an .xdf file and then read a subset of columns and rows of the data into a data frame in memory for analysis. The **rxDataStep** function makes this easy. For example, let’s consider taking subsamples from the sample data set CensusWorkers.xdf. Using a *rowSelection* expression and list of *varsToKeep*, we can extract the *age*, *perwt*, and *sex* variables for individuals over the age of 40 living in Washington State:

	#  Reading Data from an .xdf File into a Data Frame
	
	inFile <- file.path(rxGetOption("sampleDataDir"), "CensusWorkers.xdf")
	myCensusDF <- rxDataStep(inData=inFile, 
	  rowSelection = state == "Washington" & age > 40,
	  varsToKeep = c("age", "perwt", "sex"))

When subsampling rows, we need to be aware that the rowSelection is processed on each chunk of data after it is read in. Consider an example where we want to extract every 10<sup>th</sup> row of the data set. For each chunk we will create a sequence starting with the start row number in that chunk (provided by the internal variable, .rxStartRow) with a length equal to the number of rows in the data chunk. We will determine that number of rows by using the length of the of one of the variables that has been read from the data set, age. We will keep only the rows where the remainder after dividing the row number by 10 is 0:

	myCensusSample <- rxDataStep(inData=inFile, 
	    rowSelection= (seq(from=.rxStartRow,length.out=.rxNumRows) %% 10) == 0 )


We can also create transformed variables while we are reading in the data. For example, create an 10-year *ageGroup* variable, starting with 20 (the minimum age in the data set):

	myCensusDF2 <- rxDataStep(inData=inFile, 
	  varsToKeep = c("age", "perwt", "sex"),
	  transforms=list(ageGroup = cut(age, seq(from=20, to=70, by=10))))


## Split an XDF into multiple files

RevoScaleR makes it possible to analyze huge data sets easily and efficiently, and for most purposes the most efficient computations are done on a single .xdf file. However, there are many circumstances when you will want to work with only a portion of your data. For example, you may want to distribute your data over the nodes of a cluster; in such a case, RevoScaleR’s analysis functions will process each node’s data separately, combining all the results for the final return value. You might also want to split your data into training and test data so that you can fit a model using the training data and validate it using the test data.

Use the function **rxSplit** to split your data. For example, to split variables of interest in the large 2000 U.S. Census data into five files for distribution on a five node cluster, you could use **rxSplit** as follows (change the location of the bigDataDir to the location of your downloaded file):

	#  Splitting Data Files
	rxSetComputeContext("local")
	bigDataDir <- "C:/MRS/Data"
	bigCensusData <- file.path(bigDataDir, "Census5PCT2000.xdf")	
	splitFiles <- rxSplit(bigCensusData, numOutFiles = 5, splitBy = "blocks", 
		varsToKeep = c("age",  "incearn", "incwelfr", "educrec", "metro", "perwt")) 
	names(splitFiles)

By default, **rxSplit** simply appends a number in the sequence from 1 to *numOutFiles* to the base file name to create the new file names, and in this case the resulting file names, for example, “Census5PCT20001.xdf”, are a bit confusing. 

You can exercise greater control over the output file names by using the *outFilesBase* and *outFilesSuffixes* arguments. With *outFilesBase*, you can specify either a single character string to be used for all files or a character vector the same length as the desired number of files. The latter option is useful, for example, if you would like to create four files with the same file name, but different paths:

	nodePaths <- paste("compute", 10:13, sep="")
	baseNames <- file.path("C:", nodePaths, "DistCensusData")
	splitFiles2 <- rxSplit(bigCensusData, splitBy = "blocks", 
		outFilesBase = baseNames, 
		varsToKeep = c("age", "incearn", "incwelfr", "educrec", "metro", "perwt")) 
	names(splitFiles2)

This creates the four directories C:/compute10, etc., and creates a file named “DistCensusData.xdf” in each directory. You should adopt an approach like this when using distributed data with the standard RevoScaleR analysis functions such as **rxLinMod** and **rxLogit** in an **RxSpark** or **RxHadoopMR** compute context.

You can supply the *outFilesSuffixes* arguments to exercise greater control over what is appended to the end of each file. Returning to our first example, we can add a hyphen between our base file name and the sequence 1 to 5 using *outFilesSuffixes* as follows:

	splitFiles3 <- rxSplit(bigCensusData, splitBy = "blocks", 
		outFileSuffixes=paste("-", 1:5, sep=""),
		varsToKeep = c("age", "incearn", "incwelfr", "educrec", "metro", "perwt")) 
	names(splitFiles3)

The *splitBy* argument specifies whether to split your data file row-by-row or block-by-block. The default is *splitBy="rows",* which distributes data from each block into different files. The examples above use the faster split by blocks instead. The *splitBy* argument is ignored if you also specify the *splitByFactor* argument as a character string representing a valid factor variable. In this case, one file is created per level of the factor variable.

You can use the *splitByFactor* argument and a transforms argument to easily create test and training data sets from an .xdf file. Note that this will take longer than the previous examples because each block of data is being processed:

	splitFiles4 <- rxSplit(inData = bigCensusData, 
		outFilesBase="censusData", 
		splitByFactor="testSplitVar", 
		varsToKeep = c("age", "incearn", "incwelfr", "educrec", "metro", "perwt"), 
		transforms=list(testSplitVar = factor( 
			sample(0:1,	size=.rxNumRows, replace=TRUE, prob=c(.10, .9)), 
			levels=0:1, labels = c("Test", "Train"))))
	names(splitFiles4)
	rxSummary(~age, data = splitFiles4[[1]], reportProgress = 0)
	rxSummary(~age, data = splitFiles4[[2]], reportProgress = 0)


This takes approximately 10% of the data as a test data set, with the remainder going into the training data.

If your .xdf file is relatively small, you may want to set *outFilesBase = ""* so that a list of data frames is returned instead of having files created. You can also use **rxSplit** to split data frames (see the **rxSplit** help page for details).

## Re-Block an .xdf File 

After a series of data import or row selection steps, you may find that you have an .xdf file with very uneven block sizes. This may make it difficult to efficiently perform computations by “chunk.” To find the sizes of the blocks in your .xdf file, use **rxGetInf** with the *getBlockSizes* argument set to TRUE. For example, let’s look at the block sizes for the sample CensusWorkers.xdf file:

	#  Re-Blocking an .xdf File
	
	fileName <- file.path(rxGetOption("sampleDataDir"), "CensusWorkers.xdf")
	rxGetInfo(fileName, getBlockSizes = TRUE)

The following information is provided:
	
	File name: C:\Program Files\Microsoft\MRO-for-RRE\8.0\R-3.2.2\ library\RevoScaleR\SampleData\CensusWorkers.xdf 
	Number of observations: 351121 
	Number of variables: 6 
	Number of blocks: 6 
	Compression type: zlib
	Rows per block: 95420 42503 1799 131234 34726 45439

We see that, in fact, the number of rows per block varies from a low of 1799 to a high of 131,234. To create a new file with more even-sized blocks, use the *rowsPerRead* argument in **rxDataStep**:

	newFile <- "censusWorkersEvenBlocks.xdf"
	rxDataStep(inData = fileName, outFile = newFile, rowsPerRead = 60000)
	rxGetInfo(newFile, getBlockSizes = TRUE)	
	
The new file has blocks sizes of 60,000 for all but the last slightly smaller block:

	File name: C:\Users\...\censusWorkersEvenBlocks.xdf 
	Number of observations: 351121 
	Number of variables: 6 
	Number of blocks: 6 
	Compression type: zlib
	Rows per block: 60000 60000 60000 60000 60000 51121
	
## Next steps

XDF is optimized for distributed file storage and access in the Hadoop Distributed File System (HDFS). To learn more about using XDF in HDFS, see [Import and consume HDFS data files](how-to-revoscaler-data-hdfs.md).

You can import multiple text files into a single XDF. For instructions, see [Import text data](how-to-revoscaler-data-import.md).

## See also

 [Machine Learning Server](../what-is-machine-learning-server.md) 
 [Install Machine Learning Server on Windows](../install/machine-learning-server-windows-install.md)  
 [Install Machine Learning Server on Linux](../install/machine-learning-server-linux-install.md)  
 [Install Machine Learning Server on Hadoop](../install/machine-learning-server-hadoop-install.md)