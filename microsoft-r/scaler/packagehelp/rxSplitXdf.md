--- 
 
# required metadata 
title: "Split a Single Data Set into Multiple Sets" 
description: "    Split an input .xdf file or data frame into multiple .xdf files or a list of data frames. " 
keywords: "RevoScaleR, rxSplitXdf, rxSplit, manip, file" 
author: "richcalaway" 
manager: "jhubbard" 
ms.date: "04/03/2017" 
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
 
 
 
 #`rxSplitXdf`: Split a Single Data Set into Multiple Sets

 Applies to version 9.0.1 of package RevoScaleR.
 
 ##Description
 
Split an input .xdf file or data frame into multiple .xdf files or a list of data frames.
 
 
 ##Usage

```   
  rxSplit(inData, outFilesBase = NULL, outFileSuffixes = NULL,  
             numOut = NULL, splitBy = "rows", splitByFactor = NULL,
             varsToKeep = NULL, varsToDrop = NULL, rowSelection = NULL, 
             transforms = NULL, transformObjects = NULL, 
             transformFunc = NULL, transformVars = NULL, 
             transformPackages = NULL, transformEnvir = NULL, 
             overwrite = FALSE, removeMissings = FALSE, rowsPerRead = -1, 
             blocksPerRead = 1, updateLowHigh = FALSE, maxRowsByCols = NULL,
             reportProgress = rxGetOption("reportProgress"), verbose = 0,
             xdfCompressionLevel = rxGetOption("xdfCompressionLevel"),  ...)
      
  rxSplitXdf(inFile, outFilesBase = basename(rxXdfFileName(inFile)), outFileSuffixes = NULL,  
             numOutFiles = NULL, splitBy = "rows", splitByFactor = NULL,
             varsToKeep = NULL, varsToDrop = NULL, rowSelection = NULL, 
             transforms = NULL, transformObjects = NULL, 
             transformFunc = NULL, transformVars = NULL, 
             transformPackages = NULL, transformEnvir = NULL, 
             overwrite = FALSE, removeMissings = FALSE,
             rowsPerRead = -1, blocksPerRead = 1, updateLowHigh = FALSE,
             reportProgress = rxGetOption("reportProgress"), verbose = 0, 
             xdfCompressionLevel = rxGetOption("xdfCompressionLevel"), ...)
 
```
 
 ##Arguments

   
    
 ### `inData`
 a data frame, a character string defining the path to the input .xdf file,  or an [RxXdfData](RxXdfData.md) object  
  
  
    
 ### `inFile`
  a character string defining the path to the input .xdf file,  or an [RxXdfData](RxXdfData.md) object 
  
  
    
 ### `outFilesBase`
 a character string or vector defining the file names/paths to use in forming the the output .xdf files. These names/paths will be embellished with any specified `outFileSuffixes`. For `rxSplit`, `outFilesBase = NULL` means that the default value for .xdf and [RxXdfData](RxXdfData.md)`inData` objects will be `basename(rxXdfFileName(inData))` while it will be a blank string for `inData` a data frame.   
  
  
    
 ### `outFileSuffixes`
 a vector containing the suffixes to append to the base name(s)/path(s) specified in `outFilesBase`. Before appending, `outFileSuffixes` is converted to class character via the as.character function. If `NULL`, `seq(numOutFiles)` is used in its place if  `numOutFiles` is not `NULL`. 
  
  
    
 ### `numOut`
 number of outputs to create, e.g., the number of output files or number of data frames returned in the output list. 
  
  
    
 ### `numOutFiles`
 number of files to create. This argument is used only when `outFilesBase` and `outFileSuffixes` do not provide sufficient information to form unique paths to multiple output files. See the Examples section for its use. 
  
  
    
 ### `splitBy`
 a character string denoting the method to use in partitioning the `inFile` .xdf file. With  `numFiles` defined as the number of output files (formed by the combination of  `outFilesBase`, `outFileSuffixes`, and `numOutFiles` arguments), the supported values for `splitBy` are:   
