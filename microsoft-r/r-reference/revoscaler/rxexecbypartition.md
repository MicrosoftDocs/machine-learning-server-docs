--- 
 
# required metadata 
title: "RevoScaleR By Group Parallelism" 
description: " This feature allows users to run analytics computation in parallel on individual data partitions split from an input data source based on the specified variables. In **RevoScaleR** version 9.1.0, we provide the necessary rx functions to be executed for funtionalities of By-group parallelism. This document will describe different scenarios of By-group parallelism, running in a number of supported compute contexts. " 
keywords: "RevoScaleR, rxExecByPartition, partition, exec, group, execby, groupby" 
author: "HeidiSteen"
ms.author: "heidist" 
manager: "jhubbard" 
ms.date: "04/18/2017" 
ms.topic: "reference" 
ms.prod: "microsoft-r" 
ms.service: "" 
ms.assetid: "" 
 
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
 
 
 
 #rxExecByPartition: RevoScaleR By Group Parallelism

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
This feature allows users to run analytics computation in parallel on individual data partitions split from an input data source based on the specified variables. In **RevoScaleR** version 9.1.0, we provide the necessary rx functions to be executed for funtionalities of By-group parallelism. This document will describe different scenarios of By-group parallelism, running in a number of supported compute contexts.
 
 
 
 ##Details
 
By-group Parallelism provides functionalities that allow users perform the following typical operations:


* 
 Create new data partitions: given an input data set and a list of partitioning variables, split the data set into multiple small data sets based on the values of the specified partitioning variables.

* 
 Append new data to existing data partitions: given an input data set and a list of partitioning variables, split the data set and append data in partitioned data sets to the existing corresponding data partitions.
..
* 
 Perform analytics computation on data partitions in parallel multiple times as needed.

* 
 Do all the above three operations in one step initially and then repeat computation multiple times.



In **RevoScaleR** version 9.1.0, we introduce three new rx functions for partitioning data set and performing computation in parallel:



* 
 [rxExecBy](rxexecby.md)() - to partition an input data source and execute user function on each partition in parallel. If the input data source is already partitioned, the function will skip the partitioning step and directly trigger computation for user function on partitions.

* 
 rxPartition() - to partition an input data source and store data partitions on disk. For this functionality, a new xdf file format, called Partitioned Xdf (PXdf) is introduced for storing data partitions as well as partition metadata information on disk. Partitioned Xdf file can then be loaded into an in-memory Partitioned Xdf object using `RxXdfData` to be used for performing computation on data partitions repeatedly by [rxExecBy](rxexecby.md).

* 
 [rxGetPartitions](rxgetpartitions.md)() - to enumerate unique partitioning values in an existing partitioned Xdf and return it as a data frame



**Running analytics computation in parallel on partitioned data set with [rxExecBy](rxexecby.md)()**

Input data set provided as a data source object can be data sources of different types, such as data frame, text data, Xdf files, ODBC data source, SQL Server data source, etc. In version 9.1.0, [rxExecBy](rxexecby.md)() is supported in local compute context, SQL Server compute context and Spark compute context. Depending on input data sources and compute context, [rxExecBy](rxexecby.md)() will execute in different modes which are summarized in the following table:

| Col  1 | Col  2 | Col  3 |
| :---| :---:| :---: |
| **Data Source \ Compute Context**    |  **Local**  |  **SQL Server**  |
|      Data frame, text data, xdf, other data sources that are not ODBC  |  Generate PXdf for partitions and execute computation on PXdf  |  --  |
|      SQL Server data source or ODBC data source with *query* specified  |  Generate PXdf for partitions and execute computation on PXdf  |  Generate temporary Composite Xdf and PXdf for partitions and execute computation on PXdf  |
|      SQL Server data source or ODBC data source with *table* specified  |  Do streaming with SQL rewrite partition queries and execute computation on streaming partitions  |  Do streaming with SQL rewrite partition queries and execute computation on streaming  |
   

As shown in the table, when running analytics on local compute context, PXdf is first temporarily generated and saved on disk; then computation are applied on the generated PXdf. The example for running this scenario can be found in the [rxExecBy](rxexecby.md)() documentation. It's worth to note that the temporary PXdf generated will be removed once the computation is completed. If user plans to run the analytics multiple times with the same data set and different user functions, it would be more efficient to go with the following recommended flow:



1 
 Create a new partitioned Xdf object with [RxXdfData](rxxdfdata.md)() by specifying `createPartitionSet = TRUE`.

1 
 Construct data partitions for the newly created PXdf object from an input data set and save it to disk with rxPartition().

