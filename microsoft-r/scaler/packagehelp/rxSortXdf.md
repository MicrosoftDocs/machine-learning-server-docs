--- 
 
# required metadata 
title: " Variable sorting of an .xdf file or data frame. " 
description: " Efficient multi-key sorting of the variables in an .xdf file or data frame in a local compute context. " 
keywords: "RevoScaleR, rxSort, file" 
author: "richcalaway" 
manager: "jhubbard" 
ms.date: "03/23/2017" 
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
 
 
 #`rxSort`:  Variable sorting of an .xdf file or data frame. 

 Applies to version {PRODUCT_VERSION} of package RevoScaleR.
 
 ##Description
 
Efficient multi-key sorting of the variables in an .xdf file or data frame in a local compute context.
 
 
 ##Usage

```   
  
  rxSort(inData, outFile = NULL, sortByVars, decreasing = FALSE,
         type = "auto", missingsLow = TRUE, caseSensitive = FALSE,
         removeDupKeys = FALSE, varsToKeep = NULL, varsToDrop = NULL, 
         dupFreqVar = NULL, overwrite = FALSE, maxRowsByCols = 3000000, 
         bufferLimit = -1, reportProgress = rxGetOption("reportProgress"), 
         verbose = 0, xdfCompressionLevel = rxGetOption("xdfCompressionLevel"), 
         ...)
    
```
 
 
 ##Arguments

   
  
    
 ### `inData`
  a data frame, a character string denoting the path to an existing .xdf file, or an RxXdfData object.  
  
    
 ### `inFile`
  either an RxXdfData object or a character string denoting the path to an existing .xdf file.  
  
  
    
 ### `outFile`
  an .xdf path to store the sorted output. If `NULL`, a data frame with the sorted output  will be returned from `rxSort`.  
  
    
 ### `sortByVars`
  character vector containing the names of the variables to use for sorting. If multiple variables are used, the first `sortByVars` variable is sorted and common values are grouped. The second variable is then sorted within the first variable groups. The third variable is then sorted within groupings formed by the first two variables, and so on.  
  
    
 ### `decreasing`
  a logical scalar or vector defining the whether or not the `sortByVars` variables are  to be sorted in decreasing or increasing order. If a vector, the length `decreasing` must be that of `sortByVars`. If a logical scalar, the value of `decreasing` is replicated to the length `sortByVars`.  
  
  
    
 ### `type`
  a character string defining the sorting method to use.  Type `"auto"` automatically determines the sort method based on the amount of memory required for sorting. If possible, all of the data will be sorted in memory. Type `"mergeSort"` uses a merge sort method, where chunks of data are pre-sorted, then merged together. Type `"varByVar"` uses a variable-by-variable sort method,  which assumes that the `sortByVars` variables and the calculated sorted index variable can be held in memory simultaneously. If `type="varByVar"`, the variables in the sorted data are re-ordered so that the variables named in `sortByVars` come first, followed by any remaining variables.  
  
  
    
 ### `missingsLow`
  a logical scalar for controlling the treatment of missing values. If `TRUE`, missing values  in the data are treated as the lowest value; if `FALSE`, they are treated as the highest value.  
  
  
    
 ### `caseSensitive`
  a logical scalar. If `TRUE`, case sensitive sorting is performed for character data.  
  
  
   
   
   
   
  
    
 ### `removeDupKeys`
 logical scalar. If `TRUE`, only the first observation will be   kept when duplicate values of the key (`sortByVars`) are encountered. The sort  `type` must be set to `"auto"` or `"mergeSort"`.   
  
   
    
 ### `varsToKeep`
 character vector defining variables to keep when writing the output to file. If `NULL`, this argument is ignored. This argument takes precedence over `varsToDrop` if both are specified, i.e., both are not `NULL`.  If both `varsToKeep` and `varsToDrop` are `NULL` then all variables are  written to the data source. 
  
  
    
 ### `varsToDrop`
 character vector of variable names to exclude when writing the output data file. If `NULL`, this argument is ignored.  If both `varsToKeep` and `varsToDrop` are `NULL` then all variables are  written to the data source. 
  
  
    
 ### `dupFreqVar`
 character string denoting name of new variable for frequency counts if `removeDupKeys` is set to `TRUE`.  Ignored if `removeDupKeys` is set to `FALSE`.  If `NULL`, a new variable for frequency counts will not be created. 
  
  
    
 ### `overwrite`
 logical value. If `TRUE`, an existing `outFile` will be overwritten. 
  
  
  
 ### `maxRowsByCols`
 the maximum size of a data frame that will be returned if `outFile` is set to `NULL` and `inData` is an .xdf file , measured by the number of rows times the number of columns. If the number of rows times the number of columns being created from the .xdf file exceeds this, a warning will be reported and the number of rows in the returned data frame will be truncated. If `maxRowsByCols` is set to be too large, you may experience problems  from loading a huge data frame into memory. 
  
  
    
 ### `bufferLimit`
 integer specifying the maximum size of the memory buffer (in Mb)  to use in sorting when `type` is set to `"auto"`.  The default value of `bufferLimit = -1` will attempt to  determine an appropriate buffer limit based on system memory.  
  
  
    
 ### `reportProgress`
 integer value with options:  