* `"rows"` - To the extent possible, a uniform partition of the number of *rows* in the `inFile` .xdf file is performed. The minimum number of rows in each output  file is defined by `floor(numRows/numFiles)`, where `numRows` is the number of rows in `inFile`.  Additional rows will be distributed uniformly amongst the last set of files.  
* `"blocks"` - To the extent possible, a uniform partition of the number of *blocks* in the `inFile` .xdf file is performed. If `blocksPerRead = 1`,  the minimum number of blocks in each output  file is defined by `floor(numBlocks/numFiles)`, where `numBlocks` is the number of blocks in `inFile`.  Additional blocks will be distributed uniformly amongst the last set of files. For `blocksPerRead > 1`, the number of blocks in each output file is reduced accordingly since multiple blocks  read at one time are collapsed to a single block upon write. 
 This argument is ignored if `splitByFactor` is a valid factor variable name.  
  
  
    
 ### `splitByFactor`
 character string identifying the name of the factor to use in splitting the `inFile` .xdf data such that each file contains the data corresponding to a single level of the factor. The `splitBy` argument is ignored if `splitByFactor` is a valid factor variable name.  If `splitByFactor = NULL`, the `splitBy` argument is used to define the split method.   
  
  
    
 ### `varsToKeep`
 character vector of variable names to include when reading from the input data file. If `NULL`, argument is ignored. Cannot be used with `varsToDrop`. 
  
  
    
 ### `varsToDrop`
 character vector of variable names to exclude when reading from the input data file. If `NULL`, argument is ignored. Cannot be used with `varsToKeep`. 
  
  
    
 ### `rowSelection`
 name of a logical variable in the data set (in quotes) or a logical expression using variables in the data set to specify row selection.  For example, `rowSelection = "old"` will use only observations in which the value of the variable `old` is `TRUE`.  `rowSelection = (age > 20) & (age < 65) & (log(income) > 10)` will use only observations in which the value of the `age` variable is between 20 and 65 and the value of the `log` of the `income` variable is greater than 10.  The row selection is performed after processing any data transformations  (see the arguments `transforms` or `transformFunc`). As with all expressions, `rowSelection` can be defined outside of the function  call using the expression function. 
  
  
    
 ### `transforms`
 an expression of the form `list(name = expression, ...)` representing the first round of variable transformations. As with all expressions, `transforms` (or `rowSelection`)  can be defined outside of the function call using the expression function. 
  
  
    
 ### `transformObjects`
 a named list containing objects that can be referenced by `transforms`, `transformsFunc`, and `rowSelection`. Ignored for  data frames. 
  
  
    
 ### `transformFunc`
 variable transformation function. See [rxTransform](rxTransform.md) for details. 
  
  
    
 ### `transformVars`
 character vector of input data set variables needed for the transformation function. See [rxTransform](rxTransform.md) for details. 
  
  
    
 ### `transformPackages`
 character vector defining additional R packages (outside of those specified in `rxGetOption("transformPackages")`) to be made available and  preloaded for use in variable transformation functions, e.g., those explicitly defined in **RevoScaleR** functions via their `transforms` and `transformFunc` arguments or those  defined implicitly via their `formula` or `rowSelection` arguments.  The `transformPackages` argument may also be `NULL`,  indicating that no packages outside `rxGetOption("transformPackages")` will be preloaded. 
  
  
    
 ### `transformEnvir`
 user-defined environment to serve as a parent to  all environments developed internally and used for variable data transformation. If `transformEnvir = NULL`, a new "hash" environment with parent `baseenv()` is used instead. 
  
  
    
 ### `overwrite`
 logical value. If `TRUE`, an existing `outFile` will be overwritten, or if appending columns existing columns with the same name will be overwritten. `overwrite` is ignored if appending rows. Ignored for data frames. 
  
  
    
 ### `removeMissings`
 logical value. If `TRUE`, rows with missing values will not be included in the output data. 
  
  
    
 ### `rowsPerRead`
 number of rows to read for each chunk of data read from the input data source. Use this argument for finer control of the number of rows per block in the output data source. If greater than 0, `blocksPerRead` is ignored. Cannot be used if `inFile` is the same as `outFile`. 
  
  
    
 ### `blocksPerRead`
 number of blocks to read for each chunk of data read from the data source. Ignored for data frames or if `rowsPerRead` is positive. 
  
  
    
 ### `updateLowHigh`
 logical value. If `FALSE`, the low and high values for each variable in the `inFile` .xdf file will be copied to the header of each output file so that  all output files reflect the global range of the data. If `TRUE`, each variable's low and high values are updated to reflect the range of values in the corresponding file, likely resulting in different low and  high values for the same variable between output files. Note that when using `rxSplit` and the output is a list of data frames, the `updateLowHigh` argument has no effect. 
  
  
    
 ### `maxRowsByCols`
 argument sent directly to the [rxDataStep](rxDataStep.md) function behind the scenes when converting the output .xdf files (back) into a list of data frames. This parameter is provided primarily for the case where `rxSplit` is being used to split a .xdf file or `RxXdfData` data source into portions that are then read back into R as  a list of data frames. In this case, `maxRowsByCols` provides a mechanism for the user to control the maximum number of of elements read from the output .xdf file in an effort to limit the amount of memory needed for storing each partition  as a data frame object in R. 
  
  
    
 ### `reportProgress`
 integer value with options:  
   *   `0`: no progress is reported. 
   *   `1`: the number of processed rows is printed and updated. 
   *   `2`: rows processed and timings are reported. 
   *   `3`: rows processed and all timings are reported. 
  
  
  
    
 ### `verbose`
 integer value. If `0`, no additional output is printed.  If `1`, additional summary information is printed. 
  
  
     
 ### `xdfCompressionLevel`
 integer in the range of -1 to 9.  The higher the value, the greater the  amount of compression - resulting in smaller files but a longer time to create them. If  `xdfCompressionLevel` is set to 0, there will be no compression and files will be compatible  with the 6.0 release of Revolution R Enterprise.  If set to -1, a default level of compression  will be used. 
   
  
    
 ### ` ...`
 additional arguments to be passed directly to the function used to partition the data. For example,  
