--- 
 
# required metadata 
title: "rxGetVarInfo function (RevoScaleR) | Microsoft Docs" 
description: " Get variable information for a RevoScaleR data source or data frame, including variable names, descriptions, and value labels " 
keywords: "(RevoScaleR), rxGetVarInfo, attribute" 
author: "heidisteen" 
manager: "jhubbard" 
ms.date: "09/07/2017" 
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
 
 
 #rxGetVarInfo: Get Variable Information for a Data Source 
 ##Description
 
Get variable information for a RevoScaleR data source or data frame, including variable
names, descriptions, and value labels
 
 
 ##Usage

```   
  rxGetVarInfo(data, getValueLabels = TRUE, varsToKeep = NULL,
                  varsToDrop = NULL, computeInfo = FALSE, allNodes = TRUE)
                  
                  
 
```
 
 ##Arguments

   
  
    
 ### `data`
 a data frame, a character string specifying the .xdf file, or an [RxDataSource](RxDataSource.md) object.  If a local compute context is being used,  this argument may also be a list of data sources,  in which case the output will be returned in a named list. See the details section for more information. 
  
  
    
 ### `getValueLabels`
 logical value. If `TRUE`, value labels (including factor  levels) are included in the output if present. 
  
  
    
 ### `varsToKeep`
 character vector of variable names for which information is returned. If `NULL`, argument is ignored. Cannot be used with `varsToDrop`. 
  
  
    
 ### `varsToDrop`
 character vector of variable names for which information is not returned. If `NULL`, argument is ignored. Cannot be used with `varsToKeep`. 
  
  
    
 ### `computeInfo`
 logical value. If `TRUE`,  variable information  (e.g., high/low values) for non-xdf data sources will be computed  by reading through the data set. 
  
  
    
 ### `allNodes`
 logical value.  Ignored if the active [RxComputeContext](RxComputeContext.md)compute context is local.  Otherwise, if `TRUE`, a list containing the variable information for the data set on each node in the active compute context will be returned.  If `FALSE`, only information on the data set on the master node will be returned.  
  
  
 
 
 
 ##Details
  Will also give partial information for lists and matrices.

If a local compute context is being used, the `data` and `file` arguments may be a list of data source objects, e.g.,
`data = list(iris, airquality, attitude)`, 
in which case a named list of results are returned. For `rxGetVarInfo`, a mix of supported data sources
is allowed. 

If the [RxComputeContext](RxComputeContext.md) is distributed, `rxGetVarInfo` will request information from the
compute context nodes.  
 
 
 ##Value
 
list with named elements corresponding to the variables in the data set. 
Each list element is also a list with with following possible elements:

###`description`
character string specifying the variable description


###`varType`
character string specifying the variable type


###`storage`
character string specifying the storage type


###`low`
numeric giving the low values, possibly generated through a temporary factor transformation `F()`


###`high`
numeric giving the high values, possibly generated through a temporary factor transformation `F()`


###`levels`
(factor only) a character vector containing the factor levels


###`valueInfoCodes`
character vector of value codes, for informational  purposes only


###`valueInfoLabels`
character vector of value labels that is the same length as `valueInfoCodes`, used for informational purposes only


 
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[rxSetVarInfo](rxSetVarInfoXdf.md),
[rxDataStep](rxDataStep.md).
   
 ##Examples

 ```
   
  # Specify name and location of sample data file
  censusWorkers <- file.path(rxGetOption("sampleDataDir"), "CensusWorkers")
  
  # Read the variable information
  varInfo <- rxGetVarInfo(censusWorkers)
  
  # Print the variable information
  varInfo
  
  # Get variable information about a built-in data frame
  rxGetVarInfo( iris )
  
  # Obtain variable information on a variety of data sources
  fourthGradersXDF <- file.path(rxGetOption("sampleDataDir"), "fourthgraders.xdf")
  KyphosisDS <- RxXdfData(file.path(rxGetOption("sampleDataDir"), "Kyphosis.xdf"))
  rxGetVarInfo(data = list(iris, fourthGradersXDF, KyphosisDS))
 
```
 
 
 
