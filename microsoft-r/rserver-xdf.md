---

# required metadata
title: "Create an XDF file in R Server"
description: "How to import CSV and other files to create an XDF file on Microsoft R Server"
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "05/02/2017"
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

# Create an XDF file in Microsoft R Server

**Applies to: Microsoft R Server**

XDF is the native file format for persisted data in Microsoft R Server and it offers the following benefits:

+ Columnar storage, one column per variable, for efficient read-write operations of variable data. In data science and machine learning, variables rather than rowsets are the data structures typically used in analysis.  
+ Compression, applied when the file is written to disk. 
+ Modular data access and management so that you can work with chunks of data at a time.    

XDF files are not strictly required for statistical analysis and data mining, but when data sets are large or complex, XDF offers the ability to work with subsets of your data.

To create an XDF file, use the **rxImport** function in RevoScaleR to pipe external data to R Server. By default, **rxImport** loads data into an in-memory data frame, but by specifying the **outFile** parameter, **rxImport** creates an XDF file.

## Example: Simple case

This example uses an R console and [sample data](scaler-user-guide-sample-data.md) to illustrate an import of a single CSV file. On Windows, you can run **Rgui.exe**, located at \Program Files\Microsoft\R Server\R_SERVER\bin\x64. On Linux, you can type **Revo64** at the command line.

    # Set the source file location using inData argument
    > mysourcedata <- file.path(rxGetOption("sampleDataDir", "mortDefaultSmall2000.csv"))

    # On Windows, verify the working directory and change it to a writable directory if necessary
    > getwd()
    > setwd("C:/Users/Temp")

    # Set the XDF file location using the outFile argument
    > myNewXdf <- file.path("C:/users/temp/mortDefaultSmall2000.xdf")

Notice the direction of the path delimiter. By default, R script uses forward slashes as path delimiters.

Also by default, RevoScaleR uses your working directory to save the new file. The R command `getwd()` returns the current working diretory. You can use `setwd` to change it. Or you can specify the path, as we've done here.

At this point, the object is created, but the XDF file won't exist until you run **rxImport**. 

    # Create an XDF
    > rxImport(inData = mysourcedata, outFile = myNewXdf)

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

## Set compression levels

You can set data compression via the *xdfCompressionLevel* argument to **rxImport** (and most other RevoScaleR functions that write .xdf files). You specify this as an integer in the range -1 to 9. The value -1 tells *rxImport* to use the current default compression value. The integers 1 through 9 specify increasing levels of compression, where higher numbers perform more compression, but take more time. The value 0 specifies no compression.

The .xdf format allows different blocks to have different compression levels (but not within a single call to *rxImport*). This can be useful, for example, when appending to an existing data set of unknown compression level—you can specify the compression for the new data without affecting the compression of the existing data.

You can specify a standard compression level for all future .xdf file writes by setting *xdfCompressionLevel* using *rxOptions*. For example, to specify a compression level of 3, use *rxOptions* as follows:

	> rxOptions(xdfCompressionLevel = 3)

The default value of this option is 1.

If you have one or more existing .xdf files and would like to compress them, you can use the function *rxCompressXdf*. You can specify a single file or xdf data source, a character vector of files and data sources, or the path to a directory containing .xdf files. For example, to compress all the .xdf files in the C:\\data directory, you would call *rxCompressXdf* as follows:

	> rxCompressXdf("C:/data", xdfCompressionLevel = 1, overwrite = TRUE)


## Example: Read multiple text files into XDF

You can place multiple XDF files into a folder and then set *inData* to the folder. Sample data includes several mortgage default data files for a series of years. This example demonstrates how to import all of them to a single XDF by appending one after another, again using a combination of base R commands and RevoScaleR functions.

    # Set a source folder location using R `list.files` function with a pattern for selecting specific file names.
    > mySourceFiles <- list.files(rxGetOption("sampleDataDir"), pattern = "mortDefaultSmall\\d*.csv", full.names=TRUE)

    # Set XDF output
    > myLargeXdf <- file.path("C:/users/temp/mortgagelarge.xdf")

You are now ready to create and populate the XDF. Importing multiple files requires the *append* argument to avoid overwriting existing content from the first iteration. Each new chunk of data is imported as a block and added to the file in successive blocks. 

To iterate over multiple files, use the R `lapply` function and create a function to call **rxImport** with the *append* argument.

    > lapply(mySourceFiles, FUN = function(csv_file) {
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

As before, use **rxGetInfo** to view precomputed metadata. As you would expect, the block count reflects the presence of multiple concatenated data sets.

        rxGetInfo(myLargeXdf)

Results from this command prove that you have 10 blocks, one for each .csv file. On a distributed file system, you could place these blocks on separate nodes. You could also retrieve or overwrite individual blocks.

        File name: C:\Users\TEMP\mortgagelarge.xdf 
        Number of observations: 1e+05 
        Number of variables: 6 
        Number of blocks: 10 
        Compression type: zlib 

## Example: Write XDF to HDFS

This example shows how to write a data frame directly to HDFS using the built-in Iris data set:

Set the user name: 

    > username <- "revolution" 

Set folder paths:

    hdfsDataDirRoot <- paste("/user/RevoShare/", username, sep="")
    localfsDataDirRoot <- paste("/var/RevoShare/", username, sep="")
    setwd(localfsDataDirRoot)

 
Set compute context: 

    port <- 8020 # KEEP IF USING THE DEFAULT
    host <- system("hostname", intern=TRUE)
    hdfsFS <- RxHdfsFileSystem(hostName=host, port=port)

    myHadoopCluster <- RxHadoopMR(

    nameNode= host, 
    port=port, 
    consoleOutput=TRUE)
    
    rxSetComputeContext(myHadoopCluster)


Write the XDF to a text file on HDFS:

    air7x <- RxXdfData(file='/user/RevoShare/revolution/AirOnTime7Pct', fileSystem = hdfsFS)
    air7t <- RxTextData(file='/user/RevoShare/revolution/AirOnTime7PctText', fileSystem = hdfsFS, createFileSet=TRUE)
    
    rxDataStep(air7x,air7t)



## See Also

 [Introduction to R Server](rserver.md) 
 [Install R Server on Windows](rserver-install-windows.md)  
 [Install R Server on Linux](rserver-install-linux-server.md)  
 [Install R Server on Hadoop](rserver-install-hadoop.md)