* `Uniform Partition` - a `fill` argument with supported values are `"left"`, `"center"`, or `"right"`, can be used to place the remaining rows/blocks in the set of output files. For example, if `sortBy = "blocks"`, `inFile` has `15` blocks, and the number of output files is `6`, the distribution of blocks for the set of `6` output files will be:  
* `fill = "left"` - 3 3 3 2 2 2  
* `fill = "center"` - 2 3 3 3 2 2  
* `fill = "right"` - 2 2 2 3 3 3  
   
  
  
 
 
 
 ##Details
 
**rxSplit:** Use `rxSplit` as a general function to partition a data frame or .xdf file.
Behind the scenes, the `rxSplitXdf` function is called by `rxSplit` after converting the `inData` data source into
a (temporary) .xdf file. If both `outFilesBase` and `outFileSuffixes` are `NULL` (the default),
then the type of output is determined by the type of inData: if the `inData` is an .xdf file name 
or an [RxXdfData](RxXdfData.md) object, and list of `RxXdfData` data sources representing the new 
split .xdf files is returned.  If the `inData` is a data frame, then the output is returned 
as a list of data frames. 
If `outFilesBase` is an empty character string and `outFileSuffixes` is `NULL`, a list of 
data frames is always returned.
In the case that a list of data frames is returned, the names of the list are in `shortFileName.splitType.NumberOrFactor`
format, e.g., `iris.Species.versicolor` or `myXdfFile.rows.3`.

