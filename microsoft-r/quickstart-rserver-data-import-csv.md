---

# required metadata
title: "Quickstart tutorial: Import CSV data files into R Server"
description: "How to import CSV files into an XDF file on Microsoft R Server"
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

## Next steps

As you have seen for yourself, creating an XDF file is simple when you have the right functions. The XDF file can be split into smaller parts, combined into a larger file, or distributed across multiple nodes in a distributed file system like HDFS. 

To practice these tasks on the sample files you just created, continue with these lessons:

+ LINK-TBD  
+ LINK-TBD

### Try demo scripts

 Another way to learn about RevoScaleR is through demo scripts. Scripts provided in your Microsoft R installation contain code that's very similar to what you see in this tutorial. 

 Demo scripts are located in the *demoScripts* subdirectory of your Microsoft R installation. On Windows, this is typically:

 	`C:\Program Files\Microsoft\R Server\R_SERVER\library\RevoScaleR\demoScripts`

### Watch this video

This 30-minute video is the second in a 4-part video series. It covers and expands upon the techniques you learned in this Quickstart. 

 <div align=center><iframe src="https://channel9.msdn.com/Series/Microsoft-R-Server-Series/Introduction-to-Microsoft-R-Server-Session-2--Data-Ingestion/player" width="600" height="400" allowFullScreen frameBorder="0"></iframe></div>

## See Also

 [Introduction to R Server](rserver.md) 
 [Install R Server on Windows](rserver-install-windows.md)  
 [Install R Server on Linux](rserver-install-linux-server.md)  
 [Install R Server on Hadoop](rserver-install-hadoop.md)