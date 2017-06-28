--- 
 
# required metadata 
title: "Get Data Source Information" 
description: " Get basic information about an RevoScaleR data source or data frame  " 
keywords: "RevoScaleR, rxGetInfo, attribute" 
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
 
 
 #`rxGetInfo`: Get Data Source Information

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
Get basic information about an RevoScaleR data source or data frame 
 
 
 ##Usage

```   
  
  rxGetInfo(data, getVarInfo = FALSE, getBlockSizes = FALSE,
            getValueLabels = NULL, varsToKeep = NULL, varsToDrop = NULL,
            startRow = 1, numRows = 0, computeInfo = FALSE,
            allNodes = TRUE, verbose = 0) 
               
  
 
```
 
 ##Arguments

   
    
 ### `data`
 a data frame, a character string specifying an .xdf, or an [RxDataSource](../../r-reference/revoscaler/rxdatasource.md) object. If a local compute context is being used,  this argument may also be a list of data sources,  in which case the output will be returned in a named list. See the details section for more information.  
  
  
    
 ### `getVarInfo`
 logical value. If `TRUE`, variable information is returned. 
  
  
    
 ### `getBlockSizes`
 logical value. If `TRUE`, block sizes are returned in the output for an .xdf file, and when printed the first 10 block sizes are shown. 
  
  
    
 ### `getValueLabels`
 logical value. If `TRUE` and `getVarInfo` is `TRUE` or `NULL`, factor value labels are included in the output. 
  
  
    
 ### `varsToKeep`
 character vector of variable names for which information is returned. If `NULL` or `getVarInfo` is `FALSE`, argument is ignored. Cannot be used with `varsToDrop`. 
  
  
    
 ### `varsToDrop`
 character vector of variable names for which information is not returned. If `NULL` or `getVarInfo` is `FALSE`, argument is ignored. Cannot be used with `varsToKeep`. 
  
  
    
 ### `startRow`
 starting row for retrieval of data if a data frame or .xdf file. 
  
  
    
 ### `numRows`
 number of rows of data to retrieve if a data frame or .xdf file. 
  
  
    
 ### `computeInfo`
 logical value. If `TRUE`, and `getVarInfo` is `TRUE`, variable information  (e.g., high/low values) for non-xdf data sources will be computed  by reading through the data set. If `TRUE`, and `getVarInfo` is `FALSE`, the number of variables will be gotten from from non-xdf data sources  (but not the number of rows). 
  
  
    
 ### `allNodes`
 logical value.  Ignored if the active [RxComputeContext](../../r-reference/revoscaler/rxcomputecontext.md) compute context is local or `RxForeachDoPar`.  Otherwise, if `TRUE`, a list containing the information for the data set on each node in the active compute context will be returned.  If `FALSE`, only information on the data set on the master node will be returned. Note that the determination of the master node is not controlled by the end user. See the *RevoScaleR Distributed Computing Guide* for more information on master node computations.  
   
  
    
 ### `verbose`
 integer value. If `0`, no additional output is printed.  If `1`, additional summary information is printed for an .xdf file. 
  
 
 
 ##Details
 
If a local compute context is being used, the `data` and `file` 
arguments may be a list of data source objects, e.g.,
`data = list(iris, airquality, attitude)`, 
in which case a named list of results are returned. For `rxGetInfo`, a mix of supported data sources
is allowed. Note that data
frames should not be specified in quotes because, in that case, they will be interpreted as .xdf data paths.

If the [RxComputeContext](../../r-reference/revoscaler/rxcomputecontext.md) is distributed, `rxGetInfo` will request information from the
compute context nodes.  
 
 
 
 ##Value
 
list containing the following possible elements:

###`fileName`
character string containing the file name and path (if an .xdf file).


###`objName`
character string containing object name (if not an .xdf file).


###`class`
class of the object if an R object.


###`length`
length of the object if it is not an .xdf file or data frame.


###`numCompositeFiles`
number of composite data files(if a composite .xdf file).


###`numRows`
number of rows in the data set.


###`numVars`
number of variables in the data set.


###`numBlocks`
number of blocks in the data set.


###`varInfo`
list of variable information where each element is a list describing a variable. (See return value of [rxGetVarInfo](rxGetVarInfoXdf.md) for more information.)


###`rowsPerBlock`
integer vector containing number of rows in each block (if `getBlockSizes` is set to `TRUE`). Set to `NULL` if `data` is a data frame.


###`data`
data frame containing the data (if `numRows > 0`)

 If using a distributed compute context with the `allNodes` set to `TRUE`,
 a list of lists with information on the data from each node will be returned.   
 
 
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[rxDataStep](rxDataStep.md),
[rxGetVarInfo](rxGetVarInfoXdf.md),
[rxSetVarInfo](rxSetVarInfoXdf.md).
   
 ##Examples

 ```
   
  # Summary information about a data frame
  fileInfo <- rxGetInfo(data = iris, numRows = 5)
  fileInfo
  
  # Summary information about an .xdf file
  mortFile <- file.path(rxGetOption("sampleDataDir"), "CensusWorkers")
  infoObj <- rxGetInfo(mortFile, getBlockSizes=TRUE)
  numBlocks <- infoObj$numBlocks
  aveRowsPerBlock <- infoObj$numRows/numBlocks
  rowsPerBlock <- infoObj$rowsPerBlock
  aveRowsPerBlock1 <- mean(rowsPerBlock)
  
  # Obtain information on a variety of data sources
  fourthGradersXDF <- file.path(rxGetOption("sampleDataDir"), "fourthgraders.xdf")
  KyphosisDS <- RxXdfData(file.path(rxGetOption("sampleDataDir"), "Kyphosis.xdf"))
  claimsTxtDS <- RxTextData(file.path(rxGetOption("sampleDataDir"), "claims.txt"))
  rxGetInfo(data = list(iris, fourthGradersXDF, KyphosisDS, claimsTxtDS))
 
```
 
 
 
