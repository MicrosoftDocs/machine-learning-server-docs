--- 
 
# required metadata 
title: "Deprecated functions in RevoScaleR" 
description: "These functions are provided for compatibility with  older versions of RevoScaleR only, and may be defunct as soon as the next release." 
keywords: "RevoScaleR, RevoScaleR-deprecated, rxGetNodes, RxHpcServer, rxImportToXdf, rxDataStepXdf, rxDataFrameToXdf, rxXdfToDataFrame, rxSortXdf" 
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
 
 #`RevoScaleR-deprecated`: Deprecated functions in RevoScaleR

 Applies to version 9.0.1 of package RevoScaleR.
 
 ##Description
 
These functions are provided for compatibility with 
older versions of RevoScaleR only, and may be defunct as soon as the next release. 
 
 
 ##Usage

```   
  rxGetNodes(headNode, includeHeadNode = FALSE, makeRNodeNames = FALSE, getWorkersOnly=TRUE) 
  RxHpcServer(object, headNode = "", revoPath = NULL, shareDir = "", workingDir = NULL,
              dataPath = NULL, outDataPath = NULL, wait = TRUE, consoleOutput = FALSE, 
              configFile = NULL, nodes = NULL, computeOnHeadNode = FALSE, minElems = -1, maxElems = -1,
              priority = 2, exclusive = FALSE, autoCleanup = TRUE, dataDistType = "all", 
              packagesToLoad = NULL, email = NULL, resultsTimeout = 15, groups = "ComputeNodes"
              )
                
  rxImportToXdf(inSource, outSource, rowSelection = NULL, 
          transforms = NULL, transformObjects = NULL,
          transformFunc = NULL, transformVars = NULL, 
          transformPackages = NULL, transformEnvir = NULL,
          append = "none", overwrite = FALSE, numRows = -1,
          maxRowsByCols = NULL,
          reportProgress = rxGetOption("reportProgress"),
          verbose = 0, 
          xdfCompressionLevel = rxGetOption("xdfCompressionLevel"),
          createCompositeSet = NULL,
          blocksPerCompositeFile = 3
          )
  
  rxDataStepXdf(inFile, outFile, varsToKeep = NULL, varsToDrop = NULL,
          rowSelection = NULL, transforms = NULL, transformObjects = NULL,
          transformFunc = NULL, transformVars = NULL, 
          transformPackages = NULL, transformEnvir = NULL,
          append = "none", overwrite = FALSE, removeMissingsOnRead = FALSE, removeMissings = FALSE, 
          computeLowHigh = TRUE, maxRowsByCols = 3000000, 
          rowsPerRead = -1, startRow = 1, numRows = -1, 
          startBlock = 1, numBlocks = -1, returnTransformObjects = FALSE,
          inSourceArgs = NULL, blocksPerRead = rxGetOption("blocksPerRead"),
          reportProgress = rxGetOption("reportProgress"), 
          xdfCompressionLevel = rxGetOption("xdfCompressionLevel"),
          checkVarsToKeep = TRUE, cppInterp = NULL)    
          
  rxDataFrameToXdf(data, outFile, varsToKeep = NULL, varsToDrop = NULL,
                   rowVarName = NULL, append = "none", overwrite = FALSE,
                   computeLowHigh = TRUE,  
                   xdfCompressionLevel = rxGetOption("xdfCompressionLevel")) 
                   
  rxXdfToDataFrame(file, varsToKeep = NULL, varsToDrop = NULL, rowVarName = NULL,
                   rowSelection = NULL, transforms = NULL, transformObjects = NULL,
                   transformFunc = NULL, transformVars = NULL, 
                   transformPackages = NULL, transformEnvir = NULL,  
                   removeMissings = FALSE, stringsAsFactors = FALSE,
                   blocksPerRead = rxGetOption("blocksPerRead"),
                   maxRowsByCols = 3000000,
                   reportProgress = rxGetOption("reportProgress"),
                   cppInterp = NULL)   
                   
  rxSortXdf(inFile, outFile, sortByVars, decreasing = FALSE,
            type = "auto", missingsLow = TRUE, caseSensitive = FALSE,
            removeDupKeys = FALSE, varsToKeep = NULL, varsToDrop = NULL, 
            dupFreqVar = NULL, overwrite = FALSE, bufferLimit = -1, 
            reportProgress = rxGetOption("reportProgress"), 
            verbose = 0, xdfCompressionLevel = rxGetOption("xdfCompressionLevel"),
            blocksPerRead = -1,  ...)                                        		
                  
 
```
 
 ##Arguments

   
     
 ### `inSource`
 a non-RxXdfData RxDataSource object representing the input data source or a non-empty character string representing a file path. If a character string is supplied, the type of file is inferred from its extension, with the default being a text file. 
  
  
    
 ### `file`
 either an RxXdfData object or a character string specifying the .xdf file. 
  
  
    
 ### `outSource`
 an RxXdfData object or non-empty character string representing the output .xdf file. 
  
  
    
 ### `outFile`
 a character string specifying the output .xdf file, an  [RxXdfData](RxXdfData.md) object, a [RxOdbcData](RxOdbcData.md) data source, or a [RxTeradata](RxTeradata.md) data source.  If `NULL`, a data frame will be returned from `rxDataStep` unless `returnTransformObjects` is set to `TRUE`. Setting `outFile` to `NULL` and `returnTransformObjects=TRUE` allows chunkwise computations on the data without modifying the existing data or creating a new data set. `outFile` can also be a delimited [RxTextData](RxTextData.md) data source if using a native file system and not appending. 
  
  
    
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
 character vector defining additional R packages (outside of those specified in `rxGetOption("transformPackages")`) to be made available and  preloaded for use in variable transformation functions, e.g., those explicitly defined in **RevoScaleR** functions via their `transforms` and `transformFunc` arguments or those  defined implicitly via their `formula` or `rowSelection` arguments.  The `transformPackages` argument may also be `NULL`,  indicating that no packages outside `rxGetOption("transformPackages")` will be preloaded. 
  
  
    
 ### `transformEnvir`
 user-defined environment to serve as a parent to  all environments developed internally and used for variable data transformation. If `transformEnvir = NULL`, a new "hash" environment with parent `baseenv()` is used instead. 
  
  
    
 ### `append`
 either `"none"` to create a new .xdf file or `"rows"` to append rows to an existing .xdf file. If `outSource` exists and `append` is `"none"`, the `overwrite` argument must be set to `TRUE`. 
  
  
    
 ### `overwrite`
 logical value. If `TRUE`, the existing `outSource` will be overwritten. 
  
  
    
 ### `numRows`
 integer value specifying the maximum number of rows to import. If set to -1, all rows will be imported. 
  
  
    
 ### `maxRowsByCols`
 the maximum size of a data frame that will be read in if `outData` is set to `NULL`, measured by the number of rows times the number of columns. If the number of rows times the number of columns being imported exceeds this,  a warning will be reported and a smaller number of rows will be read in than requested. If `maxRowsByCols` is set to be too large, you may experience problems  from loading a huge data frame into memory. 
  
  
    
 ### `reportProgress`
 integer value with options:  
