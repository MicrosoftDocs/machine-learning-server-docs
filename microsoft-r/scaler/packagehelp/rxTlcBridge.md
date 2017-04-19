--- 
 
# required metadata 
title: "TLC Bridge" 
description: " Bridge code for additional packages " 
keywords: "RevoScaleR, rxTlcBridge" 
author: "heidisteen" 
manager: "jhubbard" 
ms.date: "04/17/2017" 
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
 
 
 #`rxTlcBridge`: TLC Bridge

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
Bridge code for additional packages
 
 
 ##Usage

```   
  rxTlcBridge(formula = NULL, data = NULL, outData = NULL,
          outDataFrame = FALSE, overwrite = FALSE,
          weightVar = NULL, groupVar = NULL, nameVar = NULL, 
          customVar = NULL, analysisType = NULL,
          mamlCode = NULL, mamlTransformVars = NULL, 
          modelInfo = NULL,    
          testModel = FALSE, saveTextData = FALSE, saveBinaryData = FALSE,
          rowSelection = NULL,  varsToKeep = NULL, 
          transforms = NULL, transformObjects = NULL,
          transformFunc = NULL, transformVars = NULL, 
          transformPackages = NULL, transformEnvir = NULL,  
          rowsPerWrite = 500000, 
          blocksPerRead = rxGetOption("blocksPerRead"), 
          reportProgress = rxGetOption("reportProgress"), verbose = 1,
          computeContext = rxGetOption("computeContext"), ...)
 
```
 
 ##Arguments

   
    
 ### `formula`
 formula as described in [rxFormula](rxFormula.md). 
  
  
    
 ### `data`
 either a data source object, a character string specifying a .xdf file, a data frame object, or `NULL`. 
  
  
    
 ### `outData`
 optional output data source. 
  
  
    
 ### `outDataFrame`
 logical.  If `TRUE`, a data frame is returned. 
  
  
    
 ### `overwrite`
 logical.  If `TRUE`, the output data source will be overwritten. 
  
  
    
 ### `weightVar`
 character string or `NULL`. Optional name of weight variable. 
  
   
    
 ### `groupVar`
 character string or `NULL`. Optional name of group variable. 
  
   
    
 ### `nameVar`
 character string or `NULL`. Optional name of name variable. 
  
   
    
 ### `customVar`
 character string or `NULL`. Optional name of custom variable. 
  
   
    
 ### `analysisType`
 character string or `NULL`. Type of analysis being processed. 
  
   
    
 ### `mamlCode`
 character string or `NULL`. Desired MAML code. 
  
  
    
 ### `mamlTransformVars`
 character vector of variable names used in MAML transforms. 
  
  
     
  
    
 ### `modelInfo`
 list or `NULL`. List containing additional model information. 
  
  
     
  
    
 ### `testModel`
 If `TRUE`, the trained model is tested. 
  
  
    
 ### `saveTextData`
 If `TRUE`, the data used in the model is saved to a text file. 
  
  
    
 ### `saveBinaryData`
 If `TRUE`, the data used in the model is saved to a binary file. 
  
  
    
 ### `rowSelection`
 name of a logical variable in the data set (in quotes) or a logical expression using variables in the data set to specify row selection.  For example, `rowSelection = "old"` will use only observations in which the value of the variable `old` is `TRUE`.  `rowSelection = (age > 20) & (age < 65) & (log(income) > 10)` will use only observations in which the value of the `age` variable is between 20 and 65 and the value of the `log` of the `income` variable is greater than 10.  The row selection is performed after processing any data transformations  (see the arguments `transforms` or `transformFunc`). As with all expressions, `rowSelection` can be defined outside of the function  call using the expression function. 
  
  
    
 ### `varsToKeep`
 `NULL` or character vector specifying the variables to keep when writing out the data set.  Ignored if a model is specified. 
  
  
    
 ### `transforms`
 an expression of the form `list(name = expression, ...)` representing the first round of variable transformations. As with all expressions, `transforms` (or `rowSelection`)  can be defined outside of the function call using the expression function. 
  
  
    
 ### `transformObjects`
 a named list containing objects that can be referenced by `transforms`, `transformsFunc`, and `rowSelection`. 
  
  
    
 ### `transformFunc`
 variable transformation function. See [rxTransform](rxTransform.md) for details. 
  
  
    
 ### `transformVars`
 character vector of input data set variables needed for the transformation function. See [rxTransform](rxTransform.md) for details. 
  
  
    
 ### `transformPackages`
 character vector defining additional R packages (outside of those specified in `rxGetOption("transformPackages")`) to be made available and  preloaded for use in variable transformation functions, e.g., those explicitly defined in **RevoScaleR** functions via their `transforms` and `transformFunc` arguments or those  defined implicitly via their `formula` or `rowSelection` arguments.  The `transformPackages` argument may also be `NULL`,  indicating that no packages outside `rxGetOption("transformPackages")` will be preloaded. 
  
  
    
 ### `transformEnvir`
 user-defined environment to serve as a parent to  all environments developed internally and used for variable data transformation. If `transformEnvir = NULL`, a new "hash" environment with parent `baseenv()` is used instead. 
  
  
    
 ### `rowsPerWrite`
 Not implemented. 
  
  
    
 ### `blocksPerRead`
 number of blocks to read for each chunk of data read from the data source. 
  
  
    
 ### `reportProgress`
 integer value with options:  
*   `0`: no progress is reported. 
*   `1`: the number of processed rows is printed and updated. 
*   `2`: rows processed and timings are reported. 
*   `3`: rows processed and all timings are reported. 
  
  
  
    
 ### `verbose`
 integer value. If `0`, no verbose output is printed during calculations. Integer values from `1` to `4` provide increasing amounts of information are provided. 
  
  
  
 ### `computeContext`
 a valid [RxComputeContext](RxComputeContext.md).  The `RxHpcServer`, `RxHadoopMR`, and `RxInTeradata` compute  contexts distribute the computation among the nodes specified by the  compute context; for other compute contexts, the  computation is distributed if possible on the local computer. 
  
   
    
 ### ` ...`
 additional arguments to be processed or passed  to the base computational function. 
  
 
 
 
 ##Details
 
This function does not provide direct user functionality.
 
 
 ##Value
 
an rxTlcBridge object, or an output data source or data frame

 
 ##Author(s)
 
Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)

 
 
