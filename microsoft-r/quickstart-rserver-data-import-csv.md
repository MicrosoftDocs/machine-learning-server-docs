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

Learn how to import multiple CSV files into the native file format of R Server: an XDF file. 

Using XDF files are not strictly required for statistics and analytics in R Server, but when data sets are large or complex, storing it as an XDF offers a range of benefits. Key benefits include chunking data in blocks, data manipulation and transformation, and very fast retrieval of variables and metadata.

To create an XDF file, we'll use the rxImport function in RevoScaleR to pipe external data to R Server. By default, rxImport loads data into an in-memory data frame, but by specifying the **outFile** parameter, rxImport creates an XDF file, which is the objective of this tutorial.

*Time estimate:*
If you have completed the prerequisites, this task will take approximately *10* minutes to complete.

Before you begin this Quickstart, have the following ready:

- [R Server for Windows](rserver-install-windows.md) or [R Server Linux](rserver-install-linux-server.md) (you could also use R Client).
- An R console application. On Windows, you can run **Rgui.exe**, located at \Program Files\Microsoft\R Server\R_SERVER\bin\x64. On Linux, you can type **Revo64** at the command line.

## Locate sample data

This tutorial uses the built-in sample data files from the RevoScaleR package, which is automatically loaded whenever you start an R Server session in a console application. 

1. Open the R console or start a Revo64 session.

2. Retrieve the list of [sample data files](scaler-user-guide-sample-data.md) provided in RevoScaleR by typing the following command at the command prompt:

    list.files(rxGetOption("sampleDataDir"))

Notice the CSV files for mortgage defaults for the years 2000 through 2009 (such as mortDefaultSmall2000.csv). This tutorial will use those files to demonstrate how to import multiple files into one or more XDF files.

> [!NOTE]
> If you are a Windows user and new to R, be aware that R script is case-sensitive. If you mistype the rxGetOption function or its parameter, you will get an error instead of the file list.

## Specify a data input for multiple files

rxImport takes several parameters, including **inData** used to specify the source of data.

Create an object named *mysourcedata* in the R session to represent the files you'll provide to **inData**:

    mysourcedata <- file.path(rxGetOption("sampleDataDir"), "mortDefaultSmall2000.csv")


## Specify the XDF output file

rxImport takes an **outFile** parameter, used to specify a path to an XDF file. In this step, specify the file name:

    myxdf <- "C:/users/temp/mortDefaultSmall2000.xdf"

By default, RevoScaleR will use your working directory to store the file. You can use the open source R command to get its location: `getwd()`. You can use `setwd` to change it. For the purposes of this tutorial, you'll create the file in the user's temp directory.

Notice the direction of the path delimiter. R script uses forward slashes to separate each component part.

## Run rxImport to create an XDF

rxImport requires an **inData** and an **outFile** (but only if you want an XDF). Since you have both, you are ready to run the command:

    rxImport(inData = mysourcedata, outFile = myxdf)

rxImport creates the file, builds and populates columns for each variable in the dataset, and then precomputes metadata for each variable and the XDF as a whole.

Output returned from this operation is as follows:

    Rows Read: 10000, Total Rows Processed: 10000, Total Chunk Time: 0.022 seconds 

## Check results

Use the rxGetInfo function to return information about an object. In this case, the object is the XDF file created in a previous step, and the info returned is the precomputed metadata for each variable, plus a summary of observations, blocks, and xxx

    rxGetInfo(myxdf, getVarInfo = TRUE)

Notice that the function returns precomputed metadata about file contents, such as number of observations, variables, blocks, and compression type. Variables are based on fields in the CSV file. In this case, there are 6 variables and precomputed metadata about each one appears in the output below.

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


## Next steps

As you have seen for yourself, creating an XDF file is a simple task when you have the right function. The XDF file can be split into smaller parts, combined into a larger file, or distributed across multiple nodes in a distributed file system like HDFS. 

To practice these tasks on the sample files you just created, continue with these lessons:

+ LINK-TBD  
+ LINK-TBD

## See Also

 [Introduction to R Server](rserver.md) 
 [Install R Server on Windows](rserver-install-windows.md)  
 [Install R Server on Linux](rserver-install-linux-server.md)  
 [Install R Server on Hadoop](rserver-install-hadoop.md)