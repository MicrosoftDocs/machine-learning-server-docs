--- 
 
# required metadata 
title: "rxXdfToText function (RevoScaleR) | Microsoft Docs" 
description: " Write .xdf file content to a delimited text file. rxDataStep recommended. " 
keywords: "(RevoScaleR), rxXdfToText, file, connection" 
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
 
 
 #rxXdfToText: Export .xdf File to Delimited Text File 
 ##Description
 
Write .xdf file content to a delimited text file. `rxDataStep` recommended.
 
 
 ##Usage

```   
  rxXdfToText(inFile, outFile, varsToKeep = NULL, 
  		varsToDrop = NULL, rowSelection = NULL, 
  		transforms = NULL, transformObjects = NULL,
  		transformFunc = NULL, transformVars = NULL,  
  		transformPackages = NULL, transformEnvir = NULL, 
  		overwrite = FALSE, sep = ",", quote = TRUE, na = "NA", 
  		eol = "\n", col.names = TRUE,
  		blocksPerRead = rxGetOption("blocksPerRead"),
  		reportProgress = rxGetOption("reportProgress"), ...) 
 
```
 
 ##Arguments

   
    
 ### `inFile`
 either an RxXdfData object or a character string specifying the input .xdf file. 
  
  
    
 ### `outFile`
 character string specifying the output delimited text file. 
  
  
    
 ### `varsToKeep`
 character vector of variable names to include when reading from `inFile`. If `NULL`, argument is ignored. Cannot be used with `varsToDrop`. 
  
  
    
 ### `varsToDrop`
 character vector of variable names to exclude when reading from `inFile`. If `NULL`, argument is ignored. Cannot be used with `varsToKeep`. 
  
  
    
 ### `rowSelection`
 name of a logical variable in the data set (in quotes) or a logical expression using variables in the data set to specify row selection.  For example, `rowSelection = "old"` will use only observations in which the value of the variable `old` is `TRUE`.  `rowSelection = (age > 20) & (age < 65) & (log(income) > 10)` will use only observations in which the value of the `age` variable is between 20 and 65 and the value of the `log` of the `income` variable is greater than 10.  The row selection is performed after processing any data transformations  (see the arguments `transforms` or `transformFunc`). As with all expressions, `rowSelection` can be defined outside of the function  call using the expression function. 
  
  
    
 ### `transforms`
 an expression of the form `list(name = expression, ...)` representing the first round of variable transformations. As with all expressions, `transforms` (or `rowSelection`)  can be defined outside of the function call using the expression function. 
  
  
    
 ### `transformObjects`
 a named list containing objects that can be referenced by `transforms`, `transformsFunc`, and `rowSelection`. 
  
  
    
 ### `transformFunc`
 variable transformation function. See [rxTransform](rxTransform.md) for details. 
  
  
    
 ### `transformVars`
 character vector of input data set variables needed for the transformation function. See [rxTransform](rxTransform.md) for details. 
  
  
    
 ### `transformPackages`
 character vector specifying additional R packages (outside of those specified in `rxGetOption("transformPackages")`) to be made available and  preloaded for use in variable transformation functions, both those explicitly defined in **RevoScaleR** functions via their `transforms` and `transformFunc` arguments and those  defined implicitly via their `formula` or `rowSelection` arguments.  The `transformPackages` argument may also be `NULL`,  indicating that no packages outside `rxGetOption("transformPackages")` will be preloaded. 
  
  
    
 ### `transformEnvir`
 user-defined environment to serve as a parent to  all environments developed internally and used for variable data transformation. If `transformEnvir = NULL`, a new "hash" environment with parent `baseenv()` is used instead. 
  
  
    
 ### `overwrite`
 logical value. If `TRUE`, the existing `outFile` will be overwritten. 
  
  
    
 ### `sep`
 character(s) specifying the separator between columns. 
  
  
    
 ### `quote`
 logical value or numeric vector. If `TRUE`, any character or factor columns will be surrounded by double quotes. If a numeric vector, its elements are taken as the indices of columns to quote. In both cases, row and column names are quoted if they are written. If `FALSE`, nothing is quoted. 
  
  
    
 ### `na`
 character string to use for missing values in the data. 
  
  
    
 ### `eol`
 character(s) to print at the end of each line (row). For example, `eol = "\r\n"` will produce Windows' line endings on a Unix-alike OS, and `eol = "\r"` will produce files as expected by Mac OS Excel 2004. 
  
  
    
 ### `col.names`
 a logical value indicating whether the column names in the `inFile` should be written as the first row in the output text file. 
  
  
    
 ### `blocksPerRead`
 number of blocks to read for each chunk of data read from the data source. 
  
  
    
 ### `reportProgress`
 integer value with options:  
*   `0`: no progress is reported. 
*   `1`: the number of processed rows is printed and updated. 
*   `2`: rows processed and timings are reported. 
*   `3`: rows processed and all timings are reported. 
  
  
  
    
 ### ` ...`
 additional arguments passed to the write.table function. 
  
 
 
 ##Details
 
For most purposes, the more general [rxDataStep](rxDataStep.md) is preferred for writing to text files.
 
 
 ##Value
 
An [RxTextData](RxTextData.md) object representing the output text file.
 
 

 
 
 
 ##See Also
 
[rxDataStep](rxDataStep.md),
[rxImport](rxImport.md),
write.table,
[rxSplit](rxSplitXdf.md).
   
 ##Examples

 ```
   
  sampleDataDir <- rxGetOption("sampleDataDir")
  censusXdfFile <- file.path(rxGetOption("sampleDataDir"), "CensusWorkers.xdf")
  
  # Write all data in a text file
  censusCsvFile <- file.path(tempdir(), "CensusWorkers.csv")
  rxXdfToText(inFile = censusXdfFile, outFile = censusCsvFile, overwrite = TRUE)
  
  # Alternatively, use rxDataStep
  rxDataStep(inData = censusXdfFile, outFile = censusCsvFile, overwrite = TRUE)
  
  # Write subset of transformed data in a text file
  censusSubsetCsvFile <- file.path(tempdir(), "CensusSubset.csv")
  rxXdfToText(inFile = censusXdfFile, outFile = censusSubsetCsvFile,
              varsToKeep = c("age", "incwage"),
              rowSelection = age < 40,
              transforms = list(logIncwage = log1p(incwage), incwage = NULL),
              overwrite = TRUE)
              
  # Again, this can be done using rxDataStep
  rxDataStep(inData = censusXdfFile, outFile = censusSubsetCsvFile,
              varsToKeep = c("age", "incwage"),
              rowSelection = age < 40,
              transforms = list(logIncwage = log1p(incwage), incwage = NULL),
              overwrite = TRUE)
  # Clean-up
  file.remove(censusCsvFile)
  file.remove(censusSubsetCsvFile)          
 
```
 
 
 
