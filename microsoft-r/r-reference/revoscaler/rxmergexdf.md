--- 
 
# required metadata 
title: " Merge two data sources " 
description: " Merge (join) two data sources on one or more match variables. In local compute context, the data sources may be sorted .xdf files or data frames. In [RxSpark](rxspark.md) compute context, the data sources may be [RxParquetData](rxsparkdata.md), [RxHiveData](rxsparkdata.md), [RxOrcData](rxsparkdata.md), [RxXdfData](rxxdfdata.md) or [RxTextData](rxtextdata.md). " 
keywords: "RevoScaleR, rxMerge, rxMergeXdf, file" 
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
 
 
 
 #rxMerge:  Merge two data sources 

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
Merge (join) two data sources on one or more match variables.
In local compute context, the data sources may be sorted .xdf files or data frames.
In [RxSpark](rxspark.md) compute context, the data sources may be [RxParquetData](rxsparkdata.md), [RxHiveData](rxsparkdata.md), [RxOrcData](rxsparkdata.md), [RxXdfData](rxxdfdata.md) or [RxTextData](rxtextdata.md).
 
 
 ##Usage

```   
  
  rxMerge( inData1, inData2 = NULL, outFile = NULL, matchVars = NULL, type = "inner",
           missingsLow = TRUE, autoSort = TRUE, duplicateVarExt = NULL,
           varsToKeep1 = NULL, varsToDrop1 = NULL, newVarNames1 = NULL,
           varsToKeep2 = NULL, varsToDrop2 = NULL, newVarNames2 = NULL,
           rowsPerOutputBlock = -1, decreasing = FALSE, overwrite = FALSE, 
           maxRowsByCols = 3000000, bufferLimit = -1, 
           reportProgress = rxGetOption("reportProgress"), verbose = 0, 
           xdfCompressionLevel = rxGetOption("xdfCompressionLevel"), ... ) 
           
  rxMergeXdf( inFile1, inFile2, outFile, matchVars = NULL, type = "inner",
              missingsLow = TRUE,	duplicateVarExt = NULL,
              varsToKeep1 = NULL, varsToDrop1 = NULL, newVarNames1 = NULL,
              varsToKeep2 = NULL, varsToDrop2 = NULL, newVarNames2 = NULL,
              rowsPerOutputBlock = -1, decreasing = FALSE, overwrite = FALSE, 
              bufferLimit = -1, reportProgress = rxGetOption("reportProgress"), 
              verbose = 0, xdfCompressionLevel = rxGetOption("xdfCompressionLevel"), ... )          
 
```
 
 
 ##Arguments

   
    
 ### inData1
  the first data set to merge.  In local compute context, a data frame, a character string denoting the path to an  existing .xdf file, or an [RxXdfData](rxxdfdata.md) object. If a list of [RxXdfData](rxxdfdata.md) objects is provided, they will be merged sequentially. In [RxSpark](rxspark.md) compute context, an [RxParquetData](rxsparkdata.md), [RxHiveData](rxsparkdata.md), [RxOrcData](rxsparkdata.md), [RxXdfData](rxxdfdata.md) or [RxTextData](rxtextdata.md) data source. If a list of data source objects is provided, they will be merged sequentially.  
  
  
    
 ### inFile1
  the first data set to merge; either a character string denoting the path to an  existing .xdf file or an [RxXdfData](rxxdfdata.md) object.   
  
  
     
 ### inData2
  the second data set to merge. In local compute context, a data frame, a character string denoting the path  to an existing .xdf file, or an [RxXdfData](rxxdfdata.md) object. Can be `NULL` if a list of [RxXdfData](rxxdfdata.md) objects is provided for `inData1`. In [RxSpark](rxspark.md) compute context, an [RxParquetData](rxsparkdata.md), [RxHiveData](rxsparkdata.md), [RxOrcData](rxsparkdata.md), [RxXdfData](rxxdfdata.md) or [RxTextData](rxtextdata.md) data source. Can be `NULL` if a list of data source objects is provided for `inData1`.  
  
  
     
 ### inFile2
  the second data set to merge; either a character string denoting the path  to an existing .xdf file or an [RxXdfData](rxxdfdata.md) object.  
  
  
    
 ### outFile
  in local compute context, an .xdf path to store the merged output. If the `outFile` already exists, `overwrite` must be set to `TRUE` to overwrite the file. If `NULL`, a data frame containing the merged data will be returned. In [RxSpark](rxspark.md) compute context, an [RxParquetData](rxsparkdata.md), [RxHiveData](rxsparkdata.md), [RxOrcData](rxsparkdata.md) or [RxXdfData](rxxdfdata.md) data source.  
  
  
    
 ### matchVars
  character vector containing the names of the variables to match for merging. In local compute context, the data sets MUST BE presorted in the same order by these variables, unless `autoSort` is set to `TRUE`. See [rxSort](rxsortxdf.md).  Not required for `type` equal to `"union"` or `"oneToOne"`.  
  
  
    
 ### type
  a character string defining the merge method to use:   