*   `0`: no progress is reported. 
*   `1`: the number of processed rows is printed and updated. 
*   `2`: rows processed and timings are reported. 
*   `3`: rows processed and all timings are reported. 
  
  
  
    
 ### `verbose`
 integer value. If `0`, no additional output is printed.  If `1`, information on the import type is printed. 
   
  
     
 ### `xdfCompressionLevel`
 integer in the range of -1 to 9.  The higher the value, the greater the  amount of compression - resulting in smaller files but a longer time to create them. If  `xdfCompressionLevel` is set to 0, there will be no compression and files will be compatible  with the 6.0 release of Revolution R Enterprise.  If set to -1, a default level of compression  will be used. 
  
  
     
 ### `createCompositeSet`
 logical value or `NULL`. If `TRUE`, a composite set of files will be created instead of a single .xdf file. A directory will be created whose name is the same as the .xdf file that would otherwise be created, but with no extension. Subdirectories data and metadata will be created. In the data subdirectory, the data will be split across a set of .xdfd files (see `blocksPerCompositeFile` below for determining how many blocks of data will be in each file). In the metadata subdirectory  there is a single .xdfm file, which contains the meta data for all of the  .xdfd files in the  data subdirectory. When the compute context is `RxHadoopMR` a composite set of files is always created. 
  
  
    
 ### `blocksPerCompositeFile`
 integer value. If `createCompositeSet=TRUE`, and if the compute context is not `RxHadoopMR`, this will be the number of blocks put into each .xdfd file in the composite set. When importing is being done on Hadoop using MapReduce, the number of rows per .xdfd file is determined by the rows assigned to each MapReduce task, and the number of blocks per .xdfd file is therefore determined by `rowsPerRead`. If the `outSource` is an [RxXdfData](RxXdfData.md) object, set the value for `blocksPerCompositeFile` there instead. 
   
  
  
    
 ### `inFile`
 either an RxXdfData object or a character string specifying the input .xdf file. 
  
  
    
 ### `varsToKeep`
 character vector of variable names to include when reading from the input data file. If `NULL`, argument is ignored. Cannot be used with `varsToDrop` or when `outFile` is the same as the input data file. Variables used in transformations or row selection will be retained even if not specified in `varsToKeep`. If `newName` is used in `colInfo` in a non-xdf  data source, the `newName` should be used in `varsToKeep`. Not supported for [RxTeradata](RxTeradata.md), [RxOdbcData](RxOdbcData.md), or [RxSqlServerData](RxSqlServerData.md) data sources. 
  
  
    
 ### `varsToDrop`
 character vector of variable names to exclude when reading from the input data file. If `NULL`, argument is ignored. Cannot be used with `varsToKeep` or when `outFile` is the same as the input data file. Variables used in transformations or row selection will be retained even if specified in `varsToDrop`. If `newName` is used in `colInfo` in a non-xdf  data source, the `newName` should be used in `varsToDrop`. Not supported for [RxTeradata](RxTeradata.md), [RxOdbcData](RxOdbcData.md), or  [RxSqlServerData](RxSqlServerData.md) data sources. 
  
  
    
 ### `removeMissingsOnRead`
 logical value. If `TRUE`, rows with missing values will be removed on read. 
  
  
    
 ### `removeMissings`
 logical value. If `TRUE`, rows with missing values will not be included in the output data. 
  
  
    
 ### `computeLowHigh`
 logical value. If `FALSE`, low and high values will not automatically be computed. This should only be set to `FALSE` in special circumstances, such as when `append` is being used repeatedly. Ignored for data frames. 
  
  
    
 ### `rowsPerRead`
 number of rows to read for each chunk of data read from the input data source. Use this argument for finer control of the number of rows per block in the output data source. If greater than 0, `blocksPerRead` is ignored. Cannot be used if `inFile` is the same as `outFile`. The default value of `-1` specifies that data should be read by blocks according to the `blocksPerRead` argument. 
  
  
    
 ### `startRow`
 the starting row to read from the input data source.   Cannot be used if `inFile` is the same as `outFile`. 
   
  
    
 ### `returnTransformObjects`
 logical value.  If `TRUE`,  the list of `transformObjects` will be returned instead of  a data frame or data source object.  If the input `transformObjects` have been modified, by using `.rxSet` or `.rxModify` in the `transformFunc`, the updated values will be returned.  Any data returned from the `transformFunc` is ignored. If no `transformObjects` are used, `NULL` is returned. This argument allows for user-defined  computations within a `transformFunc` without creating new data. `returnTransformObjects` is not supported in distributed compute contexts  such as [RxHadoopMR](RxHadoopMR.md) or [RxInTeradata](RxInTeradata.md). 
   
  
    
 ### `startBlock`
 starting block to read. Ignored if `startRow` is set to greater than 1.   
  
  
    
 ### `numBlocks`
 number of blocks to read; all are read if set to -1. Ignored if `numRows` is not set to -1.  
  
  
    
 ### `blocksPerRead`
 number of blocks to read for each chunk of data read from the data source. Ignored for data frames or if `rowsPerRead` is positive. 
   
  
    
 ### `inSourceArgs`
 an optional list of arguments to be applied to the input data source. 
   
  
    
 ### `checkVarsToKeep`
 logical value.  If `TRUE` variable names specified in `varsToKeep` will be checked against variables in the data set to make sure they exist.  An error will be reported if not found. Ignored if more than 500 variables in the data set. 
  
  
 ### `headNode`
 A compute context (preferred), a `jobInfo` object, or (deprecated) a character scalar containing the name of the head node of a Microsoft HPC cluster. 
  
  
  
 ### `includeHeadNode`
 logical scalar.  Indicates whether to include the name of the head node in the list of returned nodes. 
  
  
  
 ### `makeRNodeNames`
 Determines if the names of the nodes should be normalized for use as R variables.   See also [rxMakeRNodeNames](rxMakeRNodeNames.md) for details on name mangeling. 
  
  
  
 ### `getWorkersOnly`
 logical.  If `TRUE`, returns only those nodes within the cluster that are configured to actually execute jobs (where applicable). 
  
  
  
 ### `object`
 object of class RxHpcServer. This argument is optional. If supplied, the values of  the other specified arguments are used to replace those of `object` and the modified object is returned. 
  
  
    
 ### `revoPath`
 character string specifying the path to the directory on the cluster  nodes containing the files R.exe and Rterm.exe.  The invocation of R on each  node must be identical (this is an HPC constraint). See the Details section for more  information regarding the path format. 
  
  
    
 ### `shareDir`
 character string specifying the directory on the head node that is  shared among all the nodes of the cluster and any client host. You must have permissions to read and write  in this directory. See the Details section for more information regarding the path format. 
  
  
    
 ### `workingDir`
 character string specifying a working directory for the processes  on the cluster.  If `NULL`, will default to the standard Windows user directory (that is, the value of the environment variable `USERPROFILE`). 
  
  
    
 ### `dataPath`
 character vector defining the search path(s) for the data source(s). See the Details section for more information regarding the path format. 
  
  
    
 ### `outDataPath`
 `NULL` or character vector defining the search path(s) for   new output data file(s).  If not `NULL`, this overrides any specification for `dataPath` in [rxOptions](/rxOptions.md)  
   
  
    
 ### `wait`
 logical value. If `TRUE`, the job will be blocking and will not return until   it has completed or has failed. If `FALSE`, the job will be non-blocking return immediately,  allowing you to continue running other R code. The object `rxgLastPendingJob` is created with the job information. You can pass this object to the   [rxGetJobStatus](rxGetJobResults.md) function to check on the processing status of the job.  [rxWaitForJob](rxWaitForJob.md) will change a non-waiting job  to a waiting job. Conversely, pressing ESC changes a waiting job to a non-waiting job, provided that the HPC scheduler has accepted the job. If you press ESC before the job has been accepted, the job is canceled. 
  
  
    
 ### `consoleOutput`
 logical scalar. If `TRUE`, causes the standard output  of the R processes to be printed to the user console.  
  
  
    
 ### `configFile`
 character string specifying the path to the XML template (on your  local computer) that will be consumed by the job scheduler. If `NULL` (the default),  the standard template is used. See the Details section for more information regarding  the path format. We recommend that the user rely upon the default setting for `configFile`. 
  
  
    
 ### `nodes`
 character string containing a comma-separated list of requested  nodes, e.g., `"compute1,compute2,compute3"`, or a character vector like `c("compute1", "compute2", "compute3")`. If you specify the nodes to be used,  `minElems` and `maxElems` are ignored if greater than the number of specified nodes. See the Details section for more information.  The nodes parameter forces use of the specified nodes, rather than simply requesting them.  This feature is provided so that jobs requiring only 10 out of 100 nodes, for example,  do not force you to deploy your data to all 100 nodes.   Should you need finer control, you will need to generate a new XML template through the MS  HPC job scheduler. See the Microsoft HPC Server 2008 documentation for details.   
  
  
    
 ### `computeOnHeadNode`
 If `FALSE` and nodes are automatically being selected, the head  node of the cluster will not be among the nodes to be used.  Furthermore, if `FALSE` and the  head node is explicitly specified in the node list, a warning is issued, but the job proceeds with  a warning and the head node excluded from the node list.  If `TRUE`,  then the head node is treated exactly as any other node for the purpose of job resource allocations. Note that setting this flag to `TRUE` does not guarantee inclusion of the head node in a job; it simply allows the head node to be listed in an explicit list, or to be included by automatic node usage generation. 
  
  
    
 ### `minElems`
 minimum number of nodes required for a run. If `-1`, the value for `maxElems` is used. If both `minElems` and `maxElems` are `-1`, the number of nodes specified in the `nodes` argument is used. See the Details section for more information. 
  
  
    
 ### `maxElems`
 maximum number of nodes required for a run. If `-1`, the value for `minElems` is used. If both `minElems` and `maxElems` are `-1`, the number of nodes specified in the `nodes` argument is used. See the Details section for more information. 
  
  
    
 ### `priority`
 The priority of the job.  Allowable values are 0 (lowest), 1 (below normal), 2 (normal, the default), 3 (above normal), and 4 (highest). See the Microsoft HPC Server 2008 documentation  for using job scheduling policy options and job templates to manage job priorities. These policy options may also affect the behavior of the `exclusive` argument.   
  
    
 ### `exclusive`
 logical value. If `TRUE`, no other jobs can run on a compute node at the same time as this job. This may fail if you do not have administrative privileges  on the cluster, and may also fail depending on the settings of your job scheduling policy options. 
  
  
    
 ### `autoCleanup`
 logical scalar. If `TRUE`, the default behavior is to clean up the  temporary computational artifacts and delete the result objects upon retrival.  If `FALSE`,  then the computational results are not deleted, and the results may be acquired using  [rxGetJobResults](rxGetJobResults.md), and the output via [rxGetJobOutput](rxGetJobOutput.md) until the  [rxCleanupJobs](rxCleanup.md) is used to delete the results and other artifacts. Leaving this flag set to `FALSE` can result in accumulation of compute artifacts which you may eventually need to delete before they fill up your hard drive.If you set `autoCleanup=TRUE`and experience performance degradation on a Windows XP client, consider  setting `autoCleanup=FALSE`. 
  
  
    
 ### `dataDistType`
 a character string denoting how the data has been distributed. Type `"all"` means that the entire data set has been copied to each compute node. Type `"split"` means that the data set has been partitioned and that each compute node contains a different set of rows. 
  
  
    
 ### `packagesToLoad`
 optional character vector specifying additional packages to be  loaded on the nodes when jobs are run in this compute context.  
  
  
    
 ### `email`
 optional character string specifying an email address to which a job complete email should be sent.  Note that the cluster administrator will have to enable email notifications for such job completion mails to be received. 
  
  
    
 ### `resultsTimeout`
 A numeric value indicating for how long attempts should be made  to retrieve results from the cluster.  Under normal conditions, results are available immediately upon completion of the job.   However, under certain high load conditions, the processes on the nodes have reported as completed, but the results have not been fully committed to disk by the operating system.  Increase this parameter if results retrievial is failing on high load clusters. 
  
  
    
 ### `groups`
 Optional character vector specifying the groups from which nodes should be selected.  If `groups`is specified and `nodes` is `NULL`, all the nodes in the groups specified will be candidates for computations.   If both `nodes` and `groups` are specified, the candidate nodes will be the intersection of the set of nodes  explicitly specified in `nodes` and the set of all the nodes in the `groups` specified.    Note that the default value of `"ComputeNodes"` is the name of the Microsoft defined group that includes all physically present nodes.  Another Microsoft default group name used is `"HeadNodes"` which includes all nodes set up to act as head nodes.  While these  default names can be changed, this is not recommended.  Note also that [rxGetNodeInfo](rxGetNodeInfo.md) will honor group filtering.   See the help for `rxGetNodeInfo` for more information. 
  
  
    
 ### `data`
 data frame to put into .xdf file. See details section for a listing of the supported column types in the data frame. 
  
  
    
 ### `rowVarName`
 character string or `NULL`. If `NULL`, the data frame's row names will be dropped. If a character string, an additional variable of that name will be added to the data set containing the data frame's row names.  
  
  
    
 ### `stringsAsFactors`
 logical indicating whether or not to convert strings into factors in R. 
   
  
    
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
  
  
    
 ### `cppInterp`
 NOT SUPPORTED information to be sent to the C++ interpreter. 
  
  
    
 ### ` ...`
 additional arguments to be passed to the input data source. 
  
  
 
 
 ##Details
 
Use [rxImport](rxImport.md) instead of `rxImportToXdf`.
Use [rxDataStep](rxDataStep.md) instead of  `rxDataStepXdf`.
Use [rxDataStep](rxDataStep.md) instead of  `rxDataFrameToXdf`.
Use [rxDataStep](rxDataStep.md) instead of  `rxXdfToDataFrame`.
Use [rxSort](rxSortXdf.md) instead of  `rxSortXdf`.
Use [rxGetAvailableNodes](rxGetAvailableNodes.md) instead of  `rxGetNodes`.
 
 
 ##Value
 
For `rxSortXdf`: If sorting is successful, `TRUE`
is returned; otherwise `FALSE`is returned.
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[rxImport](rxImport.md),
[rxDataStep](rxDataStep.md),
["RevoScaleR-defunct"](RevoScaleR-defunct.md).
   
 
 