*   `0`: no progress is reported. 
*   `1`: the number of processed rows is printed and updated. 
*   `2`: rows processed and timings are reported. 
*   `3`: rows processed and all timings are reported. 
  
  
  
    
 ### `verbose`
 integer value. If `0`, no additional output is printed.  If `1`, additional summary information is printed. 
  
  
     
 ### `xdfCompressionLevel`
 integer in the range of -1 to 9.  The higher the value, the greater the  amount of compression for the output file - resulting in smaller files but a longer time to create them. If  `xdfCompressionLevel` is set to 0, there will be no compression and the output file will be compatible  with the 6.0 release of Revolution R Enterprise.  If set to -1, a default level of compression  will be used. 
   
  
    
 ### `blocksPerRead`
 deprecated argument. It will be ignored. 
  
  
    
 ### ` ...`
 additional arguments to be passed directly to the Revolution Compute Engine. 
  
 
 
 ##Details
 
Using `sortbyVars`, multiple keys can be specified to perform an iterative, within-category
sorting of the variables in the `inData` or `inFile` .xdf file. 
The argument `varsToKeep` (or alternatively `varsToDrop`) is used
to define the set of sorted variables to store in the specified `outFile` file or the
returned data frame if `outData` is `NULL`.
The `sortbyVars` variables are automatically prepended to the set of output variables
defined by `varsToKeep` or `varsToDrop`.
 
 
 
 ##Value
 
If an `outFile` is not specified, a data frame with the sorted data
is returned. If an `outFile` is specified, an 
[RxXdfData](RxXdfData.md) data source is returned that can be used in
subsequent RevoScaleR analysis. If sorting is unsuccessful, `FALSE`
is returned.



 
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 
 ##See Also
 
sort
   
 ##Examples

 ```
   
  ###
  # Small data example
  ###
  
  # sort data frame, decreasing by one column and increasing by the other
  sortByVars <- c("Sepal.Length", "Sepal.Width")
  decreasing <- c(TRUE, FALSE)
  sortedIris <- rxSort( inData = iris, sortByVars = sortByVars,
  		decreasing = decreasing)
  
  # define file names and locations
  inXDF <- file.path(tempdir(), ".rxInFileTemp.xdf")
  outXDF1 <- file.path(tempdir(), ".rxOutFileTemp1.xdf")
  
  # Create xdf file for iris data
  rxDataStep(inData = iris, outFile = inXDF, overwrite = TRUE)
  rxGetInfo(inXDF)
  
  # sort the iris data set, first by sepal length in decreasing order
  # and then by sepal width in increasing order
  sortByVars <- c("Sepal.Length", "Sepal.Width")
  decreasing <- c(TRUE, FALSE)
  rxSort(inData = inXDF, outFile = outXDF1, sortByVars = sortByVars,
     decreasing = decreasing)
  z1 <- rxDataStep(inData = outXDF1)
  print(head(z1,10))
  
  # clean up
  if (file.exists(inXDF)) file.remove(inXDF)
  if (file.exists(outXDF1)) file.remove(outXDF1)
  
  ###
  # larger data example
  ###
  sampleDataDir <- rxGetOption("sampleDataDir")
  CensusPath <- file.path(sampleDataDir, "CensusWorkers.xdf")
  outXDF <- file.path(tempdir(), ".rxCensusSorted.xdf")
  
  # sort census data by 'age' and then 'incwage' in increasing 
  # and decreasing order, respectively. drop the 'perwt' and 'wkswork1
  # variables from the output.
  rxSort(inData = CensusPath, outFile = outXDF, sortByVars = c("age", "incwage"), 
            decreasing = c(FALSE, TRUE), varsToDrop = c("perwt", "wkswork1"))
  z <- rxDataStep(outXDF)
  print(head(z, 10))
  
  if (file.exists(outXDF)) file.remove(outXDF)
  
  ###
  # example removing duplicates and creating duplicate counts variable
  ###
  sampleDataDir <- rxGetOption("sampleDataDir")
  airDemo <- file.path(sampleDataDir, "AirlineDemoSmall.xdf")
  airDedup <- file.path(tempdir(), ".rxAirDedup.xdf")
  rxSort(inData = airDemo, outFile = airDedup, 
      sortByVars =  c("DayOfWeek", "CRSDepTime", "ArrDelay"),
      removeDupKeys = TRUE, dupFreqVar = "FreqWt")
      
  # Use the duplicate frequency as frequency weights in a regression
  linModObj <- rxLinMod(ArrDelay~CRSDepTime + DayOfWeek, 
      data = airDedup, fweights = "FreqWt")
  summary(linModObj)
  
  if (file.exists(airDedup)) file.remove(airDedup)
 
```
 
 
 
