--- 
 
# required metadata 
title: "Partition Data by Key Values and Execute User Function on Each Partition" 
description: " Partition input data source by keys and apply user defined function on individual partitions. If input data source is already partitioned, apply user defined function on partitions directly. Currently supported in local, localpar, [RxInSqlServer](RxInSqlServer.md) and [RxSpark](RxSpark.md) compute contexts. " 
keywords: "RevoScaleR, rxExecBy, ExecBy" 
author: "HeidiSteen" 
manager: "jhubbard" 
ms.date: "06/23/2017" 
ms.topic: "reference" 
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
 
 
 
 #rxExecBy: Partition Data by Key Values and Execute User Function on Each Partition

 Applies to version 9.1.0 of package RevoScaleR.
 
 
 ##Description
 
Partition input data source by keys and apply user defined function on individual partitions.
If input data source is already partitioned, apply user defined function on partitions directly.
Currently supported in `local`, `localpar`, [RxInSqlServer](RxInSqlServer.md) and
[RxSpark](RxSpark.md) compute contexts.
 
 
 
 ##Usage

```   
  rxExecBy(inData, keys, func, funcParams = NULL, filterFunc = NULL, computeContext = rxGetOption("computeContext"), ...)
  
 
```
 
 
 ##Arguments

   
    
 ### `inData`
 a data source object suppported in currently active compute context, e.g., [RxSqlServerData](RxSqlServerData.md) for [RxInSqlServer](RxInSqlServer.md) and [RxHiveData](RxSparkData.md) for [RxSpark](RxSpark.md). In `local` and `localpar` compute contexts, a character string specifying a .xdf file or a data frame object can be also used. 
  
  
    
 ### `keys`
 character vector of variable names to specify the values in those variables are used for partitioning. 
  
  
    
 ### `func`
 the user function to be executed. The user function takes `keys` and `data` as two required input arguments where `keys` determines the partitioning values and `data` is a data source object of the corresponding partition. `data` can be a [RxXdfData](RxXdfData.md) object or a RxODBCData object, which can be transformed to a standard R data frame by using [rxDataStep](rxDataStep.md) method. The nodes or cores on which it is running are determined by the currently active compute context. 
  
  
    
 ### `funcParams`
 list of additional arguments for the user function `func`. 
  
  
    
 ### `filterFunc`
 the user function that takes a data frame of keys values as an input argument, applies filter to the keys values and returns a data frame containing rows whose keys values satisfy the filter conditions. The input data frame has similar format to the results returned by rxPartition which comprises of partitioning variables and an additional variable of partition data source. This `filterFunc` allows user to control what data partitions to be applied by the user function `func`. `filterFunc` currently is not supported in [RxHadoopMR](RxHadoopMR.md) and [RxSpark](RxSpark.md) compute contexts. 
  
  
    
 ### `computeContext`
 a RxComputeContext object. 
  
  
    
 ### ` ...`
 additional arguments to be passed directly to the Compute Engine. 
  
 
 
 
 ##Value
 
A list which is the same length as the number of partitions in the `inData` argument. Each
element in the top level list contains a three element list described below.

###`keys`
a list which contains key values for the partition.


###`result`
the object returned from the invocation of the user function with `keys` values. When an error occurs during the invocation of the user function the value will be `NULL`.


###`status`
a string which takes the value of either `"OK"` or `"Error"`. In [RxSpark](RxSpark.md) compute context, it may include additional warning and error messages.

 
 
 ##Note
 
The user function can call any function defined in R packages which are
loaded by default and pass parameters which are defined in base R, the default loaded packages, or
user defined S3 classes. The user function won't work with global variables or functions in
non-default loaded packages unless they are redefined or reloaded within the scope of the user function.
 
 
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 
 ##See Also
 
[rxExecByPartition](rxExecByPartition.md),
[rxImport](rxImport.md),
[rxDataStep](rxDataStep.md),
[RxTextData](RxTextData.md),
[RxXdfData](RxXdfData.md),
[RxHiveData](RxSparkData.md),
[RxSqlServerData](RxSqlServerData.md)
   
 ##Examples

 ```
   
      ## Not run:
 
  ##############################################################################
  # run analytics with local compute context
  ##############################################################################

    # create an input xdf data source.
    inFile <- "claims.xdf"
    inFile <- file.path(dataPath = rxGetOption(opt = "sampleDataDir"), inFile)
    inXdfDataSource <- RxXdfData(file = inFile)

    # define an user function that builds a linear model
    ".linMod" <- function(keys, data)
    {
      result <- rxLinMod(formula = cost ~ number, data = data)
      return(result$coefficients[[1]])
    }

    # set local compute context with 4-way parallel
    rxSetComputeContext("localpar")
    rxOptions(numCoresToUse = 4)

    # define a filter function
    ".carFilter" <- function(partDF)
    {
        subset(partDF, car.age != "10+" & type == "A")
    }

    # call rxExecBy with no filterFunction
    results1 <- rxExecBy(inData = inXdfDataSource, keys = c("car.age", "type"), func = .linMod)

    # call rxExecBy with filterFunction
    results2 <- rxExecBy(inData = inXdfDataSource, keys = c("car.age", "type"), func = .linMod, filterFunc = .carFilter)

  ##############################################################################
  # run analytics with SQL Server compute context
  ##############################################################################

    # Note: for improved security, read connection string from a file, such as
    # sqlServerConnString <- readLines("sqlServerConnString.txt")

    sqlServerConnString <- "SERVER=hostname;DATABASE=RevoTestDB;UID=DBUser;PWD=Password;"
    inTable <- paste0("airlinedemosmall")
    sqlServerDataDS <- RxSqlServerData(table = inTable, connectionString = sqlServerConnString)

    # user function
    ".Count" <- function(keys, data, params)
    {
        myDF <- rxImport(inData = data)
        return (nrow(myDF))
    }

    # Set SQL Server compute context with level of parallelism = 2
    sqlServerCC <- RxInSqlServer(connectionString = sqlServerConnString, numTasks = 4)
    rxSetComputeContext(sqlServerCC)

    # Execute rxExecBy in SQL Server compute context
    sqlServerCCResults <- rxExecBy(inData = sqlServerDataDS, keys = c("DayOfWeek"), func = .Count)

  ##############################################################################
  # run analytics with RxSpark compute context
  ##############################################################################
    # start Spark app
    sparkCC <- rxSparkConnect()

    # define function to compute average delay
    ".AverageDelay" <- function(keys, data) {
      df <- rxDataStep(data)
      mean(df$ArrDelay, na.rm = TRUE)
    }

    # define colInfo
    colInfo <-
      list(
        ArrDelay = list(type = "numeric"),
        CRSDepTime = list(type = "numeric"),
        DayOfWeek = list(type = "string")
      )

    # create text data source with airline data
    textData <-
      RxTextData(
        file = "/share/SampleData/AirlineDemoSmall.csv",
        firstRowIsColNames = TRUE,
        colInfo = colInfo,
        fileSystem = RxHdfsFileSystem()
      )

    # group textData by day of week and get average delay on each day
    objs <- rxExecBy(textData, keys = c("DayOfWeek"), func = .AverageDelay)

    # transform objs to a data frame
    do.call(rbind, lapply(objs, unlist))

    # stop Spark app
    rxSparkDisconnect(sparkCC)
   ## End(Not run) 
  
 
```
 