**rxSplitXdf:** Use `rxSplitXdf` to partition an .xdf file.
The `inFile` .xdf file is uniformly partitioned (to the extent possible) and each partition is 
written to a distinct user-specified file. As an example, if `inFile` contains a million rows, `splitBy = "rows"`, 
and the number of output files we specify to create is five, then each output file will contain two hundred 
thousand observations, assuming none were dropped via a `rowSelection` argument specification.
The number of output files is controlled either directly or indirectly through a combination of  
arguments: `inFile`, `outFilesBase`, `outFileSuffixes`, and `numOutFiles`.  
In general, the values of these arguments are pasted together to form a character vector of output file paths, 
with scalar values auto-expanded to vectors (of an appropriate size) prior to pasting.
This scheme allows the user to either specify directly the full paths to the output files they wish 
to form or do so implicitly (and perhaps more conveniently) using various combinations of these arguments.
We illustrate the use of these combinations in the examples below, which are assumed to be run under Windows. 



###``
 | Col  1 | Col  2 | Col  3 |
| :---| :---| :--- |
|  **INPUT ARGUMENTS**  |   |  **OUTPUT FILE VECTOR**  |
|  `inFile = "foo.xdf", numOutFiles = 3`  |   |  `"foo1.xdf", "foo2.xdf", "foo3.xdf"`  |
|  `outFilesBase = "out", numOutFiles = 4`  |   |  `"out1.xdf", "out2.xdf", "out3.xdf", "out4.xdf"`  |
|  `outFilesBase = "golf", outFileSuffixes = c("club", "tee", "score")`  |   |  `"golfclub.xdf", "golftee.xdf", "golfscore.xdf"`  |
|  `outFilesBase = "C:\\myDir\\", outFileSuffixes = c("a\\same", "b\\same", "c\\same")`  |   |  `"C:\\myDir\\a\\same.xdf", "C:\\myDir\\b\\same.xdf", "C:\\myDir\\c\\same.xdf"`  |
 `outFilesBase = c("C:\\here\\file1.xdf", "D:\\there\\file2.xdf", "E:\\everywhere\\file3.xdf")`  |   |  `"C:\\here\\file1.xdf", "D:\\there\\file2.xdf", "E:\\everywhere\\file3.xdf"`  



 
 
 
 ##Value
 
