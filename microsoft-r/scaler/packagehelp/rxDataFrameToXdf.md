--- 
 
# required metadata 
title: "Data Frame to .xdf File" 
description: " Deprecated. Write a data frame to an .xdf file. " 
keywords: "RevoScaleR, ScaleR, KEYWORDS" 
author: "richcalaway" 
manager: "jhubbard" 
ms.date: "02/17/2017" 
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
 
 
 #Data Frame to .xdf File 
 ##Description
 
Deprecated. Write a data frame to an .xdf file.
 
 
 ##Usage

```   
  rxDataFrameToXdf(data, outFile, varsToKeep = NULL, varsToDrop = NULL,
                   rowVarName = NULL, append = "none", overwrite = FALSE,
                   computeLowHigh = TRUE,  
                   xdfCompressionLevel = rxGetOption("xdfCompressionLevel")) 
 
```
 
 ##Arguments

   
    
 >  `data` -  data frame to put into .xdf file. See details section for a listing of the supported column types in the data frame.  
  
    
 >  `outFile` -  either an RxXdfData object or a character string specifying the .xdf file.  
  
    
 >  `varsToKeep` -  character vector of variable names to include in the data file. If `NULL`, argument is ignored. Cannot be used with `varsToDrop`.  
  
    
 >  `varsToDrop` -  character vector of variable names to not include in the data file. If `NULL`, argument is ignored. Cannot be used with `varsToKeep`.  
  
    
 >  `append` -  either `"none"`, `"rows"` to append rows to an existing file, or `"cols"` to append columns to an existing file. If `outFile` exists and `append` is `"none"`, the `overwrite` argument must be set to `TRUE`.  
  
    
 >  `overwrite` -  logical value. If `TRUE`, an existing `outFile` will be overwritten, or if appending columns existing columns with the same name will be overwritten. `overwrite` is ignored if appending rows.  
  
    
 >  `computeLowHigh` -  logical value. If `FALSE`, low and high values will not automatically be computed. This should only be set to `FALSE` in special circumstances, such as when `append` is being used repeatedly.  
  
    
 >  `rowVarName` -  character string or `NULL`. If `NULL`, the data frame's row names will be dropped. If a character string, an additional variable of that name will be added to the data set containing the data frame's row names.   
  
     
 >  `xdfCompressionLevel` -  integer in the range of -1 to 9.  The higher the value, the greater the  amount of compression - resulting in smaller files but a longer time to create them. If  `xdfCompressionLevel` is set to 0, there will be no compression and files will be compatible  with the 6.0 release of Revolution R Enterprise.  If set to -1, a default level of compression  will be used.   
 
 
 
 ##Details
 
The main side effect of this function is to create an output .xdf file
using the data from the input data frame. All columns in the input data frame
must be one of the following types:


*  `"logical"` (stored as `uchar`),

*  `"integer"` (stored as `int32`),

*  `"numeric"` (stored as `float64`),

*  `"character"` (stored as `string`),

*  `"factor"` (stored as `uint32`),

*  `"Date"` (stored as Date, i.e. `float64`)



 Note that [rxDataStep](rxDataStep.md) can also be used to create an .xdf file
 from a data frame. 
 
 
 
 ##Value
 
An [RxXdfData](RxXdfData.md) object representing the .xdf file.
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](http://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 
 ##See Also
 
[rxDataStep](rxDataStep.md).
   
 ##Examples

 ```
   
  # Convert included 'cars' data frame to .xdf file
  rxDataFrameToXdf(data = cars, outFile = file.path(tempdir(), "cars.xdf"), overwrite = TRUE)
  
  # Add columns from a data frame to an existing .xdf file
  # Create a data frame with 100 rows
  myExtraData <- data.frame(xextra = 1:100, yextra = rep(1:25, times = 4),
                            zextra = rep(1:25, each = 4))
  
  # Make a copy of an existing .xdf file that has 100 rows
  test100r5b <- file.path(rxGetOption("unitTestDataDir"), "test100r5b.xdf")
  outputFile <- file.path(tempdir(), "rxTestFile1.xdf")
  file.copy(from = test100r5b, to = outputFile, overwrite = TRUE)
  
  # Put a selection of columns from the data frame into the xdf file
  # Include the row names in a column called "myRowNames"
  varsToKeep <- c("xextra", "zextra", "myRowNames")
  rxDataFrameToXdf(data = myExtraData, outFile = outputFile,
                   varsToKeep = varsToKeep, rowVarName = "myRowNames",
                   overwrite = TRUE)
 
```
 
 
 
 
