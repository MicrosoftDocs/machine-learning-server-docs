--- 
 
# required metadata 
title: "Read .xdf File" 
description: " Read data from an .xdf file into a data frame. " 
keywords: "RevoScaleR, rxReadXdf, file, connection" 
author: "heidisteen" 
manager: "jhubbard" 
ms.date: "04/18/2017" 
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
 
 
 #`rxReadXdf`: Read .xdf File

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
Read data from an .xdf file into a data frame.
 
 
 ##Usage

```   
  rxReadXdf(file, varsToKeep = NULL, varsToDrop = NULL, rowVarName = NULL,
            startRow = 1, numRows = -1, returnDataFrame = TRUE,
            stringsAsFactors = FALSE, maxRowsByCols = NULL,
            reportProgress = rxGetOption("reportProgress"), readByBlock = FALSE,
            cppInterp = NULL) 
 
```
 
 ##Arguments

   
    
 ### `file`
 either an RxXdfData object or a character string specifying the .xdf file. 
  
  
    
 ### `varsToKeep`
 character vector of variable names to include when reading from the input data file. If `NULL`, argument is ignored. Cannot be used with `varsToDrop`. 
  
  
    
 ### `varsToDrop`
 character vector of variable names to exclude when reading from the input data file. If `NULL`, argument is ignored. Cannot be used with `varsToKeep`. 
  
  
    
 ### `rowVarName`
 optional character string specifying the variable in the data file to use as row names for the output data frame. 
  
  
    
 ### `startRow`
 starting row for retrieval. 
  
  
    
 ### `numRows`
 number of rows of data to retrieve. If -1, all are read. 
  
  
    
 ### `returnDataFrame`
 logical indicating whether or not to create a data frame. If `FALSE`, a list is returned. 
  
  
    
 ### `stringsAsFactors`
 logical indicating whether or not to convert strings into factors in R. 
  
  
    
 ### `maxRowsByCols`
 the maximum size of a data frame that will be read in, measured by the number of rows times the number of columns. If the numer of rows times the number of columns being extracted from the .xdf file exceeds this, a warning will be reported and a smaller number of rows will be read in than requested. If `maxRowsByCols` is set to be too large, you may experience problems  from loading a huge data frame into memory. To extract a subset of rows  and/or columns from an .xdf file, use [rxDataStep](../../r-reference/revoscaler/rxdatastep.md). 
  
  
    
 ### `reportProgress`
 integer value with options:  
*   `0`: no progress is reported. 
*   `1`: the number of processed rows is printed and updated. 
*   `2`: rows processed and timings are reported. 
*   `3`: rows processed and all timings are reported. 
  
  
  
    
 ### `readByBlock`
 read data by blocks. This argument is deprecated. 
  
  
    
 ### `cppInterp`
 list of information sent to C++ interpreter. 
  
 
 
 ##Value
 
if `returnDataFrame` is `TRUE`, a data frame is returned. Otherwise
a list is returned.
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[rxImport](rxImport.md),
[rxDataStep](../../r-reference/revoscaler/rxdatastep.md).
   
 ##Examples

 ```
   
  # Example reading the first 10 rows from the CensusWorkers xdf file
  censusWorkers <- file.path(rxGetOption("sampleDataDir"), "CensusWorkers")
  myCensusDF <- rxReadXdf(censusWorkers, numRows = 10)
  myCensusDF
 
```
 
 
 