1 
 Run analysis with user defined function on the data partitions of the PXdf object with [rxExecBy](rxexecby.md)(). This step can be repeated multiple times with different user defined functions and different subsets of data partitions using a `filterFunc` specified as an argument of [rxExecBy](rxexecby.md)().



 
 
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[RxXdfData](rxxdfdata.md),
[rxExecBy](rxexecby.md),
rxPartition,
[rxGetPartitions](rxgetpartitions.md),
[rxSplit](rxsplitxdf.md),
[rxExec](rxexec.md),
[rxImport](rximport.md)
   
 ##Examples

 ```
   
  ## Not run:
 

##############################################################################
# Run analytics on data partitions in one operation
##############################################################################

  # create an input text data source 
  inFile <- "claims.txt"
  inFile <- file.path(dataPath = rxGetOption(opt = "sampleDataDir"), inFile)
  inTxtDS <- RxTextData(file = inFile, delimiter = ",")

  # define an user function that builds a linear model.
  ".linMod" <- function(keys, data)
  {
    result <- rxLinMod(formula = cost ~ number, data = data)
    return(result$coefficients[[1]])
  }

  # set local compute context with 4-way parallel
  rxSetComputeContext("localpar")
  rxOptions(numCoresToUse = 4)

  # run the .linMod function on partitions split from the input Xdf data source
  # based on the variable 'car.age'.
  localCCResults <- rxExecBy(inData = inTxtDS, keys = c("car.age"), func = .linMod)

##############################################################################
# Run analytics on data partitions multiple times with different user functions
##############################################################################

  # create an input Xdf data source
  inFile <- "claims.xdf"
  inFile <- file.path(dataPath = rxGetOption(opt = "sampleDataDir"), inFile)
  inXdfDS <- RxXdfData(file = inFile)

  # create an output partitioned Xdf data source
  outFile <- file.path(tempdir(), "partitionedClaims")
  outPartXdfDataSource <- RxXdfData(file = outFile, createPartitionSet = TRUE)

  # construct and save the partitioned Xdf to disk
  partDF <- rxPartition(inData = inXdfDS, outData = outPartXdfDataSource, varsToPartition = c("car.age", "type"))

  # define an user function that counts number of rows in each partition
  ".Count" <- function(keys, data)
  {
    myDF <- rxImport(inData = data)
    return(nrow(myDF))
  }

  # local compute context
  rxSetComputeContext("localpar")
  rxOptions(numCoresToUse = 4)

  # call rxExecBy to execute the user function on partitions
  results1 <- rxExecBy(inData = outPartXdfDataSource, keys = c("car.age", "type"), func = .Count)

  # define another user function
  ".linMod" <- function(keys, data)
  {
    result <- rxLinMod(formula = cost ~ number, data = data)
    return(result$coefficients[[1]])
  }

  # call rxExecBy to execute the new user function on the same set of partitions
  results2 <- rxExecBy(inData = outPartXdfDataSource, keys = c("car.age", "type"), func = .linMod)

  # clean-up: delete the partitioned Xdf
  unlink(outFile, recursive = TRUE, force = TRUE)

##############################################################################
# Append new data to existing partitions and run analytics
##############################################################################
  # create two sets of data frames from Xdf data source
  inFile <- "claims.xdf"
  inFile <- file.path(dataPath = rxGetOption(opt = "sampleDataDir"), inFile)
  inXdfDS <- RxXdfData(file = inFile)
  inDF <- rxImport(inData = inXdfDS)

  df1 <- inDF[1:50,]
  df2 <- inDF[51:nrow(inDF),]

  # create an output partitioned Xdf data source
  outFile <- file.path(tempdir(), "partitionedClaims")
  outPartXdfDataSource <- RxXdfData(file = outFile, createPartitionSet = TRUE)

  # construct the partitioned Xdf from the first data set df1 and save to disk
  partDF1 <- rxPartition(inData = df1, outData = outPartXdfDataSource, varsToPartition = c("car.age", "type"), append = "none", overwrite = TRUE)

  # define an user function that counts number of rows in each partition
  ".Count" <- function(keys, data)
  {
    myDF <- rxImport(inData = data)
    return(nrow(myDF))
  }

  # local compute context
  rxSetComputeContext("localpar")
  rxOptions(numCoresToUse = 4)

  # call rxExecBy to execute the user function on partitions created from the first data set
  results1 <- rxExecBy(inData = outPartXdfDataSource, keys = c("car.age", "type"), func = .Count)

  # append data from the second data set to the existing partitioned Xdf
  partDF2 <- rxPartition(inData = df2, outData = outPartXdfDataSource, varsToPartition = c("car.age", "type"))

  # define another user function
  ".linMod" <- function(keys, data)
  {
    result <- rxLinMod(formula = cost ~ number, data = data)
    return(result$coefficients[[1]])
  }

  # call rxExecBy to execute the new user function on partitions created from the data combined
  # from both data sets df1 and df2
  results2 <- rxExecBy(inData = outPartXdfDataSource, keys = c("car.age", "type"), func = .linMod)

  # clean-up: delete the partitioned Xdf
  unlink(outFile, recursive = TRUE, force = TRUE)
 ## End(Not run) 
  
 
```
 
 
 
 
 
 