*   `"inner"` compares each row of `inData1` with each row of `inData2` to  find all pairs of rows in which the values of the `matchVars` are the same. 
*   `"oneToOne"` appends columns from `inData1` to `inData2`. Not supported in [RxSpark](rxspark.md) compute context. 
*   `"left"` includes all rows that match (as in `"inner"`) plus rows from `inData1` that no not have matches.  `NA`'s will be used for the values for variables from `inData2` that are not matched. 
*   `"right"` includes all rows that match (as in `"inner"`) plus rows from `inData2` that no not have matches.  `NA`'s will be used for the values for variables from `inData1` that are not matched. 
*   `"full"` is a combination of both `"left"` and `"right"` merge. 
*   `"union"` append rows from `inData2` to `inData1`. Not supported in [RxSpark](rxspark.md) compute context. The two input files must have the same number of columns with the same data types. 
  
  
  
    
 ### missingsLow
  a logical scalar for controlling the treatment of missing values. If `TRUE`, missing values  in the data are treated as the lowest value; if `FALSE`, they are treated as the highest value. Not supported in [RxSpark](rxspark.md) compute context.  
  
  
    
 ### autoSort
  a logical scalar for controlling whether or not to sort the input data sets by the `matchVars` before merging.  If `TRUE`, the data sets are sorted before merging; if `FALSE`,  it is assumed that the data sets are already sorted by the `matchVars`. Not supported in [RxSpark](rxspark.md) compute context.  
  
  
    
 ### duplicateVarExt
  a character vector of length two containing the extensions to be used for handling duplicate variable names in the two input data sets. These extensions are not applied to matching variables.  If `NULL`, file or data frame names will be used as the extension.  If the names are the same,  the extensions `1` and `2` will be used (unless there are other variables with those names.)  For example, if `duplicateVarExt = c("One", "Two")` and `inData1` and `inData2` both have the variable `y`, the output data set will contain the variables `y.One` and `y.Two`.  
  
  
    
 ### varsToKeep1
  character vector of variable names to include from the `inData1`. If `NULL`, argument is ignored. Cannot be used with `varsToDrop1`.  
  
  
    
 ### varsToDrop1
  character vector of variable names to exclude  from `inData1`. If `NULL`, argument is ignored. Cannot be used with `varsToKeep1`.  
  
  
    
 ### newVarNames1
 a named character vector of new names for variables from `inData1` when writing them to `outData`.  For example, specifying `c(x = "newx", y = "newy"` would give the input variables `x` and `y` the names `newx` and `newy` in the output data.  
  
   
  
    
 ### varsToKeep2
  character vector of variable names to include from the `inData2`. If `NULL`, argument is ignored. Cannot be used with `varsToDrop2`.  
  
  
    
 ### varsToDrop2
  character vector of variable names to exclude  from `inData2`. If `NULL`, argument is ignored. Cannot be used with `varsToKeep2`.  
  
  
    
 ### newVarNames2
  a named character vector of new names for variables from `inData2` when writing them to `outData`.  For example, specifying `c(x = "newx", y = "newy"` would give the input variables `x` and `y` the names `newx` and `newy` in the output data.  
  
  
    
 ### rowsPerOutputBlock
  an integer specifying how many rows should be written out to each block in the output .xdf file.  If set to -1, the smaller of the two average block sizes of the input data sets will be used. Ignored if `outData` is `NULL`. Not supported in [RxSpark](rxspark.md) compute context.  
  
  
    
 ### decreasing
  a logical scalar specifying whether or not the `matchVars` variables were  sorted in decreasing or increasing order. The input data must be sorted in the  same order.  
  
  
    
 ### overwrite
 logical value. If `TRUE`, an existing `outFile` will be overwritten. Ignored if `outData` is `NULL` 
  
  
  
 ### maxRowsByCols
 the maximum size of a data frame that will be returned if `outFile` is set to `NULL` and `inData` is an .xdf file , measured by the number of rows times the number of columns. If the number of rows times the number of columns being created from the .xdf file exceeds this, a warning will be reported and the number of rows in the returned data frame will be truncated. If `maxRowsByCols` is set to be too large, you may experience problems  from loading a huge data frame into memory. Not supported in [RxSpark](rxspark.md) compute context. 
  
  
    
 ### bufferLimit
 integer specifiying the maximum size of the memory buffer (in Mb)  to use in merging. The default value of `bufferLimit = -1` will attempt to  determine an appropriate buffer limit based on system memory. Not supported in [RxSpark](rxspark.md) compute context. 
  
  
    
 ### reportProgress
 integer value with options:  