a list of data frames or an invisible list of `RxXdfData` data source objects corresponding to the created output files.
 
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[rxDataStep](rxDataStep.md),
[rxImport](rxImport.md),
[rxTransform](rxTransform.md)
   
 ##Examples

 ```
   
  #####
  # rxSplit Examples
  
  # DF -> DF : Data frame input to list of data frames
  IrisDFList <- rxSplit(iris, numOut = 3, reportProgress = 0, verbose = 0)
  names(IrisDFList) 
  head(IrisDFList[[3]]) 
  
  IrisDFList <- rxSplit(iris, splitByFactor = "Species", reportProgress = 0, verbose = 0)
  names(IrisDFList) 
  head(IrisDFList[[1]])
  
  # DF -> XDF : Data frame input to .xdf outputs (list of RxXdfData data sources)
  irisXDFs <- rxSplit(iris, splitByFactor = "Species", outFilesBase = file.path(tempdir(),"iris"), 
          reportProgress = 0, verbose = 0, overwrite = TRUE)
  print(irisXDFs)
  invisible(sapply(irisXDFs, function(x) if (file.exists(x@file)) unlink(x@file)))
  
  # XDF -> DF : .xdf file to a list of data frames
  # Return a list of data frames instead of creating .xdf files by specifying
  # outFilesBase = ""
  XDF <- file.path(tempdir(), "iris.xdf")
  rxDataStep(iris, XDF, overwrite = TRUE)
  IrisDFList <- rxSplit(XDF, splitByFactor = "Species", outFilesBase = "",
     reportProgress = 0, verbose = 0, overwrite = TRUE) 
  names(IrisDFList) 
  head(IrisDFList[[1]]) 
  if (file.exists(XDF)) file.remove(XDF)
  
  # Split the fourth graders data by gender and use row selection
  # to collect information only on blue eyed children
  fourthGradersXDF <- file.path(rxGetOption("sampleDataDir"), "fourthgraders.xdf") 
  rxSplit(fourthGradersXDF, splitByFactor = "male", outFilesBase = "", 
  	rowSelection = (eyecolor == "Blue")) 	
  
  # XDF -> XDF : .xdf file into multiple .xdf files
  XDF <- tempfile(pattern = "iris", fileext = ".xdf")
  outXDF1 <- tempfile(pattern = "irisOut", fileext = ".xdf")
  outXDF2 <- tempfile(pattern = "irisOut", fileext = ".xdf")
  rxDataStep(iris, XDF)
  outFiles <- rxSplit(XDF, outFilesBase = tempdir(), outFileSuffixes = basename(c(outXDF1, outXDF2)), 
          reportProgress = 0, verbose = 0, overwrite = TRUE)
  print(outFiles)
  
  # Remove created files
  invisible(sapply(outFiles, function(x) if (file.exists(x@file)) unlink(x@file)))
  if (file.exists(XDF)) file.remove(XDF)			
  if (file.exists(outXDF1)) file.remove(outXDF1)			
  if (file.exists(outXDF2)) file.remove(outXDF2)
  
  #####
  # rxSplitXdf Examples
  
  # Split CensusWorkers.xdf data into five files
  # with a (nearly) uniform distribution of rows across the files.
  # Put the files in a temporary directory and use "Census" as the
  # basename for the files. Append each file with "-byRows" and
  # an index indicating the file number in the set.
  inFile <- file.path(rxGetOption("sampleDataDir"), "CensusWorkers")
  outFilesBase <- file.path(tempdir(), "byRowsDir", "Census-byRows")
  rxSplitXdf(inFile, outFilesBase = outFilesBase,
  	       numOutFiles = 5, splitBy = "rows", verbose = 1)
  
  # Obtain information from each of the resulting output files
  # and remove the files along with the parent directory.
  byRowFiles <- list.files(dirname(outFilesBase), full = TRUE)
  infoByRows <- sapply(byRowFiles, rxGetInfo, simplify = FALSE)
  unlink(dirname(outFilesBase), recursive = TRUE)
  
  # Perform a similar split by blocks
  outFilesBase <- file.path(tempdir(), "byBlocksDir", "Census-byBlocks")
  rxSplitXdf(inFile, outFilesBase = outFilesBase,
  	       numOutFiles = 5, splitBy = "blocks", verbose = 1)
  
  # Obtain information from each of the resulting output files
  # and remove the files along with the parent directory.
  byBlockFiles <- list.files(dirname(outFilesBase), full = TRUE)
  infoByBlocks <- sapply(byBlockFiles, rxGetInfo, simplify = FALSE)
  unlink(dirname(outFilesBase), recursive = TRUE)
  
  # Create a barplot comparing the two methods on the resulting
  # number of rows in each output file. The barplot of the
  # "rows" split case is uniform while the "blocks" split is
  # highly non-uniform.
  rows <- sapply(infoByRows, "[[", "numRows")
  blocks <- sapply(infoByBlocks, "[[", "numRows")
  numRowsData <- cbind(rows, blocks)
  barplot(numRowsData, beside = TRUE,
          main = "CensusWorkers Partition: Row Distribution",
          xlab = "sortBy Method",
          ylab = "Number of Rows",
          legend.text = basename(rownames(numRowsData)),
          args.legend = list(x = "topright"),
          ylim = c(0, 1.5 * max(numRowsData)))
  
  # Create a similar plot comparing the number of resulting blocks
  # in the output .xdf files.
  rows <- sapply(infoByRows, "[[", "numBlocks")
  blocks <- sapply(infoByBlocks, "[[", "numBlocks")
  numBlocksData <- cbind(rows, blocks)
  barplot(numBlocksData, beside = TRUE,
          main = "CensusWorkers Partition: Block Distribution",
          xlab = "sortBy Method",
          ylab = "Number of Blocks",
          legend.text = basename(rownames(numBlocksData)),
          args.legend = list(x = "topright"),
          ylim = c(0, 1.5 * max(numBlocksData)))
 
```
 
         
 
 