*   `0`: no progress is reported. 
*   `1`: the number of processed rows is printed and updated. 
*   `2`: rows processed and timings are reported. 
*   `3`: rows processed and all timings are reported. 
 Not supported in [RxSpark](rxspark.md) compute context.  
   
  
    
 ### verbose
 integer value. If `0`, no additional output is printed.  If `1`, additional summary information is printed. 
  
  
     
 ### xdfCompressionLevel
 integer in the range of -1 to 9.  The higher the value, the greater the  amount of compression for the output file - resulting in smaller files but a longer time to create them. If  `xdfCompressionLevel` is set to 0, there will be no compression and the output file will be compatible  with the 6.0 release of Revolution R Enterprise.  If set to -1, a default level of compression  will be used. 
   
  
    
 ###  ...
 additional arguments to be passed directly to the Microsoft R Server Compute Engine. 
  
 
 
 ##Details
 
The arguments `varsToKeep1` (or alternatively `varsToDrop1`) and
`varsToKeep2` (or alternatively `varsToDrop2`) are used
to define the set of variables from the input files that will be stored 
in the specified merged `outFile` file. The `matchVars` must be included
in the variables read from the input files.
A single copy of the `matchVars` variables will be saved in the
`outFile` file.
 
 
 
 ##Value
 
For `rxMerge`: If an `outFile` is not specified, a data frame with 
the merged data is returned. If an `outFile` is specified, a 
data source object is returned that can be used in
subsequent RevoScaleR analysis. 
For `rxMergeXdf`: If merging is successful, `TRUE`
is returned; otherwise `FALSE` is returned.
 
 
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 
 
 ##See Also
 
sort
   
 ##Examples

 ```
   
  ###
  # Small data frame example
  ###
  
  x <- 1:20
  y <- 20:1
  df1 <- data.frame(x=x, y=y)
  
  x <- 20:1
  y <- 1:20
  df2 <- data.frame(x=x, y=y)
  
  # Merge the two data frames into an .xdf file, matching on the variable x
  outXDF <- file.path(tempdir(), ".rxTempOut.xdf")
  rxMerge(inData1 = df1, inData2 = df2, outFile=outXDF, matchVars = "x", overwrite=TRUE)
  
  # Read the data from the .xdf file into a data frame.  y.df1 and y.df2 should be the same
  df3 <- rxDataStep(inData = outXDF )
  df3
  
  if(file.exists(outXDF)) file.remove(outXDF)
  
  ###
  # Merge two data frames, matching y1 from the first with y2 from the second
  # Notice that the match variable is sorted in decreasing order.
  x1 <- 1:20
  y1 <- 20:1
  df1 <- data.frame(x1=x1, y1=y1)
     
  x2 <- 1:20
  y2 <- 25:6
  df2 <- data.frame(x2=x2, y2=y2)
     
  dfOut <- rxMerge(inData1 = df1, inData2 = df2, matchVars = "y2", decreasing=TRUE,
           newVarNames1 = c(y1 = "y2"))
  
  # Look at the resulting merged data 
  rxGetInfo( dfOut,numRows=10,getVarInfo=TRUE )
  
  # Merge an .xdf file and a data frame into a new .xdf file
  
  # .xdf file names
  indXDF <- file.path(tempdir(), ".rxTempIn.xdf")
  outXDF <- file.path(tempdir(), ".rxTempOut.xdf")
  
  indData <- data.frame(id = 1:12, state = rep(c("CA","OR", "WA"), times = 4))
  # Put individual-level data frame in .xdf file
  rxDataStep(inData = indData, outFile = indXDF, overwrite=TRUE)
  
  # Create state-level data frame
  stateData <- data.frame(state=c("CA","OR", "WA"), stateVal = c(1000, 400, 500))
  
  # Merge individual-level .xdf file with state-level data frame
  rxMerge(inData1 = indXDF, inData2 = stateData, outFile = outXDF,
      matchVars = "state")
      
  # Re-sort data by id
  rxSort(inData = outXDF, outFile = outXDF, sortByVars = "id", overwrite = TRUE)
      
  # Look at merged, sorted data
  df4 <- rxDataStep(inData = outXDF)
  df4
 
```
 
 
